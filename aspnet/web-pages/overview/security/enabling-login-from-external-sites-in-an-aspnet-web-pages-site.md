---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Dış bir ASP.NET Web sitelerini kullanarak oturum açmaya çalışan sayfaları (Razor) sitesinde | Microsoft Docs
author: tfitzmac
description: Bu makalede, ASP.NET Web sayfaları (Razor) sitenizi Facebook, Google, Twitter, Yahoo ve diğer siteleri kullanarak oturum açmak açıklanmaktadır — diğer bir deyişle, nasıl destekleneceğini...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: b161a890d11b1c0bf86669b819b97255c075075f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396896"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="0f54c-103">Bir ASP.NET Web sayfaları (Razor) sitesinde dış sitelere kullanarak günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="0f54c-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="0f54c-104">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0f54c-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0f54c-105">Bu makalede, ASP.NET Web sayfaları (Razor) sitenizi Facebook, Google, Twitter, Yahoo ve diğer siteleri kullanarak oturum açmak açıklanmaktadır — diğer bir deyişle, OAuth ve Openıd sitenizdeki destekleme.</span><span class="sxs-lookup"><span data-stu-id="0f54c-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="0f54c-106">Öğrenecekleriniz:</span><span class="sxs-lookup"><span data-stu-id="0f54c-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="0f54c-107">WebMatrix başlangıç sitesi şablonunda kullandığınızda oturum açma diğer sitelerden elverişli hale getirme.</span><span class="sxs-lookup"><span data-stu-id="0f54c-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="0f54c-108">Bu makalede sunulan ASP.NET özelliğidir:</span><span class="sxs-lookup"><span data-stu-id="0f54c-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="0f54c-109">`OAuthWebSecurity` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="0f54c-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0f54c-110">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="0f54c-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="0f54c-111">ASP.NET Web sayfaları (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="0f54c-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="0f54c-112">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="0f54c-112">WebMatrix 3</span></span>

<span data-ttu-id="0f54c-113">ASP.NET Web sayfaları için destek içerir [OAuth](http://oauth.net/) ve [Openıd](http://openid.net/) sağlayıcıları.</span><span class="sxs-lookup"><span data-stu-id="0f54c-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="0f54c-114">Bu sağlayıcıları kullanarak, sitenizi Facebook, Twitter, Microsoft ve Google var olan kimlik bilgilerini kullanarak oturum kullanıcılar oturum sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0f54c-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="0f54c-115">Örneğin, bir Facebook hesabı kullanarak oturum açın, kullanıcılar yalnızca bunları kullanıcı bilgilerini girdiğiniz yere Facebook oturum açma sayfasına yönlendiren bir Facebook simgesi seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0f54c-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="0f54c-116">Bunlar daha sonra Facebook oturum açma hesabıyla sitenizde ilişkilendirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0f54c-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="0f54c-117">Bir ilgili Web sayfalarını üyelik özelliklerine kullanıcılar birden çok oturum açma (sosyal ağ sitelerine oturumlardan dahil) ilişkilendirebilirsiniz tek bir hesap kullanarak Web sitenizde geliştirmedir.</span><span class="sxs-lookup"><span data-stu-id="0f54c-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="0f54c-118">Bu görüntü oturum açma sayfasından gösterir **başlangıç sitesi** şablonu, burada bir kullanıcı seçebilirsiniz dış bir hesabıyla oturum açma etkinleştirmek için bir Facebook, Twitter, Google veya Microsoft simgesi:</span><span class="sxs-lookup"><span data-stu-id="0f54c-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![Dış sağlayıcılar](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="0f54c-120">Birkaç satır kod uncommenting tarafından üyelik, OAuth ve Openıd etkinleştirebilirsiniz **başlangıç sitesi** şablonu.</span><span class="sxs-lookup"><span data-stu-id="0f54c-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="0f54c-121">Özellikler ve yöntemler OAuth ile çalışmak için kullanın ve Openıd sağlayıcılarının alanlarındadır `WebMatrix.Security.OAuthWebSecurity` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="0f54c-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="0f54c-122">**Başlangıç sitesi** şablonu tam üyelik altyapı içerir, kullanıcıların oturum yerel kimlik bilgilerini veya bu başka bir siteden kullanarak sitenizi uygulamasına izin vermeniz gerekiyor bir oturum açma sayfası, bir üyelik veritabanı ve tüm kodu ile tamamlandı .</span><span class="sxs-lookup"><span data-stu-id="0f54c-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="0f54c-123">Bu bölümde temel alan bir site için dış sitelerden oturum açmasına izin vermek nasıl bir örnek **başlangıç sitesi** şablonu.</span><span class="sxs-lookup"><span data-stu-id="0f54c-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="0f54c-124">Başlangıç sitesi oluşturduktan sonra bu (ayrıntıları izleme) yapın:</span><span class="sxs-lookup"><span data-stu-id="0f54c-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="0f54c-125">Bir OAuth sağlayıcısı (Facebook, Twitter ve Microsoft) kullanan siteler için dış sitesinde bir uygulama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0f54c-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="0f54c-126">Bu, bu siteleri için oturum açma özelliği çağırmak için gereken uygulama anahtarlarını sağlar.</span><span class="sxs-lookup"><span data-stu-id="0f54c-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="0f54c-127">Bir Openıd sağlayıcısı (Google) kullanan siteler için uygulama oluşturma gerekmez.</span><span class="sxs-lookup"><span data-stu-id="0f54c-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="0f54c-128">Tüm bu sitelerden oturum açmak için ve geliştirici uygulamaları oluşturmak için hesabınız gerekir.</span><span class="sxs-lookup"><span data-stu-id="0f54c-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0f54c-129">Oturum açma bilgileri test etmek için bir yerel Web sitesi URL'si kullanamazsınız. Bu nedenle Microsoft uygulamaları yalnızca bir çalışan Web sitesi için Canlı bir URL kabul edin.</span><span class="sxs-lookup"><span data-stu-id="0f54c-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="0f54c-130">Web sitenizi birkaç dosyalarında, uygun kimlik doğrulama sağlayıcısını belirtmek için ve bir oturum açma kullanmak istediğiniz siteye göndermek için düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="0f54c-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="0f54c-131">Bu makale aşağıdaki görevleri için ayrı yönergeler sağlar:</span><span class="sxs-lookup"><span data-stu-id="0f54c-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="0f54c-132">Google oturum açma etkinleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="0f54c-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="0f54c-133">Facebook oturum açma etkinleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="0f54c-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="0f54c-134">Twitter oturum açma etkinleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="0f54c-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="0f54c-135">Google oturum açma etkinleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="0f54c-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="0f54c-136">Oluşturun veya WebMatrix başlangıç sitesi şablonu temel alan bir ASP.NET Web sayfaları sitesinde açın.</span><span class="sxs-lookup"><span data-stu-id="0f54c-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="0f54c-137">Açık  *\_AppStart.cshtml* sayfasında ve kod aşağıdaki satırı açıklamadan çıkarın.</span><span class="sxs-lookup"><span data-stu-id="0f54c-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="0f54c-138">Google oturum açma testi</span><span class="sxs-lookup"><span data-stu-id="0f54c-138">Testing Google login</span></span>

1. <span data-ttu-id="0f54c-139">Çalıştırma *default.cshtml* sitenizin sayfasını ve **oturum** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0f54c-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="0f54c-140">Üzerinde *oturum açma* sayfasında **başka bir hizmete oturum açmak için kullandığınız** bölümünde, ya da seçin **Google** veya **Yahoo** Gönder düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0f54c-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="0f54c-141">Bu örnek, Google oturum açma bilgilerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="0f54c-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="0f54c-142">Web sayfasının isteği Google oturum açma sayfasına yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="0f54c-142">The web page redirects the request to the Google login page.</span></span>

    ![Google oturum açma](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="0f54c-144">Varolan bir Google hesabı kimlik bilgilerini girin.</span><span class="sxs-lookup"><span data-stu-id="0f54c-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="0f54c-145">Google izin vermek istediğiniz isterse *Localhost* hesap bilgilerinden kullanmak için **izin**.</span><span class="sxs-lookup"><span data-stu-id="0f54c-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="0f54c-146">Kodu, kullanıcının kimliğini doğrulamak için Google belirteci kullanır ve bu sayfaya Web sitenizde döndürür.</span><span class="sxs-lookup"><span data-stu-id="0f54c-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="0f54c-147">Bu sayfa, kullanıcıların kendi Google oturum açma bilgilerini kullanarak Web sitenizde var olan bir hesapla ilişkilendirmek sağlar veya dış oturum açma ile ilişkilendirmek için sitenizde yeni bir hesap kaydedebilir.</span><span class="sxs-lookup"><span data-stu-id="0f54c-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="0f54c-149">Seçin **ilişkilendirmek** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0f54c-149">Choose the **Associate** button.</span></span> <span data-ttu-id="0f54c-150">Tarayıcıda uygulama giriş sayfasına döndürür.</span><span class="sxs-lookup"><span data-stu-id="0f54c-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="0f54c-151">Facebook oturum açma etkinleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="0f54c-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="0f54c-152">Git [Facebook geliştiriciler site](https://developers.facebook.com/apps) (henüz oturum açmadıysanız oturum açın).</span><span class="sxs-lookup"><span data-stu-id="0f54c-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="0f54c-153">Seçin **yeni uygulama oluştur** düğmesini ve sonra adı ve yeni uygulama oluşturmak için istemleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="0f54c-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="0f54c-154">Bölümünde **uygulamanızı Facebook ile nasıl tümleştirilecek seçin**, seçin **Web sitesi** bölümü.</span><span class="sxs-lookup"><span data-stu-id="0f54c-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="0f54c-155">Doldurun **Site URL'si** alanına sitenizin URL'siyle (örneğin, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="0f54c-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="0f54c-156">**Etki alanı** alan isteğe bağlıdır; bunu tüm etki alanı için kimlik doğrulaması sağlamak için kullanabilirsiniz (örneğin *example.com*).</span><span class="sxs-lookup"><span data-stu-id="0f54c-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="0f54c-157">Yerel bilgisayarınızda bir URL ile bir site çalıştırıyorsanız, ister `http://localhost:12345` (sayıdır bir yerel bağlantı noktası numarası), bu değeri ekleyebileceğiniz **Site URL'si** sitenizi test etmek için alan.</span><span class="sxs-lookup"><span data-stu-id="0f54c-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="0f54c-158">Ancak, yerel site değişikliği bağlantı noktası numarası için istediğiniz zaman, güncelleştirmeniz gerekecektir **Site URL'si** uygulamanızın alan.</span><span class="sxs-lookup"><span data-stu-id="0f54c-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="0f54c-159">Seçin **Değişiklikleri Kaydet** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0f54c-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="0f54c-160">Seçin **uygulamaları** sekmesine ve ardından uygulamanız için başlangıç sayfasını görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="0f54c-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="0f54c-161">Kopyalama **uygulama kimliği** ve **uygulama gizli anahtarı** uygulamanız için değerleri ve bunları bir geçici bir metin dosyasına yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="0f54c-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="0f54c-162">Bu değerler, Web sitesi kodunuzda Facebook sağlayıcıya geçer.</span><span class="sxs-lookup"><span data-stu-id="0f54c-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="0f54c-163">Facebook Geliştirici sitesi çıkın.</span><span class="sxs-lookup"><span data-stu-id="0f54c-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="0f54c-164">Kullanıcıların böylece artık iki siteniz Facebook hesaplarını kullanarak siteye kayıt yapabiliyor değişiklik.</span><span class="sxs-lookup"><span data-stu-id="0f54c-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="0f54c-165">Oluşturun veya WebMatrix başlangıç sitesi şablonu temel alan bir ASP.NET Web sayfaları sitesinde açın.</span><span class="sxs-lookup"><span data-stu-id="0f54c-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="0f54c-166">Açık  *\_AppStart.cshtml* sayfasında ve Facebook OAuth sağlayıcısı için kodun açıklamasını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="0f54c-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="0f54c-167">Açıklamalı olmayan kod bloğunu aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="0f54c-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="0f54c-168">Kopyalama **uygulama kimliği** değeri olarak Facebook uygulaması değerinden `appId` parametre (tırnak işaretleri içinde).</span><span class="sxs-lookup"><span data-stu-id="0f54c-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="0f54c-169">Kopyalama **uygulama gizli anahtarı** Facebook uygulaması değerinden `appSecret` parametre değeri.</span><span class="sxs-lookup"><span data-stu-id="0f54c-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="0f54c-170">Dosyayı kaydedin ve kapatın.</span><span class="sxs-lookup"><span data-stu-id="0f54c-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="0f54c-171">Facebook oturum açma testi</span><span class="sxs-lookup"><span data-stu-id="0f54c-171">Testing Facebook login</span></span>

1. <span data-ttu-id="0f54c-172">Sitenin çalıştırması *default.cshtml* sayfasında ve **oturum açma** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0f54c-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="0f54c-173">Üzerinde *oturum açma* sayfasında **başka bir hizmete oturum açmak için kullandığınız** bölümünde, seçin **Facebook** simgesi.</span><span class="sxs-lookup"><span data-stu-id="0f54c-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="0f54c-174">Web sayfasının isteği Facebook oturum açma sayfasına yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="0f54c-174">The web page redirects the request to the Facebook login page.</span></span>

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="0f54c-176">Bir Facebook hesabınızda oturum açmak.</span><span class="sxs-lookup"><span data-stu-id="0f54c-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="0f54c-177">Kod, kimlik doğrulaması için Facebook belirteci kullanır ve burada Facebook oturum açma bilgilerinizi sitenizin oturum açma ile ilişkilendirebilirsiniz bir sayfasına döndürür.</span><span class="sxs-lookup"><span data-stu-id="0f54c-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="0f54c-178">Kullanıcı adı veya e-posta adresinizi içine doldurulur **e-posta** formdaki alan.</span><span class="sxs-lookup"><span data-stu-id="0f54c-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="0f54c-180">Seçin **ilişkilendirmek** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0f54c-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="0f54c-181">Tarayıcı giriş sayfasına döndürür ve günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="0f54c-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="0f54c-182">Twitter oturum açma etkinleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="0f54c-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="0f54c-183">Gözat [Twitter geliştiriciler site](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="0f54c-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="0f54c-184">Seçin **uygulama oluşturma** bağlamak ve siteye oturum.</span><span class="sxs-lookup"><span data-stu-id="0f54c-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="0f54c-185">Üzerinde **uygulama oluşturma** oluşturmak, doldurmak **adı** ve **açıklama** alanları.</span><span class="sxs-lookup"><span data-stu-id="0f54c-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="0f54c-186">İçinde **Web sitesi** sitenizin URL'sini girin (örneğin, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="0f54c-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="0f54c-187">Siteniz yerel olarak test ediyorsanız (gibi bir URL kullanarak `http://localhost:12345`), Twitter URL'si kabul.</span><span class="sxs-lookup"><span data-stu-id="0f54c-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="0f54c-188">Ancak, yerel bir geri döngü IP adresini kullanmanız mümkün olabilir (örneğin `http://127.0.0.1:12345`).</span><span class="sxs-lookup"><span data-stu-id="0f54c-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="0f54c-189">Bu, uygulamanızı yerel olarak test etme işlemini basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="0f54c-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="0f54c-190">Ancak, yerel site bağlantı noktası numarası her değiştiğinde güncellemeniz gerekecektir **Web sitesi** uygulamanızın alan.</span><span class="sxs-lookup"><span data-stu-id="0f54c-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="0f54c-191">İçinde **geri çağırma URL'si** alan, kullanıcıların Twitter ile oturum açtıktan sonra dönmek için istediğiniz Web sayfası için bir URL girin.</span><span class="sxs-lookup"><span data-stu-id="0f54c-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="0f54c-192">Örneğin, kullanıcıların (Bu, oturum açma durumu algılar) başlangıç sitesi giriş sayfasına göndermek için girdiğiniz aynı URL'yi girin. **Web sitesi** alan.</span><span class="sxs-lookup"><span data-stu-id="0f54c-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="0f54c-193">Koşulları kabul edin ve seçin **kendi Twitter uygulamanızı oluşturun** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0f54c-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="0f54c-194">Üzerinde **uygulamalarım** giriş sayfası, oluşturduğunuz uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="0f54c-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="0f54c-195">Üzerinde **ayrıntıları** için sekmesinde, kaydırın ve seçin **oluşturma My erişim belirteci** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0f54c-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="0f54c-196">Üzerinde **ayrıntıları** sekmesinde, kopya **tüketici anahtarı** ve **tüketici gizli** uygulamanız için değerleri ve bunları bir geçici bir metin dosyasına yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="0f54c-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="0f54c-197">Bu değerler, Web sitesi kodunuzda Twitter sağlayıcıya ileteceksiniz.</span><span class="sxs-lookup"><span data-stu-id="0f54c-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="0f54c-198">Twitter site çıkın.</span><span class="sxs-lookup"><span data-stu-id="0f54c-198">Exit the Twitter site.</span></span>

<span data-ttu-id="0f54c-199">Kullanıcıların Twitter hesaplarını kullanarak siteye bağlanmanız mümkün olacaktır, böylece artık iki Web sitenize değişiklik.</span><span class="sxs-lookup"><span data-stu-id="0f54c-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="0f54c-200">Oluşturun veya WebMatrix başlangıç sitesi şablonu temel alan bir ASP.NET Web sayfaları sitesinde açın.</span><span class="sxs-lookup"><span data-stu-id="0f54c-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="0f54c-201">Açık  *\_AppStart.cshtml* sayfasında ve Twitter OAuth sağlayıcısı için kodun açıklamasını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="0f54c-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="0f54c-202">Açıklamalı olmayan kod bloğu şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="0f54c-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="0f54c-203">Kopyalama **tüketici anahtarı** değeri olarak bir Twitter uygulaması değerinden `consumerKey` parametre (tırnak işaretleri içinde).</span><span class="sxs-lookup"><span data-stu-id="0f54c-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="0f54c-204">Kopyalama **tüketici gizli** değeri olarak bir Twitter uygulaması değerinden `consumerSecret` parametresi.</span><span class="sxs-lookup"><span data-stu-id="0f54c-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="0f54c-205">Dosyayı kaydedin ve kapatın.</span><span class="sxs-lookup"><span data-stu-id="0f54c-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="0f54c-206">Twitter oturum açma testi</span><span class="sxs-lookup"><span data-stu-id="0f54c-206">Testing Twitter login</span></span>

1. <span data-ttu-id="0f54c-207">Çalıştırma *default.cshtml* sitenizin sayfasını ve **oturum açma** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0f54c-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="0f54c-208">Üzerinde *oturum açma* sayfasında **başka bir hizmete oturum açmak için kullandığınız** bölümünde, seçin **Twitter** simgesi.</span><span class="sxs-lookup"><span data-stu-id="0f54c-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="0f54c-209">Web sayfasının isteği oluşturduğunuz uygulama için bir Twitter oturum açma sayfasına yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="0f54c-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="0f54c-211">Bir Twitter hesabınızda oturum açmak.</span><span class="sxs-lookup"><span data-stu-id="0f54c-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="0f54c-212">Kod kullanıcının kimliğini doğrulamak için Twitter belirtecini kullanır ve size bir sayfaya burada ilişkilendirebilirsiniz Web sitesi hesabınız ile oturum açma bilgilerinizi döndürür.</span><span class="sxs-lookup"><span data-stu-id="0f54c-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="0f54c-213">Ad veya e-posta adresinizi içine doldurulur **e-posta** formdaki alan.</span><span class="sxs-lookup"><span data-stu-id="0f54c-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="0f54c-215">Seçin **ilişkilendirmek** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0f54c-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="0f54c-216">Tarayıcı giriş sayfasına döndürür ve günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="0f54c-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="0f54c-217">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0f54c-217">Additional Resources</span></span>


- [<span data-ttu-id="0f54c-218">Site Geneline Yönelik Davranışını Özelleştirme</span><span class="sxs-lookup"><span data-stu-id="0f54c-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="0f54c-219">Bir ASP.NET Web sayfaları sitesinde için güvenlik ve üyelik ekleme</span><span class="sxs-lookup"><span data-stu-id="0f54c-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
