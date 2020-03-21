---
title: ASP.NET core'da Facebook dış oturum açma Kurulumu
author: rick-anderson
description: Facebook hesabı kullanıcı kimlik doğrulamasının mevcut bir ASP.NET Core uygulamasına tümleştirilmesini gösteren kod örnekleri ile öğretici.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: bb26a27f026e744c7d4925aa2281bf0625fff8a2
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989789"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="686ff-103">ASP.NET core'da Facebook dış oturum açma Kurulumu</span><span class="sxs-lookup"><span data-stu-id="686ff-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="686ff-104">Tarafından [Valeriy Novyıtskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="686ff-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="686ff-105">Kod örnekleri ile bu öğreticide, kullanıcılarınızın [önceki sayfada](xref:security/authentication/social/index)oluşturulmuş bir örnek ASP.NET Core 3,0 projesi kullanarak Facebook hesabıyla oturum açmasını nasıl etkinleştireceğinizi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="686ff-105">This tutorial with code examples shows how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="686ff-106">[Resmi adımları](https://developers.facebook.com)Izleyerek Facebook uygulama kimliği oluşturarak başladık.</span><span class="sxs-lookup"><span data-stu-id="686ff-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="686ff-107">İçinde Facebook uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="686ff-107">Create the app in Facebook</span></span>

* <span data-ttu-id="686ff-108">Projeye [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="686ff-108">Add the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package to the project.</span></span>

* <span data-ttu-id="686ff-109">[Facebook geliştiricileri uygulama](https://developers.facebook.com/apps/) sayfasına gidin ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="686ff-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="686ff-110">Henüz bir Facebook hesabınız yoksa, bir tane oluşturmak için oturum açma sayfasındaki **Facebook 'A kaydolun** bağlantısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="686ff-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>  <span data-ttu-id="686ff-111">Facebook hesabınız olduğunda, Facebook geliştiricisi olarak kaydolmak için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="686ff-111">Once you have a Facebook account, follow the instructions to register as a Facebook Developer.</span></span>

* <span data-ttu-id="686ff-112">Yeni bir uygulama KIMLIĞI oluşturmak için **uygulamalarım** menüsünde **uygulama oluştur** ' u seçin.</span><span class="sxs-lookup"><span data-stu-id="686ff-112">From the **My Apps** menu select **Create App** to create a new App ID.</span></span>

   ![Microsoft Edge'de Facebook geliştiriciler portalı açın](index/_static/FBMyApps.png)

* <span data-ttu-id="686ff-114">Formu doldurun ve **uygulama kimliği oluştur** düğmesine dokunun.</span><span class="sxs-lookup"><span data-stu-id="686ff-114">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Yeni uygulama Kimliğini formu oluşturma](index/_static/FBNewAppId.png)

* <span data-ttu-id="686ff-116">Yeni uygulama kartında **Ürün Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="686ff-116">On the new App card, select **Add a Product**.</span></span>  <span data-ttu-id="686ff-117">**Facebook oturum açma** kartında **Ayarla** ' ya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="686ff-117">On the **Facebook Login** card, click **Set Up**</span></span> 

  ![Ürün kurulum sayfası](index/_static/FBProductSetup.png)

* <span data-ttu-id="686ff-119">**Hızlı başlangıç** Sihirbazı, ilk sayfa olarak **bir platform Seç** ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="686ff-119">The **Quickstart** wizard launches with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="686ff-120">Sol alt taraftaki menüdeki **Facebook oturum açma** **ayarları** bağlantısına tıklayarak Sihirbazı Şimdi atlayın:</span><span class="sxs-lookup"><span data-stu-id="686ff-120">Bypass the wizard for now by clicking the **FaceBook Login** **Settings** link in the menu on the lower left:</span></span>

  ![Ziyaretimde hızlı başlangıç](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="686ff-122">**Istemci OAuth ayarları** sayfası görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="686ff-122">You are presented with the **Client OAuth Settings** page:</span></span>

  ![İstemci OAuth Ayarları sayfası](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="686ff-124">**Geçerli OAuth yeniden yönlendirme URI 'leri** alanına */SignIn-Facebook* eklenmiş olan geliştirme URI 'nizi girin (örneğin: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="686ff-124">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="686ff-125">Bu öğreticide daha sonra yapılandırılan Facebook kimlik doğrulaması, OAuth akışını uygulamak için */SignIn-Facebook* rotasındaki istekleri otomatik olarak işleymeyecektir.</span><span class="sxs-lookup"><span data-stu-id="686ff-125">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="686ff-126">URI */SignIn-Facebook* , Facebook kimlik doğrulama sağlayıcısı 'nın varsayılan geri çağırması olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="686ff-126">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="686ff-127">Facebook kimlik doğrulama ara yazılımını, çok [yönlü önyükleme](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) sınıfının devralınan [remoteauthenticationoptions. callbackpath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği aracılığıyla YAPıLANDıRıRKEN varsayılan geri çağırma URI 'sini değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="686ff-127">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="686ff-128">**Değişiklikleri Kaydet**' e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="686ff-128">Click **Save Changes**.</span></span>

* <span data-ttu-id="686ff-129">Sol gezinti bölmesinde **ayarlar** > **temel** bağlantı ' ya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="686ff-129">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="686ff-130">Bu sayfada, `App ID` ve `App Secret`bir kopyasını alın.</span><span class="sxs-lookup"><span data-stu-id="686ff-130">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="686ff-131">Sonraki bölümde, ASP.NET Core uygulamasına hem de ekleyeceksiniz:</span><span class="sxs-lookup"><span data-stu-id="686ff-131">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="686ff-132">Siteyi dağıttığınızda **Facebook oturum açma** kurulumu sayfasını yeniden ziyaret etmeniz ve yeni BIR ortak URI kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="686ff-132">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-the-facebook-app-id-and-secret"></a><span data-ttu-id="686ff-133">Facebook uygulama KIMLIĞI ve gizli anahtarını depolayın</span><span class="sxs-lookup"><span data-stu-id="686ff-133">Store the Facebook app ID and secret</span></span>

<span data-ttu-id="686ff-134">Facebook uygulama KIMLIĞI ve gizli anahtar değerleri gibi hassas ayarları [gizli bir yöneticiye](xref:security/app-secrets)depolayın.</span><span class="sxs-lookup"><span data-stu-id="686ff-134">Store sensitive settings such as the Facebook app ID and secret values with [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="686ff-135">Bu örnek için aşağıdaki adımları kullanın:</span><span class="sxs-lookup"><span data-stu-id="686ff-135">For this sample, use the following steps:</span></span>

1. <span data-ttu-id="686ff-136">[Gizli depolamayı etkinleştirme](xref:security/app-secrets#enable-secret-storage)konusundaki yönergeler temelinde projeyi gizli depolama için başlatın.</span><span class="sxs-lookup"><span data-stu-id="686ff-136">Initialize the project for secret storage per the instructions at [Enable secret storage](xref:security/app-secrets#enable-secret-storage).</span></span>
1. <span data-ttu-id="686ff-137">Hassas ayarları, gizli anahtarlar `Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret`ile yerel gizli depolama alanına depolayın:</span><span class="sxs-lookup"><span data-stu-id="686ff-137">Store the sensitive settings in the local secret store with the secret keys `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`:</span></span>

    ```dotnetcli
    dotnet user-secrets set "Authentication:Facebook:AppId" "<app-id>"
    dotnet user-secrets set "Authentication:Facebook:AppSecret" "<app-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-facebook-authentication"></a><span data-ttu-id="686ff-138">Facebook kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="686ff-138">Configure Facebook Authentication</span></span>

<span data-ttu-id="686ff-139">Facebook hizmetini *Startup.cs* dosyasındaki `ConfigureServices` yöntemine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="686ff-139">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="686ff-140">Facebook kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. çok [yönlü Bookoptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API başvurusu.</span><span class="sxs-lookup"><span data-stu-id="686ff-140">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="686ff-141">İçin yapılandırma seçenekleri kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="686ff-141">Configuration options can be used to:</span></span>

* <span data-ttu-id="686ff-142">Kullanıcı ile ilgili farklı bilgi isteyin.</span><span class="sxs-lookup"><span data-stu-id="686ff-142">Request different information about the user.</span></span>
* <span data-ttu-id="686ff-143">Oturum açma deneyimi özelleştirmek için sorgu dizesi bağımsız değişkenleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="686ff-143">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="686ff-144">Facebook ile oturum açma</span><span class="sxs-lookup"><span data-stu-id="686ff-144">Sign in with Facebook</span></span>

<span data-ttu-id="686ff-145">Uygulamanızı çalıştırın ve **oturum aç**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="686ff-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="686ff-146">Facebook ile oturum açmak için bir seçenek görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="686ff-146">You see an option to sign in with Facebook.</span></span>

![Web uygulaması: kullanıcı kimliği](index/_static/DoneFacebook.png)

<span data-ttu-id="686ff-148">**Facebook**'a tıkladığınızda, kimlik doğrulama için Facebook 'a yönlendirilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="686ff-148">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Facebook kimlik doğrulaması sayfası](index/_static/FBLogin.png)

<span data-ttu-id="686ff-150">Facebook kimlik doğrulaması varsayılan olarak genel profil ve e-posta adresi ister:</span><span class="sxs-lookup"><span data-stu-id="686ff-150">Facebook authentication requests public profile and email address by default:</span></span>

![Facebook kimlik doğrulaması sayfası onay ekranı](index/_static/FBLoginDone.png)

<span data-ttu-id="686ff-152">Facebook kimlik bilgilerinizi girdikten sonra e-postanızı ayarlayabildiğiniz sitenize geri yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="686ff-152">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="686ff-153">Şimdi, Facebook kimlik bilgilerinizi kullanarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="686ff-153">You are now logged in using your Facebook credentials:</span></span>

![Web uygulaması: kimliği doğrulanmış kullanıcı](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="686ff-155">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="686ff-155">Troubleshooting</span></span>

* <span data-ttu-id="686ff-156">**Yalnızca 2. x ASP.NET Core:** Kimlik, `ConfigureServices``services.AddIdentity` çağırarak yapılandırılmamışsa, kimlik doğrulamaya çalışmak ArgumentException ile sonuçlanır *: ' Signınscheme ' seçeneği sağlanmalıdır*.</span><span class="sxs-lookup"><span data-stu-id="686ff-156">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="686ff-157">Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="686ff-157">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="686ff-158">Site veritabanı ilk geçiş uygulanarak oluşturulmadıysa, *istek hatasını Işlerken bir veritabanı işlemi başarısız oldu* .</span><span class="sxs-lookup"><span data-stu-id="686ff-158">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="686ff-159">Veritabanını oluşturmak için **geçişleri Uygula** ' ya dokunun ve hatanın ötesinde devam etmek için yenileyin.</span><span class="sxs-lookup"><span data-stu-id="686ff-159">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="686ff-160">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="686ff-160">Next steps</span></span>

* <span data-ttu-id="686ff-161">Bu makalede, Facebook ile kimliğini nasıl doğrulayabileceğiniz gösterdi.</span><span class="sxs-lookup"><span data-stu-id="686ff-161">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="686ff-162">[Önceki sayfada](xref:security/authentication/social/index)listelenen diğer sağlayıcılarla kimlik doğrulaması yapmak için benzer bir yaklaşımı izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="686ff-162">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="686ff-163">Web sitenizi Azure Web App 'e yayımladığınızda, Facebook Geliştirici portalındaki `AppSecret` sıfırlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="686ff-163">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="686ff-164">`Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret` Azure portal uygulama ayarları olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="686ff-164">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="686ff-165">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="686ff-165">The configuration system is set up to read keys from environment variables.</span></span>
