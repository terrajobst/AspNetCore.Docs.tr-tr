---
title: Facebook, Google ve ASP.NET Core dış sağlayıcı kimlik doğrulaması
author: rick-anderson
description: Bu öğreticide bir ASP.NET Core ile dış kimlik doğrulama sağlayıcıları için OAuth 2.0 kullanarak 2.x uygulamasının nasıl oluşturulacağını gösterir.
ms.author: riande
ms.custom: mvc
ms.date: 1/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: 48dd8b772234ff18158423a36ed1716102bc2f31
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396148"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="2fa0e-103">Facebook, Google ve ASP.NET Core dış sağlayıcı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="2fa0e-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="2fa0e-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2fa0e-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2fa0e-105">Bu öğretici, OAuth 2.0 ile dış kimlik sağlayıcılarına ait kimlik bilgilerini kullanarak oturum açmasına olanak tanıyan bir ASP.NET Core 2.2 uygulamasının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="2fa0e-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), ve [Microsoft](xref:security/authentication/microsoft-logins) sağlayıcıları aşağıdaki bölümlerde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="2fa0e-107">Diğer sağlayıcılar gibi üçüncü taraf paketleri kullanılabilir [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) ve [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="2fa0e-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Facebook, Twitter, Google artı ve Windows için sosyal medya simgeler](index/_static/social.png)

