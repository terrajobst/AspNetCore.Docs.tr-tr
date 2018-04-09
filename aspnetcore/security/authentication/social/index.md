---
title: Facebook, Google ve ASP.NET Core dış sağlayıcı kimlik doğrulaması
author: rick-anderson
description: Bu öğretici, bir ASP.NET Core Dış kimlik doğrulama sağlayıcıları ile OAuth 2.0 kullanan 2.x uygulamasının nasıl oluşturulacağını gösterir.
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/social/index
ms.openlocfilehash: 93fa42be9c551f5bbdf3851aec1d9e01139fdb76
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="3f6e9-103">Facebook, Google ve ASP.NET Core dış sağlayıcı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="3f6e9-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<a name="security-authentication-social-logins"></a>

<span data-ttu-id="3f6e9-104">Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3f6e9-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3f6e9-105">Bu öğretici, bir ASP.NET Core, OAuth 2.0 Dış kimlik doğrulama sağlayıcıları kimlik bilgilerini kullanarak oturum açmalarını sağlar 2.x uygulamasının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="3f6e9-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), ve [Microsoft](xref:security/authentication/microsoft-logins) sağlayıcıları aşağıdaki bölümlerde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="3f6e9-107">Diğer sağlayıcıları gibi üçüncü taraf paketlerden kullanılabilir [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) ve [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="3f6e9-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Facebook, Twitter, Google artı ve Windows için sosyal medya simgeleri](index/_static/social.png)

<span data-ttu-id="3f6e9-109">Var olan kullanıcıların kimlik bilgileriyle oturum kullanıcıları etkinleştirme kullanıcılar için uygun olan ve birçok üzerine bir üçüncü taraf oturum açma işlemini yönetme karmaşıklığını kaydırır.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="3f6e9-110">Örnek olay incelemeleri tarafından nasıl sosyal oturum açmalar örnekleri trafiği ve müşteri dönüşümleri sürücü için bkz: [Facebook](https://www.facebook.com/unsupportedbrowser) ve [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="3f6e9-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="3f6e9-111">Not: Burada sunulan paketleri OAuth kimlik doğrulaması akışı karmaşıklığını önemli miktarda soyut ancak Ayrıntıları Anlama sorunlarını giderirken gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="3f6e9-112">Birçok kaynaklar kullanılabilir; Örneğin, [OAuth 2 giriş](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) veya [anlama OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span><span class="sxs-lookup"><span data-stu-id="3f6e9-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="3f6e9-113">Bazı sorunlar bakarak çözülebilir [ASP.NET Core sağlayıcısı paketler için kaynak kodunu](https://github.com/aspnet/Security/tree/dev/src).</span><span class="sxs-lookup"><span data-stu-id="3f6e9-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="3f6e9-114">Yeni bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="3f6e9-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="3f6e9-115">Başlangıç sayfasından veya aracılığıyla Visual Studio 2017 ' yeni bir proje oluşturun **Dosya > Yeni > Proje**.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-115">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="3f6e9-116">Seçin **ASP.NET çekirdek Web uygulaması** şablonu kullanılabilir **Visual C# > .NET Core** kategorisi:</span><span class="sxs-lookup"><span data-stu-id="3f6e9-116">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![Yeni Proje iletişim kutusu](index/_static/new-project.png)

* <span data-ttu-id="3f6e9-118">Dokunun **Web uygulaması** ve doğrulama **kimlik doğrulaması** ayarlanır **tek tek kullanıcı hesaplarını**:</span><span class="sxs-lookup"><span data-stu-id="3f6e9-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Yeni Web uygulaması iletişim kutusu](index/_static/select-project.png)

<span data-ttu-id="3f6e9-120">Not: Bu öğreticide Sihirbazı'nın en üstte seçilebilir ASP.NET Core 2.0 SDK sürümü için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="3f6e9-121">Geçişleri uygulayın</span><span class="sxs-lookup"><span data-stu-id="3f6e9-121">Apply migrations</span></span>

* <span data-ttu-id="3f6e9-122">Uygulamayı çalıştırın ve seçin **oturum** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-122">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="3f6e9-123">Seçin **kayıt yeni bir kullanıcı olarak** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-123">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="3f6e9-124">Yeni hesap için e-posta ve parola girin ve ardından **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-124">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="3f6e9-125">Geçişler uygulamak için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-125">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="3f6e9-126">SSL iste</span><span class="sxs-lookup"><span data-stu-id="3f6e9-126">Require SSL</span></span>

<span data-ttu-id="3f6e9-127">OAuth 2.0 HTTPS protokolü üzerinden kimlik doğrulaması için SSL kullanımını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-127">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="3f6e9-128">Not: kullanılarak oluşturulan projeleri **Web uygulaması** veya **Web API** ASP.NET 2.x SSL etkinleştirmek ve varsa https URL'si ile başlatmak için otomatik olarak yapılandırılır çekirdek için proje şablonları **tek tek Kullanıcı hesaplarını** seçeneği seçildiğinde üzerinde **kimlik doğrulamayı Değiştir iletişim** yukarıda gösterildiği gibi Proje Sihirbazı'nda.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-128">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="3f6e9-129">İçindeki adımları izleyerek, sitenizde SSL iste [Zorla SSL bir ASP.NET Core uygulamasında](xref:security/enforcing-ssl) konu.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-129">Require SSL on your site by following the steps in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="3f6e9-130">Oturum açma sağlayıcıları tarafından atanan belirteçleri depolamak için SecretManager kullanın</span><span class="sxs-lookup"><span data-stu-id="3f6e9-130">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="3f6e9-131">Sosyal oturum açma sağlayıcılarıyla atamak **uygulama kimliği** ve **uygulama gizli anahtarı** (sağlayıcı tarafından değişir tam adlandırma) kayıt işlemi sırasında belirteçleri.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-131">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process (exact naming varies by provider).</span></span>

<span data-ttu-id="3f6e9-132">Bu değerler etkili bir şekilde *kullanıcı adı* ve *parola* uygulamanız kendi API erişip "uygulama yapılandırmasında yardımıyla bağlı gizli" oluşturan kullanır **Gizli Yöneticisi** doğrudan yapılandırma dosyalarını depolamak veya bunlara kodlamak yerine.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-132">These values are effectively the *user name* and *password* your application uses to access their API, and constitute the "secrets" that can be linked to your application configuration with the help of **Secret Manager** instead of storing them in configuration files directly or hard-coding them.</span></span>

<span data-ttu-id="3f6e9-133">Adımları [ASP.NET Core geliştirme uygulama gizli anahtarları Güvenli Depolama](xref:security/app-secrets) konu böylece her oturum açma sağlayıcısı atanan belirteçleri depolayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-133">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic so that you can store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="3f6e9-134">Uygulamanızın gerektirdiği oturum açma sağlayıcılarıyla Kurulumu</span><span class="sxs-lookup"><span data-stu-id="3f6e9-134">Setup login providers required by your application</span></span>

<span data-ttu-id="3f6e9-135">Uygulamanızı ilgili sağlayıcılarını kullanacak şekilde yapılandırmak için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="3f6e9-135">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="3f6e9-136">[Facebook](xref:security/authentication/facebook-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="3f6e9-136">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="3f6e9-137">[Twitter](xref:security/authentication/twitter-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="3f6e9-137">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="3f6e9-138">[Google](xref:security/authentication/google-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="3f6e9-138">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="3f6e9-139">[Microsoft](xref:security/authentication/microsoft-logins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="3f6e9-139">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="3f6e9-140">[Diğer sağlayıcı](xref:security/authentication/otherlogins) yönergeleri</span><span class="sxs-lookup"><span data-stu-id="3f6e9-140">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

## <a name="optionally-set-password"></a><span data-ttu-id="3f6e9-141">İsteğe bağlı olarak parola ayarlama</span><span class="sxs-lookup"><span data-stu-id="3f6e9-141">Optionally set password</span></span>

<span data-ttu-id="3f6e9-142">Bir dış oturum açma sağlayıcısıyla kaydederken uygulamayla kayıtlı bir parola yok.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-142">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="3f6e9-143">Bu, oluşturma ve site için bir parola anımsama azaltır, ancak aynı zamanda, dış oturum açma sağlayıcısı üzerinde bağımlı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-143">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="3f6e9-144">Dış oturum açma sağlayıcısı kullanılamıyorsa, web sitesine oturum açamaz olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-144">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="3f6e9-145">Bir parola oluşturmasını ve dış sağlayıcıları ile oturum açma işlemi sırasında ayarlanan e-posta kullanarak oturum için:</span><span class="sxs-lookup"><span data-stu-id="3f6e9-145">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="3f6e9-146">Dokunun **Hello <email alias>**  gitmek için sağ üst köşedeki bağlantıda **Yönet** görünümü.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-146">Tap the **Hello <email alias>** link at the top right corner to navigate to the **Manage** view.</span></span>

![Web uygulaması yönet görünümü](index/_static/pass1a.png)

* <span data-ttu-id="3f6e9-148">Dokunun **oluşturma**</span><span class="sxs-lookup"><span data-stu-id="3f6e9-148">Tap **Create**</span></span>

![Parola sayfanızı ayarlama](index/_static/pass2a.png)

* <span data-ttu-id="3f6e9-150">Geçerli bir parola ayarlayın ve bu e-posta ile imzalamak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-150">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f6e9-151">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="3f6e9-151">Next steps</span></span>

* <span data-ttu-id="3f6e9-152">Bu makalede, dış kimlik doğrulama sunulan ve dış oturumlar ASP.NET Core uygulamanıza eklemek için gereken önkoşulları açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-152">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="3f6e9-153">Uygulamanız tarafından gerekli sağlayıcıları için oturum açma bilgileri yapılandırmak için başvuru sağlayıcıya özgü sayfaları.</span><span class="sxs-lookup"><span data-stu-id="3f6e9-153">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
