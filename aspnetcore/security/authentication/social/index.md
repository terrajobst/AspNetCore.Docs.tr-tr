---
title: Facebook, Google ve ASP.NET Core dış sağlayıcı kimlik doğrulaması
author: rick-anderson
description: Bu öğreticide bir ASP.NET Core ile dış kimlik doğrulama sağlayıcıları için OAuth 2.0 kullanarak 2.x uygulamasının nasıl oluşturulacağını gösterir.
ms.author: riande
ms.custom: mvc
ms.date: 4/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: e2d68ac93bdcfa2fc015e8447ea38626787cdb02
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65451050"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="575b1-103">Facebook, Google ve ASP.NET Core dış sağlayıcı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="575b1-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="575b1-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="575b1-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="575b1-105">Bu öğretici, OAuth 2.0 kullanarak dış kimlik sağlayıcılarına ait kimlik bilgileriyle oturum açmasını sağlayan bir ASP.NET Core 2.2 uygulamasının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="575b1-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="575b1-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), ve [Microsoft](xref:security/authentication/microsoft-logins) sağlayıcıları aşağıdaki bölümlerde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="575b1-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="575b1-107">Diğer sağlayıcılar gibi üçüncü taraf paketleri kullanılabilir [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) ve [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="575b1-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Facebook, Twitter, Google artı ve Windows için sosyal medya simgeler](index/_static/social.png)

<span data-ttu-id="575b1-109">Kullanıcıların mevcut kimlik bilgileriyle oturum açmasına etkinleştirme:</span><span class="sxs-lookup"><span data-stu-id="575b1-109">Enabling users to sign in with their existing credentials:</span></span>
* <span data-ttu-id="575b1-110">Kullanıcılar için uygundur.</span><span class="sxs-lookup"><span data-stu-id="575b1-110">Is convenient for the users.</span></span>
* <span data-ttu-id="575b1-111">Birçok üzerine bir üçüncü taraf oturum açma işlemi yönetmenin karmaşıklığını kaydırır.</span><span class="sxs-lookup"><span data-stu-id="575b1-111">Shifts many of the complexities of managing the sign-in process onto a third party.</span></span> 

<span data-ttu-id="575b1-112">Örnek olay incelemeleri tarafından nasıl sosyal oturum açma bilgileri, örnek trafiği ve müşteri dönüştürmeler yönlendirebilirsiniz için bkz: [Facebook](https://www.facebook.com/unsupportedbrowser) ve [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="575b1-112">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="575b1-113">Yeni bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="575b1-113">Create a New ASP.NET Core Project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="575b1-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="575b1-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="575b1-115">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="575b1-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="575b1-116">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="575b1-116">Create a new ASP.NET Core Web Application.</span></span>
* <span data-ttu-id="575b1-117">Seçin **ASP.NET Core 2.2** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="575b1-117">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>
* <span data-ttu-id="575b1-118">Seçin **kimlik doğrulamayı Değiştir** ve kimlik doğrulaması için **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="575b1-118">Select **Change Authentication** and set authentication to **Individual User Accounts**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="575b1-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="575b1-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="575b1-120">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="575b1-120">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="575b1-121">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="575b1-121">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="575b1-122">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="575b1-122">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o WebApp1 -au Individual -uld
  code -r WebApp1
  ```

  * <span data-ttu-id="575b1-123">`dotnet new` Komut yeni bir Razor sayfaları projesindeki oluşturur *WebApp1* klasör.</span><span class="sxs-lookup"><span data-stu-id="575b1-123">The `dotnet new` command creates a new Razor Pages project in the *WebApp1* folder.</span></span>
  * <span data-ttu-id="575b1-124">`-uld` LocalDB yerine SQLite kullanır.</span><span class="sxs-lookup"><span data-stu-id="575b1-124">`-uld` uses LocalDB instead of SQLite.</span></span> <span data-ttu-id="575b1-125">Atlamak `-uld` SQLite kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="575b1-125">Omit `-uld` to use SQLite.</span></span>
  * <span data-ttu-id="575b1-126">`-au Individual` Bireysel kimlik doğrulama için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="575b1-126">`-au Individual` creates the code for Individual authentication.</span></span>
  * <span data-ttu-id="575b1-127">`code` Komutu açılır *WebApp1* Visual Studio Code yeni bir örneğini klasöründe.</span><span class="sxs-lookup"><span data-stu-id="575b1-127">The `code` command opens the *WebApp1* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="575b1-128">Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'WebApp1' eksik. Bunları eklensin mi?**</span><span class="sxs-lookup"><span data-stu-id="575b1-128">A dialog box appears with **Required assets to build and debug are missing from 'WebApp1'. Add them?**</span></span>

* <span data-ttu-id="575b1-129">Seçin **Evet**</span><span class="sxs-lookup"><span data-stu-id="575b1-129">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="575b1-130">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="575b1-130">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="575b1-131">Bir terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="575b1-131">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o WebApp1 -au Individual
```

<span data-ttu-id="575b1-132">Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) Razor sayfaları projesi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="575b1-132">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="575b1-133">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="575b1-133">Open the project</span></span>

<span data-ttu-id="575b1-134">Visual Studio'dan seçin **Dosya > Aç**ve ardından *WebApp1.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="575b1-134">From Visual Studio, select **File > Open**, and then select the *WebApp1.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="apply-migrations"></a><span data-ttu-id="575b1-135">Geçişleri Uygula</span><span class="sxs-lookup"><span data-stu-id="575b1-135">Apply migrations</span></span>