<span data-ttu-id="2fa0e-109">Kullanıcıların mevcut kimlik bilgileri ile oturum açmanız, kullanıcılar için uygun olan ve birçok üzerine bir üçüncü taraf oturum açma işlemi yönetmenin karmaşıklığını kaydırır.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="2fa0e-110">Örnek olay incelemeleri tarafından nasıl sosyal oturum açma bilgileri, örnek trafiği ve müşteri dönüştürmeler yönlendirebilirsiniz için bkz: [Facebook](https://www.facebook.com/unsupportedbrowser) ve [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="2fa0e-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="2fa0e-111">Yeni bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="2fa0e-111">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="2fa0e-112">Başlangıç sayfasından ya da aracılığıyla Visual Studio 2017'de yeni bir proje oluşturma **dosya** > **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-112">In Visual Studio 2017, create a new project from the Start Page, or via **File** > **New** > **Project**.</span></span>

* <span data-ttu-id="2fa0e-113">Seçin **ASP.NET Core Web uygulaması** kullanılabilir şablon **Visual C#**   >  **.NET Core** kategorisi:</span><span class="sxs-lookup"><span data-stu-id="2fa0e-113">Select the **ASP.NET Core Web Application** template available in the **Visual C#** > **.NET Core** category:</span></span>
* <span data-ttu-id="2fa0e-114">Seçin **kimlik doğrulamayı Değiştir** ve kimlik doğrulaması için **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-114">Select **Change Authentication** and set authentication to **Individual User Accounts**.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="2fa0e-115">Geçişleri Uygula</span><span class="sxs-lookup"><span data-stu-id="2fa0e-115">Apply migrations</span></span>

* <span data-ttu-id="2fa0e-116">Uygulamayı çalıştırmak ve seçmek **kaydetme** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-116">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="2fa0e-117">Yeni hesap için e-posta ve parola girin ve ardından **kaydetme**.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-117">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="2fa0e-118">Geçişleri uygulamak için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-118">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="2fa0e-119">Oturum açma sağlayıcıları tarafından atanan belirteçlerini depolamak üzere SecretManager kullanın</span><span class="sxs-lookup"><span data-stu-id="2fa0e-119">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="2fa0e-120">Sosyal oturum açma sağlayıcılarıyla atama **uygulama kimliği** ve **uygulama gizli anahtarı** kayıt işlemi sırasında belirteçleri.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-120">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="2fa0e-121">Tam bir belirteç adları, sağlayıcı tarafından farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-121">The exact token names vary by provider.</span></span> <span data-ttu-id="2fa0e-122">Bu belirteçler, uygulamanızı kendi API'ye erişmek için kullandığı kimlik bilgilerini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-122">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="2fa0e-123">"Yardım uygulama yapılandırmanıza bağlı gizli dizileri" belirteçleri oluşturan [gizli dizi Yöneticisi](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="2fa0e-123">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="2fa0e-124">Gizli dizi yöneticisidir belirteçleri gibi bir yapılandırma dosyasında saklamak için daha güvenli bir alternatif *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-124">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2fa0e-125">Gizli dizi Yöneticisi, yalnızca geliştirme amaçlıdır.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-125">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="2fa0e-126">Depolama ve Azure test ve üretim parolalarını ile korumak [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="2fa0e-126">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="2fa0e-127">Bağlantısındaki [ASP.NET core'da geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets) her oturum açma sağlayıcısı atanan belirteçlerini depolamak üzere konu.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-127">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="2fa0e-128">Uygulamanız için gereken kurulum oturum açma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="2fa0e-128">Setup login providers required by your application</span></span>

<span data-ttu-id="2fa0e-129">Uygulamanızı ilgili sağlayıcı kullanacak şekilde yapılandırmak için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="2fa0e-129">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="2fa0e-130">[Facebook](xref:security/authentication/facebook-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="2fa0e-130">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="2fa0e-131">[Twitter](xref:security/authentication/twitter-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="2fa0e-131">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="2fa0e-132">[Google](xref:security/authentication/google-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="2fa0e-132">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="2fa0e-133">[Microsoft](xref:security/authentication/microsoft-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="2fa0e-133">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="2fa0e-134">[Diğer sağlayıcı](xref:security/authentication/otherlogins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="2fa0e-134">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="2fa0e-135">İsteğe bağlı olarak parolasını ayarlayın</span><span class="sxs-lookup"><span data-stu-id="2fa0e-135">Optionally set password</span></span>

<span data-ttu-id="2fa0e-136">Bir dış oturum açma sağlayıcısıyla kaydolduğunuzda, bir parola ile uygulamanın kayıtlı sahip değilsiniz.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-136">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="2fa0e-137">Bu, oluşturma ve site için bir parola hatırlamak azaltır, ancak Ayrıca, dış oturum açma sağlayıcısına bağımlı olur.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-137">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="2fa0e-138">Dış oturum sağlayıcısının kullanılamıyorsa, web sitesine oturum açmak mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-138">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="2fa0e-139">Bir parola oluşturmasını ve dış sağlayıcılarıyla oturum açma işlemi sırasında ayarladığınız e-postanızı kullanarak oturum için:</span><span class="sxs-lookup"><span data-stu-id="2fa0e-139">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="2fa0e-140">Seçin **Hello &lt;e-posta diğer&gt;**  gitmek için sağ üst köşedeki bağlantıda **Yönet** görünümü.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-140">Select the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Web uygulaması yönetme görünümü](index/_static/pass1a.png)

* <span data-ttu-id="2fa0e-142">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="2fa0e-142">Select **Create**</span></span>

![Parola sayfanızı ayarlama](index/_static/pass2a.png)

* <span data-ttu-id="2fa0e-144">Geçerli bir parola ayarlayın ve bu oturum, e-postayı imzalamak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-144">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2fa0e-145">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="2fa0e-145">Next steps</span></span>

* <span data-ttu-id="2fa0e-146">Bu makalede, dış kimlik doğrulama sunulan ve ASP.NET Core uygulamanızı harici oturum açmaları eklemek için gereken önkoşulları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-146">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="2fa0e-147">Uygulamanızın gerektirdiği sağlayıcıları için oturum açma bilgileri yapılandırmak için sağlayıcıya özel sayfa başvurusu.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-147">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="2fa0e-148">Kullanıcı ve bunların erişim ve yenileme belirteçleri ile ilgili ek veriler kalıcı hale getirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-148">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="2fa0e-149">Daha fazla bilgi için bkz. <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="2fa0e-149">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
