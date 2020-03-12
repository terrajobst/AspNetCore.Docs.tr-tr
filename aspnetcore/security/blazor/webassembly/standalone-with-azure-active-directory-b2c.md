---
title: Azure Active Directory B2C ile bir ASP.NET Core Blazor WebAssembly tek başına uygulamasını güvenli hale getirme
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: 0ea42943c908d8cf9d083c1cfc568c1835588ce9
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083834"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a>Azure Active Directory B2C ile bir ASP.NET Core Blazor WebAssembly tek başına uygulamasını güvenli hale getirme

, [Javier Calvarro Nelson](https://github.com/javiercn) ve [Luke Latham](https://github.com/guardrex) 'e göre

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Kimlik doğrulaması için [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) kullanan bir Blazor WebAssembly tek başına uygulaması oluşturmak için:

1. Bir kiracı oluşturmak ve bir Web uygulamasını Azure portalında kaydetmek için aşağıdaki konulardaki yönergeleri izleyin:

   * [AAD B2C kiracı oluşturun](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; aşağıdaki bilgileri kaydedin:

     1 \. AAD B2C örneği (örneğin, sonunda eğik çizgi içeren `https://contoso.b2clogin.com/`)<br>
     2 \. AAD B2C kiracı etki alanı (örneğin, `contoso.onmicrosoft.com`)

   * [Bir Web uygulamasını kaydedin](/azure/active-directory-b2c/tutorial-register-applications) &ndash; uygulama kaydı sırasında aşağıdaki seçimleri yapın:

     1 \. **Web uygulaması/Web API 'Sini** **Evet**olarak ayarlayın.<br>
     2 \. **Örtük akışa Izin ver** ' i **Evet**olarak ayarlayın.<br>
     3 \. `https://localhost:5001/authentication/login-callback`bir **yanıt URL 'si** ekleyin.

     Uygulama KIMLIĞINI (Istemci KIMLIĞI) kaydedin (örneğin, `11111111-1111-1111-1111-111111111111`).

   * & Ndash; [Kullanıcı akışları oluşturun](/azure/active-directory-b2c/tutorial-create-user-flows) Kaydolma ve oturum açma Kullanıcı akışı oluşturun.

     Uygulama için oluşturulan kaydolma ve oturum açma Kullanıcı akış adını kaydedin (örneğin, `B2C_1_signupsignin`).

1. Aşağıdaki komutta yer tutucuları, daha önce kaydedilen bilgilerle değiştirin ve komutu bir komut kabuğu 'nda yürütün:

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   Mevcut değilse bir proje klasörü oluşturan çıkış konumunu belirtmek için, çıkış seçeneğini komuta bir yol (örneğin, `-o BlazorSample`) ile birlikte ekleyin. Klasör adı Ayrıca projenin adının bir parçası haline gelir.

## <a name="authentication-package"></a>Kimlik doğrulama paketi

Tek bir B2C hesabı (`IndividualB2C`) kullanmak üzere bir uygulama oluşturulduğunda, uygulama otomatik olarak [Microsoft kimlik doğrulama kitaplığı](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`) için bir paket başvurusu alır. Paket, uygulamanın kullanıcıların kimliğini doğrulamasına ve korunan API 'Leri çağırmak için belirteçleri almasına yardımcı olan bir dizi temel sunar.

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
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
});
```

`AddMsalAuthentication` yöntemi, bir uygulamanın kimliğini doğrulamak için gereken parametreleri yapılandırmak için bir geri çağırma işlemini kabul eder. Uygulamanın yapılandırılması için gereken değerler, uygulamayı kaydettiğinizde Azure Portal AAD yapılandırmasından elde edilebilir.

Blazor WebAssembly şablonu, uygulamayı güvenli bir API için erişim belirteci isteyecek şekilde otomatik olarak yapılandırmaz. Oturum açma akışının bir parçası olarak bir belirteç sağlamak için, kapsamı `MsalProviderOptions`varsayılan erişim belirteci kapsamlarına ekleyin:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{API ID URI}/{SCOPE}");
});
```

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

* <xref:security/authentication/azure-ad-b2c>
* [Öğretici: Azure Active Directory B2C kiracısı oluşturma](/azure/active-directory-b2c/tutorial-create-tenant)
