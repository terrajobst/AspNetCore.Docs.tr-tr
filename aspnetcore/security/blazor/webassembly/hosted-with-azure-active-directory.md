---
title: Azure Active Directory ile bir ASP.NET Core Blazor Weelsembly barındırılan uygulaması güvenli hale getirme
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: 2ddbc9791ec9b31d55c9c6017d9d6d5be5c8dec8
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434492"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a>Azure Active Directory ile bir ASP.NET Core Blazor Weelsembly barındırılan uygulaması güvenli hale getirme

, [Javier Calvarro Nelson](https://github.com/javiercn) ve [Luke Latham](https://github.com/guardrex) 'e göre

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]



Bu makalede, kimlik doğrulaması için [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) kullanan bir [Blazor weelsembly barındırılan uygulamasının](xref:blazor/hosting-models#blazor-webassembly) nasıl oluşturulacağı açıklanmaktadır.

## <a name="register-apps-in-aad-b2c-and-create-solution"></a>Uygulamaları AAD B2C kaydetme ve çözüm oluşturma

### <a name="create-a-tenant"></a>Kiracı oluşturma

Hızlı başlangıç: AAD 'de kiracı oluşturmak için [bir kiracı ayarlama](/azure/active-directory/develop/quickstart-create-new-tenant) bölümündeki yönergeleri izleyin.

### <a name="register-a-server-api-app"></a>Sunucu API 'SI uygulaması kaydetme

Hızlı Başlangıç bölümündeki yönergeleri izleyin: *sunucu API 'si* **Azure Active Directory** uygulamasına yönelik bir AAD uygulamasını Azure Portal > **uygulama kayıtları** alanına kaydetmek Için MICROSOFT Identity platformu ve sonraki Azure AAD [ile uygulamayı kaydetme](/azure/active-directory/develop/quickstart-register-app) konusuna bakın:

1. **Yeni kayıt**seçeneğini belirleyin.
1. Uygulama için bir **ad** sağlayın (örneğin, **Blazor Server AAD**).
1. Desteklenen bir **Hesap türü**seçin. Bu deneyim için **yalnızca bu kuruluş dizininde** (tek kiracı) hesaplar seçebilirsiniz.
1. *Sunucu API 'si uygulaması* Bu senaryoda **yeniden yönlendirme URI 'si** gerektirmez, bu nedenle açılan kutudan **Web** 'e ve yeniden yönlendirme URI 'si girmeyin.
1. **Yönetici tarafından OpenID ve offline_access izinleri için Izin ver** onay kutusunu > **izinleri** devre dışı bırakın.
1. **Kaydol**’u seçin.

Uygulama, oturum açma veya uer profil erişimi gerektirmediğinden, **API izinlerinde** **Microsoft Graph** > **User. Read** iznini kaldırın.

**API 'Yi kullanıma**sunma bölümünde:

1. **Kapsam ekle**’yi seçin.
1. **Kaydet ve devam et**’i seçin.
1. Bir **kapsam adı** sağlayın (örneğin, `API.Access`).
1. **Yönetici izni görünen adı** sağlayın (örneğin, `Access API`).
1. **Yönetici onay açıklaması** sağlayın (örneğin, `Allows the app to access server app API endpoints.`).
1. **Durumun** **etkin**olarak ayarlandığını onaylayın.
1. **Kapsam Ekle**' yi seçin.

Aşağıdaki bilgileri kaydedin:

* *Sunucu API 'si uygulaması* Uygulama KIMLIĞI (Istemci KIMLIĞI) (örneğin, `11111111-1111-1111-1111-111111111111`)
* Dizin KIMLIĞI (kiracı KIMLIĞI) (örneğin, `222222222-2222-2222-2222-222222222222`)
* AAD kiracı etki alanı (örneğin, `contoso.onmicrosoft.com`)
* Varsayılan kapsam (örneğin, `API.Access`)

### <a name="register-a-client-app"></a>İstemci uygulamasını kaydetme

Hızlı başlangıç: **Azure Active Directory** > **uygulama kayıtları** Azure Portal alanına *istemci uygulaması* Için bir AAD uygulaması kaydetmek üzere MICROSOFT Identity platformu ve sonraki Azure AAD [ile uygulamayı kaydetme](/azure/active-directory/develop/quickstart-register-app) bölümündeki yönergeleri izleyin:

1. **Yeni kayıt**seçeneğini belirleyin.
1. Uygulama için bir **ad** sağlayın (örneğin, **Blazor istemci AAD**).
1. Desteklenen bir **Hesap türü**seçin. Bu deneyim için **yalnızca bu kuruluş dizininde** (tek kiracı) hesaplar seçebilirsiniz.
1. **Yeniden yönlendirme URI 'si** açılan listesini **Web**olarak ayarlayın ve `https://localhost:5001/authentication/login-callback`yeniden yönlendirme URI 'si sağlayın.
1. **Yönetici tarafından OpenID ve offline_access izinleri için Izin ver** onay kutusunu > **izinleri** devre dışı bırakın.
1. **Kaydol**’u seçin.

**Kimlik doğrulama** > **Platform yapılandırmalarında** **Web** > :

1. `https://localhost:5001/authentication/login-callback` **yeniden yönlendirme URI 'sinin** mevcut olduğunu onaylayın.
1. **Örtük izin**Için, **erişim belirteçleri** ve **Kimlik belirteçleri**onay kutularını seçin.
1. Uygulamanın kalan varsayılan değerleri bu deneyim için kabul edilebilir.
1. **Kaydet** düğmesini seçin.

**API izinleri**:

1. Uygulamanın **Microsoft Graph** > **User. Read** iznine sahip olduğunu doğrulayın.
1. **Izin Ekle** ' yi ve ardından **API 'lerim**' i seçin.
1. **Ad** SÜTUNUNDAN *sunucu API uygulamasını* (ÖRNEĞIN, **Blazor Server AAD**) seçin.
1. **API** listesini açın.
1. API 'ye erişimi etkinleştirin (örneğin, `API.Access`).
1. **Izin Ekle**' yi seçin.
1. **{Tenant Name} için yönetici Içeriği ver** düğmesini seçin. Onaylamak için **Evet**’i seçin.

*İstemci* UYGULAMASı uygulama kimliğini (istemci kimliği) kaydedin (örneğin, `33333333-3333-3333-3333-333333333333`).

### <a name="create-the-app"></a>Uygulama oluşturma

Aşağıdaki komutta yer tutucuları, daha önce kaydedilen bilgilerle değiştirin ve komutu bir komut kabuğu 'nda yürütün:

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP CLIENT ID}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

