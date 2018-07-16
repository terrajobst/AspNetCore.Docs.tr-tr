---
title: Facebook, Google ve ASP.NET Core dış sağlayıcı kimlik doğrulaması
author: rick-anderson
description: Bu öğreticide bir ASP.NET Core ile dış kimlik doğrulama sağlayıcıları için OAuth 2.0 kullanarak 2.x uygulamasının nasıl oluşturulacağını gösterir.
ms.author: riande
ms.date: 11/01/2016
uid: security/authentication/social/index
ms.openlocfilehash: b2176ef40faaee02c5ef53dbb92a802212e58e8b
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063331"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="8c9e4-103">Facebook, Google ve ASP.NET Core dış sağlayıcı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="8c9e4-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="8c9e4-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8c9e4-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8c9e4-105">Bu öğreticide bir ASP.NET Core, OAuth 2.0 ile dış kimlik sağlayıcılarına ait kimlik bilgilerini kullanarak oturum açmasına sağlayan 2.x uygulamasının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="8c9e4-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), ve [Microsoft](xref:security/authentication/microsoft-logins) sağlayıcıları aşağıdaki bölümlerde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="8c9e4-107">Diğer sağlayıcılar gibi üçüncü taraf paketleri kullanılabilir [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) ve [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="8c9e4-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Facebook, Twitter, Google artı ve Windows için sosyal medya simgeler](index/_static/social.png)

