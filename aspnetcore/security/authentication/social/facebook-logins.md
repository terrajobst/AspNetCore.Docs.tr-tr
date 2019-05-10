---
title: ASP.NET core'da Facebook dış oturum açma Kurulumu
author: rick-anderson
description: Mevcut bir ASP.NET Core uygulamasına Facebook hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösteren kod örnekleri içeren öğretici.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 03/04/2019
uid: security/authentication/facebook-logins
ms.openlocfilehash: 84b891f6cee71a26726d49a9d42ae45007d2b429
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899861"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="f33e9-103">ASP.NET core'da Facebook dış oturum açma Kurulumu</span><span class="sxs-lookup"><span data-stu-id="f33e9-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="f33e9-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f33e9-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f33e9-105">Kod örnekleri bu öğreticiyle oluşturulan bir örnek ASP.NET Core 2.0 proje kullanarak kendi Facebook hesabı ile oturum açmalarına gösterilmektedir [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="f33e9-105">This tutorial with code examples shows how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="f33e9-106">İzleyerek bir Facebook uygulama kimliği oluşturarak başlayın [resmi adımları](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="f33e9-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="f33e9-107">İçinde Facebook uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="f33e9-107">Create the app in Facebook</span></span>

* <span data-ttu-id="f33e9-108">Gidin [Facebook geliştiricilerin uygulama](https://developers.facebook.com/apps/) sayfasında ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="f33e9-108">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="f33e9-109">Bir Facebook hesabınız yoksa kullanırsanız **Facebook'a kaydolma** bağlantısı oluşturmak için oturum açma sayfası.</span><span class="sxs-lookup"><span data-stu-id="f33e9-109">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="f33e9-110">Dokunun **yeni uygulama Ekle** yeni bir uygulama kimliği oluşturmak için sağ üst köşedeki düğmesi</span><span class="sxs-lookup"><span data-stu-id="f33e9-110">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Microsoft Edge'de Facebook geliştiriciler portalı açın](index/_static/FBMyApps.png)

* <span data-ttu-id="f33e9-112">Formu doldurun ve dokunun **uygulama kimliği oluşturma** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="f33e9-112">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Yeni uygulama Kimliğini formu oluşturma](index/_static/FBNewAppId.png)

* <span data-ttu-id="f33e9-114">Üzerinde **bir ürün seçin** sayfasında **Ayarla** üzerinde **Facebook oturum açma** kart.</span><span class="sxs-lookup"><span data-stu-id="f33e9-114">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

  ![Ürün kurulum sayfası](index/_static/FBProductSetup.png)

* <span data-ttu-id="f33e9-116">**Hızlı** Sihirbazı ile başlar **Platform seçin** ilk sayfası.</span><span class="sxs-lookup"><span data-stu-id="f33e9-116">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="f33e9-117">Sihirbaz tıklayarak şimdilik atla **ayarları** sol taraftaki menüden bağlantı:</span><span class="sxs-lookup"><span data-stu-id="f33e9-117">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

  ![Ziyaretimde hızlı başlangıç](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="f33e9-119">Size sunulan **istemci OAuth ayarları** sayfası:</span><span class="sxs-lookup"><span data-stu-id="f33e9-119">You are presented with the **Client OAuth Settings** page:</span></span>

  ![İstemci OAuth Ayarları sayfası](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="f33e9-121">Geliştirme sürecinizi URI girin ile */signin-facebook* içine eklenen **geçerli OAuth yeniden yönlendirme URI'leri** alan (örneğin: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="f33e9-121">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="f33e9-122">Bu öğreticinin ilerleyen bölümlerinde yapılandırılmış Facebook kimlik doğrulaması istekleri otomatik olarak işleyecek */signin-facebook* OAuth akışını uygulamak için rota.</span><span class="sxs-lookup"><span data-stu-id="f33e9-122">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="f33e9-123">URI */signin-facebook* Facebook kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="f33e9-123">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="f33e9-124">Facebook kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) sınıf.</span><span class="sxs-lookup"><span data-stu-id="f33e9-124">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="f33e9-125">Tıklayın **değişiklikleri kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="f33e9-125">Click **Save Changes**.</span></span>

* <span data-ttu-id="f33e9-126">Tıklayın **ayarları** > **temel** sol gezinti bağlantısı.</span><span class="sxs-lookup"><span data-stu-id="f33e9-126">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="f33e9-127">Bu sayfada, Not, `App ID` ve `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="f33e9-127">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="f33e9-128">Sonraki bölümde, ASP.NET Core uygulamasına hem de ekleyeceksiniz:</span><span class="sxs-lookup"><span data-stu-id="f33e9-128">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="f33e9-129">Sitenin dağıtım yaparken yeniden ziyaret etmeniz gerekir **Facebook oturum açma** Kurulum sayfasında ve yeni bir ortak URI kaydedin.</span><span class="sxs-lookup"><span data-stu-id="f33e9-129">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="f33e9-130">Facebook uygulama Kimliğiniz ve parolanız App Store</span><span class="sxs-lookup"><span data-stu-id="f33e9-130">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="f33e9-131">Bağlantı Facebook gibi hassas ayarları `App ID` ve `App Secret` yapılandırma kullanarak uygulama [gizli dizi Yöneticisi](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="f33e9-131">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="f33e9-132">Bu öğreticinin amaçları doğrultusunda, belirteçleri ad `Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="f33e9-132">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="f33e9-133">Güvenli bir şekilde depolamak için aşağıdaki komutları yürütün `App ID` ve `App Secret` gizli dizi Yöneticisi'ni kullanarak:</span><span class="sxs-lookup"><span data-stu-id="f33e9-133">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="f33e9-134">Facebook kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f33e9-134">Configure Facebook Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f33e9-135">Facebook hizmetinde ekleme `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f33e9-135">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

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

<span data-ttu-id="f33e9-136">Yükleme [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) paket.</span><span class="sxs-lookup"><span data-stu-id="f33e9-136">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="f33e9-137">Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="f33e9-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="f33e9-138">.NET Core CLI ile yüklemek için proje dizininizde aşağıdakileri yürütün:</span><span class="sxs-lookup"><span data-stu-id="f33e9-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="f33e9-139">Facebook Ara yazılımında ekleme `Configure` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f33e9-139">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

<span data-ttu-id="f33e9-140">Bkz: [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) Facebook kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu.</span><span class="sxs-lookup"><span data-stu-id="f33e9-140">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="f33e9-141">İçin yapılandırma seçenekleri kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="f33e9-141">Configuration options can be used to:</span></span>

* <span data-ttu-id="f33e9-142">Kullanıcı ile ilgili farklı bilgi isteyin.</span><span class="sxs-lookup"><span data-stu-id="f33e9-142">Request different information about the user.</span></span>
* <span data-ttu-id="f33e9-143">Oturum açma deneyimi özelleştirmek için sorgu dizesi bağımsız değişkenleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f33e9-143">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="f33e9-144">Facebook ile oturum açma</span><span class="sxs-lookup"><span data-stu-id="f33e9-144">Sign in with Facebook</span></span>

<span data-ttu-id="f33e9-145">Uygulamanızı çalıştırın ve tıklayın **oturum**.</span><span class="sxs-lookup"><span data-stu-id="f33e9-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="f33e9-146">Facebook ile oturum açmak için bir seçenek görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="f33e9-146">You see an option to sign in with Facebook.</span></span>

![Web uygulaması: Kullanıcı Kimliği](index/_static/DoneFacebook.png)

<span data-ttu-id="f33e9-148">Tıkladığınızda **Facebook**, kimlik doğrulaması için Facebook için yönlendirilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="f33e9-148">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Facebook kimlik doğrulaması sayfası](index/_static/FBLogin.png)

<span data-ttu-id="f33e9-150">Facebook kimlik doğrulaması varsayılan olarak genel profil ve e-posta adresi ister:</span><span class="sxs-lookup"><span data-stu-id="f33e9-150">Facebook authentication requests public profile and email address by default:</span></span>

![Facebook kimlik doğrulaması sayfası onay ekranı](index/_static/FBLoginDone.png)

<span data-ttu-id="f33e9-152">Facebook kimlik bilgilerinizi girdikten sonra e-postanızı ayarlayabildiğiniz sitenize geri yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="f33e9-152">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="f33e9-153">Şimdi, Facebook kimlik bilgilerinizi kullanarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="f33e9-153">You are now logged in using your Facebook credentials:</span></span>

![Web uygulaması: Kimliği doğrulanmış kullanıcı](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="f33e9-155">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="f33e9-155">Troubleshooting</span></span>

* <span data-ttu-id="f33e9-156">**ASP.NET Core 2.x yalnızca:** Kimlik çağırarak yapılandırılmazsa `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması yapmaya sonuçlanır *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*.</span><span class="sxs-lookup"><span data-stu-id="f33e9-156">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="f33e9-157">Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="f33e9-157">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="f33e9-158">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış varsa *bir veritabanı işlemi başarısız istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="f33e9-158">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="f33e9-159">Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="f33e9-159">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f33e9-160">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="f33e9-160">Next steps</span></span>

* <span data-ttu-id="f33e9-161">Ekleme [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet paketini projenize Gelişmiş Facebook kimlik doğrulama senaryoları için.</span><span class="sxs-lookup"><span data-stu-id="f33e9-161">Add the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package to your project for advanced Facebook authentication scenarios.</span></span> <span data-ttu-id="f33e9-162">Bu paket, Facebook dış oturum açma işlevselliği uygulamanızla tümleştirmek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="f33e9-162">This package isn't required to integrate Facebook external login functionality with your app.</span></span> 

* <span data-ttu-id="f33e9-163">Bu makalede, Facebook ile kimliğini nasıl doğrulayabileceğiniz gösterdi.</span><span class="sxs-lookup"><span data-stu-id="f33e9-163">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="f33e9-164">Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="f33e9-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="f33e9-165">Web sitenizi Azure web uygulaması yayımladıktan sonra sıfırlamalısınız `AppSecret` Facebook Geliştirici Portalı.</span><span class="sxs-lookup"><span data-stu-id="f33e9-165">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="f33e9-166">Ayarlama `Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret` Azure portalında uygulama ayarları olarak.</span><span class="sxs-lookup"><span data-stu-id="f33e9-166">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="f33e9-167">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="f33e9-167">The configuration system is set up to read keys from environment variables.</span></span>
