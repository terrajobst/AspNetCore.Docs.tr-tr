---
title: ASP.NET Core Facebook, Google ve dış sağlayıcı kimlik doğrulaması
author: rick-anderson
description: Bu öğreticide, dış kimlik doğrulama sağlayıcılarıyla OAuth 2,0 kullanarak bir ASP.NET Core 2. x uygulamasının nasıl oluşturulacağı gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2019
uid: security/authentication/social/index
ms.openlocfilehash: edaf9eeaf02879b2f7816bab0eb373a7de640c05
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082512"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="16f4b-103">ASP.NET Core Facebook, Google ve dış sağlayıcı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="16f4b-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="16f4b-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="16f4b-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="16f4b-105">Bu öğreticide, kullanıcıların dış kimlik doğrulama sağlayıcılarından kimlik bilgileriyle OAuth 2,0 kullanarak oturum açmasını sağlayan ASP.NET Core 2,2 uygulamasının nasıl oluşturulacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="16f4b-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="16f4b-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins)ve [Microsoft](xref:security/authentication/microsoft-logins) sağlayıcıları aşağıdaki bölümlerde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="16f4b-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="16f4b-107">Diğer sağlayıcılar, [Aspnet. Security. OAuth. Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) ve [Aspnet. Security. OpenID. Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers)gibi üçüncü taraf paketlerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="16f4b-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Facebook, Twitter, Google Plus ve Windows için sosyal medya simgeleri](index/_static/social.png)

<span data-ttu-id="16f4b-109">Kullanıcıların mevcut kimlik bilgileriyle oturum açmasını sağlama:</span><span class="sxs-lookup"><span data-stu-id="16f4b-109">Enabling users to sign in with their existing credentials:</span></span>
* <span data-ttu-id="16f4b-110">Kullanıcılar için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="16f4b-110">Is convenient for the users.</span></span>
* <span data-ttu-id="16f4b-111">Oturum açma işlemini üçüncü bir tarafa yönetmenin karmaşıklıklarını birçok kaydırır.</span><span class="sxs-lookup"><span data-stu-id="16f4b-111">Shifts many of the complexities of managing the sign-in process onto a third party.</span></span> 

