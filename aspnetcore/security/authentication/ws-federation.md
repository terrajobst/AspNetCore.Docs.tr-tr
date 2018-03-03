---
title: "WS-Federasyon'da, ASP.NET Core kullanıcılarla kimlik doğrulaması"
author: chlowell
description: "Bu öğretici, bir ASP.NET Core uygulamada WS-Federasyon kullanmayı gösterir."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: 0532f866e9c58b2e45623f522f62438e15017e54
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="455c2-103">WS-Federasyon'da, ASP.NET Core kullanıcılarla kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="455c2-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="455c2-104">Bu öğretici, kullanıcıların bir WS-Federasyon kimlik doğrulama sağlayıcısı Active Directory Federasyon Hizmetleri (ADFS) gibi oturum gösterilmiştir veya [Azure Active Directory](/azure/active-directory/) (AAD).</span><span class="sxs-lookup"><span data-stu-id="455c2-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="455c2-105">Açıklanan ASP.NET Core 2.0 örnek uygulamanın kullandığı [Facebook, Google ve dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="455c2-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="455c2-106">ASP.NET Core 2.0 uygulamaları için WS-Federasyon desteği tarafından sağlanan [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span><span class="sxs-lookup"><span data-stu-id="455c2-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="455c2-107">Bu bileşen gelen bağlantı noktalı [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) ve bileşenin mekanizması çoğunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="455c2-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="455c2-108">Ancak, bileşenlerin önemli yolları birkaç içinde farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="455c2-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="455c2-109">Varsayılan olarak, yeni ara:</span><span class="sxs-lookup"><span data-stu-id="455c2-109">By default, the new middleware:</span></span>

