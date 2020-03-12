---
title: Azure Active Directory ile bir ASP.NET Core Blazor WebAssembly tek başına uygulamasını güvenli hale getirme
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory
ms.openlocfilehash: 76bcac29d86a236e56c0eaea24a694c4845ecbcf
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083750"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a>Azure Active Directory ile bir ASP.NET Core Blazor WebAssembly tek başına uygulamasını güvenli hale getirme

, [Javier Calvarro Nelson](https://github.com/javiercn) ve [Luke Latham](https://github.com/guardrex) 'e göre

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Kimlik doğrulaması için [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) kullanan bir Blazor WebAssembly tek başına uygulaması oluşturmak için:

[AAD kiracısı ve Web uygulaması oluşturma](/azure/active-directory/develop/v2-overview):

Azure portal **Azure Active Directory** > **uygulama KAYıTLARı** alanına bir AAD uygulaması kaydedin:

1. Uygulama için bir **ad** sağlayın (örneğin, **Blazor istemci AAD**).
1. Desteklenen bir **Hesap türü**seçin. Bu **kuruluş dizininde yalnızca** bu deneyim için hesaplar seçebilirsiniz.
1. **Yeniden yönlendirme URI 'si** açılan listesini **Web**olarak ayarlayın ve `https://localhost:5001/authentication/login-callback`yeniden yönlendirme URI 'si sağlayın.
1. **Yönetici tarafından OpenID ve offline_access izinleri için Izin ver** onay kutusunu > **izinleri** devre dışı bırakın.
1. **Kaydol**’u seçin.

**Kimlik doğrulama** > **Platform yapılandırmalarında** **Web** > :

1. `https://localhost:5001/authentication/login-callback` **yeniden yönlendirme URI 'sinin** mevcut olduğunu onaylayın.
1. **Örtük izin**Için, **erişim belirteçleri** ve **Kimlik belirteçleri**onay kutularını seçin.
1. Uygulamanın kalan varsayılan değerleri bu deneyim için kabul edilebilir.
1. **Kaydet** düğmesini seçin.

Aşağıdaki bilgileri kaydedin:

* Uygulama KIMLIĞI (Istemci KIMLIĞI) (örneğin, `11111111-1111-1111-1111-111111111111`)
* Dizin KIMLIĞI (kiracı KIMLIĞI) (örneğin, `22222222-2222-2222-2222-222222222222`)

Aşağıdaki komutta yer tutucuları, daha önce kaydedilen bilgilerle değiştirin ve komutu bir komut kabuğu 'nda yürütün:

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

Mevcut değilse bir proje klasörü oluşturan çıkış konumunu belirtmek için, çıkış seçeneğini komuta bir yol (örneğin, `-o BlazorSample`) ile birlikte ekleyin. Klasör adı Ayrıca projenin adının bir parçası haline gelir.

## <a name="authentication-package"></a>Kimlik doğrulama paketi

Iş veya okul hesaplarını (`SingleOrg`) kullanmak üzere bir uygulama oluşturulduğunda, uygulama [Microsoft kimlik doğrulama kitaplığı](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`) için bir paket başvurusu otomatik olarak alır. Paket, uygulamanın kullanıcıların kimliğini doğrulamasına ve korunan API 'Leri çağırmak için belirteçleri almasına yardımcı olan bir dizi temel sunar.

Bir uygulamaya kimlik doğrulaması ekliyorsanız, paketi uygulamanın proje dosyasına el ile ekleyin:

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

Önceki paket başvurusunda `{VERSION}`, <xref:blazor/get-started> makalesinde gösterilen `Microsoft.AspNetCore.Blazor.Templates` paketinin sürümü ile değiştirin.

`Microsoft.Authentication.WebAssembly.Msal` paketi, `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paketini uygulamaya göre geçişli olarak ekler.

## <a name="authentication-service-support"></a>Kimlik doğrulama hizmeti desteği

Kullanıcıları kimlik doğrulama desteği, hizmet kapsayıcısına `Microsoft.Authentication.WebAssembly.Msal` paketi tarafından sağlanmış `AddMsalAuthentication` uzantısı yöntemiyle kaydedilir. Bu yöntem, uygulamanın kimlik sağlayıcısıyla (IP) etkileşim kurması için gereken tüm hizmetleri ayarlar.

*Program.cs*:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
});
```

`AddMsalAuthentication` yöntemi, bir uygulamanın kimliğini doğrulamak için gereken parametreleri yapılandırmak için bir geri çağırma işlemini kabul eder. Uygulamanın yapılandırılması için gereken değerler, uygulamayı kaydettiğinizde Azure Portal AAD yapılandırmasından elde edilebilir.

Blazor WebAssembly şablonu, uygulamayı güvenli bir API için erişim belirteci isteyecek şekilde otomatik olarak yapılandırmaz. Oturum açma akışının bir parçası olarak bir belirteç sağlamak için, kapsamı `MsalProviderOptions`varsayılan erişim belirteci kapsamlarına ekleyin:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> Varsayılan erişim belirteci kapsamı `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` biçiminde olmalıdır (örneğin, `11111111-1111-1111-1111-111111111111/API.Access`). Kapsam ayarına bir düzen veya şema ve ana bilgisayar sağlanmışsa (Azure portalında gösterildiği gibi), *istemci uygulaması* *sunucu apı*uygulamasından bir *401 Yetkisiz* yanıt aldığında işlenmeyen bir özel durum oluşturur.

## <a name="index-page"></a>Dizin sayfası

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a>Uygulama bileşeni

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a>RedirectToLogin bileşeni

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>LoginDisplay bileşeni

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a>Kimlik doğrulama bileşeni

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authentication/azure-active-directory/index>
