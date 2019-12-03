---
title: ASP.NET Core 'da Facebook dış oturum açma kurulumu
author: rick-anderson
description: Facebook hesabı kullanıcı kimlik doğrulamasının mevcut bir ASP.NET Core uygulamasına tümleştirilmesini gösteren kod örnekleri ile öğretici.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 12/02/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: 2e4cc04c6e7ff8e5f5701cc7f9ede73dbc1b4685
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717152"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="bbd31-103">ASP.NET Core 'da Facebook dış oturum açma kurulumu</span><span class="sxs-lookup"><span data-stu-id="bbd31-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="bbd31-104">Tarafından [Valeriy Novyıtskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bbd31-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bbd31-105">Kod örnekleri ile bu öğreticide, kullanıcılarınızın [önceki sayfada](xref:security/authentication/social/index)oluşturulmuş bir örnek ASP.NET Core 3,0 projesi kullanarak Facebook hesabıyla oturum açmasını nasıl etkinleştireceğinizi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="bbd31-105">This tutorial with code examples shows how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="bbd31-106">[Resmi adımları](https://developers.facebook.com)Izleyerek Facebook uygulama kimliği oluşturarak başladık.</span><span class="sxs-lookup"><span data-stu-id="bbd31-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="bbd31-107">Facebook 'ta uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="bbd31-107">Create the app in Facebook</span></span>

* <span data-ttu-id="bbd31-108">Projeye [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bbd31-108">Add the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package to the project.</span></span>

* <span data-ttu-id="bbd31-109">[Facebook geliştiricileri uygulama](https://developers.facebook.com/apps/) sayfasına gidin ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="bbd31-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="bbd31-110">Henüz bir Facebook hesabınız yoksa, bir tane oluşturmak için oturum açma sayfasındaki **Facebook 'A kaydolun** bağlantısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="bbd31-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>  <span data-ttu-id="bbd31-111">Facebook hesabınız olduğunda, Facebook geliştiricisi olarak kaydolmak için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="bbd31-111">Once you have a Facebook account, follow the instructions to register as a Facebook Developer.</span></span>

* <span data-ttu-id="bbd31-112">Yeni bir uygulama KIMLIĞI oluşturmak için **uygulamalarım** menüsünde **uygulama oluştur** ' u seçin.</span><span class="sxs-lookup"><span data-stu-id="bbd31-112">From the **My Apps** menu select **Create App** to create a new App ID.</span></span>

   ![Microsoft Edge 'de açık geliştiriciler portalı için Facebook](index/_static/FBMyApps.png)

* <span data-ttu-id="bbd31-114">Formu doldurun ve **uygulama kimliği oluştur** düğmesine dokunun.</span><span class="sxs-lookup"><span data-stu-id="bbd31-114">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Yeni bir uygulama KIMLIĞI formu oluşturun](index/_static/FBNewAppId.png)

* <span data-ttu-id="bbd31-116">Yeni uygulama kartında **Ürün Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="bbd31-116">On the new App card, select **Add a Product**.</span></span>  <span data-ttu-id="bbd31-117">**Facebook oturum açma** kartında **Ayarla** ' ya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bbd31-117">On the **Facebook Login** card, click **Set Up**</span></span> 

  ![Ürün kurulum sayfası](index/_static/FBProductSetup.png)

* <span data-ttu-id="bbd31-119">**Hızlı başlangıç** Sihirbazı, ilk sayfa olarak **bir platform Seç** ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="bbd31-119">The **Quickstart** wizard launches with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="bbd31-120">Sol alt taraftaki menüdeki **Facebook oturum açma** **ayarları** bağlantısına tıklayarak Sihirbazı Şimdi atlayın:</span><span class="sxs-lookup"><span data-stu-id="bbd31-120">Bypass the wizard for now by clicking the **FaceBook Login** **Settings** link in the menu on the lower left:</span></span>

  ![hızlı başlangıç atla](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="bbd31-122">**Istemci OAuth ayarları** sayfası görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="bbd31-122">You are presented with the **Client OAuth Settings** page:</span></span>

  ![İstemci OAuth ayarları sayfası](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="bbd31-124">**Geçerli OAuth yeniden yönlendirme URI 'leri** alanına */SignIn-Facebook* eklenmiş olan geliştirme URI 'nizi girin (örneğin: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="bbd31-124">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="bbd31-125">Bu öğreticide daha sonra yapılandırılan Facebook kimlik doğrulaması, OAuth akışını uygulamak için */SignIn-Facebook* rotasındaki istekleri otomatik olarak işleymeyecektir.</span><span class="sxs-lookup"><span data-stu-id="bbd31-125">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="bbd31-126">URI */SignIn-Facebook* , Facebook kimlik doğrulama sağlayıcısı 'nın varsayılan geri çağırması olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="bbd31-126">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="bbd31-127">Facebook kimlik doğrulama ara yazılımını, çok [yönlü önyükleme](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) sınıfının devralınan [remoteauthenticationoptions. callbackpath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği aracılığıyla YAPıLANDıRıRKEN varsayılan geri çağırma URI 'sini değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bbd31-127">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="bbd31-128">**Değişiklikleri Kaydet**' e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bbd31-128">Click **Save Changes**.</span></span>

* <span data-ttu-id="bbd31-129">Sol gezinti bölmesinde **ayarlar** > **temel** bağlantı ' ya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bbd31-129">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="bbd31-130">Bu sayfada, `App ID` ve `App Secret`bir kopyasını alın.</span><span class="sxs-lookup"><span data-stu-id="bbd31-130">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="bbd31-131">Bir sonraki bölümde ASP.NET Core uygulamanıza her ikisini de ekleyeceksiniz:</span><span class="sxs-lookup"><span data-stu-id="bbd31-131">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="bbd31-132">Siteyi dağıttığınızda **Facebook oturum açma** kurulumu sayfasını yeniden ziyaret etmeniz ve yeni BIR ortak URI kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="bbd31-132">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="bbd31-133">Facebook uygulama KIMLIĞI ve uygulama gizli anahtarını depolayın</span><span class="sxs-lookup"><span data-stu-id="bbd31-133">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="bbd31-134">Gizli ayarları, Facebook `App ID` ve `App Secret` gibi hassas ayarları, uygulama yapılandırmanıza [gizli yönetici](xref:security/app-secrets)kullanarak bağlayın.</span><span class="sxs-lookup"><span data-stu-id="bbd31-134">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="bbd31-135">Bu öğreticinin amaçları doğrultusunda belirteçleri `Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret`adlandırın.</span><span class="sxs-lookup"><span data-stu-id="bbd31-135">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="bbd31-136">`App ID` ve `App Secret` gizli Yöneticisi kullanarak güvenli bir şekilde depolamak için aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="bbd31-136">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="bbd31-137">Facebook kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bbd31-137">Configure Facebook Authentication</span></span>

<span data-ttu-id="bbd31-138">Facebook hizmetini *Startup.cs* dosyasındaki `ConfigureServices` yöntemine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="bbd31-138">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="bbd31-139">Facebook kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. çok [yönlü Bookoptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API başvurusu.</span><span class="sxs-lookup"><span data-stu-id="bbd31-139">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="bbd31-140">Yapılandırma seçenekleri şu şekilde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="bbd31-140">Configuration options can be used to:</span></span>

* <span data-ttu-id="bbd31-141">Kullanıcı hakkında farklı bilgiler isteyin.</span><span class="sxs-lookup"><span data-stu-id="bbd31-141">Request different information about the user.</span></span>
* <span data-ttu-id="bbd31-142">Oturum açma deneyimini özelleştirmek için sorgu dizesi bağımsız değişkenleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bbd31-142">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="bbd31-143">Facebook ile oturum açma</span><span class="sxs-lookup"><span data-stu-id="bbd31-143">Sign in with Facebook</span></span>

<span data-ttu-id="bbd31-144">Uygulamanızı çalıştırın ve **oturum aç**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bbd31-144">Run your application and click **Log in**.</span></span> <span data-ttu-id="bbd31-145">Facebook ile oturum açma seçeneği görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="bbd31-145">You see an option to sign in with Facebook.</span></span>

![Web uygulaması: kimliği doğrulanmamış Kullanıcı](index/_static/DoneFacebook.png)

<span data-ttu-id="bbd31-147">**Facebook**'a tıkladığınızda, kimlik doğrulama için Facebook 'a yönlendirilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bbd31-147">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Facebook kimlik doğrulama sayfası](index/_static/FBLogin.png)

<span data-ttu-id="bbd31-149">Facebook kimlik doğrulaması, varsayılan olarak genel profil ve e-posta adresini ister:</span><span class="sxs-lookup"><span data-stu-id="bbd31-149">Facebook authentication requests public profile and email address by default:</span></span>

![Facebook kimlik doğrulama sayfası onay ekranı](index/_static/FBLoginDone.png)

<span data-ttu-id="bbd31-151">Facebook kimlik bilgilerinizi girdikten sonra, e-postanızı ayarlayabileceğiniz sitenize geri yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bbd31-151">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="bbd31-152">Artık Facebook kimlik bilgilerinizi kullanarak oturumunuz açıldı:</span><span class="sxs-lookup"><span data-stu-id="bbd31-152">You are now logged in using your Facebook credentials:</span></span>

![Web uygulaması: kullanıcının kimliği doğrulandı](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="bbd31-154">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="bbd31-154">Troubleshooting</span></span>

* <span data-ttu-id="bbd31-155">**Yalnızca 2. x ASP.NET Core:** Kimlik, `ConfigureServices``services.AddIdentity` çağırarak yapılandırılmamışsa, kimlik doğrulamaya çalışmak ArgumentException ile sonuçlanır *: ' Signınscheme ' seçeneği sağlanmalıdır*.</span><span class="sxs-lookup"><span data-stu-id="bbd31-155">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="bbd31-156">Bu öğreticide kullanılan proje şablonu bunun yapılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="bbd31-156">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="bbd31-157">Site veritabanı ilk geçiş uygulanarak oluşturulmadıysa, *istek hatasını Işlerken bir veritabanı işlemi başarısız oldu* .</span><span class="sxs-lookup"><span data-stu-id="bbd31-157">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="bbd31-158">Veritabanını oluşturmak için **geçişleri Uygula** ' ya dokunun ve hatanın ötesinde devam etmek için yenileyin.</span><span class="sxs-lookup"><span data-stu-id="bbd31-158">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bbd31-159">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="bbd31-159">Next steps</span></span>

* <span data-ttu-id="bbd31-160">Bu makalede Facebook ile kimlik doğrulaması yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bbd31-160">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="bbd31-161">[Önceki sayfada](xref:security/authentication/social/index)listelenen diğer sağlayıcılarla kimlik doğrulaması yapmak için benzer bir yaklaşımı izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bbd31-161">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="bbd31-162">Web sitenizi Azure Web App 'e yayımladığınızda, Facebook Geliştirici portalındaki `AppSecret` sıfırlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="bbd31-162">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="bbd31-163">`Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret` Azure portal uygulama ayarları olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="bbd31-163">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="bbd31-164">Yapılandırma sistemi, ortam değişkenlerinden anahtarları okumak üzere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="bbd31-164">The configuration system is set up to read keys from environment variables.</span></span>
