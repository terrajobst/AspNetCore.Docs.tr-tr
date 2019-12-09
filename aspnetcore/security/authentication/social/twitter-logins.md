---
title: ASP.NET Core ile Twitter dışarıdan oturum açma kurulumu
author: rick-anderson
description: Bu öğreticide, Twitter hesabı kullanıcı kimlik doğrulamasının mevcut bir ASP.NET Core uygulamasına tümleştirilmesi gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: 5d0695160d90d0c5d31b8e35bc6c4cc984829333
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944219"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="33e9e-103">ASP.NET Core ile Twitter dışarıdan oturum açma kurulumu</span><span class="sxs-lookup"><span data-stu-id="33e9e-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="33e9e-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="33e9e-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="33e9e-105">Bu örnek, [önceki sayfada](xref:security/authentication/social/index)oluşturulmuş bir örnek ASP.NET Core 3,0 projesi kullanarak kullanıcıların [Twitter hesabıyla oturum açmasını](https://dev.twitter.com/web/sign-in/desktop-browser) nasıl olanaklı hale kullanabileceğinizi gösterir.</span><span class="sxs-lookup"><span data-stu-id="33e9e-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="33e9e-106">Uygulamayı Twitter 'da oluşturma</span><span class="sxs-lookup"><span data-stu-id="33e9e-106">Create the app in Twitter</span></span>

* <span data-ttu-id="33e9e-107">Projeye [Microsoft. AspNetCore. Authentication. Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="33e9e-107">Add the [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet package to the project.</span></span>

* <span data-ttu-id="33e9e-108">Gidin [ https://apps.twitter.com/ ](https://apps.twitter.com/) ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="33e9e-108">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="33e9e-109">Zaten bir Twitter hesabınız yoksa, oluşturmak için **[Şimdi kaydolun](https://twitter.com/signup)** bağlantısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="33e9e-109">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="33e9e-110">**Uygulama oluştur**'u seçin.</span><span class="sxs-lookup"><span data-stu-id="33e9e-110">Select **Create an app**.</span></span> <span data-ttu-id="33e9e-111">**Uygulama adı**, **uygulama açıklaması** ve genel **Web sitesi** URI 'sini doldurun (etki alanı adı kaydoluncaya kadar geçici olabilir):</span><span class="sxs-lookup"><span data-stu-id="33e9e-111">Fill out the **App name**, **Application description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="33e9e-112">**Geri arama URL 'leri** alanına eklenen `/signin-twitter` geliştirme URI 'nizi girin (örneğin: `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="33e9e-112">Enter your development URI with `/signin-twitter` appended into the **Callback URLs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="33e9e-113">Bu örnekte daha sonra yapılandırılan Twitter kimlik doğrulaması şeması, OAuth akışını uygulamak için `/signin-twitter` rotadaki istekleri otomatik olarak işleymeyecektir.</span><span class="sxs-lookup"><span data-stu-id="33e9e-113">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="33e9e-114">`/signin-twitter` URI segmenti Twitter kimlik doğrulama sağlayıcısının varsayılan geri çağırması olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="33e9e-114">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="33e9e-115">Twitter kimlik doğrulama ara [yazılımını, '](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) ın devralınan [remoteauthenticationoptions. callbackpath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliğini kullanarak YAPıLANDıRıRKEN varsayılan geri çağırma URI 'sini değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33e9e-115">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="33e9e-116">Formun geri kalanını doldurun ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="33e9e-116">Fill out the rest of the form and select **Create**.</span></span> <span data-ttu-id="33e9e-117">Yeni uygulama ayrıntıları görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="33e9e-117">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="33e9e-118">Twitter tüketici API anahtarı ve gizli dizisi depolanıyor</span><span class="sxs-lookup"><span data-stu-id="33e9e-118">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="33e9e-119">`ClientId` ve `ClientSecret` [gizli Yöneticisi](xref:security/app-secrets)kullanarak güvenli bir şekilde depolamak için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="33e9e-119">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

<span data-ttu-id="33e9e-120">Gizli ayarları, Twitter `Consumer Key` ve `Consumer Secret` [gizli Yöneticisi](xref:security/app-secrets)kullanarak uygulama yapılandırmanıza bağlayın.</span><span class="sxs-lookup"><span data-stu-id="33e9e-120">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="33e9e-121">Bu örneğin amaçları doğrultusunda belirteçleri `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret`olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="33e9e-121">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="33e9e-122">Bu belirteçler, yeni bir Twitter uygulaması oluşturduktan sonra **anahtarlar ve erişim belirteçleri** sekmesinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="33e9e-122">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="33e9e-123">Twitter kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="33e9e-123">Configure Twitter Authentication</span></span>

<span data-ttu-id="33e9e-124">Twitter hizmetini *Startup.cs* dosyasındaki `ConfigureServices` yöntemine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="33e9e-124">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="33e9e-125">Twitter kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için, bkz. [dallı ve seçenekleri](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API başvurusu.</span><span class="sxs-lookup"><span data-stu-id="33e9e-125">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="33e9e-126">Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="33e9e-126">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="33e9e-127">Twitter ile oturum açma</span><span class="sxs-lookup"><span data-stu-id="33e9e-127">Sign in with Twitter</span></span>

<span data-ttu-id="33e9e-128">Uygulamayı çalıştırın ve **oturum aç '** ı seçin.</span><span class="sxs-lookup"><span data-stu-id="33e9e-128">Run the app and select **Log in**.</span></span> <span data-ttu-id="33e9e-129">Twitter ile oturum açma seçeneği görünür:</span><span class="sxs-lookup"><span data-stu-id="33e9e-129">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="33e9e-130">**Twitter** 'ya tıkladığınızda, kimlik doğrulaması için Twitter 'a yeniden yönlendirme yapılır:</span><span class="sxs-lookup"><span data-stu-id="33e9e-130">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="33e9e-131">Twitter kimlik bilgilerinizi girdikten sonra, e-postanızı ayarlayabileceğiniz Web sitesine geri yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33e9e-131">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="33e9e-132">Twitter kimlik bilgilerinizi kullanarak oturumunuz açıldı:</span><span class="sxs-lookup"><span data-stu-id="33e9e-132">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="33e9e-133">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="33e9e-133">Troubleshooting</span></span>

* <span data-ttu-id="33e9e-134">**ASP.NET Core 2.x yalnızca:** , kimlik, çağırarak yapılandırılmamış `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması yapmaya sonuçlanır *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*.</span><span class="sxs-lookup"><span data-stu-id="33e9e-134">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="33e9e-135">Bu örnekte kullanılan proje şablonu bunun yapılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="33e9e-135">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="33e9e-136">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="33e9e-136">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="33e9e-137">Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="33e9e-137">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="33e9e-138">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="33e9e-138">Next steps</span></span>

* <span data-ttu-id="33e9e-139">Bu makalede Twitter ile kimlik doğrulaması yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33e9e-139">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="33e9e-140">Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="33e9e-140">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="33e9e-141">Web sitenizi Azure Web App 'e yayımladığınızda, Twitter geliştirici portalındaki `ConsumerSecret` sıfırlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="33e9e-141">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="33e9e-142">Ayarlama `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret` Azure portalında uygulama ayarları olarak.</span><span class="sxs-lookup"><span data-stu-id="33e9e-142">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="33e9e-143">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="33e9e-143">The configuration system is set up to read keys from environment variables.</span></span>
