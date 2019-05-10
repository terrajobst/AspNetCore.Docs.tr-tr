---
title: ASP.NET core'da WS-Federasyon ile kullanıcıların kimlik doğrulaması
author: chlowell
description: Bu öğretici ASP.NET Core uygulaması WS-Federation kullanmayı gösterir.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: 7967410686da0e59742b721c0154e143bf19ba01
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903089"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="16d92-103">ASP.NET core'da WS-Federasyon ile kullanıcıların kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="16d92-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="16d92-104">Bu öğreticide, kullanıcıların Active Directory Federasyon Hizmetleri (ADFS) gibi bir WS-Federasyon kimlik doğrulama sağlayıcısı ile oturum açmak gösterilmektedir veya [Azure Active Directory](/azure/active-directory/) (AAD).</span><span class="sxs-lookup"><span data-stu-id="16d92-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="16d92-105">Açıklanan ASP.NET Core 2.0 örnek uygulamanın kullandığı [Facebook, Google ve dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="16d92-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="16d92-106">ASP.NET Core 2.0 uygulamaları için WS-Federasyon desteği tarafından sağlanan [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span><span class="sxs-lookup"><span data-stu-id="16d92-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="16d92-107">Bu bileşen öğesinden alınır [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) ve bileşenin mechanics çoğunu paylaşır.</span><span class="sxs-lookup"><span data-stu-id="16d92-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="16d92-108">Ancak, bileşenlerin önemli şekilde birkaç içinde farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="16d92-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="16d92-109">Varsayılan olarak, yeni bir ara yazılım:</span><span class="sxs-lookup"><span data-stu-id="16d92-109">By default, the new middleware:</span></span>

* <span data-ttu-id="16d92-110">İstenmeyen oturumları izin vermez.</span><span class="sxs-lookup"><span data-stu-id="16d92-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="16d92-111">Bu özellik WS-Federation Protokolü XSRF saldırılarına karşı savunmasız durumdadır.</span><span class="sxs-lookup"><span data-stu-id="16d92-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="16d92-112">Ancak, bunu ile etkin hale getirilebilir `AllowUnsolicitedLogins` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="16d92-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="16d92-113">Her oturum açma iletileri için form post denetlemez.</span><span class="sxs-lookup"><span data-stu-id="16d92-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="16d92-114">Yalnızca ister `CallbackPath` oturum hedefleyecek için denetlenir `CallbackPath` varsayılan olarak `/signin-wsfed` ancak değiştirilebilir devralınan aracılığıyla [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [ WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="16d92-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) class.</span></span> <span data-ttu-id="16d92-115">Bu yol sağlayarak diğer kimlik doğrulama sağlayıcıları ile paylaşılabilen [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) seçeneği.</span><span class="sxs-lookup"><span data-stu-id="16d92-115">This path can be shared with other authentication providers by enabling the [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="16d92-116">Active Directory ile uygulamayı kaydetme</span><span class="sxs-lookup"><span data-stu-id="16d92-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="16d92-117">Active Directory Federasyon Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="16d92-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="16d92-118">Sunucunun açık **ekleme bağlı olan taraf güveni Sihirbazı** AD FS yönetim konsolundan:</span><span class="sxs-lookup"><span data-stu-id="16d92-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![Bağlı olan taraf güveni Ekle Sihirbazı: Hoş Geldiniz](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="16d92-120">Verileri el ile girmek bu seçeneği seçin:</span><span class="sxs-lookup"><span data-stu-id="16d92-120">Choose to enter data manually:</span></span>

![Bağlı olan taraf güveni Ekle Sihirbazı: Veri kaynağını seçin](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="16d92-122">Bağlı olan taraf için bir görünen ad girin.</span><span class="sxs-lookup"><span data-stu-id="16d92-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="16d92-123">ASP.NET Core uygulaması için önemli adı değil.</span><span class="sxs-lookup"><span data-stu-id="16d92-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="16d92-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span><span class="sxs-lookup"><span data-stu-id="16d92-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![Bağlı olan taraf güveni Ekle Sihirbazı: Sertifika yapılandırma](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="16d92-126">Uygulamanın URL'sini kullanan, WS-Federasyon Edilgen Protokolü desteğini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="16d92-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="16d92-127">Bağlantı noktası uygulama için doğru olduğunu doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="16d92-127">Verify the port is correct for the app:</span></span>

![Bağlı olan taraf güveni Ekle Sihirbazı: URL yapılandırma](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="16d92-129">Bu, bir HTTPS URL'si olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="16d92-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="16d92-130">IIS Express otomatik olarak imzalanan bir sertifika uygulama geliştirme sırasında barındırırken sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="16d92-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="16d92-131">Kestrel'i el ile sertifika yapılandırma gerektirir.</span><span class="sxs-lookup"><span data-stu-id="16d92-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="16d92-132">Bkz: [Kestrel belgeleri](xref:fundamentals/servers/kestrel) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="16d92-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="16d92-133">Tıklayın **sonraki** sihirbazın geri kalanını aracılığıyla ve **Kapat** sonunda.</span><span class="sxs-lookup"><span data-stu-id="16d92-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="16d92-134">ASP.NET Core kimliği gerektiren bir **ad kimliği** talep.</span><span class="sxs-lookup"><span data-stu-id="16d92-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="16d92-135">Bir ekleme **talep kurallarını Düzenle** iletişim:</span><span class="sxs-lookup"><span data-stu-id="16d92-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![Talep kurallarını Düzenle](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="16d92-137">İçinde **dönüştürme Kuralı Ekleme Sihirbazı**, varsayılan değeri bırakın **LDAP özniteliklerini talep olarak Gönder** şablonu seçili seçeneğine tıklayıp **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="16d92-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="16d92-138">Bir kural eklemesi **SAM-Account-Name** LDAP özniteliği **ad kimliği** giden talep:</span><span class="sxs-lookup"><span data-stu-id="16d92-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![Dönüşüm talebi Kuralı Ekle Sihirbazı: Talep kuralı Yapılandır](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="16d92-140">Tıklayın **son** > **Tamam** içinde **talep kurallarını Düzenle** penceresi.</span><span class="sxs-lookup"><span data-stu-id="16d92-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="16d92-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="16d92-141">Azure Active Directory</span></span>

* <span data-ttu-id="16d92-142">AAD kiracısının uygulama kayıtları dikey penceresine gidin.</span><span class="sxs-lookup"><span data-stu-id="16d92-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="16d92-143">Tıklayın **yeni uygulama kaydı**:</span><span class="sxs-lookup"><span data-stu-id="16d92-143">Click **New application registration**:</span></span>

![Azure Active Directory: Uygulama kayıtları](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="16d92-145">Uygulama kaydı için bir ad girin.</span><span class="sxs-lookup"><span data-stu-id="16d92-145">Enter a name for the app registration.</span></span> <span data-ttu-id="16d92-146">Bu, ASP.NET Core uygulaması için önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="16d92-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="16d92-147">Uygulama dinlediği olarak URL'sini **oturum açma URL'si**:</span><span class="sxs-lookup"><span data-stu-id="16d92-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory: Uygulama kaydı oluşturma](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="16d92-149">Tıklayın **uç noktaları** ve Not **Federasyon meta veri belgesi** URL'si.</span><span class="sxs-lookup"><span data-stu-id="16d92-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="16d92-150">Bu WS-Federation Ara, `MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="16d92-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory: Uç Noktalar](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="16d92-152">Yeni uygulama kaydı için gidin.</span><span class="sxs-lookup"><span data-stu-id="16d92-152">Navigate to the new app registration.</span></span> <span data-ttu-id="16d92-153">Tıklayın **ayarları** > **özellikleri** ve Not **uygulama kimliği URI'si**.</span><span class="sxs-lookup"><span data-stu-id="16d92-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="16d92-154">Bu WS-Federation Ara, `Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="16d92-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory: Uygulama kayıt özellikleri](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="16d92-156">WS-Federation, ASP.NET Core kimliği için bir dış oturum açma sağlayıcısı Ekle</span><span class="sxs-lookup"><span data-stu-id="16d92-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="16d92-157">Üzerinde bir bağımlılık ekleme [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) projeye.</span><span class="sxs-lookup"><span data-stu-id="16d92-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="16d92-158">WS-Federasyon ekleme `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="16d92-158">Add WS-Federation to `Startup.ConfigureServices`:</span></span>

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

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="16d92-159">WS-Federasyon ile oturum açın</span><span class="sxs-lookup"><span data-stu-id="16d92-159">Log in with WS-Federation</span></span>

<span data-ttu-id="16d92-160">Gözat'a tıklayın ve uygulama için **oturum** Gezinti üst bilgisindeki bağlantı.</span><span class="sxs-lookup"><span data-stu-id="16d92-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="16d92-161">WsFederation ile oturum açmak için bir seçenek vardır: ![Sayfasında oturum açın](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="16d92-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="16d92-162">Sağlayıcı olarak AD FS ile düğme ADFS oturum açma sayfasına yönlendirir: ![ADFS oturum açma sayfası](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="16d92-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="16d92-163">Azure Active Directory ile sağlayıcı olarak, düğmeyi AAD oturum açma sayfasına yönlendirir: ![AAD oturum açma sayfası](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="16d92-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="16d92-164">Bir başarılı oturum açma için yeni bir kullanıcı uygulamanın kullanıcı kayıt sayfasına yönlendirir: ![Kayıt sayfası](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="16d92-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="16d92-165">WS-Federation ASP.NET Core kimliği olmadan kullanın</span><span class="sxs-lookup"><span data-stu-id="16d92-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="16d92-166">WS-Federation ara yazılım kimlik kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="16d92-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="16d92-167">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="16d92-167">For example:</span></span>

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
