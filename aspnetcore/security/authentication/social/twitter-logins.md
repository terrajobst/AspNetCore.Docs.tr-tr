---
title: ASP.NET Core ile Twitter dışarıdan oturum açma kurulumu
author: rick-anderson
description: Bu öğreticide, Twitter hesabı kullanıcı kimlik doğrulamasının mevcut bir ASP.NET Core uygulamasına tümleştirilmesi gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: b848486415fd72ce6180b4cf8fc1ba00410d694a
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989748"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="479b8-103">ASP.NET Core ile Twitter dışarıdan oturum açma kurulumu</span><span class="sxs-lookup"><span data-stu-id="479b8-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="479b8-104">Tarafından [Valeriy Novyıtskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="479b8-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="479b8-105">Bu örnek, [önceki sayfada](xref:security/authentication/social/index)oluşturulmuş bir örnek ASP.NET Core 3,0 projesi kullanarak kullanıcıların [Twitter hesabıyla oturum açmasını](https://dev.twitter.com/web/sign-in/desktop-browser) nasıl olanaklı hale kullanabileceğinizi gösterir.</span><span class="sxs-lookup"><span data-stu-id="479b8-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="479b8-106">Uygulamayı Twitter 'da oluşturma</span><span class="sxs-lookup"><span data-stu-id="479b8-106">Create the app in Twitter</span></span>

* <span data-ttu-id="479b8-107">Projeye [Microsoft. AspNetCore. Authentication. Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="479b8-107">Add the [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet package to the project.</span></span>

* <span data-ttu-id="479b8-108">[https://apps.twitter.com/](https://apps.twitter.com/) gidin ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="479b8-108">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="479b8-109">Zaten bir Twitter hesabınız yoksa, oluşturmak için **[Şimdi kaydolun](https://twitter.com/signup)** bağlantısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="479b8-109">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="479b8-110">**Uygulama oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="479b8-110">Select **Create an app**.</span></span> <span data-ttu-id="479b8-111">**Uygulama adı**, **uygulama açıklaması** ve genel **Web sitesi** URI 'sini doldurun (etki alanı adı kaydoluncaya kadar geçici olabilir):</span><span class="sxs-lookup"><span data-stu-id="479b8-111">Fill out the **App name**, **Application description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="479b8-112">**Twitter Ile oturum açmayı etkinleştir** seçeneğinin yanındaki kutuyu işaretleyin</span><span class="sxs-lookup"><span data-stu-id="479b8-112">Check the box next to **Enable Sign in with Twitter**</span></span>

* <span data-ttu-id="479b8-113">Microsoft. AspNetCore. Identity, kullanıcıların varsayılan olarak bir e-posta adresine sahip olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="479b8-113">Microsoft.AspNetCore.Identity requires users to have an email address by default.</span></span> <span data-ttu-id="479b8-114">**İzinler** sekmesine gidin, **Düzenle** düğmesine tıklayın ve **kullanıcılardan e-posta adresi iste**' nin yanındaki kutuyu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="479b8-114">Go to the **Permissions** tab, click the **Edit** button and check the box next to **Request email address from users**.</span></span>

* <span data-ttu-id="479b8-115">**Geri arama URL 'leri** alanına eklenen `/signin-twitter` geliştirme URI 'nizi girin (örneğin: `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="479b8-115">Enter your development URI with `/signin-twitter` appended into the **Callback URLs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="479b8-116">Bu örnekte daha sonra yapılandırılan Twitter kimlik doğrulaması şeması, OAuth akışını uygulamak için `/signin-twitter` rotadaki istekleri otomatik olarak işleymeyecektir.</span><span class="sxs-lookup"><span data-stu-id="479b8-116">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="479b8-117">`/signin-twitter` URI segmenti Twitter kimlik doğrulama sağlayıcısının varsayılan geri çağırması olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="479b8-117">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="479b8-118">Twitter kimlik doğrulama ara [yazılımını, '](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) ın devralınan [remoteauthenticationoptions. callbackpath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliğini kullanarak YAPıLANDıRıRKEN varsayılan geri çağırma URI 'sini değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="479b8-118">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="479b8-119">Formun geri kalanını doldurun ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="479b8-119">Fill out the rest of the form and select **Create**.</span></span> <span data-ttu-id="479b8-120">Yeni uygulama ayrıntıları görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="479b8-120">New application details are displayed:</span></span>

## <a name="store-the-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="479b8-121">Twitter tüketici API anahtarını ve gizli dizisini depolayın</span><span class="sxs-lookup"><span data-stu-id="479b8-121">Store the Twitter consumer API key and secret</span></span>

<span data-ttu-id="479b8-122">Twitter tüketicisi API anahtarı ve gizli dizi [Yöneticisi](xref:security/app-secrets)ile gizli dizi gibi hassas ayarları depolayın.</span><span class="sxs-lookup"><span data-stu-id="479b8-122">Store sensitive settings such as the Twitter consumer API key and secret with [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="479b8-123">Bu örnek için aşağıdaki adımları kullanın:</span><span class="sxs-lookup"><span data-stu-id="479b8-123">For this sample, use the following steps:</span></span>

1. <span data-ttu-id="479b8-124">[Gizli depolamayı etkinleştirme](xref:security/app-secrets#enable-secret-storage)konusundaki yönergeler temelinde projeyi gizli depolama için başlatın.</span><span class="sxs-lookup"><span data-stu-id="479b8-124">Initialize the project for secret storage per the instructions at [Enable secret storage](xref:security/app-secrets#enable-secret-storage).</span></span>
1. <span data-ttu-id="479b8-125">Gizli ayarları gizli anahtar deposunda depolar `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret`:</span><span class="sxs-lookup"><span data-stu-id="479b8-125">Store the sensitive settings in the local secret store with the secrets keys `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`:</span></span>

    ```dotnetcli
    dotnet user-secrets set "Authentication:Twitter:ConsumerAPIKey" "<consumer-api-key>"
    dotnet user-secrets set "Authentication:Twitter:ConsumerSecret" "<consumer-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="479b8-126">Bu belirteçler, yeni bir Twitter uygulaması oluşturduktan sonra **anahtarlar ve erişim belirteçleri** sekmesinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="479b8-126">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="479b8-127">Twitter kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="479b8-127">Configure Twitter Authentication</span></span>

<span data-ttu-id="479b8-128">Twitter hizmetini *Startup.cs* dosyasındaki `ConfigureServices` yöntemine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="479b8-128">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-15)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="479b8-129">Twitter kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için, bkz. [dallı ve seçenekleri](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API başvurusu.</span><span class="sxs-lookup"><span data-stu-id="479b8-129">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="479b8-130">Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="479b8-130">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="479b8-131">Twitter ile oturum açma</span><span class="sxs-lookup"><span data-stu-id="479b8-131">Sign in with Twitter</span></span>

<span data-ttu-id="479b8-132">Uygulamayı çalıştırın ve **oturum aç '** ı seçin.</span><span class="sxs-lookup"><span data-stu-id="479b8-132">Run the app and select **Log in**.</span></span> <span data-ttu-id="479b8-133">Twitter ile oturum açma seçeneği görünür:</span><span class="sxs-lookup"><span data-stu-id="479b8-133">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="479b8-134">**Twitter** 'ya tıkladığınızda, kimlik doğrulaması için Twitter 'a yeniden yönlendirme yapılır:</span><span class="sxs-lookup"><span data-stu-id="479b8-134">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="479b8-135">Twitter kimlik bilgilerinizi girdikten sonra, e-postanızı ayarlayabileceğiniz Web sitesine geri yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="479b8-135">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="479b8-136">Twitter kimlik bilgilerinizi kullanarak oturumunuz açıldı:</span><span class="sxs-lookup"><span data-stu-id="479b8-136">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="479b8-137">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="479b8-137">Troubleshooting</span></span>

* <span data-ttu-id="479b8-138">**Yalnızca 2. x ASP.NET Core:** Kimlik, `ConfigureServices``services.AddIdentity` çağırarak yapılandırılmamışsa, kimlik doğrulamaya çalışmak ArgumentException ile sonuçlanır *: ' Signınscheme ' seçeneği sağlanmalıdır*.</span><span class="sxs-lookup"><span data-stu-id="479b8-138">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="479b8-139">Bu örnekte kullanılan proje şablonu bunun yapılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="479b8-139">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="479b8-140">Site veritabanı ilk geçiş uygulanarak oluşturulmadıysa, *istek hatasını Işlerken bir veritabanı işlemi başarısız* olur.</span><span class="sxs-lookup"><span data-stu-id="479b8-140">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="479b8-141">Veritabanını oluşturmak için **geçişleri Uygula** ' ya dokunun ve hatanın ötesinde devam etmek için yenileyin.</span><span class="sxs-lookup"><span data-stu-id="479b8-141">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="479b8-142">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="479b8-142">Next steps</span></span>

* <span data-ttu-id="479b8-143">Bu makalede Twitter ile kimlik doğrulaması yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="479b8-143">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="479b8-144">[Önceki sayfada](xref:security/authentication/social/index)listelenen diğer sağlayıcılarla kimlik doğrulaması yapmak için benzer bir yaklaşımı izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="479b8-144">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="479b8-145">Web sitenizi Azure Web App 'e yayımladığınızda, Twitter geliştirici portalındaki `ConsumerSecret` sıfırlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="479b8-145">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="479b8-146">`Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret` Azure portal uygulama ayarları olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="479b8-146">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="479b8-147">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="479b8-147">The configuration system is set up to read keys from environment variables.</span></span>
