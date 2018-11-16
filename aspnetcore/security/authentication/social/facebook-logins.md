---
title: ASP.NET core'da Facebook dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Facebook hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/facebook-logins
ms.openlocfilehash: e8ae16538b5d6844af7d983071fad629ebbe6217
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708510"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="8e6c6-103">ASP.NET core'da Facebook dış oturum açma Kurulumu</span><span class="sxs-lookup"><span data-stu-id="8e6c6-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="8e6c6-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8e6c6-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8e6c6-105">Bu öğreticide, oluşturulan bir örnek ASP.NET Core 2.0 proje kullanarak kendi Facebook hesabı ile oturum açmalarına işlemini göstermektedir [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="8e6c6-105">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="8e6c6-106">Facebook kimlik doğrulaması gerektiren [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-106">Facebook authentication requires the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package.</span></span> <span data-ttu-id="8e6c6-107">İzleyerek bir Facebook uygulama kimliği oluşturarak başlayın [resmi adımları](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="8e6c6-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="8e6c6-108">İçinde Facebook uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="8e6c6-108">Create the app in Facebook</span></span>

* <span data-ttu-id="8e6c6-109">Gidin [Facebook geliştiricilerin uygulama](https://developers.facebook.com/apps/) sayfasında ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="8e6c6-110">Bir Facebook hesabınız yoksa kullanırsanız **Facebook'a kaydolma** bağlantısı oluşturmak için oturum açma sayfası.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="8e6c6-111">Dokunun **yeni uygulama Ekle** yeni bir uygulama kimliği oluşturmak için sağ üst köşedeki düğmesi</span><span class="sxs-lookup"><span data-stu-id="8e6c6-111">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Microsoft Edge'de Facebook geliştiriciler portalı açın](index/_static/FBMyApps.png)

* <span data-ttu-id="8e6c6-113">Formu doldurun ve dokunun **uygulama kimliği oluşturma** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-113">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Yeni uygulama Kimliğini formu oluşturma](index/_static/FBNewAppId.png)

* <span data-ttu-id="8e6c6-115">Üzerinde **bir ürün seçin** sayfasında **Ayarla** üzerinde **Facebook oturum açma** kart.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-115">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

  ![Ürün kurulum sayfası](index/_static/FBProductSetup.png)

* <span data-ttu-id="8e6c6-117">**Hızlı** Sihirbazı ile başlar **Platform seçin** ilk sayfası.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="8e6c6-118">Sihirbaz tıklayarak şimdilik atla **ayarları** sol taraftaki menüden bağlantı:</span><span class="sxs-lookup"><span data-stu-id="8e6c6-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

  ![Ziyaretimde hızlı başlangıç](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="8e6c6-120">Size sunulan **istemci OAuth ayarları** sayfası:</span><span class="sxs-lookup"><span data-stu-id="8e6c6-120">You are presented with the **Client OAuth Settings** page:</span></span>

  ![İstemci OAuth Ayarları sayfası](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="8e6c6-122">Geliştirme sürecinizi URI girin ile */signin-facebook* içine eklenen **geçerli OAuth yeniden yönlendirme URI'leri** alan (örneğin: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="8e6c6-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="8e6c6-123">Bu öğreticinin ilerleyen bölümlerinde yapılandırılmış Facebook kimlik doğrulaması istekleri otomatik olarak işleyecek */signin-facebook* OAuth akışını uygulamak için rota.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="8e6c6-124">URI */signin-facebook* Facebook kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-124">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="8e6c6-125">Facebook kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) sınıf.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-125">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="8e6c6-126">Tıklayın **değişiklikleri kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-126">Click **Save Changes**.</span></span>

* <span data-ttu-id="8e6c6-127">Tıklayın **ayarları** > **temel** sol gezinti bağlantısı.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-127">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="8e6c6-128">Bu sayfada, Not, `App ID` ve `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-128">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="8e6c6-129">Sonraki bölümde, ASP.NET Core uygulamasına hem de ekleyeceksiniz:</span><span class="sxs-lookup"><span data-stu-id="8e6c6-129">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="8e6c6-130">Sitenin dağıtım yaparken yeniden ziyaret etmeniz gerekir **Facebook oturum açma** Kurulum sayfasında ve yeni bir ortak URI kaydedin.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-130">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="8e6c6-131">Facebook uygulama Kimliğiniz ve parolanız App Store</span><span class="sxs-lookup"><span data-stu-id="8e6c6-131">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="8e6c6-132">Bağlantı Facebook gibi hassas ayarları `App ID` ve `App Secret` yapılandırma kullanarak uygulama [gizli dizi Yöneticisi](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="8e6c6-132">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="8e6c6-133">Bu öğreticinin amaçları doğrultusunda, belirteçleri ad `Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-133">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="8e6c6-134">Güvenli bir şekilde depolamak için aşağıdaki komutları yürütün `App ID` ve `App Secret` gizli dizi Yöneticisi'ni kullanarak:</span><span class="sxs-lookup"><span data-stu-id="8e6c6-134">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="8e6c6-135">Facebook kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8e6c6-135">Configure Facebook Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8e6c6-136">Facebook hizmetinde ekleme `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8e6c6-136">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

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

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8e6c6-137">Yükleme [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) paket.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-137">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="8e6c6-138">Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-138">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="8e6c6-139">.NET Core CLI ile yüklemek için proje dizininizde aşağıdakileri yürütün:</span><span class="sxs-lookup"><span data-stu-id="8e6c6-139">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="8e6c6-140">Facebook Ara yazılımında ekleme `Configure` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8e6c6-140">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

<span data-ttu-id="8e6c6-141">Bkz: [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) Facebook kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-141">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="8e6c6-142">İçin yapılandırma seçenekleri kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="8e6c6-142">Configuration options can be used to:</span></span>

* <span data-ttu-id="8e6c6-143">Kullanıcı ile ilgili farklı bilgi isteyin.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-143">Request different information about the user.</span></span>
* <span data-ttu-id="8e6c6-144">Oturum açma deneyimi özelleştirmek için sorgu dizesi bağımsız değişkenleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-144">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="8e6c6-145">Facebook ile oturum açma</span><span class="sxs-lookup"><span data-stu-id="8e6c6-145">Sign in with Facebook</span></span>

<span data-ttu-id="8e6c6-146">Uygulamanızı çalıştırın ve tıklayın **oturum**.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="8e6c6-147">Facebook ile oturum açmak için bir seçenek görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-147">You see an option to sign in with Facebook.</span></span>

![Web uygulaması: kullanıcı kimliği](index/_static/DoneFacebook.png)

<span data-ttu-id="8e6c6-149">Tıkladığınızda **Facebook**, kimlik doğrulaması için Facebook için yönlendirilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="8e6c6-149">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Facebook kimlik doğrulaması sayfası](index/_static/FBLogin.png)

<span data-ttu-id="8e6c6-151">Facebook kimlik doğrulaması varsayılan olarak genel profil ve e-posta adresi ister:</span><span class="sxs-lookup"><span data-stu-id="8e6c6-151">Facebook authentication requests public profile and email address by default:</span></span>

![Facebook kimlik doğrulaması sayfası](index/_static/FBLoginDone.png)

<span data-ttu-id="8e6c6-153">Facebook kimlik bilgilerinizi girdikten sonra e-postanızı ayarlayabildiğiniz sitenize geri yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-153">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="8e6c6-154">Şimdi, Facebook kimlik bilgilerinizi kullanarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="8e6c6-154">You are now logged in using your Facebook credentials:</span></span>

![Web uygulaması: kimliği doğrulanmış kullanıcı](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="8e6c6-156">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="8e6c6-156">Troubleshooting</span></span>

* <span data-ttu-id="8e6c6-157">**ASP.NET Core 2.x yalnızca:** , kimlik, çağırarak yapılandırılmamış `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması yapmaya sonuçlanır *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="8e6c6-158">Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="8e6c6-159">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış varsa *bir veritabanı işlemi başarısız istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-159">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="8e6c6-160">Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e6c6-161">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="8e6c6-161">Next steps</span></span>

* <span data-ttu-id="8e6c6-162">Bu makalede, Facebook ile kimliğini nasıl doğrulayabileceğiniz gösterdi.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-162">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="8e6c6-163">Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="8e6c6-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="8e6c6-164">Web sitenizi Azure web uygulaması yayımladıktan sonra sıfırlamalısınız `AppSecret` Facebook Geliştirici Portalı.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-164">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="8e6c6-165">Ayarlama `Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret` Azure portalında uygulama ayarları olarak.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-165">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="8e6c6-166">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8e6c6-166">The configuration system is set up to read keys from environment variables.</span></span>
