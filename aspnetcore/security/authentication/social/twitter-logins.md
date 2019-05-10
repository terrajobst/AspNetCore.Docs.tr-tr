---
title: ASP.NET Core ile twitter dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Twitter hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 5/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 486d58b600ca5326a0728de40bb386fbb9440f67
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65516878"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="0538c-103">ASP.NET Core ile twitter dış oturum açma Kurulumu</span><span class="sxs-lookup"><span data-stu-id="0538c-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="0538c-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0538c-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0538c-105">Bu örnek, kullanıcıların etkinleştirmek gösterilmektedir [kendi Twitter hesabıyla oturum](https://dev.twitter.com/web/sign-in/desktop-browser) kullanarak örnek bir ASP.NET Core 2.2 projesi oluşturduğunuza [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="0538c-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="0538c-106">İçinde bir Twitter uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="0538c-106">Create the app in Twitter</span></span>

* <span data-ttu-id="0538c-107">Gidin [ https://apps.twitter.com/ ](https://apps.twitter.com/) ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="0538c-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="0538c-108">Twitter hesabıyla yoksa kullanın **[şimdi kaydolun](https://twitter.com/signup)** bağlantı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="0538c-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="0538c-109">Dokunun **yeni uygulama oluştur** ve uygulamanın ölçeğini doldurun **adı**, **açıklama** ve genel **Web sitesi** (Bu olabilir geçici dek URI'si etki alanı adı kayıt):</span><span class="sxs-lookup"><span data-stu-id="0538c-109">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="0538c-110">Geliştirme sürecinizi URI girin ile `/signin-twitter` içine eklenen **geçerli OAuth yeniden yönlendirme URI'leri** alan (örneğin: `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="0538c-110">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="0538c-111">Bu örnekte, yapılandırılmış Twitter kimlik doğrulaması düzeni istekleri otomatik olarak işleyecek `/signin-twitter` OAuth akışını uygulamak için rota.</span><span class="sxs-lookup"><span data-stu-id="0538c-111">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="0538c-112">URI segmenti `/signin-twitter` Twitter kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="0538c-112">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="0538c-113">Twitter kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) sınıf.</span><span class="sxs-lookup"><span data-stu-id="0538c-113">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="0538c-114">Kalan formu doldurun ve dokunun **kendi Twitter uygulamanızı oluşturun**.</span><span class="sxs-lookup"><span data-stu-id="0538c-114">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="0538c-115">Yeni uygulama ayrıntıları görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="0538c-115">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="0538c-116">Twitter tüketici API anahtarınızı ve gizli dizi depolama</span><span class="sxs-lookup"><span data-stu-id="0538c-116">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="0538c-117">Güvenli bir şekilde depolamak için aşağıdaki komutları çalıştırın `ClientId` ve `ClientSecret` kullanarak [gizli dizi Yöneticisi](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="0538c-117">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerAPISecret <Secret>
```

<span data-ttu-id="0538c-118">Bağlantı Twitter gibi hassas ayarları `Consumer Key` ve `Consumer Secret` yapılandırma kullanarak uygulama [gizli dizi Yöneticisi](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="0538c-118">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="0538c-119">Bu örnek amacı doğrultusunda, belirteçleri ad `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="0538c-119">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="0538c-120">Bu belirteçler bulunabilir **anahtarlar ve erişim belirteçleri** sekmesinde yeni bir Twitter uygulaması oluşturduktan sonra:</span><span class="sxs-lookup"><span data-stu-id="0538c-120">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="0538c-121">Twitter kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0538c-121">Configure Twitter Authentication</span></span>

<span data-ttu-id="0538c-122">Twitter hizmetinde ekleme `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="0538c-122">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="0538c-123">Bkz: [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) Twitter kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu.</span><span class="sxs-lookup"><span data-stu-id="0538c-123">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="0538c-124">Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0538c-124">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="0538c-125">Oturum Twitter ile oturum aç</span><span class="sxs-lookup"><span data-stu-id="0538c-125">Sign in with Twitter</span></span>

<span data-ttu-id="0538c-126">Uygulamayı çalıştırmak ve seçmek **oturum**.</span><span class="sxs-lookup"><span data-stu-id="0538c-126">Run the app and select **Log in**.</span></span> <span data-ttu-id="0538c-127">Twitter ile oturum açmak için bir seçenek görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="0538c-127">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="0538c-128">Tıklayarak **Twitter** kimlik doğrulaması için Twitter'a yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="0538c-128">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="0538c-129">Twitter kimlik bilgilerinizi girdikten sonra web, e-posta ayarlayabileceğiniz siteye geri yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="0538c-129">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="0538c-130">Artık Twitter kimlik bilgilerinizi kullanarak günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="0538c-130">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="0538c-131">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="0538c-131">Troubleshooting</span></span>

* <span data-ttu-id="0538c-132">**ASP.NET Core 2.x yalnızca:** Kimlik çağırarak yapılandırılmazsa `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması yapmaya sonuçlanır *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*.</span><span class="sxs-lookup"><span data-stu-id="0538c-132">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="0538c-133">Bu örnekte kullanılan proje şablonu, bu gerçekleştirilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="0538c-133">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="0538c-134">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="0538c-134">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="0538c-135">Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="0538c-135">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0538c-136">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="0538c-136">Next steps</span></span>

* <span data-ttu-id="0538c-137">Bu makalede, Twitter ile kimliğini nasıl doğrulayabileceğiniz gösterdi.</span><span class="sxs-lookup"><span data-stu-id="0538c-137">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="0538c-138">Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="0538c-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="0538c-139">Web sitenizi Azure web uygulaması yayımladıktan sonra sıfırlamalısınız `ConsumerSecret` Twitter Geliştirici Portalı.</span><span class="sxs-lookup"><span data-stu-id="0538c-139">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="0538c-140">Ayarlama `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret` Azure portalında uygulama ayarları olarak.</span><span class="sxs-lookup"><span data-stu-id="0538c-140">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="0538c-141">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="0538c-141">The configuration system is set up to read keys from environment variables.</span></span>