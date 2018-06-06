---
title: ASP.NET Core Microsoft Account dış oturum açma ayarı
author: rick-anderson
description: Bu öğretici, var olan bir ASP.NET Core uygulamaya Microsoft hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterir.
manager: wpickett
ms.author: riande
ms.date: 08/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/microsoft-logins
ms.openlocfilehash: a9bf7b49b1cfdfff65c639eed1e14c94c5432350
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34689028"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="986d6-103">ASP.NET Core Microsoft Account dış oturum açma ayarı</span><span class="sxs-lookup"><span data-stu-id="986d6-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="986d6-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="986d6-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="986d6-105">Bu öğreticide, kullanıcılarınızın üzerinde oluşturulmuş bir örnek ASP.NET Core 2.0 projeyi kullanarak kendi Microsoft hesabıyla oturum olanak tanımak nasıl gösterilir [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="986d6-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="986d6-106">Microsoft Geliştirici Portalı'nda uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="986d6-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="986d6-107">Gidin [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) ve oluşturun veya bir Microsoft hesabı oturum açın:</span><span class="sxs-lookup"><span data-stu-id="986d6-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![İletişim kutusunda oturum](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="986d6-109">Zaten bir Microsoft hesabınız yoksa, dokunun  **[oluşturun!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="986d6-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="986d6-110">Oturum açtıktan sonra yönlendirilirsiniz **uygulamalarım** sayfa:</span><span class="sxs-lookup"><span data-stu-id="986d6-110">After signing in you are redirected to **My applications** page:</span></span>

![Microsoft Geliştirici Portalı Microsoft Edge'de Aç](index/_static/MicrosoftDev.png)

* <span data-ttu-id="986d6-112">Dokunun **bir uygulama ekleyin** sağ üst köşe ve girin, **uygulama adı** ve **ilgili kişi e-posta**:</span><span class="sxs-lookup"><span data-stu-id="986d6-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Yeni bir uygulama kaydı iletişim kutusu](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="986d6-114">Bu öğreticinin amaçları doğrultusunda temizleyin **destekli Kurulum** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="986d6-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="986d6-115">Dokunun **oluşturma** devam etmek için **kayıt** sayfası.</span><span class="sxs-lookup"><span data-stu-id="986d6-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="986d6-116">Sağlayan bir **adı** ve değerini not edin **uygulama kimliği**, olarak kullanacağınız `ClientId` öğreticide daha sonra:</span><span class="sxs-lookup"><span data-stu-id="986d6-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Kayıt sayfası](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="986d6-118">Dokunun **eklemek Platform** içinde **platformları** bölümünde ve seçin **Web** platform:</span><span class="sxs-lookup"><span data-stu-id="986d6-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Platform iletişim ekleyin](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="986d6-120">Yeni **Web** platform bölümünde, geliştirme URL'nizi girin */signin-microsoft* içine eklenen **yönlendirme URL'si** alan (örneğin: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="986d6-120">In the new **Web** platform section, enter your development URL with */signin-microsoft* appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="986d6-121">Daha sonra Bu öğreticide yapılandırılmış Microsoft kimlik doğrulama şeması istekleri otomatik olarak işleyecek */signin-microsoft* OAuth akış uygulamak için rota:</span><span class="sxs-lookup"><span data-stu-id="986d6-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at */signin-microsoft* route to implement the OAuth flow:</span></span>

![Web Platformu bölümünde](index/_static/MicrosoftRedirectUri.png)

* <span data-ttu-id="986d6-123">Dokunun **URL Ekle** URL eklenen emin olmak için.</span><span class="sxs-lookup"><span data-stu-id="986d6-123">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="986d6-124">Gerekiyorsa, herhangi bir uygulama ayarı doldurun ve dokunun **kaydetmek** için uygulama yapılandırma değişiklikleri kaydetmek için sayfanın altındaki.</span><span class="sxs-lookup"><span data-stu-id="986d6-124">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="986d6-125">Site dağıtırken yeniden ziyaret etmeniz gerekir **kayıt** sayfasında ve yeni bir ortak URL'sini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="986d6-125">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="986d6-126">Microsoft uygulama kimliği ve parola depolama</span><span class="sxs-lookup"><span data-stu-id="986d6-126">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="986d6-127">Not `Application Id` görüntülenen **kayıt** sayfası.</span><span class="sxs-lookup"><span data-stu-id="986d6-127">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="986d6-128">Dokunun **yeni bir parola oluşturmak** içinde **uygulama parolaları** bölümü.</span><span class="sxs-lookup"><span data-stu-id="986d6-128">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="986d6-129">Bu uygulama parolasını burada kopyalayabilirsiniz kutusunu görüntüler:</span><span class="sxs-lookup"><span data-stu-id="986d6-129">This displays a box where you can copy the application password:</span></span>

![Yeni oluşturulan parola iletişim kutusu](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="986d6-131">Bağlantı Microsoft gibi hassas ayarları `Application ID` ve `Password` kullanarak uygulamanızı yapılandırma için [gizli Yöneticisi](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="986d6-131">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="986d6-132">Bu öğreticinin amaçları doğrultusunda belirteçleri ad `Authentication:Microsoft:ApplicationId` ve `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="986d6-132">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="986d6-133">Microsoft hesabı kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="986d6-133">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="986d6-134">Bu öğreticide kullanılan proje şablonu sağlar [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) paket zaten yüklü.</span><span class="sxs-lookup"><span data-stu-id="986d6-134">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="986d6-135">Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="986d6-135">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="986d6-136">.NET Core CLI ile yüklemek için proje dizininde aşağıdakileri yürütün:</span><span class="sxs-lookup"><span data-stu-id="986d6-136">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="986d6-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="986d6-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="986d6-138">Microsoft Account hizmetini eklemek `ConfigureServices` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="986d6-138">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="986d6-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="986d6-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="986d6-140">Microsoft Account Ara eklemek `Configure` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="986d6-140">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

<span data-ttu-id="986d6-141">Bu belirteçler Microsoft Geliştirici Portalı'ndaki kullanılan terminolojiyi adları olsa da `ApplicationId` ve `Password`, olarak sunulan `ClientId` ve `ClientSecret` yapılandırma API'si için.</span><span class="sxs-lookup"><span data-stu-id="986d6-141">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="986d6-142">Bkz: [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Microsoft Account kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu.</span><span class="sxs-lookup"><span data-stu-id="986d6-142">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="986d6-143">Bu kullanıcı hakkında farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="986d6-143">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="986d6-144">Microsoft hesapla oturum açın</span><span class="sxs-lookup"><span data-stu-id="986d6-144">Sign in with Microsoft Account</span></span>

<span data-ttu-id="986d6-145">Uygulamanızı çalıştırın ve tıklatın **oturum**.</span><span class="sxs-lookup"><span data-stu-id="986d6-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="986d6-146">Microsoft ile imzalamak için bir seçenek görünür:</span><span class="sxs-lookup"><span data-stu-id="986d6-146">An option to sign in with Microsoft appears:</span></span>

![Web uygulaması günlük sayfasındaki: doğrulanmayan kullanıcı](index/_static/DoneMicrosoft.png)

<span data-ttu-id="986d6-148">Microsoft tıkladığınızda, kimlik doğrulaması için Microsoft'a yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="986d6-148">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="986d6-149">(Henüz oturum açtıysanız) Microsoft Account oturum sonra bilgilere erişmesine izin verilsin istenir:</span><span class="sxs-lookup"><span data-stu-id="986d6-149">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Microsoft kimlik doğrulama iletişim](index/_static/MicrosoftLogin.png)

<span data-ttu-id="986d6-151">Dokunun **Evet** ve geri e-postanızı ayarlayabileceğiniz web sitesine yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="986d6-151">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="986d6-152">Microsoft kimlik bilgilerinizi kullanarak şimdi kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="986d6-152">You are now logged in using your Microsoft credentials:</span></span>

![Web uygulaması: kimliği doğrulanmış kullanıcı](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="986d6-154">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="986d6-154">Troubleshooting</span></span>

* <span data-ttu-id="986d6-155">Microsoft Account sağlayıcısı, bir oturum açma hatası sayfasına yönlendirir, doğrudan aşağıdaki hata başlığı ve açıklamayı sorgu dizesi parametreleri Not `#` (diyez) URI.</span><span class="sxs-lookup"><span data-stu-id="986d6-155">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="986d6-156">Microsoft kimlik doğrulaması ile ilgili bir sorun belirtmek için hata iletisini görünüyor en yaygın nedeni, uygulamanızın herhangi birini eşleşmeyen URI olsa da **yeniden yönlendirme URI'ler** için belirtilen **Web** platform .</span><span class="sxs-lookup"><span data-stu-id="986d6-156">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="986d6-157">**ASP.NET Core 2.x yalnızca:** ise kimlik çağırarak yapılandırılmamış `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması gerçekleştirmeye neden olur *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*.</span><span class="sxs-lookup"><span data-stu-id="986d6-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="986d6-158">Bu öğreticide kullanılan proje şablonu, bu yapılır sağlar.</span><span class="sxs-lookup"><span data-stu-id="986d6-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="986d6-159">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız oldu istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="986d6-159">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="986d6-160">Dokunun **uygulamak geçişler** veritabanını oluşturmak ve hata devam etmek için yenileyin.</span><span class="sxs-lookup"><span data-stu-id="986d6-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="986d6-161">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="986d6-161">Next steps</span></span>

* <span data-ttu-id="986d6-162">Bu makalede, Microsoft ile nasıl doğrulayabilir gösterdi.</span><span class="sxs-lookup"><span data-stu-id="986d6-162">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="986d6-163">Listelenen diğer sağlayıcıları, kimlik doğrulaması için benzer bir yaklaşım izleyebilirsiniz [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="986d6-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="986d6-164">Azure web uygulaması için web sitenizi yayımladıktan sonra yeni bir oluşturmalısınız `Password` Microsoft Geliştirici Portalı'nda.</span><span class="sxs-lookup"><span data-stu-id="986d6-164">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="986d6-165">Ayarlama `Authentication:Microsoft:ApplicationId` ve `Authentication:Microsoft:Password` olarak Azure portalında uygulama ayarları.</span><span class="sxs-lookup"><span data-stu-id="986d6-165">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="986d6-166">Yapılandırma sistemi ortam değişkenlerinin anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="986d6-166">The configuration system is set up to read keys from environment variables.</span></span>
