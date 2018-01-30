---
title: "ASP.NET Core Google dış oturum açma Kurulumu"
author: rick-anderson
description: "Bu öğretici, var olan bir ASP.NET Core uygulamaya Google hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterir."
manager: wpickett
ms.author: riande
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/google-logins
ms.openlocfilehash: 1ca63593a7cf2b0eff1e52c0beda7ef2b826d474
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="configuring-google-authentication-in-aspnet-core"></a><span data-ttu-id="767cc-103">ASP.NET Core Google kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="767cc-103">Configuring Google authentication in ASP.NET Core</span></span>

<span data-ttu-id="767cc-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="767cc-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="767cc-105">Bu öğreticide, kullanıcılarınızın üzerinde oluşturulmuş bir örnek ASP.NET Core 2.0 projeyi kullanarak Google + hesaplarında oturum olanak tanımak nasıl gösterilir [önceki sayfaya](index.md).</span><span class="sxs-lookup"><span data-stu-id="767cc-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="767cc-106">İzleyerek Başlangıç [resmi adımları](https://developers.google.com/identity/sign-in/web/devconsole-project) Google API Konsolu'nda yeni bir uygulama oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="767cc-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="767cc-107">Google API Konsolu'nda uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="767cc-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="767cc-108">Gidin [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="767cc-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="767cc-109">Bir Google hesabınız zaten yoksa, kullanın **daha fazla seçenek** > **[hesabı oluşturma](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  bağlantı oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="767cc-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Google API Console](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="767cc-111">Yönlendirilirsiniz **API Yöneticisi Kitaplığı** sayfa:</span><span class="sxs-lookup"><span data-stu-id="767cc-111">You are redirected to **API Manager Library** page:</span></span>