Mevcut değilse bir proje klasörü oluşturan çıkış konumunu belirtmek için, çıkış seçeneğini komuta bir yol (örneğin, `-o BlazorSample`) ile birlikte ekleyin. Klasör adı Ayrıca projenin adının bir parçası haline gelir.

> [!NOTE]
> Varsayılan erişim belirteci kapsamında önemli bir yapılandırma değişikliği için [kimlik doğrulama hizmeti desteği](#Authentication service support) bölümüne bakın. Blazor WebAssembly şablonu tarafından belirtilen değerin, *istemci uygulaması* şablondan oluşturulduktan sonra el ile değiştirilmesi gerekir.

## <a name="server-app-configuration"></a>Sunucu uygulaması yapılandırması

*Bu bölüm, çözümün **sunucu** uygulamasıyla ilgilidir.*

### <a name="authentication-package"></a>Kimlik doğrulama paketi

ASP.NET Core Web API 'Lerine yapılan kimlik doğrulama ve yetkilendirme desteği `Microsoft.AspNetCore.Authentication.AzureAD.UI`tarafından sağlanır:

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a>Kimlik doğrulama hizmeti desteği

`AddAuthentication` yöntemi, uygulama içinde kimlik doğrulama hizmetlerini ayarlar ve JWT taşıyıcı işleyicisini varsayılan kimlik doğrulama yöntemi olarak yapılandırır. `AddAzureADBearer` yöntemi, Azure Active Directory tarafından yayılan belirteçleri doğrulamak için gereken JWT taşıyıcı işleyicisinde belirli parametreleri ayarlar:

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

`UseAuthentication` ve `UseAuthorization` şunları doğrulayın:

* Uygulama, gelen isteklerde belirteçleri ayrıştırmaya ve doğrulamaya çalışır.
* Uygun kimlik bilgileri olmadan korunan kaynağa erişmeye çalışan istekler başarısız olur.

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a>Uygulama ayarları

*AppSettings. JSON* dosyası, erişim belirteçlerini doğrulamak IÇIN kullanılan JWT taşıyıcı işleyicisini yapılandırma seçeneklerini içerir.

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{API CLIENT ID}",
  }
}
```

### <a name="weatherforecast-controller"></a>Hava tahmin denetleyicisi

Dalgalı tahmin denetleyicisi (*denetleyiciler/dalgalı) denetleyici. cs*), denetleyiciye uygulanan `[Authorize]` özniteliği ile korunan bir API sunar. Bunun anlaşılması **önemlidir** :

* Bu API denetleyicisindeki `[Authorize]` özniteliği, bu API 'yi yetkisiz erişime karşı koruyan tek şeydir.
* Blazor WebAssembly uygulamasında kullanılan `[Authorize]` özniteliği yalnızca uygulamanın, uygulamanın düzgün şekilde çalışması için yetkilendirilmiş olması gerektiği konusunda bir ipucu işlevi görür.

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a>İstemci uygulama yapılandırması

*Bu bölüm, çözümün **istemci** uygulaması ile ilgilidir.*

### <a name="authentication-package"></a>Kimlik doğrulama paketi

Iş veya okul hesaplarını (`SingleOrg`) kullanmak üzere bir uygulama oluşturulduğunda, uygulama [Microsoft kimlik doğrulama kitaplığı](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`) için bir paket başvurusu otomatik olarak alır. Paket, uygulamanın kullanıcıların kimliğini doğrulamasına ve korunan API 'Leri çağırmak için belirteçleri almasına yardımcı olan bir dizi temel sunar.

