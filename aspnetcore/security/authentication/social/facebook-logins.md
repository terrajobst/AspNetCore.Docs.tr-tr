---
title: ASP.NET Core Facebook dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğretici, var olan bir ASP.NET Core uygulamaya Facebook hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterir.
manager: wpickett
ms.author: riande
ms.date: 08/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/facebook-logins
ms.openlocfilehash: 70a4b2e53be335b8854b0aef3cfbf8f4e21e6ebe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="697de-103">ASP.NET Core Facebook dış oturum açma Kurulumu</span><span class="sxs-lookup"><span data-stu-id="697de-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="697de-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="697de-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="697de-105">Bu öğreticide, kullanıcılarınızın üzerinde oluşturulmuş bir örnek ASP.NET Core 2.0 projeyi kullanarak kendi Facebook hesabıyla oturum olanak tanımak nasıl gösterilir [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="697de-105">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="697de-106">İzleyerek bir Facebook uygulama kimliği oluşturarak başlangıç [resmi adımları](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="697de-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="697de-107">İçinde Facebook uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="697de-107">Create the app in Facebook</span></span>

*  <span data-ttu-id="697de-108">Gidin [Facebook geliştiricilerin uygulama](https://developers.facebook.com/apps/) sayfasında ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="697de-108">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="697de-109">Bir Facebook hesabıyla zaten yoksa kullanmak **Facebook için kaydolun** bağlantısı oluşturmak için oturum açma sayfası.</span><span class="sxs-lookup"><span data-stu-id="697de-109">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="697de-110">Dokunun **yeni bir uygulama eklemek** yeni bir uygulama kimliği oluşturmak için sağ üst köşesinde düğmesi</span><span class="sxs-lookup"><span data-stu-id="697de-110">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Geliştiriciler portalı için Facebook Microsoft Edge'de açın](index/_static/FBMyApps.png)

* <span data-ttu-id="697de-112">Formu doldurun ve dokunun **uygulama kimliği oluşturma** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="697de-112">Fill out the form and tap the **Create App ID** button.</span></span>

   ![Yeni uygulama kimliği formu oluşturma](index/_static/FBNewAppId.png)

* <span data-ttu-id="697de-114">Üzerinde **bir ürün seçmek** sayfasında, **Ayarla** üzerinde **Facebook oturum açma** kart.</span><span class="sxs-lookup"><span data-stu-id="697de-114">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

   ![Ürün kurulum sayfası](index/_static/FBProductSetup.png)

* <span data-ttu-id="697de-116">**Hızlı Başlangıç** Sihirbazı ile başlar **bir Platform seçin** ilk sayfası.</span><span class="sxs-lookup"><span data-stu-id="697de-116">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="697de-117">Sihirbazı'nı tıklatarak şimdilik atlama **ayarları** soldaki menüde bağlantı:</span><span class="sxs-lookup"><span data-stu-id="697de-117">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Atla hızlı başlangıç](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="697de-119">İle sunulan **istemci OAuth ayarları** sayfa:</span><span class="sxs-lookup"><span data-stu-id="697de-119">You are presented with the **Client OAuth Settings** page:</span></span>

![İstemci OAuth Ayarları sayfası](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="697de-121">Geliştirme URI girin ile */signin-facebook* içine eklenen **geçerli OAuth yeniden yönlendirme URI'ler** alan (örneğin: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="697de-121">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="697de-122">Daha sonra Bu öğreticide yapılandırılmış Facebook kimlik doğrulama istekleri otomatik olarak işleyecek */signin-facebook* OAuth akış uygulamak için rota.</span><span class="sxs-lookup"><span data-stu-id="697de-122">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="697de-123">Tıklatın **değişiklikleri kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="697de-123">Click **Save Changes**.</span></span>

* <span data-ttu-id="697de-124">Tıklatın **Pano** sol gezinti bağlantısı.</span><span class="sxs-lookup"><span data-stu-id="697de-124">Click the **Dashboard** link in the left navigation.</span></span> 

    <span data-ttu-id="697de-125">Bu sayfada, Not, `App ID` ve `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="697de-125">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="697de-126">ASP.NET Core uygulamanıza hem de sonraki bölümde ekleyecek:</span><span class="sxs-lookup"><span data-stu-id="697de-126">You will add both into your ASP.NET Core application in the next section:</span></span>

   ![Facebook Geliştirici Panosu](index/_static/FBDashboard.png)

* <span data-ttu-id="697de-128">Site dağıtırken yeniden ziyaret etmeniz gereken **Facebook oturum açma** Kurulum sayfasında ve yeni bir ortak URI kaydedin.</span><span class="sxs-lookup"><span data-stu-id="697de-128">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="697de-129">Facebook uygulama kimliği ve uygulama gizli anahtarı depolar</span><span class="sxs-lookup"><span data-stu-id="697de-129">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="697de-130">Bağlantı Facebook gibi hassas ayarları `App ID` ve `App Secret` kullanarak uygulamanızı yapılandırma için [gizli Yöneticisi](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="697de-130">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="697de-131">Bu öğreticinin amaçları doğrultusunda belirteçleri ad `Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="697de-131">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="697de-132">Güvenli bir şekilde depolamak için aşağıdaki komutları yürütün `App ID` ve `App Secret` gizli Yöneticisi'ni kullanarak:</span><span class="sxs-lookup"><span data-stu-id="697de-132">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="697de-133">Facebook kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="697de-133">Configure Facebook Authentication</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="697de-134">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="697de-134">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="697de-135">Facebook hizmetinde eklemek `ConfigureServices` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="697de-135">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="697de-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="697de-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="697de-137">Yükleme [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) paket.</span><span class="sxs-lookup"><span data-stu-id="697de-137">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="697de-138">Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="697de-138">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="697de-139">.NET Core CLI ile yüklemek için proje dizininde aşağıdakileri yürütün:</span><span class="sxs-lookup"><span data-stu-id="697de-139">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="697de-140">Facebook Ara yazılımında eklemek `Configure` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="697de-140">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

* * *
<span data-ttu-id="697de-141">Bkz: [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) Facebook kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu.</span><span class="sxs-lookup"><span data-stu-id="697de-141">See the [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="697de-142">Yapılandırma seçenekleri için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="697de-142">Configuration options can be used to:</span></span>

* <span data-ttu-id="697de-143">Farklı kullanıcı bilgilerini ister.</span><span class="sxs-lookup"><span data-stu-id="697de-143">Request different information about the user.</span></span>
* <span data-ttu-id="697de-144">Oturum açma deneyimini özelleştirmesini sorgu dizesi bağımsız değişkenleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="697de-144">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="697de-145">Facebook ile oturum açma</span><span class="sxs-lookup"><span data-stu-id="697de-145">Sign in with Facebook</span></span>

<span data-ttu-id="697de-146">Uygulamanızı çalıştırın ve tıklatın **oturum**.</span><span class="sxs-lookup"><span data-stu-id="697de-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="697de-147">Facebook ile oturum için bir seçenek görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="697de-147">You see an option to sign in with Facebook.</span></span>

![Web uygulaması: doğrulanmayan kullanıcı](index/_static/DoneFacebook.png)

<span data-ttu-id="697de-149">Tıkladığınızda **Facebook**, kimlik doğrulaması için Facebook için yönlendirilir:</span><span class="sxs-lookup"><span data-stu-id="697de-149">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Facebook kimlik doğrulaması sayfası](index/_static/FBLogin.png)

<span data-ttu-id="697de-151">Facebook kimlik doğrulaması varsayılan olarak genel profil ve e-posta adresi ister:</span><span class="sxs-lookup"><span data-stu-id="697de-151">Facebook authentication requests public profile and email address by default:</span></span>

![Facebook kimlik doğrulaması sayfası](index/_static/FBLoginDone.png)

<span data-ttu-id="697de-153">Facebook kimlik bilgilerinizi girdikten sonra e-posta ayarlayabileceğiniz geri sitenize yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="697de-153">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="697de-154">Facebook kimlik bilgilerinizi kullanarak şimdi kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="697de-154">You are now logged in using your Facebook credentials:</span></span>

![Web uygulaması: kimliği doğrulanmış kullanıcı](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="697de-156">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="697de-156">Troubleshooting</span></span>

* <span data-ttu-id="697de-157">**ASP.NET Core 2.x yalnızca:** ise kimlik çağırarak yapılandırılmamış `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması gerçekleştirmeye neden olur *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*.</span><span class="sxs-lookup"><span data-stu-id="697de-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="697de-158">Bu öğreticide kullanılan proje şablonu, bu yapılır sağlar.</span><span class="sxs-lookup"><span data-stu-id="697de-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="697de-159">Site veritabanı, ilk geçiş uygulayarak oluşturulmadı, alırsınız *bir veritabanı işlemi başarısız oldu istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="697de-159">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="697de-160">Dokunun **uygulamak geçişler** veritabanını oluşturmak ve hata devam etmek için yenileyin.</span><span class="sxs-lookup"><span data-stu-id="697de-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="697de-161">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="697de-161">Next steps</span></span>

* <span data-ttu-id="697de-162">Bu makalede, Facebook ile nasıl doğrulayabilir gösterdi.</span><span class="sxs-lookup"><span data-stu-id="697de-162">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="697de-163">Listelenen diğer sağlayıcıları, kimlik doğrulaması için benzer bir yaklaşım izleyebilirsiniz [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="697de-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="697de-164">Azure web uygulaması için web sitenizi yayımladıktan sonra sıfırlamalıdır `AppSecret` Facebook Geliştirici Portalı'nda.</span><span class="sxs-lookup"><span data-stu-id="697de-164">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="697de-165">Ayarlama `Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret` olarak Azure portalında uygulama ayarları.</span><span class="sxs-lookup"><span data-stu-id="697de-165">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="697de-166">Yapılandırma sistemi ortam değişkenlerinin anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="697de-166">The configuration system is set up to read keys from environment variables.</span></span>
