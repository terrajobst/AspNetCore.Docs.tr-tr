---
title: WS-Federasyon'da, ASP.NET Core kullanıcılarla kimlik doğrulaması
author: chlowell
description: Bu öğretici, bir ASP.NET Core uygulamada WS-Federasyon kullanmayı gösterir.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: d4621c7b97678903b9f2562e353da3883334b599
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898810"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>WS-Federasyon'da, ASP.NET Core kullanıcılarla kimlik doğrulaması

Bu öğretici, kullanıcıların bir WS-Federasyon kimlik doğrulama sağlayıcısı Active Directory Federasyon Hizmetleri (ADFS) gibi oturum gösterilmiştir veya [Azure Active Directory](/azure/active-directory/) (AAD). Açıklanan ASP.NET Core 2.0 örnek uygulamanın kullandığı [Facebook, Google ve dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index).

ASP.NET Core 2.0 uygulamaları için WS-Federasyon desteği tarafından sağlanan [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Bu bileşen gelen bağlantı noktalı [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) ve bileşenin mekanizması çoğunu paylaşır. Ancak, bileşenlerin önemli yolları birkaç içinde farklılık gösterir.

Varsayılan olarak, yeni ara:

* İstenmeyen oturumları izin vermez. Bu özellik WS-Federasyon protokolünün XSRF saldırılarına açıktır. Ancak, bunu ile etkinleştirilebilir `AllowUnsolicitedLogins` seçeneği.
* Her form post oturum açma iletileri için denetlemez. Yalnızca ister `CallbackPath` oturum eklentiler için denetlenir `CallbackPath` varsayılan olarak `/signin-wsfed` ancak değiştirilebilir. Bu yol sağlayarak diğer kimlik doğrulama sağlayıcıları ile paylaşılabilir `SkipUnrecognizedRequests` seçeneği.

## <a name="register-the-app-with-active-directory"></a>Uygulama Active Directory'ye kaydetme

### <a name="active-directory-federation-services"></a>Active Directory Federasyon Hizmetleri

* Sunucunun açık **bağlı olan taraf güveni Ekleme Sihirbazı** ADFS yönetim konsolundan:

![Bağlı olan taraf güveni Sihirbazı ekleme: Hoş Geldiniz](ws-federation/_static/AdfsAddTrust.png)

* Verileri el ile girmek seçin:

![Bağlı olan taraf güveni Ekleme Sihirbazı: Veri kaynağını seçin](ws-federation/_static/AdfsSelectDataSource.png)

* Bağlı olan taraf için bir görünen ad girin. Ad ASP.NET Core uygulamasını önemli değildir.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:

![Bağlı olan taraf güveni Ekleme Sihirbazı: Sertifika yapılandırma](ws-federation/_static/AdfsConfigureCert.png)

* Uygulamanın URL'yi kullanarak WS-Federasyon Edilgen Protokolü desteğini etkinleştir. Bağlantı noktası uygulama için doğru olduğundan emin olun:

![Bağlı olan taraf güveni Ekleme Sihirbazı: URL Yapılandır](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Bu bir HTTPS URL'si olmalıdır. IIS Express otomatik olarak imzalanan bir sertifika uygulama geliştirme sırasında barındırdığında sağlayabilir. Kestrel el ile sertifika yapılandırma gerektirir. Bkz: [Kestrel belgelerine](xref:fundamentals/servers/kestrel) daha fazla ayrıntı için.

* Tıklatın **sonraki** sihirbaz kalanında ve **Kapat** sonunda.

* ASP.NET Core kimliği gerektiren bir **ad kimliği** talep. Bir ekleme **talep kurallarını Düzenle** iletişim:

![Talep kurallarını Düzenle](ws-federation/_static/EditClaimRules.png)

* İçinde **dönüştürme talep Kuralı Ekleme Sihirbazı**, varsayılan adı bırakın **LDAP özniteliklerini talep olarak Gönder** Seçili şablon ve tıklatın **sonraki**. Bir kural eklemesi **SAM hesap adı** LDAP özniteliği için **ad kimliği** giden talep:

![Dönüşüm talebi Kuralı Ekle Sihirbazı: Talep kuralı Yapılandır](ws-federation/_static/AddTransformClaimRule.png)

* Tıklatın **son** > **Tamam** içinde **talep kurallarını Düzenle** penceresi.

### <a name="azure-active-directory"></a>Azure Active Directory

* AAD kiracının uygulama kayıtlar dikey penceresine gidin. Tıklatın **yeni uygulama kaydı**:

![Azure Active Directory: Uygulama kayıtlar](ws-federation/_static/AadNewAppRegistration.png)

* Uygulama kaydı için bir ad girin. ASP.NET Core uygulamaya önemli değildir.
* Uygulama dinlediği olarak URL'sini girin **oturum açma URL'si**:

![Azure Active Directory: uygulama kaydı oluşturun](ws-federation/_static/AadCreateAppRegistration.png)

* Tıklatın **uç noktaları** ve not edin **Federasyon meta veri belgesi** URL. WS-Federasyon Ara budur `MetadataAddress`:

![Azure Active Directory: uç noktaları](ws-federation/_static/AadFederationMetadataDocument.png)

* Yeni uygulama kaydı gidin. Tıklatın **ayarları** > **özellikleri** ve Not **uygulama kimliği URI'si**. WS-Federasyon Ara budur `Wtrealm`:

![Azure Active Directory: Uygulama kaydı Özellikler](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>WS-Federasyon ASP.NET Core kimliği için bir dış oturum açma sağlayıcısı olarak Ekle

* Bir bağımlılık eklemek [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) projeye.
* WS-Federasyon eklemek `Configure` yönteminde *haline*:

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddAuthentication()
        .AddWsFederation(options =>
        {
            // MetadataAddress represents the Active Directory instance used to authenticate users.
            options.MetadataAddress = "https://<ADFS FQDN or AAD tenant>/FederationMetadata/2007-06/FederationMetadata.xml";

            // Wtrealm is the app's identifier in the Active Directory instance.
            // For ADFS, use the relying party's identifier, its WS-Federation Passive protocol URL:
            options.Wtrealm = "https://localhost:44307/";

            // For AAD, use the App ID URI from the app registration's Properties blade:
            options.Wtrealm = "https://wsfedsample.onmicrosoft.com/bf0e7e6d-056e-4e37-b9a6-2c36797b9f01";
        });

    services.AddMvc()
     // ...
    ```

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>WS-Federasyon oturum oturum

Gözat'ı tıklatın ve uygulama için **oturum** nav üstbilgisinde bağlantı. WsFederation oturum açmak için bir seçenek vardır: ![sayfasında oturum açın](ws-federation/_static/WsFederationButton.png)

Sağlayıcı olarak ADFS ile düğmesi ADFS oturum açma sayfasına yönlendirir: ![ADFS oturum açma sayfası](ws-federation/_static/AdfsLoginPage.png)

Azure Active Directory ile sağlayıcısı olarak, düğme AAD oturum açma sayfasına yönlendirir: ![AAD oturum açma sayfası](ws-federation/_static/AadSignIn.png)

Bir başarılı oturum açma için yeni bir kullanıcı, uygulamanın kullanıcı kayıt sayfasına yeniden yönlendirir: ![kayıt sayfası](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>WS-Federasyon ASP.NET Core kimlik olmadan kullanma

WS-Federasyon ara yazılım kimlik kullanılabilir. Örneğin:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = WsFederationDefaults.AuthenticationScheme;
    })
    .AddWsFederation(options =>
    {
        options.Wtrealm = Configuration["wsfed:realm"];
        options.MetadataAddress = Configuration["wsfed:metadata"];
    })
    .AddCookie();
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
        // …
}
```
