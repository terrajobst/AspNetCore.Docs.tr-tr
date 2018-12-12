---
title: ASP.NET core'da Google dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Google hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: 372504eb4f6fea412b5b160e0d5e9251dafe0d56
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284500"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="2c605-103">ASP.NET core'da Google dış oturum açma Kurulumu</span><span class="sxs-lookup"><span data-stu-id="2c605-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="2c605-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2c605-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2c605-105">Bu öğreticide, oluşturulan bir örnek ASP.NET Core 2.0 proje kullanarak kendi Google + hesabı ile oturum açmalarına işlemini göstermektedir [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="2c605-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="2c605-106">İzleyerek Başlangıç [resmi adımları](https://developers.google.com/identity/sign-in/web/devconsole-project) Google API konsolu içinde yeni bir uygulama oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="2c605-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="2c605-107">Google API konsolunda uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="2c605-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="2c605-108">Gidin [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="2c605-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="2c605-109">Bir Google hesabınız yoksa kullanırsanız **daha fazla seçenek** > **[hesabı oluşturma](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  bağlantı oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="2c605-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Google API Konsolu](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="2c605-111">Yönlendirilirsiniz **API Yöneticisi Kitaplığı** sayfası:</span><span class="sxs-lookup"><span data-stu-id="2c605-111">You are redirected to **API Manager Library** page:</span></span>

![API Yöneticisi kitaplık sayfasında giriş](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="2c605-113">Dokunun **Oluştur** girin, **proje adı**:</span><span class="sxs-lookup"><span data-stu-id="2c605-113">Tap **Create** and enter your **Project name**:</span></span>

![Yeni Proje iletişim kutusu](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="2c605-115">İletişim kutusunu kabul ettikten sonra yeni uygulamanız için Özellikler'i seçmenize olanak tanıyan kitaplığı sayfasına yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c605-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="2c605-116">Bulma **Google + API** listesi ve API özelliği eklemek için kendi bağlantısına tıklayın:</span><span class="sxs-lookup"><span data-stu-id="2c605-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

!["Google + API için" API Yöneticisi kitaplığı sayfada arama](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="2c605-118">Yeni eklenen API için sayfa görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2c605-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="2c605-119">Dokunun **etkinleştirme** Google + oturum özelliği uygulamanıza eklemek için:</span><span class="sxs-lookup"><span data-stu-id="2c605-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![API Yöneticisi Google + API sayfasındaki giriş](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="2c605-121">API'ı etkinleştirdikten sonra dokunun **kimlik bilgilerini oluştur** gizli dizileri yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="2c605-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![API Yöneticisi Google + API sayfasında kimlik bilgilerini düğme oluşturma](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="2c605-123">Şunu seçin:</span><span class="sxs-lookup"><span data-stu-id="2c605-123">Choose:</span></span>
  * <span data-ttu-id="2c605-124">**Google + API**</span><span class="sxs-lookup"><span data-stu-id="2c605-124">**Google+ API**</span></span>
  * <span data-ttu-id="2c605-125">**Web sunucusu (örn: node.js, Tomcat)**, ve</span><span class="sxs-lookup"><span data-stu-id="2c605-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
  * <span data-ttu-id="2c605-126">**Kullanıcı verilerini**:</span><span class="sxs-lookup"><span data-stu-id="2c605-126">**User data**:</span></span>

![API Yöneticisi kimlik bilgileri sayfası: Ne tür, kimlik bilgileri kullanıma Bul paneli gerekir](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="2c605-128">Dokunun **hangi kimlik bilgileri gerekiyor?** aldığı, uygulama yapılandırması, ikinci adım için **OAuth 2.0 istemci kimlik oluşturma**:</span><span class="sxs-lookup"><span data-stu-id="2c605-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![API Yöneticisi kimlik bilgileri sayfası: OAuth 2.0 istemci kimlik oluşturma](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="2c605-130">Tek bir özellik (biz girebilirsiniz aynı oturum açma), Google + proje oluşturuyoruz çünkü **adı** proje için kullandığımız bir OAuth 2.0 istemci kimliği.</span><span class="sxs-lookup"><span data-stu-id="2c605-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="2c605-131">Geliştirme sürecinizi URI girin ile `/signin-google` içine eklenen **yetkili yeniden yönlendirme URI'leri** alan (örneğin: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="2c605-131">Enter your development URI with `/signin-google` appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="2c605-132">Bu öğreticinin ilerleyen bölümlerinde yapılandırılmış Google kimlik doğrulama istekleri otomatik olarak işleyecek `/signin-google` OAuth akışını uygulamak için rota.</span><span class="sxs-lookup"><span data-stu-id="2c605-132">The Google authentication configured later in this tutorial will automatically handle requests at `/signin-google` route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="2c605-133">URI segmenti `/signin-google` Google kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2c605-133">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="2c605-134">Google kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2c605-134">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

* <span data-ttu-id="2c605-135">Eklemek için SEKME tuşuna basın **yetkili yeniden yönlendirme URI'leri** girişi.</span><span class="sxs-lookup"><span data-stu-id="2c605-135">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="2c605-136">Dokunun **istemci kodu oluşturma**, aldığı, üçüncü adım **OAuth 2.0 onay ekranı ayarlama**:</span><span class="sxs-lookup"><span data-stu-id="2c605-136">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![API Yöneticisi kimlik bilgileri sayfası: OAuth 2.0 onay ekranı ayarlama](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="2c605-138">Genel kullanıma girin **e-posta adresi** ve **ürün adı** uygulamanız için gösterilen Google + oturum açmak için kullanıcıya sorar.</span><span class="sxs-lookup"><span data-stu-id="2c605-138">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="2c605-139">Ek seçenekleri altında kullanılabilir **daha fazla özelleştirme seçenekleri**.</span><span class="sxs-lookup"><span data-stu-id="2c605-139">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="2c605-140">Dokunun **devam** son adım, devam etmek için **kimlik bilgilerini indirme**:</span><span class="sxs-lookup"><span data-stu-id="2c605-140">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![API Yöneticisi kimlik bilgileri sayfası: Kimlik bilgilerini indirin.](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="2c605-142">Dokunun **indirme** uygulama gizli öğeleri bir JSON dosyası kaydetmek için ve **Bitti** yeni uygulama oluşturmayı tamamlamak için.</span><span class="sxs-lookup"><span data-stu-id="2c605-142">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="2c605-143">Site dağıtırken yeniden ziyaret etmeniz gerekir **Google konsolunu** ve yeni bir genel url kaydedin.</span><span class="sxs-lookup"><span data-stu-id="2c605-143">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="2c605-144">Store Google ClientID ve ClientSecret</span><span class="sxs-lookup"><span data-stu-id="2c605-144">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="2c605-145">Bağlantı Google gibi hassas ayarları `Client ID` ve `Client Secret` yapılandırma kullanarak uygulama [gizli dizi Yöneticisi](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="2c605-145">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="2c605-146">Bu öğreticinin amaçları doğrultusunda, belirteçleri ad `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="2c605-146">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="2c605-147">Bu belirteçler değerlerini altında önceki adımda yüklenen JSON dosyasında bulunabilir `web.client_id` ve `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="2c605-147">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="2c605-148">Google kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2c605-148">Configure Google Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2c605-149">Google hizmet ekleme `ConfigureServices` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="2c605-149">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2c605-150">Bu öğreticide kullanılan proje şablonu sağlar [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) paket yüklenir.</span><span class="sxs-lookup"><span data-stu-id="2c605-150">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

* <span data-ttu-id="2c605-151">Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="2c605-151">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="2c605-152">.NET Core CLI ile yüklemek için proje dizininizde aşağıdakileri yürütün:</span><span class="sxs-lookup"><span data-stu-id="2c605-152">To install with .NET Core CLI, execute the following in your project directory:</span></span>

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="2c605-153">Google Ara yazılımında ekleme `Configure` yönteminde *Startup.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="2c605-153">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

<span data-ttu-id="2c605-154">Bkz: [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) Google kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu.</span><span class="sxs-lookup"><span data-stu-id="2c605-154">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="2c605-155">Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2c605-155">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="2c605-156">Google ile oturum aç</span><span class="sxs-lookup"><span data-stu-id="2c605-156">Sign in with Google</span></span>

<span data-ttu-id="2c605-157">Uygulamanızı çalıştırın ve tıklayın **oturum**.</span><span class="sxs-lookup"><span data-stu-id="2c605-157">Run your application and click **Log in**.</span></span> <span data-ttu-id="2c605-158">Google ile oturum açmak için bir seçenek görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="2c605-158">An option to sign in with Google appears:</span></span>

![Microsoft Edge'de çalışan web uygulaması: Kullanıcı Kimliği](index/_static/DoneGoogle.png)

<span data-ttu-id="2c605-160">Google'da tıkladığınızda, kimlik doğrulaması için Google'a yönlendirilir:</span><span class="sxs-lookup"><span data-stu-id="2c605-160">When you click on Google, you are redirected to Google for authentication:</span></span>

![Google kimlik doğrulama iletişim](index/_static/GoogleLogin.png)

<span data-ttu-id="2c605-162">Google kimlik bilgilerinizi girdikten sonra ardından web e-postanızı ayarlayabileceğiniz siteye geri yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2c605-162">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="2c605-163">Şimdi, Google kimlik bilgilerinizi kullanarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="2c605-163">You are now logged in using your Google credentials:</span></span>

![Microsoft Edge'de çalışan web uygulaması: Kimliği doğrulanmış kullanıcı](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="2c605-165">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="2c605-165">Troubleshooting</span></span>

* <span data-ttu-id="2c605-166">Alırsanız bir `403 (Forbidden)` geliştirme modu (veya aynı hata ayıklayıcısına kesme) çalışıyor olun, kendi uygulamanızı hata sayfasından **Google + API** içinde etkin **APIYöneticisikitaplığı** listelenen adımları takip ederek [bu sayfadaki önceki](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="2c605-166">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="2c605-167">Hataları almıyor olabilirler ve oturum açma işe yaramazsa, sorunu hata ayıklama daha kolay hale getirmek için geliştirme moduna geçin.</span><span class="sxs-lookup"><span data-stu-id="2c605-167">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="2c605-168">**ASP.NET Core 2.x yalnızca:** Kimlik çağırarak yapılandırılmazsa `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması yapmaya sonuçlanır *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*.</span><span class="sxs-lookup"><span data-stu-id="2c605-168">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="2c605-169">Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c605-169">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="2c605-170">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="2c605-170">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="2c605-171">Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="2c605-171">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c605-172">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="2c605-172">Next steps</span></span>

* <span data-ttu-id="2c605-173">Bu makalede, Google ile kimliğini nasıl doğrulayabileceğiniz gösterdi.</span><span class="sxs-lookup"><span data-stu-id="2c605-173">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="2c605-174">Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="2c605-174">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="2c605-175">Web sitenizi Azure web uygulaması yayımladıktan sonra sıfırlamalısınız `ClientSecret` Google API konsolunda.</span><span class="sxs-lookup"><span data-stu-id="2c605-175">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="2c605-176">Ayarlama `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret` Azure portalında uygulama ayarları olarak.</span><span class="sxs-lookup"><span data-stu-id="2c605-176">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="2c605-177">Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2c605-177">The configuration system is set up to read keys from environment variables.</span></span>