* <span data-ttu-id="455c2-110">İstenmeyen oturumları izin vermez.</span><span class="sxs-lookup"><span data-stu-id="455c2-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="455c2-111">Bu özellik WS-Federasyon protokolünün XSRF saldırılarına açıktır.</span><span class="sxs-lookup"><span data-stu-id="455c2-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="455c2-112">Ancak, bunu ile etkinleştirilebilir `AllowUnsolicitedLogins` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="455c2-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="455c2-113">Her form post oturum açma iletileri için denetlemez.</span><span class="sxs-lookup"><span data-stu-id="455c2-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="455c2-114">Yalnızca ister `CallbackPath` oturum eklentiler için denetlenir `CallbackPath` varsayılan olarak `/signin-wsfed` ancak değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="455c2-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed.</span></span> <span data-ttu-id="455c2-115">Bu yol sağlayarak diğer kimlik doğrulama sağlayıcıları ile paylaşılabilir `SkipUnrecognizedRequests` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="455c2-115">This path can be shared with other authentication providers by enabling the `SkipUnrecognizedRequests` option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="455c2-116">Uygulama Active Directory'ye kaydetme</span><span class="sxs-lookup"><span data-stu-id="455c2-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="455c2-117">Active Directory Federasyon Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="455c2-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="455c2-118">Sunucunun açık **bağlı olan taraf güveni Ekleme Sihirbazı** ADFS yönetim konsolundan:</span><span class="sxs-lookup"><span data-stu-id="455c2-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![Bağlı olan taraf güveni Sihirbazı ekleme: Hoş Geldiniz](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="455c2-120">Verileri el ile girmek seçin:</span><span class="sxs-lookup"><span data-stu-id="455c2-120">Choose to enter data manually:</span></span>

![Bağlı olan taraf güveni Ekleme Sihirbazı: Veri kaynağını seçin](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="455c2-122">Bağlı olan taraf için bir görünen ad girin.</span><span class="sxs-lookup"><span data-stu-id="455c2-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="455c2-123">Ad ASP.NET Core uygulamasını önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="455c2-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="455c2-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span><span class="sxs-lookup"><span data-stu-id="455c2-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![Bağlı olan taraf güveni Ekleme Sihirbazı: Sertifika yapılandırma](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="455c2-126">Uygulamanın URL'yi kullanarak WS-Federasyon Edilgen Protokolü desteğini etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="455c2-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="455c2-127">Bağlantı noktası uygulama için doğru olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="455c2-127">Verify the port is correct for the app:</span></span>

![Bağlı olan taraf güveni Ekleme Sihirbazı: URL Yapılandır](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="455c2-129">Bu bir HTTPS URL'si olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="455c2-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="455c2-130">IIS Express otomatik olarak imzalanan bir sertifika uygulama geliştirme sırasında barındırdığında sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="455c2-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="455c2-131">Kestrel el ile sertifika yapılandırma gerektirir.</span><span class="sxs-lookup"><span data-stu-id="455c2-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="455c2-132">Bkz: [Kestrel belgelerine](xref:fundamentals/servers/kestrel) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="455c2-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="455c2-133">Tıklatın **sonraki** sihirbaz kalanında ve **Kapat** sonunda.</span><span class="sxs-lookup"><span data-stu-id="455c2-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="455c2-134">ASP.NET Core kimliği gerektiren bir **ad kimliği** talep.</span><span class="sxs-lookup"><span data-stu-id="455c2-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="455c2-135">Bir ekleme **talep kurallarını Düzenle** iletişim:</span><span class="sxs-lookup"><span data-stu-id="455c2-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![Talep kurallarını Düzenle](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="455c2-137">İçinde **dönüştürme talep Kuralı Ekleme Sihirbazı**, varsayılan adı bırakın **LDAP özniteliklerini talep olarak Gönder** Seçili şablon ve tıklatın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="455c2-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="455c2-138">Bir kural eklemesi **SAM hesap adı** LDAP özniteliği için **ad kimliği** giden talep:</span><span class="sxs-lookup"><span data-stu-id="455c2-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![Dönüşüm talebi Kuralı Ekle Sihirbazı: Talep kuralı Yapılandır](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="455c2-140">Tıklatın **son** > **Tamam** içinde **talep kurallarını Düzenle** penceresi.</span><span class="sxs-lookup"><span data-stu-id="455c2-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="455c2-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="455c2-141">Azure Active Directory</span></span>

* <span data-ttu-id="455c2-142">AAD kiracının uygulama kayıtlar dikey penceresine gidin.</span><span class="sxs-lookup"><span data-stu-id="455c2-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="455c2-143">Tıklatın **yeni uygulama kaydı**:</span><span class="sxs-lookup"><span data-stu-id="455c2-143">Click **New application registration**:</span></span>

![Azure Active Directory: Uygulama kayıtlar](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="455c2-145">Uygulama kaydı için bir ad girin.</span><span class="sxs-lookup"><span data-stu-id="455c2-145">Enter a name for the app registration.</span></span> <span data-ttu-id="455c2-146">ASP.NET Core uygulamaya önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="455c2-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="455c2-147">Uygulama dinlediği olarak URL'sini girin **oturum açma URL'si**:</span><span class="sxs-lookup"><span data-stu-id="455c2-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory: uygulama kaydı oluşturun](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="455c2-149">Tıklatın **uç noktaları** ve not edin **Federasyon meta veri belgesi** URL.</span><span class="sxs-lookup"><span data-stu-id="455c2-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="455c2-150">WS-Federasyon Ara budur `MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="455c2-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory: uç noktaları](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="455c2-152">Yeni uygulama kaydı gidin.</span><span class="sxs-lookup"><span data-stu-id="455c2-152">Navigate to the new app registration.</span></span> <span data-ttu-id="455c2-153">Tıklatın **ayarları** > **özellikleri** ve Not **uygulama kimliği URI'si**.</span><span class="sxs-lookup"><span data-stu-id="455c2-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="455c2-154">WS-Federasyon Ara budur `Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="455c2-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory: Uygulama kaydı Özellikler](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="455c2-156">WS-Federasyon ASP.NET Core kimliği için bir dış oturum açma sağlayıcısı olarak Ekle</span><span class="sxs-lookup"><span data-stu-id="455c2-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="455c2-157">Bir bağımlılık eklemek [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) projeye.</span><span class="sxs-lookup"><span data-stu-id="455c2-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="455c2-158">WS-Federasyon eklemek `Configure` yönteminde *haline*:</span><span class="sxs-lookup"><span data-stu-id="455c2-158">Add WS-Federation to the `Configure` method in *Startup.cs*:</span></span>

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

[!INCLUDE[default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="455c2-159">WS-Federasyon oturum oturum</span><span class="sxs-lookup"><span data-stu-id="455c2-159">Log in with WS-Federation</span></span>

<span data-ttu-id="455c2-160">Gözat'ı tıklatın ve uygulama için **oturum** nav üstbilgisinde bağlantı.</span><span class="sxs-lookup"><span data-stu-id="455c2-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="455c2-161">WsFederation oturum açmak için bir seçenek vardır: ![sayfasında oturum açın](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="455c2-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="455c2-162">Sağlayıcı olarak ADFS ile düğmesi ADFS oturum açma sayfasına yönlendirir: ![ADFS oturum açma sayfası](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="455c2-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="455c2-163">Azure Active Directory ile sağlayıcısı olarak, düğme AAD oturum açma sayfasına yönlendirir: ![AAD oturum açma sayfası](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="455c2-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="455c2-164">Bir başarılı oturum açma için yeni bir kullanıcı, uygulamanın kullanıcı kayıt sayfasına yeniden yönlendirir: ![kayıt sayfası](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="455c2-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="455c2-165">WS-Federasyon ASP.NET Core kimlik olmadan kullanma</span><span class="sxs-lookup"><span data-stu-id="455c2-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="455c2-166">WS-Federasyon ara yazılım kimlik kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="455c2-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="455c2-167">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="455c2-167">For example:</span></span>

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