![API Yöneticisi kitaplığı sayfası](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="767cc-113">Dokunun **oluşturma** ve girin, **proje adı**:</span><span class="sxs-lookup"><span data-stu-id="767cc-113">Tap **Create** and enter your **Project name**:</span></span>

![Yeni Proje iletişim kutusu](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="767cc-115">İletişim kabul ettikten sonra yeni uygulamanız için özellikler seçmenizi sağlayan kitaplığı sayfasına yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="767cc-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="767cc-116">Bul **Google + API** listesi ve API özelliği eklemek için bağlantıyı tıklatın:</span><span class="sxs-lookup"><span data-stu-id="767cc-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![API Yöneticisi kitaplığı sayfası](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="767cc-118">Yeni eklenen API sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="767cc-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="767cc-119">Dokunun **etkinleştirmek** Google + oturum özelliği uygulamanıza eklemek için:</span><span class="sxs-lookup"><span data-stu-id="767cc-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![API Yöneticisi Google + API sayfası](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="767cc-121">API etkinleştirdikten sonra dokunun **kimlik bilgileri oluşturma** gizli yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="767cc-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![API Yöneticisi Google + API sayfası](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="767cc-123">Şunu seçin:</span><span class="sxs-lookup"><span data-stu-id="767cc-123">Choose:</span></span>
   * <span data-ttu-id="767cc-124">**Google + API**</span><span class="sxs-lookup"><span data-stu-id="767cc-124">**Google+ API**</span></span>
   * <span data-ttu-id="767cc-125">**Web sunucusu (örneğin node.js, Tomcat)**, ve</span><span class="sxs-lookup"><span data-stu-id="767cc-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
   * <span data-ttu-id="767cc-126">**Kullanıcı verilerini**:</span><span class="sxs-lookup"><span data-stu-id="767cc-126">**User data**:</span></span>

![API Yöneticisi kimlik bilgileri sayfası: ne tür, kimlik bilgileri çıkışı Bul paneli gerekir](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="767cc-128">Dokunun **kimlik bilgileri ne yapmalıyım?** aldığı, uygulama yapılandırması, ikinci adım **OAuth 2.0 istemci kimlik oluşturun**:</span><span class="sxs-lookup"><span data-stu-id="767cc-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![API Yöneticisi kimlik bilgileri sayfası: OAuth 2.0 istemci kimlik oluşturun](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="767cc-130">Tek bir özellik (biz girebilirsiniz aynı oturum açma), bir Google + proje oluşturuyoruz çünkü **adı** OAuth 2.0 istemci kimliği olarak proje için kullandık.</span><span class="sxs-lookup"><span data-stu-id="767cc-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="767cc-131">Geliştirme URI girin ile */signin-google* içine eklenen **yetkili yönlendirmek URI'ler** alan (örneğin: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="767cc-131">Enter your development URI with */signin-google* appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="767cc-132">Daha sonra Bu öğreticide yapılandırılmış Google kimlik doğrulama istekleri otomatik olarak işleyecek */signin-google* OAuth akış uygulamak için rota.</span><span class="sxs-lookup"><span data-stu-id="767cc-132">The Google authentication configured later in this tutorial will automatically handle requests at */signin-google* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="767cc-133">Eklemek için SEKME tuşuna basın **yetkili yönlendirmek URI'ler** girişi.</span><span class="sxs-lookup"><span data-stu-id="767cc-133">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="767cc-134">Dokunun **istemci kimliği oluşturma**, aldığı, üçüncü adım **OAuth 2.0 izin ekranı ayarlayabilirsiniz**:</span><span class="sxs-lookup"><span data-stu-id="767cc-134">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![API Yöneticisi kimlik bilgileri sayfası: OAuth 2.0 izin ekranı ayarlayabilirsiniz](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="767cc-136">Ortak yönelik girin **e-posta adresi** ve **ürün adı** uygulamanız için gösterilen Google + oturum açmak için kullanıcıya sorar.</span><span class="sxs-lookup"><span data-stu-id="767cc-136">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="767cc-137">Ek Seçenekler altında kullanılabilir **daha fazla özelleştirme seçenekleri**.</span><span class="sxs-lookup"><span data-stu-id="767cc-137">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="767cc-138">Dokunun **devam** son adım, devam etmek için **Download credentials**:</span><span class="sxs-lookup"><span data-stu-id="767cc-138">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![API Yöneticisi kimlik bilgileri sayfası: kimlik bilgilerini indirme](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="767cc-140">Dokunun **karşıdan** uygulama parolaları ile JSON dosyasının kaydedileceği ve **Bitti** yeni uygulama oluşturma işlemini tamamlamak için.</span><span class="sxs-lookup"><span data-stu-id="767cc-140">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="767cc-141">Site dağıtırken yeniden ziyaret etmeniz gerekir **Google konsol** ve yeni genel bir url kaydedin.</span><span class="sxs-lookup"><span data-stu-id="767cc-141">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="767cc-142">Mağaza Google ClientID ve ClientSecret</span><span class="sxs-lookup"><span data-stu-id="767cc-142">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="767cc-143">Bağlantı Google gibi hassas ayarları `Client ID` ve `Client Secret` kullanarak uygulamanızı yapılandırma için [gizli Yöneticisi](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="767cc-143">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="767cc-144">Bu öğreticinin amaçları doğrultusunda belirteçleri ad `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="767cc-144">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="767cc-145">Bu belirteçler için değerleri altında önceki adımda indirdiğiniz JSON dosyasını bulunabilir `web.client_id` ve `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="767cc-145">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="767cc-146">Google kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="767cc-146">Configure Google Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="767cc-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="767cc-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="767cc-148">Google hizmetinde eklemek `ConfigureServices` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="767cc-148">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="767cc-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="767cc-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="767cc-150">Bu öğreticide kullanılan proje şablonu sağlar [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) paketi yüklenir.</span><span class="sxs-lookup"><span data-stu-id="767cc-150">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

 * <span data-ttu-id="767cc-151">Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="767cc-151">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
 * <span data-ttu-id="767cc-152">.NET Core CLI ile yüklemek için proje dizininde aşağıdakileri yürütün:</span><span class="sxs-lookup"><span data-stu-id="767cc-152">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="767cc-153">Google Ara yazılımında eklemek `Configure` yönteminde *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="767cc-153">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

<span data-ttu-id="767cc-154">Bkz: [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) Google kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu.</span><span class="sxs-lookup"><span data-stu-id="767cc-154">See the [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="767cc-155">Bu kullanıcı hakkında farklı bilgi istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="767cc-155">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="767cc-156">Oturum Google ile oturum aç</span><span class="sxs-lookup"><span data-stu-id="767cc-156">Sign in with Google</span></span>

<span data-ttu-id="767cc-157">Uygulamanızı çalıştırın ve tıklatın **oturum**.</span><span class="sxs-lookup"><span data-stu-id="767cc-157">Run your application and click **Log in**.</span></span> <span data-ttu-id="767cc-158">Google ile oturum aç imzalamak için bir seçenek görünür:</span><span class="sxs-lookup"><span data-stu-id="767cc-158">An option to sign in with Google appears:</span></span>

![Microsoft Edge'de çalışan web uygulaması: doğrulanmayan kullanıcı](index/_static/DoneGoogle.png)

<span data-ttu-id="767cc-160">Google üzerinde tıkladığınızda, Google için kimlik doğrulaması için yönlendirilir:</span><span class="sxs-lookup"><span data-stu-id="767cc-160">When you click on Google, you are redirected to Google for authentication:</span></span>

![Google kimlik doğrulama iletişim](index/_static/GoogleLogin.png)

<span data-ttu-id="767cc-162">Google kimlik bilgilerinizi girdikten sonra daha sonra geri e-postanızı ayarlayabileceğiniz web sitesine yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="767cc-162">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="767cc-163">Google kimlik bilgilerinizi kullanarak şimdi kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="767cc-163">You are now logged in using your Google credentials:</span></span>

![Microsoft Edge'de çalışan web uygulaması: kimliği doğrulanmış kullanıcı](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="767cc-165">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="767cc-165">Troubleshooting</span></span>

* <span data-ttu-id="767cc-166">Alırsanız, bir `403 (Forbidden)` geliştirme modunu (veya aynı hata hata ayıklayıcısı içine sonu) çalıştığından emin olun, kendi uygulama hata sayfasından **Google + API** içindeki etkin **API Yöneticisi Kitaplığı** listelenen adımları izleyerek [bu sayfadaki önceki](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="767cc-166">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="767cc-167">Oturum açma çalışmaz ve hataları alma değil, sorunu hata ayıklama kolay hale getirmek için geliştirme moduna geçin.</span><span class="sxs-lookup"><span data-stu-id="767cc-167">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="767cc-168">**ASP.NET Core 2.x yalnızca:** ise kimlik çağırarak yapılandırılmamış `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması gerçekleştirmeye neden olur *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*.</span><span class="sxs-lookup"><span data-stu-id="767cc-168">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="767cc-169">Bu öğreticide kullanılan proje şablonu, bu yapılır sağlar.</span><span class="sxs-lookup"><span data-stu-id="767cc-169">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="767cc-170">Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız oldu istek işlenirken* hata.</span><span class="sxs-lookup"><span data-stu-id="767cc-170">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="767cc-171">Dokunun **uygulamak geçişler** veritabanını oluşturmak ve hata devam etmek için yenileyin.</span><span class="sxs-lookup"><span data-stu-id="767cc-171">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="767cc-172">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="767cc-172">Next steps</span></span>

* <span data-ttu-id="767cc-173">Bu makalede, Google ile nasıl doğrulayabilir gösterdi.</span><span class="sxs-lookup"><span data-stu-id="767cc-173">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="767cc-174">Listelenen diğer sağlayıcıları, kimlik doğrulaması için benzer bir yaklaşım izleyebilirsiniz [önceki sayfaya](index.md).</span><span class="sxs-lookup"><span data-stu-id="767cc-174">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="767cc-175">Azure web uygulaması için web sitenizi yayımladıktan sonra sıfırlamalıdır `ClientSecret` Google API Konsolu'nda.</span><span class="sxs-lookup"><span data-stu-id="767cc-175">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="767cc-176">Ayarlama `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret` olarak Azure portalında uygulama ayarları.</span><span class="sxs-lookup"><span data-stu-id="767cc-176">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="767cc-177">Yapılandırma sistemi ortam değişkenlerinin anahtarları okumak için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="767cc-177">The configuration system is set up to read keys from environment variables.</span></span>