Bir uygulamaya kimlik doğrulaması ekliyorsanız, paketi uygulamanın proje dosyasına el ile ekleyin:

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

Önceki paket başvurusunda `{VERSION}`, <xref:blazor/get-started> makalesinde gösterilen `Microsoft.AspNetCore.Blazor.Templates` paketinin sürümü ile değiştirin.

`Microsoft.Authentication.WebAssembly.Msal` paketi, `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paketini uygulamaya göre geçişli olarak ekler.

### <a name="authentication-service-support"></a>Kimlik doğrulama hizmeti desteği

Kullanıcıları kimlik doğrulama desteği, hizmet kapsayıcısına `Microsoft.Authentication.WebAssembly.Msal` paketi tarafından sağlanmış `AddMsalAuthentication` uzantısı yöntemiyle kaydedilir. Bu yöntem, uygulamanın kimlik sağlayıcısıyla (IP) etkileşim kurması için gereken tüm hizmetleri ayarlar.

*Program.cs*:

*İstemci uygulaması* oluşturulduğunda, varsayılan erişim belirteci kapsamı `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`biçimindedir. **Kapsam değerinin `api://` kısmını kaldırın.** Bu sorun gelecekteki bir önizleme sürümünde giderilecektir.

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> Varsayılan erişim belirteci kapsamı `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` biçiminde olmalıdır (örneğin, `11111111-1111-1111-1111-111111111111/API.Access`). Kapsam ayarına bir düzen veya şema ve ana bilgisayar sağlanmışsa (Azure portalında gösterildiği gibi), *istemci uygulaması* *sunucu apı*uygulamasından bir *401 Yetkisiz* yanıt aldığında işlenmeyen bir özel durum oluşturur.

`AddMsalAuthentication` yöntemi, bir uygulamanın kimliğini doğrulamak için gereken parametreleri yapılandırmak için bir geri çağırma işlemini kabul eder. Uygulamanın yapılandırılması için gereken değerler, uygulamayı kaydettiğinizde Azure Portal AAD yapılandırmasından elde edilebilir.

Varsayılan erişim belirteci kapsamları, erişim belirteci kapsamlarının listesini temsil eder:

* Oturum açma isteğine varsayılan olarak dahildir.
* Kimlik doğrulamasından hemen sonra bir erişim belirteci sağlamak için kullanılır.

Tüm kapsamlar Azure Active Directory kuralları başına aynı uygulamaya ait olmalıdır. Gerektiğinde ek API uygulamaları için ek kapsamlar eklenebilir:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{SCOPE}");
});
```

### <a name="index-page"></a>Dizin sayfası

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a>Uygulama bileşeni

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>RedirectToLogin bileşeni

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>LoginDisplay bileşeni

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a>Kimlik doğrulama bileşeni

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>FetchData bileşeni

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Uygulamayı sunucu projesinden çalıştırın. Visual Studio 'Yu kullanırken **Çözüm Gezgini** ' de sunucu projesini seçin ve araç çubuğundaki **Çalıştır** düğmesini seçin veya uygulamayı **Hata Ayıkla** menüsünden başlatın.

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authentication/azure-active-directory/index>