<span data-ttu-id="8c9e4-109">Kullanıcıların mevcut kimlik bilgileri ile oturum açmanız, kullanıcılar için uygun olan ve birçok üzerine bir üçüncü taraf oturum açma işlemi yönetmenin karmaşıklığını kaydırır.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="8c9e4-110">Örnek olay incelemeleri tarafından nasıl sosyal oturum açma bilgileri, örnek trafiği ve müşteri dönüştürmeler yönlendirebilirsiniz için bkz: [Facebook](https://www.facebook.com/unsupportedbrowser) ve [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="8c9e4-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="8c9e4-111">Not: Burada sunulan paketler OAuth kimlik doğrulaması akışı karmaşıklığını büyük ölçüde abstract, ancak Ayrıntıları Anlama sorunlarını giderirken gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="8c9e4-112">Birçok kaynak bulunur; Örneğin, [OAuth 2 giriş](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) veya [anlama OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span><span class="sxs-lookup"><span data-stu-id="8c9e4-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="8c9e4-113">Bazı sorunlar bakarak çözülebilir [sağlayıcısı paketleri için ASP.NET Core kaynak kodu](https://github.com/aspnet/Security/tree/master/src).</span><span class="sxs-lookup"><span data-stu-id="8c9e4-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/master/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="8c9e4-114">Yeni bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="8c9e4-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="8c9e4-115">Başlangıç sayfasından ya da aracılığıyla Visual Studio 2017'de yeni bir proje oluşturma **Dosya > Yeni > Proje**.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-115">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="8c9e4-116">Seçin **ASP.NET Core Web uygulaması** kullanılabilir şablon **Visual C# > .NET Core** kategorisi:</span><span class="sxs-lookup"><span data-stu-id="8c9e4-116">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![Yeni Proje iletişim kutusu](index/_static/new-project.png)

* <span data-ttu-id="8c9e4-118">Dokunun **Web uygulaması** ve doğrulama **kimlik doğrulaması** ayarlanır **bireysel kullanıcı hesapları**:</span><span class="sxs-lookup"><span data-stu-id="8c9e4-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Yeni Web uygulaması iletişim kutusu](index/_static/select-project.png)

<span data-ttu-id="8c9e4-120">Not: Bu öğretici, sihirbazın başında seçtiğiniz ASP.NET Core 2.0 SDK'sı sürümü için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="8c9e4-121">Geçişleri Uygula</span><span class="sxs-lookup"><span data-stu-id="8c9e4-121">Apply migrations</span></span>

* <span data-ttu-id="8c9e4-122">Uygulamayı çalıştırmak ve seçmek **oturum** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-122">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="8c9e4-123">Seçin **kayıt yeni bir kullanıcı olarak** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-123">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="8c9e4-124">Yeni hesap için e-posta ve parola girin ve ardından **kaydetme**.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-124">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="8c9e4-125">Geçişleri uygulamak için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-125">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="8c9e4-126">SSL iste</span><span class="sxs-lookup"><span data-stu-id="8c9e4-126">Require SSL</span></span>

<span data-ttu-id="8c9e4-127">OAuth 2.0, HTTPS protokolü üzerinden kimlik doğrulaması için SSL kullanılmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-127">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="8c9e4-128">Not: kullanılarak oluşturulan projeler **Web uygulaması** veya **Web API** proje şablonları ASP.NET Core 2.x SSL'i etkinleştirin ve varsa https URL'si ile başlatma için otomatik olarak yapılandırılır **bireysel Kullanıcı hesaplarını** seçeneği seçilmiştir **kimlik doğrulamayı Değiştir iletişim** yukarıda da gösterildiği gibi Proje Sihirbazı'nda.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-128">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="8c9e4-129">İçindeki adımları izleyerek sitenizde SSL gerektirmesini [SSL'yi zorunlu bir ASP.NET Core uygulaması](xref:security/enforcing-ssl) konu.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-129">Require SSL on your site by following the steps in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="8c9e4-130">Oturum açma sağlayıcıları tarafından atanan belirteçlerini depolamak üzere SecretManager kullanın</span><span class="sxs-lookup"><span data-stu-id="8c9e4-130">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="8c9e4-131">Sosyal oturum açma sağlayıcılarıyla atama **uygulama kimliği** ve **uygulama gizli anahtarı** kayıt işlemi sırasında belirteçleri.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-131">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="8c9e4-132">Tam bir belirteç adları, sağlayıcı tarafından farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-132">The exact token names vary by provider.</span></span> <span data-ttu-id="8c9e4-133">Bu belirteçler, uygulamanızı kendi API'ye erişmek için kullandığı kimlik bilgilerini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-133">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="8c9e4-134">"Yardım uygulama yapılandırmanıza bağlı gizli dizileri" belirteçleri oluşturan [gizli dizi Yöneticisi](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="8c9e4-134">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="8c9e4-135">Gizli dizi yöneticisidir belirteçleri gibi bir yapılandırma dosyasında saklamak için daha güvenli bir alternatif *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-135">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8c9e4-136">Gizli dizi Yöneticisi, yalnızca geliştirme amaçlıdır.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-136">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="8c9e4-137">Depolama ve Azure test ve üretim parolalarını ile korumak [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="8c9e4-137">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="8c9e4-138">Bağlantısındaki [ASP.NET core'da geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets) her oturum açma sağlayıcısı atanan belirteçlerini depolamak üzere konu.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-138">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="8c9e4-139">Uygulamanız için gereken kurulum oturum açma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="8c9e4-139">Setup login providers required by your application</span></span>

<span data-ttu-id="8c9e4-140">Uygulamanızı ilgili sağlayıcı kullanacak şekilde yapılandırmak için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="8c9e4-140">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="8c9e4-141">[Facebook](xref:security/authentication/facebook-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="8c9e4-141">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="8c9e4-142">[Twitter](xref:security/authentication/twitter-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="8c9e4-142">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="8c9e4-143">[Google](xref:security/authentication/google-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="8c9e4-143">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="8c9e4-144">[Microsoft](xref:security/authentication/microsoft-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="8c9e4-144">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="8c9e4-145">[Diğer sağlayıcı](xref:security/authentication/otherlogins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="8c9e4-145">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](~/includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="8c9e4-146">İsteğe bağlı olarak parolasını ayarlayın</span><span class="sxs-lookup"><span data-stu-id="8c9e4-146">Optionally set password</span></span>

<span data-ttu-id="8c9e4-147">Bir dış oturum açma sağlayıcısıyla kaydolduğunuzda, bir parola ile uygulamanın kayıtlı sahip değilsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-147">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="8c9e4-148">Bu, oluşturma ve site için bir parola hatırlamak azaltır, ancak Ayrıca, dış oturum açma sağlayıcısına bağımlı olur.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-148">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="8c9e4-149">Dış oturum sağlayıcısının kullanılamıyorsa, web sitesine oturum açmak mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-149">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="8c9e4-150">Bir parola oluşturmasını ve dış sağlayıcılarıyla oturum açma işlemi sırasında ayarladığınız e-postanızı kullanarak oturum için:</span><span class="sxs-lookup"><span data-stu-id="8c9e4-150">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="8c9e4-151">Dokunun **Hello &lt;e-posta diğer&gt;**  gitmek için sağ üst köşedeki bağlantıda **Yönet** görünümü.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-151">Tap the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Web uygulaması yönetme görünümü](index/_static/pass1a.png)

* <span data-ttu-id="8c9e4-153">Dokunun **oluşturma**</span><span class="sxs-lookup"><span data-stu-id="8c9e4-153">Tap **Create**</span></span>

![Parola sayfanızı ayarlama](index/_static/pass2a.png)

* <span data-ttu-id="8c9e4-155">Geçerli bir parola ayarlayın ve bu oturum, e-postayı imzalamak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-155">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c9e4-156">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="8c9e4-156">Next steps</span></span>

* <span data-ttu-id="8c9e4-157">Bu makalede, dış kimlik doğrulama sunulan ve ASP.NET Core uygulamanızı harici oturum açmaları eklemek için gereken önkoşulları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-157">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="8c9e4-158">Uygulamanızın gerektirdiği sağlayıcıları için oturum açma bilgileri yapılandırmak için sağlayıcıya özel sayfa başvurusu.</span><span class="sxs-lookup"><span data-stu-id="8c9e4-158">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
