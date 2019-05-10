---
title: ASP.NET Core ile Microsoft Account dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Microsoft hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 1733d049d6752c24d7749b5b5ae2a4b866492358
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903425"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="0ef22-103">ASP.NET Core ile Microsoft Account dış oturum açma Kurulumu</span><span class="sxs-lookup"><span data-stu-id="0ef22-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="0ef22-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0ef22-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0ef22-105">Bu öğreticide, oluşturulan bir örnek ASP.NET Core 2.0 proje kullanarak, Microsoft hesabı ile oturum açmalarına işlemini göstermektedir [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="0ef22-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="0ef22-106">Microsoft Geliştirici portalında uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="0ef22-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="0ef22-107">Gidin [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) oluşturarak veya mevcut bir Microsoft hesabı olarak oturum açın:</span><span class="sxs-lookup"><span data-stu-id="0ef22-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![İletişim kutusunda oturum](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="0ef22-109">Bir Microsoft hesabınız yoksa, dokunun  **[oluşturun!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="0ef22-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="0ef22-110">Oturum açtıktan sonra yönlendirilirsiniz **uygulamalarım** sayfası:</span><span class="sxs-lookup"><span data-stu-id="0ef22-110">After signing in you are redirected to **My applications** page:</span></span>

![Microsoft Geliştirici Portalı Microsoft Edge'de Aç](index/_static/MicrosoftDev.png)

* <span data-ttu-id="0ef22-112">Dokunun **uygulama ekleme** sağ üst köşedeki ve girin, **uygulama adı** ve **ilgili kişi e-posta**:</span><span class="sxs-lookup"><span data-stu-id="0ef22-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Yeni uygulama kaydı iletişim kutusu](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="0ef22-114">Bu öğreticinin amaçları doğrultusunda, Temizle **destekli Kurulum** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="0ef22-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="0ef22-115">Dokunun **Oluştur** devam etmek için **kayıt** sayfası.</span><span class="sxs-lookup"><span data-stu-id="0ef22-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="0ef22-116">Sağlayan bir **adı** ve değerini not edin **uygulama kimliği**, olarak kullanacağınız `ClientId` sonraki öğreticide:</span><span class="sxs-lookup"><span data-stu-id="0ef22-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Kayıt sayfası](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="0ef22-118">Dokunun **Platform Ekle** içinde **platformları** seçin ve bölüm **Web** platform:</span><span class="sxs-lookup"><span data-stu-id="0ef22-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Platform iletişim kutusu Ekle](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="0ef22-120">Yeni **Web** platform bölümünde, geliştirme URL'nizi girin `/signin-microsoft` içine eklenen **yeniden yönlendirme URL'lerini** alan (örneğin: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="0ef22-120">In the new **Web** platform section, enter your development URL with `/signin-microsoft` appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="0ef22-121">Bu öğreticinin ilerleyen bölümlerinde yapılandırılmış Microsoft kimlik doğrulama düzeni istekleri otomatik olarak işleyecek `/signin-microsoft` rota OAuth akışını uygulamak için:</span><span class="sxs-lookup"><span data-stu-id="0ef22-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow:</span></span>

![Web Platformu bölümü](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> <span data-ttu-id="0ef22-123">URI segmenti `/signin-microsoft` Microsoft kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="0ef22-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="0ef22-124">Microsoft kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="0ef22-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

* <span data-ttu-id="0ef22-125">Dokunun **URL Ekle** URL eklenen emin olmak için.</span><span class="sxs-lookup"><span data-stu-id="0ef22-125">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="0ef22-126">Gerekirse, diğer tüm uygulama ayarlarını doldurun ve dokunun **Kaydet** uygulama yapılandırma değişikliklerini kaydetmek için sayfanın alt kısmındaki.</span><span class="sxs-lookup"><span data-stu-id="0ef22-126">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="0ef22-127">Site dağıtırken yeniden ziyaret etmeniz gerekir **kayıt** sayfasında ve yeni bir genel URL'ye ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="0ef22-127">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="0ef22-128">Microsoft uygulama kimliği ve parola Store</span><span class="sxs-lookup"><span data-stu-id="0ef22-128">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="0ef22-129">Not `Application Id` görüntülenen **kayıt** sayfası.</span><span class="sxs-lookup"><span data-stu-id="0ef22-129">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="0ef22-130">Dokunun **yeni parola oluştur** içinde **uygulama gizli dizilerini** bölümü.</span><span class="sxs-lookup"><span data-stu-id="0ef22-130">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="0ef22-131">Böylece, uygulama parolasını burada kopyalayabilirsiniz kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="0ef22-131">This displays a box where you can copy the application password:</span></span>

![Yeni oluşturulan parolası iletişim kutusu](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="0ef22-133">Bağlantı Microsoft gibi hassas ayarlar `Application ID` ve `Password` yapılandırma kullanarak uygulama [gizli dizi Yöneticisi](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="0ef22-133">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="0ef22-134">Bu öğreticinin amaçları doğrultusunda, belirteçleri ad `Authentication:Microsoft:ApplicationId` ve `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="0ef22-134">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="0ef22-135">Microsoft hesabı kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0ef22-135">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="0ef22-136">Bu öğreticide kullanılan proje şablonu sağlar [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) paket zaten yüklü.</span><span class="sxs-lookup"><span data-stu-id="0ef22-136">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="0ef22-137">Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="0ef22-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="0ef22-138">.NET Core CLI ile yüklemek için proje dizininizde aşağıdakileri yürütün:</span><span class="sxs-lookup"><span data-stu-id="0ef22-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0ef22-139">Microsoft Account hizmetini de eklemek `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="0ef22-139">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0ef22-140">Microsoft Account Ara yazılımında ekleme `Configure` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="0ef22-140">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

<span data-ttu-id="0ef22-141">Microsoft Geliştirici portalında kullanılan terimler, bu belirteçleri adları olsa da `ApplicationId` ve `Password`, olarak gösterilen `ClientId` ve `ClientSecret` yapılandırma API'si için.</span><span class="sxs-lookup"><span data-stu-id="0ef22-141">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="0ef22-142">Bkz: [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Microsoft Account kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu.</span><span class="sxs-lookup"><span data-stu-id="0ef22-142">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="0ef22-143">Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0ef22-143">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="0ef22-144">Microsoft hesabıyla oturum açın</span><span class="sxs-lookup"><span data-stu-id="0ef22-144">Sign in with Microsoft Account</span></span>

<span data-ttu-id="0ef22-145">Uygulamanızı çalıştırın ve tıklayın **oturum**.</span><span class="sxs-lookup"><span data-stu-id="0ef22-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="0ef22-146">Microsoft'ta oturum açmak için bir seçenek görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="0ef22-146">An option to sign in with Microsoft appears:</span></span>

![Web uygulaması günlüğü sayfasında: Kullanıcı Kimliği](index/_static/DoneMicrosoft.png)

<span data-ttu-id="0ef22-148">Microsoft'a tıkladığınızda, kimlik doğrulaması için Microsoft'a yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="0ef22-148">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="0ef22-149">(Zaten açtıysanız) Microsoft Account imzaladıktan sonra uygulamayı bilgilere erişmesine izin istenir:</span><span class="sxs-lookup"><span data-stu-id="0ef22-149">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Microsoft kimlik doğrulama iletişim](index/_static/MicrosoftLogin.png)

<span data-ttu-id="0ef22-151">Dokunun **Evet** ve web e-postanızı ayarlayabileceğiniz siteye geri yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="0ef22-151">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="0ef22-152">Artık, Microsoft kimlik bilgilerinizi kullanarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="0ef22-152">You are now logged in using your Microsoft credentials:</span></span>

![Web uygulaması: Kimliği doğrulanmış kullanıcı](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="0ef22-154">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="0ef22-154">Troubleshooting</span></span>

* <span data-ttu-id="0ef22-155">Microsoft Account sağlayıcısı oturum açma hatası sayfasına yönlendirir, doğrudan aşağıdaki hata başlığı ve açıklamayı sorgu dizesi parametrelerini Not `#` (diyez etiketi) URI.</span><span class="sxs-lookup"><span data-stu-id="0ef22-155">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="0ef22-156">Microsoft kimlik doğrulama ile ilgili bir sorun göstermek için hata iletisini gibi görünüyor ancak uygulamanızı herhangi bir eşleşmeyen URI en yaygın nedeni **yeniden yönlendirme URI'leri** için belirtilen **Web** platformu .</span><span class="sxs-lookup"><span data-stu-id="0ef22-156">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="0ef22-157">**ASP.NET Core 2.x yalnızca:** Kimlik çağırarak yapılandırılmazsa `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması yapmaya sonuçlanır *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*.</span><span class="sxs-lookup"><span data-stu-id="0ef22-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="0ef22-158">Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="0ef22-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="0ef22-159">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="0ef22-159">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="0ef22-160">Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="0ef22-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ef22-161">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="0ef22-161">Next steps</span></span>

* <span data-ttu-id="0ef22-162">Bu makalede, Microsoft ile kimliğini nasıl doğrulayabileceğiniz gösterdi.</span><span class="sxs-lookup"><span data-stu-id="0ef22-162">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="0ef22-163">Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="0ef22-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="0ef22-164">Web sitenizi Azure web uygulaması yayımladıktan sonra yeni bir oluşturmalısınız `Password` Microsoft Geliştirici Portalı'nda.</span><span class="sxs-lookup"><span data-stu-id="0ef22-164">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="0ef22-165">Ayarlama `Authentication:Microsoft:ApplicationId` ve `Authentication:Microsoft:Password` Azure portalında uygulama ayarları olarak.</span><span class="sxs-lookup"><span data-stu-id="0ef22-165">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="0ef22-166">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="0ef22-166">The configuration system is set up to read keys from environment variables.</span></span>
