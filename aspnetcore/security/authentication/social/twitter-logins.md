---
title: "Twitter dış oturum açma Kurulumu"
author: rick-anderson
description: "Bu öğretici, var olan bir ASP.NET Core uygulamaya Twitter hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterir."
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/twitter-logins
ms.openlocfilehash: d92f9b1f55c03018f88cf9298e981fc4b2c29f41
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="configuring-twitter-authentication"></a><span data-ttu-id="0d423-103">Twitter kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0d423-103">Configuring Twitter authentication</span></span>

<span data-ttu-id="0d423-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0d423-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0d423-105">Bu öğreticide, kullanıcılarınıza etkinleştirme gösterilir [kendi Twitter hesabıyla oturum](https://dev.twitter.com/web/sign-in/desktop-browser) örnek ASP.NET Core 2.0 proje kullanılarak oluşturulan üzerinde [önceki sayfaya](index.md).</span><span class="sxs-lookup"><span data-stu-id="0d423-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="0d423-106">Twitter içinde uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="0d423-106">Create the app in Twitter</span></span>

* <span data-ttu-id="0d423-107">Gidin [https://apps.twitter.com/](https://apps.twitter.com/) ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="0d423-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="0d423-108">Bir Twitter hesabı zaten yoksa kullanın  **[şimdi kaydolun](https://twitter.com/signup)**  bağlantı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0d423-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="0d423-109">' De oturum açtıktan sonra **Uygulama Yönetimi** sayfası gösterilir:</span><span class="sxs-lookup"><span data-stu-id="0d423-109">After signing in, the **Application Management** page is shown:</span></span>

![Microsoft Edge'de açık twitter uygulama yönetimi](index/_static/TwitterAppManage.png)

* <span data-ttu-id="0d423-111">Dokunun **yeni uygulama oluşturma** ve uygulamayı doldurun **adı**, **açıklama** ve ortak **Web sitesi** (Bu olabilir geçici dek URI etki alanı adını kaydettirerek):</span><span class="sxs-lookup"><span data-stu-id="0d423-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![Uygulama sayfası oluşturma](index/_static/TwitterCreate.png)

* <span data-ttu-id="0d423-113">Geliştirme URI girin ile */signin-twitter* içine eklenen **geçerli OAuth yeniden yönlendirme URI'ler** alan (örneğin: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="0d423-113">Enter your development URI with */signin-twitter* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="0d423-114">Daha sonra Bu öğreticide yapılandırılmış Twitter kimlik doğrulama şeması istekleri otomatik olarak işleyecek */signin-twitter* OAuth akış uygulamak için rota.</span><span class="sxs-lookup"><span data-stu-id="0d423-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at */signin-twitter* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="0d423-115">Kalan formu doldurun ve dokunun **Twitter uygulamanızı oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="0d423-115">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="0d423-116">Yeni uygulama ayrıntıları görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="0d423-116">New application details are displayed:</span></span>

![Ayrıntılar sekmesinde uygulama sayfası](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="0d423-118">Site dağıtırken yeniden ziyaret etmeniz gerekir **Uygulama Yönetimi** sayfasında ve yeni bir ortak URI kaydedin.</span><span class="sxs-lookup"><span data-stu-id="0d423-118">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="0d423-119">Twitter ConsumerKey ve ConsumerSecret depolama</span><span class="sxs-lookup"><span data-stu-id="0d423-119">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="0d423-120">Bağlantı Twitter gibi hassas ayarları `Consumer Key` ve `Consumer Secret` kullanarak uygulamanızı yapılandırma için [gizli Yöneticisi](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="0d423-120">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="0d423-121">Bu öğreticinin amaçları doğrultusunda belirteçleri ad `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="0d423-121">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="0d423-122">Bu belirteçler bulunabilir **anahtarları ve erişim belirteçleri** yeni Twitter uygulamanızı oluşturduktan sonra sekmesi:</span><span class="sxs-lookup"><span data-stu-id="0d423-122">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Anahtarlar ve erişim belirteçleri sekmesi](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="0d423-124">Twitter kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0d423-124">Configure Twitter Authentication</span></span>

<span data-ttu-id="0d423-125">Bu öğreticide kullanılan proje şablonu sağlar [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) paket zaten yüklü.</span><span class="sxs-lookup"><span data-stu-id="0d423-125">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="0d423-126">Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="0d423-126">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="0d423-127">.NET Core CLI ile yüklemek için proje dizininde aşağıdakileri yürütün:</span><span class="sxs-lookup"><span data-stu-id="0d423-127">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0d423-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0d423-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0d423-129">Twitter hizmetinde eklemek `ConfigureServices` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="0d423-129">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0d423-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0d423-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0d423-131">Twitter Ara yazılımında eklemek `Configure` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="0d423-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

<span data-ttu-id="0d423-132">Bkz: [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) Twitter kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu.</span><span class="sxs-lookup"><span data-stu-id="0d423-132">See the [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="0d423-133">Bu kullanıcı hakkında farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0d423-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="0d423-134">Oturum Twitter ile oturum aç</span><span class="sxs-lookup"><span data-stu-id="0d423-134">Sign in with Twitter</span></span>

<span data-ttu-id="0d423-135">Uygulamanızı çalıştırın ve tıklatın **oturum**.</span><span class="sxs-lookup"><span data-stu-id="0d423-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="0d423-136">Twitter ile oturum aç imzalamak için bir seçenek görünür:</span><span class="sxs-lookup"><span data-stu-id="0d423-136">An option to sign in with Twitter appears:</span></span>

![Web uygulaması: doğrulanmayan kullanıcı](index/_static/DoneTwitter.png)

<span data-ttu-id="0d423-138">Tıklayarak **Twitter** için Twitter kimlik doğrulaması için yeniden yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="0d423-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Twitter kimlik doğrulaması sayfası](index/_static/TwitterLogin.png)

<span data-ttu-id="0d423-140">Twitter kimlik bilgilerinizi girdikten sonra geri e-postanızı ayarlayabileceğiniz web sitesine yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0d423-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="0d423-141">Twitter kimlik bilgilerinizi kullanarak şimdi kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="0d423-141">You are now logged in using your Twitter credentials:</span></span>

![Web uygulaması: kimliği doğrulanmış kullanıcı](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="0d423-143">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="0d423-143">Troubleshooting</span></span>

* <span data-ttu-id="0d423-144">**ASP.NET Core 2.x yalnızca:** ise kimlik çağırarak yapılandırılmamış `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması gerçekleştirmeye neden olur *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*.</span><span class="sxs-lookup"><span data-stu-id="0d423-144">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="0d423-145">Bu öğreticide kullanılan proje şablonu, bu yapılır sağlar.</span><span class="sxs-lookup"><span data-stu-id="0d423-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="0d423-146">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız oldu istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="0d423-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="0d423-147">Dokunun **uygulamak geçişler** veritabanını oluşturmak ve hata devam etmek için yenileyin.</span><span class="sxs-lookup"><span data-stu-id="0d423-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d423-148">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="0d423-148">Next steps</span></span>

* <span data-ttu-id="0d423-149">Bu makalede, Twitter ile nasıl doğrulayabilir gösterdi.</span><span class="sxs-lookup"><span data-stu-id="0d423-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="0d423-150">Listelenen diğer sağlayıcıları, kimlik doğrulaması için benzer bir yaklaşım izleyebilirsiniz [önceki sayfaya](index.md).</span><span class="sxs-lookup"><span data-stu-id="0d423-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="0d423-151">Azure web uygulaması için web sitenizi yayımladıktan sonra sıfırlamalıdır `ConsumerSecret` Twitter Geliştirici Portalı'nda.</span><span class="sxs-lookup"><span data-stu-id="0d423-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="0d423-152">Ayarlama `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret` olarak Azure portalında uygulama ayarları.</span><span class="sxs-lookup"><span data-stu-id="0d423-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="0d423-153">Yapılandırma sistemi ortam değişkenlerinin anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="0d423-153">The configuration system is set up to read keys from environment variables.</span></span>
