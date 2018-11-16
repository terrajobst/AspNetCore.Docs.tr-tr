---
title: ASP.NET Core ile twitter dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Twitter hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 43a5ea59d8853d297ae2c1ec3f4b1c0c14ec80c3
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708432"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="c4070-103">ASP.NET Core ile twitter dış oturum açma Kurulumu</span><span class="sxs-lookup"><span data-stu-id="c4070-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="c4070-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c4070-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c4070-105">Bu öğretici, kullanıcılarınıza etkinleştirme gösterir [kendi Twitter hesabıyla oturum](https://dev.twitter.com/web/sign-in/desktop-browser) üzerinde oluşturulmuş bir örnek ASP.NET Core 2.0 proje kullanarak [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="c4070-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="c4070-106">İçinde bir Twitter uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="c4070-106">Create the app in Twitter</span></span>

* <span data-ttu-id="c4070-107">Gidin [ https://apps.twitter.com/ ](https://apps.twitter.com/) ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="c4070-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="c4070-108">Twitter hesabıyla yoksa kullanın **[şimdi kaydolun](https://twitter.com/signup)** bağlantı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="c4070-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="c4070-109">Oturum açtıktan sonra **Uygulama Yönetimi** sayfası gösterilir:</span><span class="sxs-lookup"><span data-stu-id="c4070-109">After signing in, the **Application Management** page is shown:</span></span>

  ![Microsoft Edge'de açık twitter uygulama yönetimi](index/_static/TwitterAppManage.png)

* <span data-ttu-id="c4070-111">Dokunun **yeni uygulama oluştur** ve uygulamanın ölçeğini doldurun **adı**, **açıklama** ve genel **Web sitesi** (Bu olabilir geçici dek URI'si etki alanı adı kayıt):</span><span class="sxs-lookup"><span data-stu-id="c4070-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

  ![Uygulama sayfası oluşturma](index/_static/TwitterCreate.png)

* <span data-ttu-id="c4070-113">Geliştirme sürecinizi URI girin ile `/signin-twitter` içine eklenen **geçerli OAuth yeniden yönlendirme URI'leri** alan (örneğin: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="c4070-113">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="c4070-114">Bu öğreticinin ilerleyen bölümlerinde yapılandırılmış Twitter kimlik doğrulaması düzeni istekleri otomatik olarak işleyecek `/signin-twitter` OAuth akışını uygulamak için rota.</span><span class="sxs-lookup"><span data-stu-id="c4070-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="c4070-115">URI segmenti `/signin-twitter` Twitter kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c4070-115">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="c4070-116">Twitter kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) sınıf.</span><span class="sxs-lookup"><span data-stu-id="c4070-116">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="c4070-117">Kalan formu doldurun ve dokunun **kendi Twitter uygulamanızı oluşturun**.</span><span class="sxs-lookup"><span data-stu-id="c4070-117">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="c4070-118">Yeni uygulama ayrıntıları görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="c4070-118">New application details are displayed:</span></span>

  ![Uygulama sayfasında Ayrıntılar sekmesi](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="c4070-120">Site dağıtırken yeniden ziyaret etmeniz gerekir **Uygulama Yönetimi** sayfasında ve yeni bir ortak URI kaydedin.</span><span class="sxs-lookup"><span data-stu-id="c4070-120">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="c4070-121">Twitter ConsumerKey ve ConsumerSecret depolama</span><span class="sxs-lookup"><span data-stu-id="c4070-121">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="c4070-122">Bağlantı Twitter gibi hassas ayarları `Consumer Key` ve `Consumer Secret` yapılandırma kullanarak uygulama [gizli dizi Yöneticisi](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="c4070-122">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="c4070-123">Bu öğreticinin amaçları doğrultusunda, belirteçleri ad `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="c4070-123">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="c4070-124">Bu belirteçler bulunabilir **anahtarlar ve erişim belirteçleri** yeni Twitter uygulamanızı oluşturduktan sonra sekmesinde:</span><span class="sxs-lookup"><span data-stu-id="c4070-124">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Anahtarlar ve erişim belirteçleri sekmesi](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="c4070-126">Twitter kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c4070-126">Configure Twitter Authentication</span></span>

<span data-ttu-id="c4070-127">Bu öğreticide kullanılan proje şablonu sağlar [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) paket zaten yüklü.</span><span class="sxs-lookup"><span data-stu-id="c4070-127">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="c4070-128">Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="c4070-128">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="c4070-129">.NET Core CLI ile yüklemek için proje dizininizde aşağıdakileri yürütün:</span><span class="sxs-lookup"><span data-stu-id="c4070-129">To install with .NET Core CLI, execute the following in your project directory:</span></span>

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c4070-130">Twitter hizmetinde ekleme `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="c4070-130">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c4070-131">Twitter Ara yazılımında ekleme `Configure` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="c4070-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

<span data-ttu-id="c4070-132">Bkz: [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) Twitter kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu.</span><span class="sxs-lookup"><span data-stu-id="c4070-132">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="c4070-133">Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c4070-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="c4070-134">Oturum Twitter ile oturum aç</span><span class="sxs-lookup"><span data-stu-id="c4070-134">Sign in with Twitter</span></span>

<span data-ttu-id="c4070-135">Uygulamanızı çalıştırın ve tıklayın **oturum**.</span><span class="sxs-lookup"><span data-stu-id="c4070-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="c4070-136">Twitter ile oturum açmak için bir seçenek görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="c4070-136">An option to sign in with Twitter appears:</span></span>

![Web uygulaması: kullanıcı kimliği](index/_static/DoneTwitter.png)

<span data-ttu-id="c4070-138">Tıklayarak **Twitter** kimlik doğrulaması için Twitter'a yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="c4070-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Twitter kimlik doğrulaması sayfası](index/_static/TwitterLogin.png)

<span data-ttu-id="c4070-140">Twitter kimlik bilgilerinizi girdikten sonra web, e-posta ayarlayabileceğiniz siteye geri yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="c4070-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="c4070-141">Artık Twitter kimlik bilgilerinizi kullanarak günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="c4070-141">You are now logged in using your Twitter credentials:</span></span>

![Web uygulaması: kimliği doğrulanmış kullanıcı](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="c4070-143">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="c4070-143">Troubleshooting</span></span>

* <span data-ttu-id="c4070-144">**ASP.NET Core 2.x yalnızca:** , kimlik, çağırarak yapılandırılmamış `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması yapmaya sonuçlanır *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*.</span><span class="sxs-lookup"><span data-stu-id="c4070-144">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="c4070-145">Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="c4070-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="c4070-146">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="c4070-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="c4070-147">Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="c4070-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4070-148">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="c4070-148">Next steps</span></span>

* <span data-ttu-id="c4070-149">Bu makalede, Twitter ile kimliğini nasıl doğrulayabileceğiniz gösterdi.</span><span class="sxs-lookup"><span data-stu-id="c4070-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="c4070-150">Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="c4070-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="c4070-151">Web sitenizi Azure web uygulaması yayımladıktan sonra sıfırlamalısınız `ConsumerSecret` Twitter Geliştirici Portalı.</span><span class="sxs-lookup"><span data-stu-id="c4070-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="c4070-152">Ayarlama `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret` Azure portalında uygulama ayarları olarak.</span><span class="sxs-lookup"><span data-stu-id="c4070-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="c4070-153">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c4070-153">The configuration system is set up to read keys from environment variables.</span></span>