* <span data-ttu-id="575b1-136">Uygulamayı çalıştırmak ve seçmek **kaydetme** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="575b1-136">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="575b1-137">Yeni hesap için e-posta ve parola girin ve ardından **kaydetme**.</span><span class="sxs-lookup"><span data-stu-id="575b1-137">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="575b1-138">Geçişleri uygulamak için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="575b1-138">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="575b1-139">Oturum açma sağlayıcıları tarafından atanan belirteçlerini depolamak üzere SecretManager kullanın</span><span class="sxs-lookup"><span data-stu-id="575b1-139">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="575b1-140">Sosyal oturum açma sağlayıcılarıyla atama **uygulama kimliği** ve **uygulama gizli anahtarı** kayıt işlemi sırasında belirteçleri.</span><span class="sxs-lookup"><span data-stu-id="575b1-140">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="575b1-141">Tam bir belirteç adları, sağlayıcı tarafından farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="575b1-141">The exact token names vary by provider.</span></span> <span data-ttu-id="575b1-142">Bu belirteçler, uygulamanızı kendi API'ye erişmek için kullandığı kimlik bilgilerini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="575b1-142">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="575b1-143">"Yardım uygulama yapılandırmanıza bağlı gizli dizileri" belirteçleri oluşturan [gizli dizi Yöneticisi](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="575b1-143">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="575b1-144">Gizli dizi yöneticisidir belirteçleri gibi bir yapılandırma dosyasında saklamak için daha güvenli bir alternatif *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="575b1-144">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="575b1-145">Gizli dizi Yöneticisi, yalnızca geliştirme amaçlıdır.</span><span class="sxs-lookup"><span data-stu-id="575b1-145">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="575b1-146">Depolama ve Azure test ve üretim parolalarını ile korumak [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="575b1-146">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="575b1-147">Bağlantısındaki [ASP.NET core'da geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets) her oturum açma sağlayıcısı atanan belirteçlerini depolamak üzere konu.</span><span class="sxs-lookup"><span data-stu-id="575b1-147">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="575b1-148">Uygulamanız için gereken kurulum oturum açma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="575b1-148">Setup login providers required by your application</span></span>

<span data-ttu-id="575b1-149">Uygulamanızı ilgili sağlayıcı kullanacak şekilde yapılandırmak için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="575b1-149">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="575b1-150">[Facebook](xref:security/authentication/facebook-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="575b1-150">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="575b1-151">[Twitter](xref:security/authentication/twitter-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="575b1-151">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="575b1-152">[Google](xref:security/authentication/google-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="575b1-152">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="575b1-153">[Microsoft](xref:security/authentication/microsoft-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="575b1-153">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="575b1-154">[Diğer sağlayıcı](xref:security/authentication/otherlogins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="575b1-154">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="575b1-155">İsteğe bağlı olarak parolasını ayarlayın</span><span class="sxs-lookup"><span data-stu-id="575b1-155">Optionally set password</span></span>

<span data-ttu-id="575b1-156">Bir dış oturum açma sağlayıcısıyla kaydolduğunuzda, bir parola ile uygulamanın kayıtlı sahip değilsiniz.</span><span class="sxs-lookup"><span data-stu-id="575b1-156">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="575b1-157">Bu, oluşturma ve site için bir parola hatırlamak azaltır, ancak Ayrıca, dış oturum açma sağlayıcısına bağımlı olur.</span><span class="sxs-lookup"><span data-stu-id="575b1-157">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="575b1-158">Dış oturum sağlayıcısının kullanılamıyorsa, web sitesine oturum açmanız mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="575b1-158">If the external login provider is unavailable, you won't be able to sign in to the web site.</span></span>

<span data-ttu-id="575b1-159">Bir parola oluşturmasını ve dış sağlayıcılarıyla oturum açma işlemi sırasında ayarladığınız e-postanızı kullanarak oturum için:</span><span class="sxs-lookup"><span data-stu-id="575b1-159">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="575b1-160">Seçin **Hello &lt;e-posta diğer&gt;**  bağlantı gitmek için sağ üst köşesinde **Yönet** görünümü.</span><span class="sxs-lookup"><span data-stu-id="575b1-160">Select the **Hello &lt;email alias&gt;** link at the top-right corner to navigate to the **Manage** view.</span></span>

![Web uygulaması yönetme görünümü](index/_static/pass1a.png)

* <span data-ttu-id="575b1-162">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="575b1-162">Select **Create**</span></span>

![Parola sayfanızı ayarlama](index/_static/pass2a.png)

* <span data-ttu-id="575b1-164">Geçerli bir parola ayarlayın ve bu oturum, e-postayı imzalamak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="575b1-164">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="575b1-165">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="575b1-165">Next steps</span></span>

* <span data-ttu-id="575b1-166">Bu makalede, dış kimlik doğrulama sunulan ve ASP.NET Core uygulamanızı harici oturum açmaları eklemek için gereken önkoşulları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="575b1-166">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="575b1-167">Uygulamanızın gerektirdiği sağlayıcıları için oturum açma bilgileri yapılandırmak için sağlayıcıya özel sayfa başvurusu.</span><span class="sxs-lookup"><span data-stu-id="575b1-167">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="575b1-168">Kullanıcı ve bunların erişim ve yenileme belirteçleri ile ilgili ek veriler kalıcı hale getirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="575b1-168">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="575b1-169">Daha fazla bilgi için bkz. <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="575b1-169">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
