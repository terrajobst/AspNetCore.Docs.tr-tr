---
title: ASP.NET çekirdeği engellemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını
author: steve-smith
description: Web uygulamaları burada kötü amaçlı bir Web sitesi istemci tarayıcısına ve uygulama arasındaki etkileşimi etkileyebilir saldırıları önlemek nasıl bulur.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 3bca96f4a2e247eeeb93140df93221371d88d4d3
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341866"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="7e59b-103">ASP.NET çekirdeği engellemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını</span><span class="sxs-lookup"><span data-stu-id="7e59b-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="7e59b-104">Tarafından [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7e59b-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7e59b-105">Siteler arası istek sahtekarlığı (olarak da bilinen XSRF veya CSRF, belirgin *bakın surf*) web barındırılan uygulamalar alınabildiği bir kötü amaçlı web uygulaması etkilemek istemci tarayıcısına ve, güvendiği bir web uygulaması arasındaki etkileşimi karşı bir saldırı Tarayıcı.</span><span class="sxs-lookup"><span data-stu-id="7e59b-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="7e59b-106">Web tarayıcıları bazı kimlik doğrulama belirteçleri türleri her istek ile otomatik olarak bir Web sitesine göndermek için bu tür saldırıları mümkündür.</span><span class="sxs-lookup"><span data-stu-id="7e59b-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="7e59b-107">Bu yararlanma olarak da bilinen biçimidir bir *tek tıklatmayla saldırı* veya *arabası oturum* saldırı yararlandığı için kullanıcı daha önce oturum kimliği doğrulanmış.</span><span class="sxs-lookup"><span data-stu-id="7e59b-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="7e59b-108">CSRF saldırı örneği:</span><span class="sxs-lookup"><span data-stu-id="7e59b-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="7e59b-109">İçine bir kullanıcı oturum açtığında `www.good-banking-site.com` forms kimlik doğrulaması kullanma.</span><span class="sxs-lookup"><span data-stu-id="7e59b-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="7e59b-110">Sunucu kullanıcının kimliğini doğrular ve bir kimlik doğrulama tanımlama bilgisini içeren bir yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="7e59b-111">Site için geçerli bir kimlik doğrulama tanımlama bilgisi ile aldığı herhangi bir istek güvendiği için saldırılara karşı savunmasızdır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="7e59b-112">Kullanıcı kötü amaçlı bir sitesini ziyaret `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="7e59b-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="7e59b-113">Kötü amaçlı site `www.bad-crook-site.com`, aşağıdakine benzer bir HTML formuna içerir:</span><span class="sxs-lookup"><span data-stu-id="7e59b-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="7e59b-114">Şunlara dikkat edin formun `action` gönderileri savunmasız siteye değil, zararlı site.</span><span class="sxs-lookup"><span data-stu-id="7e59b-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="7e59b-115">CSRF "siteler arası" parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="7e59b-116">Kullanıcı düğmesi seçer.</span><span class="sxs-lookup"><span data-stu-id="7e59b-116">The user selects the submit button.</span></span> <span data-ttu-id="7e59b-117">Tarayıcı isteği yapar ve otomatik olarak istenen etki alanı için kimlik doğrulama tanımlama bilgisi içeren `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="7e59b-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="7e59b-118">İstek çalıştığı `www.good-banking-site.com` kullanıcının kimlik doğrulaması bağlamı sunucuyla ve kimliği doğrulanmış bir kullanıcı gerçekleştirmek için izin verilen herhangi bir eylem gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="7e59b-119">Kullanıcı form gönderme düğmesi nerede seçer senaryo ek olarak, kötü amaçlı site olabilir:</span><span class="sxs-lookup"><span data-stu-id="7e59b-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="7e59b-120">Otomatik olarak form gönderen bir komut dosyasını çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7e59b-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="7e59b-121">Form gönderme bir AJAX isteği gönder.</span><span class="sxs-lookup"><span data-stu-id="7e59b-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="7e59b-122">CSS kullanarak form gizleyin.</span><span class="sxs-lookup"><span data-stu-id="7e59b-122">Hide the form using CSS.</span></span>

<span data-ttu-id="7e59b-123">Bu alternatif senaryoların herhangi bir eylem veya başlangıçta amaçlı sitesini ziyaret dışındaki kullanıcı girişi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="7e59b-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="7e59b-124">HTTPS kullanarak CSRF saldırı engellemez.</span><span class="sxs-lookup"><span data-stu-id="7e59b-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="7e59b-125">Kötü amaçlı site gönderebilirsiniz bir `https://www.good-banking-site.com/` kolayca güvenli olmayan bir istek gönderebilir gibi isteyin.</span><span class="sxs-lookup"><span data-stu-id="7e59b-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="7e59b-126">Bazı saldırılar, eylemi gerçekleştirmek için bir resim etiketi, bu durumda kullanılabilir GET isteklerine yanıt uç noktaları hedefleyin.</span><span class="sxs-lookup"><span data-stu-id="7e59b-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="7e59b-127">Bu saldırı, görüntüleri izin ancak JavaScript engelleme Forumu sitelerinde ortak biçimidir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="7e59b-128">GET isteklerinde burada değişkenleri veya kaynakları değiştirilir, durum değişikliği kötü amaçlı saldırılara açık uygulamalardır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="7e59b-129">**Durum değişikliği GET istekleri güvenli değil. Hiçbir zaman bir GET isteği durumu değiştirmek en iyi bir uygulamadır.**</span><span class="sxs-lookup"><span data-stu-id="7e59b-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="7e59b-130">CSRF saldırıları, çünkü kimlik doğrulaması için tanımlama bilgileri kullanan web uygulamaları karşı desteklenir:</span><span class="sxs-lookup"><span data-stu-id="7e59b-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="7e59b-131">Tarayıcıları bir web uygulaması tarafından verilen tanımlama bilgilerini depolar.</span><span class="sxs-lookup"><span data-stu-id="7e59b-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="7e59b-132">Saklı tanımlama bilgileri, kimliği doğrulanmış kullanıcılar için oturum tanımlama bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="7e59b-133">Tanımlama bilgileri web uygulaması için bir etki alanı ile uygulama isteği tarayıcı içinde nasıl oluşturulan bağımsız olarak her istek ilişkili tüm tarayıcılar gönderin.</span><span class="sxs-lookup"><span data-stu-id="7e59b-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="7e59b-134">Ancak, CSRF saldırıları için sınırlı olmayan tanımlama bilgisinden faydalanmakta.</span><span class="sxs-lookup"><span data-stu-id="7e59b-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="7e59b-135">Örneğin, temel ve Özet kimlik doğrulaması da savunmasız.</span><span class="sxs-lookup"><span data-stu-id="7e59b-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="7e59b-136">Temel veya Özet kimlik doğrulaması ile oturum kullanıcının oturum açtığı sonra tarayıcı kadar oturum kimlik bilgileri otomatik olarak gönderir.&dagger; sona erer.</span><span class="sxs-lookup"><span data-stu-id="7e59b-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="7e59b-137">&dagger;Bu bağlamda *oturum* sırasında kullanıcının kimliğinin istemci-tarafı oturumuna başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="7e59b-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="7e59b-138">Bu sunucu tarafı oturumlarını ilgisiz veya [ASP.NET Core oturum Ara](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="7e59b-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="7e59b-139">Kullanıcılar CSRF güvenlik açıklarına karşı önlem alarak önleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="7e59b-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="7e59b-140">Dışına kullanmadan bittiğinde, web uygulamalarını imzalayın.</span><span class="sxs-lookup"><span data-stu-id="7e59b-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="7e59b-141">Düzenli aralıklarla Temizle tarayıcı tanımlama.</span><span class="sxs-lookup"><span data-stu-id="7e59b-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="7e59b-142">Ancak, CSRF güvenlik açıkları temelde web uygulaması, son kullanıcı ile ilgili bir sorun değildir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="7e59b-143">Kimlik doğrulaması temelleri</span><span class="sxs-lookup"><span data-stu-id="7e59b-143">Authentication fundamentals</span></span>

<span data-ttu-id="7e59b-144">Tanımlama bilgisi tabanlı kimlik doğrulaması, kimlik doğrulama popüler şeklidir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="7e59b-145">Belirteç tabanlı kimlik doğrulama sistemleriyle popülerliği içinde özellikle tek sayfa uygulamaları için (SPAs) büyüyor.</span><span class="sxs-lookup"><span data-stu-id="7e59b-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="7e59b-146">Tanımlama bilgisi tabanlı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="7e59b-146">Cookie-based authentication</span></span>

<span data-ttu-id="7e59b-147">Bir kullanıcının kullanıcı adı ve parola kullanarak kimlik doğrulaması, kimlik doğrulama ve yetkilendirme için kullanılan kimlik doğrulama biletini içeren bir belirteç alacakları.</span><span class="sxs-lookup"><span data-stu-id="7e59b-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="7e59b-148">Her istek istemci eşlik bir tanımlama bilgisi getirir belirteç depolanır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="7e59b-149">Oluşturma ve bu tanımlama bilgisi doğrulama tanımlama bilgisi kimlik doğrulaması ara yazılım tarafından gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="7e59b-150">[Ara yazılım](xref:fundamentals/middleware/index) kullanıcı asıl şifrelenmiş bir tanımlama bilgisine serileştirir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="7e59b-151">Sonraki isteklerde ara yazılım tanımlama bilgisini doğrular, asıl yeniden oluşturur ve asıl atar [kullanıcı](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) özelliği [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="7e59b-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="7e59b-152">Belirteç tabanlı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="7e59b-152">Token-based authentication</span></span>

<span data-ttu-id="7e59b-153">Bir kullanıcının kimliği doğrulandığında, bir belirteç (antiforgery bir belirteç değil) alacakları.</span><span class="sxs-lookup"><span data-stu-id="7e59b-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="7e59b-154">Belirteç biçiminde kullanıcı bilgilerini içeren [talep](/dotnet/framework/security/claims-based-identity-model) veya uygulama kullanıcı durumu uygulamada tutulan işaret eden bir başvuru belirteci.</span><span class="sxs-lookup"><span data-stu-id="7e59b-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="7e59b-155">Bir kullanıcı kimlik doğrulaması gerektiren bir kaynağa erişmeyi denediğinde, belirteç taşıyıcı belirteci biçiminde bir ek authorization üstbilgisi uygulamayla gönderilir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="7e59b-156">Bu uygulamayı durum bilgisiz hale getirir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-156">This makes the app stateless.</span></span> <span data-ttu-id="7e59b-157">Sonraki her istek belirteci istek için sunucu tarafı doğrulama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="7e59b-158">Bu belirteç değil *şifrelenmiş*; bunun *kodlanmış*.</span><span class="sxs-lookup"><span data-stu-id="7e59b-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="7e59b-159">Sunucu üzerinde kendi bilgilerine erişmek için belirteç kodu.</span><span class="sxs-lookup"><span data-stu-id="7e59b-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="7e59b-160">Belirtecin sonraki isteklerde göndermek için tarayıcının yerel depolama alanına belirteç depolar.</span><span class="sxs-lookup"><span data-stu-id="7e59b-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="7e59b-161">Belirteç tarayıcının yerel depolama alanında depolanıyorsa, CSRF güvenlik açığı hakkında endişelenmeyin.</span><span class="sxs-lookup"><span data-stu-id="7e59b-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="7e59b-162">Belirtecin bir tanımlama bilgisinde depolandığında CSRF bir konudur.</span><span class="sxs-lookup"><span data-stu-id="7e59b-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="7e59b-163">Bir etki alanında barındırılan birden fazla uygulama</span><span class="sxs-lookup"><span data-stu-id="7e59b-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="7e59b-164">Paylaşılan barındırma ortamları oturumu ele geçirme, oturum açma CSRF ve diğer saldırılara karşı savunmasız.</span><span class="sxs-lookup"><span data-stu-id="7e59b-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="7e59b-165">Ancak `example1.contoso.net` ve `example2.contoso.net` farklı ana altında ana bilgisayarlar arasında örtük güven ilişkisi yoktur `*.contoso.net` etki alanı.</span><span class="sxs-lookup"><span data-stu-id="7e59b-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="7e59b-166">Bu örtük güven ilişkisi, büyük olasılıkla güvenilmeyen ana (AJAX istekleri yöneten kaynak aynı ilkeleri mutlaka HTTP tanımlama bilgileri için geçerli olmayan) birbirlerinin tanımlama bilgileri etkiler olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="7e59b-167">Aynı etki alanında barındırılan uygulamalar arasında güvenilir tanımlama bilgilerini yararlanma saldırıları, etki alanları paylaşmıyor engellenebilir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="7e59b-168">Her uygulamanın kendi etki alanı üzerinde barındırıldığında yararlanmak için örtük tanımlama bilgisi güven ilişkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="7e59b-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="7e59b-169">ASP.NET Core antiforgery yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7e59b-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="7e59b-170">ASP.NET Core uygulayan antiforgery kullanarak [ASP.NET Core veri koruması](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="7e59b-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="7e59b-171">Veri koruma yığını bir sunucu grubunda çalışmak için yapılandırılmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="7e59b-172">Bkz: [veri korumasını yapılandırma](xref:security/data-protection/configuration/overview) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="7e59b-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="7e59b-173">ASP.NET Core 2.0 veya sonraki sürümlerde, [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) antiforgery belirteçleri HTML form öğelerini yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="7e59b-174">Razor dosyasının aşağıdaki biçimlendirmede otomatik olarak antiforgery belirteçleri oluşturur:</span><span class="sxs-lookup"><span data-stu-id="7e59b-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="7e59b-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) formun yöntemi GET değilse varsayılan olarak antiforgery belirteçleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7e59b-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="7e59b-176">Otomatik olarak oluşturulmasını antiforgery belirteçleri HTML form öğelerini için olur zaman `<form>` etiketinde `method="post"` özniteliği ve aşağıdakilerden birini geçerliyse:</span><span class="sxs-lookup"><span data-stu-id="7e59b-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="7e59b-177">Eylem öznitelik boştur (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="7e59b-177">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="7e59b-178">Eylem öznitelik sağlanan değil (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="7e59b-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="7e59b-179">Otomatik antiforgery belirteçleri oluşturulmasında HTML form öğelerini devre dışı bırakılabilir:</span><span class="sxs-lookup"><span data-stu-id="7e59b-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="7e59b-180">Açıkça antiforgery belirteçleri ile devre dışı `asp-antiforgery` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="7e59b-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="7e59b-181">Form öğesi seçti etiket Yardımcıları etiket Yardımcısını kullanarak genişletme [! çevirin simgesi](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="7e59b-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="7e59b-182">Kaldırma `FormTagHelper` görünümünden.</span><span class="sxs-lookup"><span data-stu-id="7e59b-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="7e59b-183">`FormTagHelper` Razor görünümüne aşağıdaki yönergesi ekleyerek bir görünümden kaldırılabilir:</span><span class="sxs-lookup"><span data-stu-id="7e59b-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="7e59b-184">[Razor sayfalarının](xref:mvc/razor-pages/index) XSRF/CSRF otomatik olarak korunur.</span><span class="sxs-lookup"><span data-stu-id="7e59b-184">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="7e59b-185">Daha fazla bilgi için bkz: [XSRF/CSRF ve Razor sayfalarının](xref:mvc/razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="7e59b-185">For more information, see [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf).</span></span>

<span data-ttu-id="7e59b-186">CSRF saldırılarına karşı savunma en yaygın yaklaşım kullanmaktır *Eşitleyici belirteci düzeni* (STP).</span><span class="sxs-lookup"><span data-stu-id="7e59b-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="7e59b-187">Kullanıcı form verilerini içeren bir sayfa istediğinde STP kullanılır:</span><span class="sxs-lookup"><span data-stu-id="7e59b-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="7e59b-188">Sunucu, istemci için geçerli kullanıcının kimliği ile ilişkili bir belirteç gönderir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="7e59b-189">İstemci belirteci doğrulama için sunucuya geri gönderir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="7e59b-190">Sunucunun kimliği doğrulanmış kullanıcının kimliğini eşleşmeyen bir belirteç alırsa, isteği reddedilir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="7e59b-191">Belirteç, benzersiz ve tahmin edilemez.</span><span class="sxs-lookup"><span data-stu-id="7e59b-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="7e59b-192">Belirteç isteklerini bir dizi uygun sıralama emin olmak için de kullanılabilir (örneğin, istek sırası sağlama: sayfa 1 &ndash; sayfa 2 &ndash; sayfa 3).</span><span class="sxs-lookup"><span data-stu-id="7e59b-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="7e59b-193">Tüm ASP.NET Core MVC ve Razor sayfalarının şablonları formlarında antiforgery belirteçleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7e59b-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="7e59b-194">Görünüm örnekleri aşağıdaki çiftinin antiforgery belirteçleri oluşturur:</span><span class="sxs-lookup"><span data-stu-id="7e59b-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="7e59b-195">Açık bir antiforgery belirteci eklemek bir `<form>` etiket Yardımcıları ile HTML Yardımcısı kullanmadan öğesi [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="7e59b-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="7e59b-196">Her önceki durumlarda, ASP.NET Core aşağıdakine benzer bir gizli bir form alanı ekler:</span><span class="sxs-lookup"><span data-stu-id="7e59b-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="7e59b-197">ASP.NET Core içeren üç [filtreleri](xref:mvc/controllers/filters) antiforgery belirteçleri ile çalışmak için:</span><span class="sxs-lookup"><span data-stu-id="7e59b-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="7e59b-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="7e59b-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="7e59b-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="7e59b-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="7e59b-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="7e59b-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="7e59b-201">Antiforgery seçenekleri</span><span class="sxs-lookup"><span data-stu-id="7e59b-201">Antiforgery options</span></span>

<span data-ttu-id="7e59b-202">Özelleştirme [antiforgery seçenekleri](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7e59b-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| <span data-ttu-id="7e59b-203">Seçenek</span><span class="sxs-lookup"><span data-stu-id="7e59b-203">Option</span></span> | <span data-ttu-id="7e59b-204">Açıklama</span><span class="sxs-lookup"><span data-stu-id="7e59b-204">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="7e59b-205">Tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="7e59b-205">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="7e59b-206">Antiforgery tanımlama bilgisi oluşturmak için kullanılan ayarları belirler.</span><span class="sxs-lookup"><span data-stu-id="7e59b-206">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="7e59b-207">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="7e59b-207">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="7e59b-208">Tanımlama bilgisinin etki alanı.</span><span class="sxs-lookup"><span data-stu-id="7e59b-208">The domain of the cookie.</span></span> <span data-ttu-id="7e59b-209">Varsayılan olarak `null`.</span><span class="sxs-lookup"><span data-stu-id="7e59b-209">Defaults to `null`.</span></span> <span data-ttu-id="7e59b-210">Bu özellik artık kullanılmıyor ve gelecek sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="7e59b-210">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="7e59b-211">Önerilen Cookie.Domain alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-211">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="7e59b-212">CookieName</span><span class="sxs-lookup"><span data-stu-id="7e59b-212">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="7e59b-213">Tanımlama bilgisinin adı.</span><span class="sxs-lookup"><span data-stu-id="7e59b-213">The name of the cookie.</span></span> <span data-ttu-id="7e59b-214">Ayarlanmadı, sistem ile başlayan bir benzersiz ad oluşturursa [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="7e59b-214">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="7e59b-215">Bu özellik artık kullanılmıyor ve gelecek sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="7e59b-215">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="7e59b-216">Önerilen Cookie.Name alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-216">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="7e59b-217">CookiePath</span><span class="sxs-lookup"><span data-stu-id="7e59b-217">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="7e59b-218">Yolu tanımlama bilgisinde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-218">The path set on the cookie.</span></span> <span data-ttu-id="7e59b-219">Bu özellik artık kullanılmıyor ve gelecek sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="7e59b-219">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="7e59b-220">Önerilen Cookie.Path alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-220">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="7e59b-221">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="7e59b-221">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="7e59b-222">Görünümlerde antiforgery belirteçleri oluşturmak için antiforgery sistem tarafından kullanılan gizli form alanının adı.</span><span class="sxs-lookup"><span data-stu-id="7e59b-222">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="7e59b-223">HeaderName</span><span class="sxs-lookup"><span data-stu-id="7e59b-223">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="7e59b-224">Antiforgery sistem tarafından kullanılan üstbilginin adı.</span><span class="sxs-lookup"><span data-stu-id="7e59b-224">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="7e59b-225">Varsa `null`, yalnızca form verilerini sistem göz önünde bulundurur.</span><span class="sxs-lookup"><span data-stu-id="7e59b-225">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="7e59b-226">requireSsl</span><span class="sxs-lookup"><span data-stu-id="7e59b-226">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="7e59b-227">SSL antiforgery sistem tarafından gerekip gerekmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-227">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="7e59b-228">Varsa `true`, SSL olmayan istekleri başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="7e59b-228">If `true`, non-SSL requests fail.</span></span> <span data-ttu-id="7e59b-229">Varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="7e59b-229">Defaults to `false`.</span></span> <span data-ttu-id="7e59b-230">Bu özellik artık kullanılmıyor ve gelecek sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="7e59b-230">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="7e59b-231">Önerilen alternatif Cookie.SecurePolicy ayarlamaktır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-231">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="7e59b-232">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="7e59b-232">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="7e59b-233">Nesil engellenip engellenmeyeceğini belirtir `X-Frame-Options` üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="7e59b-233">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="7e59b-234">Varsayılan olarak, "SAMEORIGIN" değerine sahip üstbilgi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7e59b-234">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="7e59b-235">Varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="7e59b-235">Defaults to `false`.</span></span> |

<span data-ttu-id="7e59b-236">Daha fazla bilgi için bkz: [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="7e59b-236">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="7e59b-237">IAntiforgery ile antiforgery özellikleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7e59b-237">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="7e59b-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) antiforgery özelliklerini yapılandırmak için API sağlar.</span><span class="sxs-lookup"><span data-stu-id="7e59b-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="7e59b-239">`IAntiforgery` içinde istenen `Configure` yöntemi `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7e59b-239">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="7e59b-240">Aşağıdaki örnek, antiforgery bir belirteç oluşturmak ve yanıtta (Bu konuda açıklanan varsayılan Açısal adlandırma kuralını kullanarak) bir tanımlama bilgisi olarak göndermek için uygulamanın giriş sayfasından ara yazılımını kullanır:</span><span class="sxs-lookup"><span data-stu-id="7e59b-240">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a><span data-ttu-id="7e59b-241">Antiforgery doğrulaması iste</span><span class="sxs-lookup"><span data-stu-id="7e59b-241">Require antiforgery validation</span></span>

<span data-ttu-id="7e59b-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) tek tek bir eylem, bir denetleyici uygulanabilir bir eylem filtresi veya genel olarak.</span><span class="sxs-lookup"><span data-stu-id="7e59b-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="7e59b-243">İsteğin geçerli bir antiforgery belirteci içermedikçe Bu filtre uygulanmış olan eylemler için yapılan istekleri engellenir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-243">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="7e59b-244">`ValidateAntiForgeryToken` Özniteliği bu süsler, HTTP GET istekleri dahil olmak üzere istekleri eylem yöntemleri için bir belirteç gerektirir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-244">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="7e59b-245">Varsa `ValidateAntiForgeryToken` özniteliği, uygulamanın denetleyicilerinde uygulandığında, ile geçersiz kılınabilir `IgnoreAntiforgeryToken` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="7e59b-245">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="7e59b-246">ASP.NET Core antiforgery belirteçleri GET istekleri için otomatik olarak eklemeyi desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="7e59b-246">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="7e59b-247">Otomatik olarak yalnızca güvenli olmayan HTTP yöntemleri için antiforgery belirteçleri doğrulamak</span><span class="sxs-lookup"><span data-stu-id="7e59b-247">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="7e59b-248">ASP.NET Core uygulamaları için Güvenli HTTP yöntemleri (GET, HEAD, seçenekleri ve izleme) antiforgery belirteçleri oluşturmak yok.</span><span class="sxs-lookup"><span data-stu-id="7e59b-248">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="7e59b-249">Kapsamlı uygulama yerine `ValidateAntiForgeryToken` özniteliği ve ile geçersiz kılma `IgnoreAntiforgeryToken` öznitelikleri [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) özniteliği kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-249">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="7e59b-250">Bu öznitelik için aynı şekilde çalışır `ValidateAntiForgeryToken` için aşağıdaki HTTP yöntemleri kullanılarak yapılan istekleri belirteçleri gerektirmeyen dışında öznitelik:</span><span class="sxs-lookup"><span data-stu-id="7e59b-250">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="7e59b-251">AL</span><span class="sxs-lookup"><span data-stu-id="7e59b-251">GET</span></span>
* <span data-ttu-id="7e59b-252">HEAD</span><span class="sxs-lookup"><span data-stu-id="7e59b-252">HEAD</span></span>
* <span data-ttu-id="7e59b-253">SEÇENEKLER</span><span class="sxs-lookup"><span data-stu-id="7e59b-253">OPTIONS</span></span>
* <span data-ttu-id="7e59b-254">TRACE</span><span class="sxs-lookup"><span data-stu-id="7e59b-254">TRACE</span></span>

<span data-ttu-id="7e59b-255">Kullanılmasını öneririz `AutoValidateAntiforgeryToken` API olmayan senaryolar için kapsamlı.</span><span class="sxs-lookup"><span data-stu-id="7e59b-255">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="7e59b-256">Bu işlem sonrası eylemler varsayılan olarak korunan sağlar.</span><span class="sxs-lookup"><span data-stu-id="7e59b-256">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="7e59b-257">Varsayılan olarak, antiforgery belirteçleri yoksaymayı sürece alternatiftir `ValidateAntiForgeryToken` tek tek eylem yöntemlerine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-257">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="7e59b-258">Bu büyük olasılıkla bırakılması POST eylem yöntemi için bu senaryoda yanlışlıkla, uygulama CSRF saldırılara karşı savunmasız bırakır korumasız.</span><span class="sxs-lookup"><span data-stu-id="7e59b-258">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="7e59b-259">Tüm gönderileri antiforgery belirteci göndermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-259">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="7e59b-260">API tanımlama bilgisi olmayan belirtecinin bir parçası göndermek için bir otomatik mekanizması yok.</span><span class="sxs-lookup"><span data-stu-id="7e59b-260">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="7e59b-261">Uygulama, büyük olasılıkla istemci kod uygulamasına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-261">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="7e59b-262">Bazı örnekleri aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="7e59b-262">Some examples are shown below:</span></span>

<span data-ttu-id="7e59b-263">Sınıf düzeyi örnek:</span><span class="sxs-lookup"><span data-stu-id="7e59b-263">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="7e59b-264">Genel örnek:</span><span class="sxs-lookup"><span data-stu-id="7e59b-264">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="7e59b-265">Geçersiz kılma genel veya denetleyicisi antiforgery öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="7e59b-265">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="7e59b-266">[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtre, belirli bir eylem (veya denetleyici) için bir antiforgery belirteci gereksinimini ortadan kaldırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-266">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="7e59b-267">Uygulandığında, bu filtre geçersiz kılmaları `ValidateAntiForgeryToken` ve `AutoValidateAntiforgeryToken` daha yüksek bir düzeyde (genel olarak veya bir denetleyicisinde) belirtilen filtreler.</span><span class="sxs-lookup"><span data-stu-id="7e59b-267">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="7e59b-268">Belirteçlerin kimlik doğrulamasından sonra Yenile</span><span class="sxs-lookup"><span data-stu-id="7e59b-268">Refresh tokens after authentication</span></span>

<span data-ttu-id="7e59b-269">Kullanıcı, kullanıcı bir görünüm veya Razor sayfalarının sayfasına yönlendirerek doğrulandıktan sonra belirteçleri yenilenecek.</span><span class="sxs-lookup"><span data-stu-id="7e59b-269">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="7e59b-270">JavaScript, AJAX ve SPAs</span><span class="sxs-lookup"><span data-stu-id="7e59b-270">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="7e59b-271">Geleneksel HTML tabanlı uygulamalarda antiforgery belirteçleri gizli form alanlarını kullanarak sunucuya geçirilir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-271">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="7e59b-272">Modern JavaScript tabanlı uygulamalar ve SPAs, birçok istek program aracılığıyla yapılır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-272">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="7e59b-273">AJAX istekleri belirteç göndermek için başka teknikler (örneğin, istek üstbilgileri veya tanımlama bilgileri) kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-273">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="7e59b-274">Tanımlama bilgileri kimlik doğrulama belirteçleri depolamak ve API isteklerinin sunucusunda kimlik doğrulaması için kullanılıyorsa, CSRF olası bir sorundur.</span><span class="sxs-lookup"><span data-stu-id="7e59b-274">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="7e59b-275">Belirteç depolamak için yerel depolama alanı kullandıysanız, yerel depolama biriminden değerleri otomatik olarak her istek ile sunucuya gönderilen değil çünkü CSRF güvenlik açığı azaltıldığından.</span><span class="sxs-lookup"><span data-stu-id="7e59b-275">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="7e59b-276">Bu nedenle, istemci ve bir istek üstbilgisini önerilen yaklaşımdır olarak belirtecin gönderme antiforgery belirteci depolamak için yerel depolama kullanma.</span><span class="sxs-lookup"><span data-stu-id="7e59b-276">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="7e59b-277">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7e59b-277">JavaScript</span></span>

<span data-ttu-id="7e59b-278">JavaScript görünümlerle kullanarak, belirteç görünümü içinde hizmetinden kullanılarak oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="7e59b-278">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="7e59b-279">Eklenmeye [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) görüntüleyebileceği ve çağırabileceği içine hizmet [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="7e59b-279">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="7e59b-280">Bu yaklaşım doğrudan sunucudan tanımlama bilgilerini ayarlama veya istemciden okuma uğraşmanız gereğini ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-280">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="7e59b-281">Önceki örnekte AJAX POST başlığı için gizli alan değeri okumak için JavaScript kullanır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-281">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="7e59b-282">JavaScript ayrıca tanımlama bilgilerini belirteçleri erişebilir ve tanımlama bilgisinin içeriği belirtecin değeri ile bir üstbilgi oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="7e59b-282">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="7e59b-283">Komut dosyası varsayılarak istekleri belirteç olarak adlandırılan bir üstbilgisinde göndermek için `X-CSRF-TOKEN`, aranacak antiforgery hizmetini yapılandırma `X-CSRF-TOKEN` üstbilgisi:</span><span class="sxs-lookup"><span data-stu-id="7e59b-283">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="7e59b-284">Aşağıdaki örnek, uygun üstbilgiyle AJAX isteği yapmak için JavaScript kullanır:</span><span class="sxs-lookup"><span data-stu-id="7e59b-284">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a><span data-ttu-id="7e59b-285">AngularJS</span><span class="sxs-lookup"><span data-stu-id="7e59b-285">AngularJS</span></span>

<span data-ttu-id="7e59b-286">AngularJS CSRF adresine bir kuralı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-286">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="7e59b-287">Sunucu tanımlama bilgisi adı ile gönderirse `XSRF-TOKEN`, AngularJS `$http` hizmeti sunucusuna bir istek gönderdiğinde bu tanımlama bilgisi değeri için bir başlık ekler.</span><span class="sxs-lookup"><span data-stu-id="7e59b-287">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="7e59b-288">Bu işlem otomatik olarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-288">This process is automatic.</span></span> <span data-ttu-id="7e59b-289">Üstbilgi açıkça ayarlanmış olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="7e59b-289">The header doesn't need to be set explicitly.</span></span> <span data-ttu-id="7e59b-290">Üstbilgi adı `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="7e59b-290">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="7e59b-291">Sunucu, bu başlığı algılamak ve içeriğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="7e59b-291">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="7e59b-292">ASP.NET Core API çalışmak için bu kural:</span><span class="sxs-lookup"><span data-stu-id="7e59b-292">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="7e59b-293">Adlı bir tanımlama bilgisine bir belirteç sağlamak için uygulamanızın yapılandırma `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="7e59b-293">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="7e59b-294">Adında bir başlık aramak için antiforgery hizmetini yapılandırma `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="7e59b-294">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="7e59b-295">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7e59b-295">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="7e59b-296">Antiforgery genişletme</span><span class="sxs-lookup"><span data-stu-id="7e59b-296">Extend antiforgery</span></span>

<span data-ttu-id="7e59b-297">[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) türü her belirteci ek veriler gidiş tarafından kötü CSRF sistem davranışını genişletmek geliştiricilere sağlar.</span><span class="sxs-lookup"><span data-stu-id="7e59b-297">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="7e59b-298">[GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) yöntemi her çağrıldığında bir alan belirteci oluşturulur ve dönüş değeri içinde oluşturulan belirteç katıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-298">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="7e59b-299">Bir uygulayan bir zaman damgası, nonce veya başka bir değer döndürür ve ardından arama [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) belirteç doğrulandığında bu verileri doğrulamak için.</span><span class="sxs-lookup"><span data-stu-id="7e59b-299">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="7e59b-300">Bu yüzden bu bilgiyi içer gerek yoktur istemcinin kullanıcı adı zaten oluşturulan belirteçlere katıştırılır.</span><span class="sxs-lookup"><span data-stu-id="7e59b-300">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="7e59b-301">Bir belirteci ek veriler ancak hiçbir içeriyorsa `IAntiForgeryAdditionalDataProvider` olan yapılandırılmış, ek veriler doğrulanmış değil.</span><span class="sxs-lookup"><span data-stu-id="7e59b-301">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7e59b-302">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7e59b-302">Additional resources</span></span>

* <span data-ttu-id="7e59b-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) üzerinde [Web uygulaması güvenlik projeyi açın](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="7e59b-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