<span data-ttu-id="16f4b-112">Sosyal oturumların trafik ve müşteri dönüştürmelerini nasıl ve ne şekilde, [Facebook](https://www.facebook.com/unsupportedbrowser) ve [Twitter](https://dev.twitter.com/resources/case-studies)tarafından örnek olay incelemeleri hakkında örnekler için bkz.</span><span class="sxs-lookup"><span data-stu-id="16f4b-112">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="16f4b-113">Yeni bir ASP.NET Core projesi oluştur</span><span class="sxs-lookup"><span data-stu-id="16f4b-113">Create a New ASP.NET Core Project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="16f4b-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16f4b-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="16f4b-115">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="16f4b-115">Create a new project.</span></span>
* <span data-ttu-id="16f4b-116">**ASP.NET Core Web uygulaması** ' nı ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-116">Select **ASP.NET Core Web Application** and **Next**.</span></span>
* <span data-ttu-id="16f4b-117">Bir **Proje adı** girin ve **konumu**onaylayın veya değiştirin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-117">Provide a **Project name** and confirm or change the **Location**.</span></span> <span data-ttu-id="16f4b-118">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-118">Select **Create**.</span></span>
* <span data-ttu-id="16f4b-119">Açılan kutuda **ASP.NET Core 2,2** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-119">Select **ASP.NET Core 2.2** in the drop down.</span></span> <span data-ttu-id="16f4b-120">Şablon listesinden **Web uygulaması** ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-120">Select **Web Application** in the template list.</span></span>
* <span data-ttu-id="16f4b-121">**Kimlik doğrulaması**altında **Değiştir** ' i seçin ve kimlik doğrulamasını **bireysel kullanıcı hesapları**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="16f4b-121">Under **Authentication**, select **Change** and set the authentication to **Individual User Accounts**.</span></span> <span data-ttu-id="16f4b-122">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-122">Select **OK**.</span></span>
* <span data-ttu-id="16f4b-123">**Yeni ASP.NET Core Web uygulaması oluştur** penceresinde **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-123">In the **Create a new ASP.NET Core Web Application** window, select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="16f4b-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="16f4b-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="16f4b-125">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="16f4b-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="16f4b-126">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-126">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="16f4b-127">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="16f4b-127">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o WebApp1 -au Individual -uld
  code -r WebApp1
  ```

  * <span data-ttu-id="16f4b-128">Komut WebApp1 klasöründe yeni bir Razor Pages projesi oluşturur. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="16f4b-128">The `dotnet new` command creates a new Razor Pages project in the *WebApp1* folder.</span></span>
  * <span data-ttu-id="16f4b-129">`-uld`SQLite yerine LocalDB kullanır.</span><span class="sxs-lookup"><span data-stu-id="16f4b-129">`-uld` uses LocalDB instead of SQLite.</span></span> <span data-ttu-id="16f4b-130">SQLite `-uld` kullanmak için atlayın.</span><span class="sxs-lookup"><span data-stu-id="16f4b-130">Omit `-uld` to use SQLite.</span></span>
  * <span data-ttu-id="16f4b-131">`-au Individual`Tek kimlik doğrulaması için kodu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="16f4b-131">`-au Individual` creates the code for Individual authentication.</span></span>
  * <span data-ttu-id="16f4b-132">Komutu, Visual Studio Code yeni bir örneğinde WebApp1 klasörünü açar. `code`</span><span class="sxs-lookup"><span data-stu-id="16f4b-132">The `code` command opens the *WebApp1* folder in a new instance of Visual Studio Code.</span></span>

* <span data-ttu-id="16f4b-133">' WebApp1 ' içinde derleme **ve hata ayıklama için gerekli varlıkların bulunduğu bir iletişim kutusu görünür. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="16f4b-133">A dialog box appears with **Required assets to build and debug are missing from 'WebApp1'. Add them?**</span></span> <span data-ttu-id="16f4b-134">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-134">Select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="16f4b-135">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16f4b-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="16f4b-136">Seçin **dosya** > **yeni çözüm**.</span><span class="sxs-lookup"><span data-stu-id="16f4b-136">Select **File** > **New Solution**.</span></span>
* <span data-ttu-id="16f4b-137">Kenar çubuğunda **.NET Core** > **uygulaması** ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-137">Select **.NET Core** > **App** in the sidebar.</span></span> <span data-ttu-id="16f4b-138">**Web uygulaması** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-138">Select the **Web Application** template.</span></span> <span data-ttu-id="16f4b-139">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-139">Select **Next**.</span></span>
* <span data-ttu-id="16f4b-140">**Hedef Framework** açılan öğesini **.NET Core 2,2**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="16f4b-140">Set the **Target Framework** drop down to **.NET Core 2.2**.</span></span> <span data-ttu-id="16f4b-141">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-141">Select **Next**.</span></span>
* <span data-ttu-id="16f4b-142">Bir **Proje adı**girin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-142">Provide a **Project Name**.</span></span> <span data-ttu-id="16f4b-143">**Konumu**onaylayın veya değiştirin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-143">Confirm or change the **Location**.</span></span> <span data-ttu-id="16f4b-144">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-144">Select **Create**.</span></span>

---

## <a name="apply-migrations"></a><span data-ttu-id="16f4b-145">Geçişleri Uygula</span><span class="sxs-lookup"><span data-stu-id="16f4b-145">Apply migrations</span></span>

* <span data-ttu-id="16f4b-146">Uygulamayı çalıştırın ve **Kaydet** bağlantısını seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-146">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="16f4b-147">Yeni hesap için e-posta ve parolayı girip **Kaydet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-147">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="16f4b-148">Geçişleri uygulamak için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-148">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="16f4b-149">Oturum açma sağlayıcıları tarafından atanan belirteçleri depolamak için SecretManager kullanın</span><span class="sxs-lookup"><span data-stu-id="16f4b-149">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="16f4b-150">Sosyal oturum açma sağlayıcıları, kayıt işlemi sırasında **uygulama kimliği** ve **uygulama gizli** belirteçleri atar.</span><span class="sxs-lookup"><span data-stu-id="16f4b-150">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="16f4b-151">Tam belirteç adları sağlayıcıya göre farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="16f4b-151">The exact token names vary by provider.</span></span> <span data-ttu-id="16f4b-152">Bu belirteçler, uygulamanızın API 'lerine erişmek için kullandığı kimlik bilgilerini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="16f4b-152">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="16f4b-153">Belirteçler, [gizli yöneticinin](xref:security/app-secrets#secret-manager)yardımıyla uygulama yapılandırmanızla bağlantılı olabilecek "gizli dizileri" oluşturur.</span><span class="sxs-lookup"><span data-stu-id="16f4b-153">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="16f4b-154">Gizli dizi, belirteçleri *appSettings. JSON*gibi bir yapılandırma dosyasında depolamanın daha güvenli bir alternatifidir.</span><span class="sxs-lookup"><span data-stu-id="16f4b-154">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="16f4b-155">Gizli yönetici yalnızca geliştirme amaçlıdır.</span><span class="sxs-lookup"><span data-stu-id="16f4b-155">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="16f4b-156">[Azure Key Vault yapılandırma sağlayıcısıyla](xref:security/key-vault-configuration)Azure test ve üretim gizli dizilerini saklayabilir ve koruyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16f4b-156">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="16f4b-157">Aşağıdaki her bir oturum açma sağlayıcısı tarafından atanan belirteçleri depolamak için [ASP.NET Core konusundaki geliştirme sırasında uygulama gizli dizileri Için güvenli depolama](xref:security/app-secrets) bölümündeki adımları izleyin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-157">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="16f4b-158">Uygulamanız için gereken oturum açma sağlayıcılarını ayarlama</span><span class="sxs-lookup"><span data-stu-id="16f4b-158">Setup login providers required by your application</span></span>

<span data-ttu-id="16f4b-159">Uygulamanızı ilgili sağlayıcıları kullanacak şekilde yapılandırmak için aşağıdaki konuları kullanın:</span><span class="sxs-lookup"><span data-stu-id="16f4b-159">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="16f4b-160">[Facebook](xref:security/authentication/facebook-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="16f4b-160">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="16f4b-161">[Twitter](xref:security/authentication/twitter-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="16f4b-161">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="16f4b-162">[Google](xref:security/authentication/google-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="16f4b-162">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="16f4b-163">[Microsoft](xref:security/authentication/microsoft-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="16f4b-163">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="16f4b-164">[Diğer sağlayıcı](xref:security/authentication/otherlogins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="16f4b-164">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="16f4b-165">İsteğe bağlı olarak parola ayarla</span><span class="sxs-lookup"><span data-stu-id="16f4b-165">Optionally set password</span></span>

<span data-ttu-id="16f4b-166">Bir dış oturum açma sağlayıcısına kaydolduysanız, uygulamaya kayıtlı bir parolanız yok demektir.</span><span class="sxs-lookup"><span data-stu-id="16f4b-166">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="16f4b-167">Bu, site için bir parola oluşturup hatırlamanızı konuma almayı azaltır, ancak aynı zamanda dış oturum açma sağlayıcısına bağımlı hale getirir.</span><span class="sxs-lookup"><span data-stu-id="16f4b-167">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="16f4b-168">Dış oturum açma sağlayıcısı kullanılamıyorsa, Web sitesinde oturum açamazsınız.</span><span class="sxs-lookup"><span data-stu-id="16f4b-168">If the external login provider is unavailable, you won't be able to sign in to the web site.</span></span>

<span data-ttu-id="16f4b-169">Bir parola oluşturmak ve dış sağlayıcılar ile oturum açma işlemi sırasında ayarladığınız e-postanızı kullanarak oturum açmak için:</span><span class="sxs-lookup"><span data-stu-id="16f4b-169">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="16f4b-170">**Yönet** görünümüne gitmek için sağ üst köşedeki **Merhaba &lt;e-&gt; posta diğer adı** bağlantısını seçin.</span><span class="sxs-lookup"><span data-stu-id="16f4b-170">Select the **Hello &lt;email alias&gt;** link at the top-right corner to navigate to the **Manage** view.</span></span>

![Web uygulaması yönetme görünümü](index/_static/pass1a.png)

* <span data-ttu-id="16f4b-172">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="16f4b-172">Select **Create**</span></span>

![Parola sayfanızı ayarlama](index/_static/pass2a.png)

* <span data-ttu-id="16f4b-174">Geçerli bir parola ayarlayın ve bunu kullanarak e-postalarınız ile oturum açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16f4b-174">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="16f4b-175">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="16f4b-175">Next steps</span></span>

* <span data-ttu-id="16f4b-176">Bu makalede dış kimlik doğrulaması tanıtılmıştır ve ASP.NET Core uygulamanıza dış oturum açma bilgileri eklemek için gereken önkoşullar açıklanacaktır.</span><span class="sxs-lookup"><span data-stu-id="16f4b-176">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="16f4b-177">Uygulamanız için gereken sağlayıcıları için oturum açma işlemlerini yapılandırmak üzere sağlayıcıya özgü sayfalara başvurun.</span><span class="sxs-lookup"><span data-stu-id="16f4b-177">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="16f4b-178">Kullanıcı ve bunların erişim ve yenileme belirteçleriyle ilgili ek verileri kalıcı hale getirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16f4b-178">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="16f4b-179">Daha fazla bilgi için bkz. <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="16f4b-179">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
