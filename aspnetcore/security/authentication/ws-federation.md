---
title: ASP.NET core'da WS-Federasyon ile kullanıcıların kimlik doğrulaması
author: chlowell
description: Bu öğretici ASP.NET Core uygulaması WS-Federation kullanmayı gösterir.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: 7967410686da0e59742b721c0154e143bf19ba01
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833546"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>ASP.NET core'da WS-Federasyon ile kullanıcıların kimlik doğrulaması

Bu öğreticide, kullanıcıların Active Directory Federasyon Hizmetleri (ADFS) gibi bir WS-Federasyon kimlik doğrulama sağlayıcısı ile oturum açmak gösterilmektedir veya [Azure Active Directory](/azure/active-directory/) (AAD). Açıklanan ASP.NET Core 2.0 örnek uygulamanın kullandığı [Facebook, Google ve dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index).

ASP.NET Core 2.0 uygulamaları için WS-Federasyon desteği tarafından sağlanan [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Bu bileşen öğesinden alınır [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) ve bileşenin mechanics çoğunu paylaşır. Ancak, bileşenlerin önemli şekilde birkaç içinde farklılık gösterir.

Varsayılan olarak, yeni bir ara yazılım:

* İstenmeyen oturumları izin vermez. Bu özellik WS-Federation Protokolü XSRF saldırılarına karşı savunmasız durumdadır. Ancak, bunu ile etkin hale getirilebilir `AllowUnsolicitedLogins` seçeneği.
* Her oturum açma iletileri için form post denetlemez. Yalnızca ister `CallbackPath` oturum hedefleyecek için denetlenir `CallbackPath` varsayılan olarak `/signin-wsfed` ancak değiştirilebilir devralınan aracılığıyla [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [ WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) sınıfı. Bu yol sağlayarak diğer kimlik doğrulama sağlayıcıları ile paylaşılabilen [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) seçeneği.

## <a name="register-the-app-with-active-directory"></a>Active Directory ile uygulamayı kaydetme

### <a name="active-directory-federation-services"></a>Active Directory Federasyon Hizmetleri

* Sunucunun açık **ekleme bağlı olan taraf güveni Sihirbazı** AD FS yönetim konsolundan:

![Bağlı olan taraf güveni Ekle Sihirbazı: Hoş Geldiniz](ws-federation/_static/AdfsAddTrust.png)

* Verileri el ile girmek bu seçeneği seçin:

![Bağlı olan taraf güveni Ekle Sihirbazı: Veri kaynağını seçin](ws-federation/_static/AdfsSelectDataSource.png)

* Bağlı olan taraf için bir görünen ad girin. ASP.NET Core uygulaması için önemli adı değil.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:

![Bağlı olan taraf güveni Ekle Sihirbazı: Sertifika yapılandırma](ws-federation/_static/AdfsConfigureCert.png)

* Uygulamanın URL'sini kullanan, WS-Federasyon Edilgen Protokolü desteğini etkinleştirin. Bağlantı noktası uygulama için doğru olduğunu doğrulayın:

![Bağlı olan taraf güveni Ekle Sihirbazı: URL yapılandırma](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Bu, bir HTTPS URL'si olması gerekir. IIS Express otomatik olarak imzalanan bir sertifika uygulama geliştirme sırasında barındırırken sağlayabilir. Kestrel'i el ile sertifika yapılandırma gerektirir. Bkz: [Kestrel belgeleri](xref:fundamentals/servers/kestrel) daha fazla ayrıntı için.

* Tıklayın **sonraki** sihirbazın geri kalanını aracılığıyla ve **Kapat** sonunda.

* ASP.NET Core kimliği gerektiren bir **ad kimliği** talep. Bir ekleme **talep kurallarını Düzenle** iletişim:

![Talep kurallarını Düzenle](ws-federation/_static/EditClaimRules.png)

* İçinde **dönüştürme Kuralı Ekleme Sihirbazı**, varsayılan değeri bırakın **LDAP özniteliklerini talep olarak Gönder** şablonu seçili seçeneğine tıklayıp **sonraki**. Bir kural eklemesi **SAM-Account-Name** LDAP özniteliği **ad kimliği** giden talep:

![Dönüşüm talebi Kuralı Ekle Sihirbazı: Talep kuralı Yapılandır](ws-federation/_static/AddTransformClaimRule.png)

* Tıklayın **son** > **Tamam** içinde **talep kurallarını Düzenle** penceresi.

### <a name="azure-active-directory"></a>Azure Active Directory

* AAD kiracısının uygulama kayıtları dikey penceresine gidin. Tıklayın **yeni uygulama kaydı**:

![Azure Active Directory: Uygulama kayıtları](ws-federation/_static/AadNewAppRegistration.png)

* Uygulama kaydı için bir ad girin. Bu, ASP.NET Core uygulaması için önemli değildir.
* Uygulama dinlediği olarak URL'sini **oturum açma URL'si**:

![Azure Active Directory: Uygulama kaydı oluşturma](ws-federation/_static/AadCreateAppRegistration.png)

* Tıklayın **uç noktaları** ve Not **Federasyon meta veri belgesi** URL'si. Bu WS-Federation Ara, `MetadataAddress`:

![Azure Active Directory: Uç Noktalar](ws-federation/_static/AadFederationMetadataDocument.png)

* Yeni uygulama kaydı için gidin. Tıklayın **ayarları** > **özellikleri** ve Not **uygulama kimliği URI'si**. Bu WS-Federation Ara, `Wtrealm`:

![Azure Active Directory: Uygulama kayıt özellikleri](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>WS-Federation, ASP.NET Core kimliği için bir dış oturum açma sağlayıcısı Ekle

* Üzerinde bir bağımlılık ekleme [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) projeye.
* WS-Federasyon ekleme `Startup.ConfigureServices`:

    ```csharp
    services.AddIdentity<IdentityUser, IdentityRole>()
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

### <a name="log-in-with-ws-federation"></a>WS-Federasyon ile oturum açın

Gözat'a tıklayın ve uygulama için **oturum** Gezinti üst bilgisindeki bağlantı. WsFederation ile oturum açmak için bir seçenek vardır: ![Sayfasında oturum açın](ws-federation/_static/WsFederationButton.png)

Sağlayıcı olarak AD FS ile düğme ADFS oturum açma sayfasına yönlendirir: ![ADFS oturum açma sayfası](ws-federation/_static/AdfsLoginPage.png)

Azure Active Directory ile sağlayıcı olarak, düğmeyi AAD oturum açma sayfasına yönlendirir: ![AAD oturum açma sayfası](ws-federation/_static/AadSignIn.png)

Bir başarılı oturum açma için yeni bir kullanıcı uygulamanın kullanıcı kayıt sayfasına yönlendirir: ![Kayıt sayfası](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>WS-Federation ASP.NET Core kimliği olmadan kullanın

WS-Federation ara yazılım kimlik kullanılabilir. Örneğin:

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
