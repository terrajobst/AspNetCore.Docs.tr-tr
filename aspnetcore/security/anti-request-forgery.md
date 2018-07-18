---
title: ASP.NET core'da önlemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını
author: steve-smith
description: Web uygulamaları kötü amaçlı bir Web sitesi istemci tarayıcısına ve uygulama arasındaki etkileşim burada etkileyebilir saldırıları önlemek nasıl keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
uid: security/anti-request-forgery
ms.openlocfilehash: 6a30e1e2321ca3a81d6e1a320d1d87dddb3033c7
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095794"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="9ac3c-103">ASP.NET core'da önlemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını</span><span class="sxs-lookup"><span data-stu-id="9ac3c-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="9ac3c-104">Tarafından [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9ac3c-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9ac3c-105">Siteler arası istek sahteciliği (XSRF veya CSRF, olarak da telaffuz *bakın surf*) barındırılan web uygulamaları ile bir kötü amaçlı web uygulaması etkilemek istemci tarayıcısı, güvenen bir web uygulaması arasındaki etkileşimi karşı bir saldırı olma Tarayıcı.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="9ac3c-106">Web tarayıcıları, bir Web sitesine kimlik doğrulama belirteçlerinizi bazı türleri her istek ile otomatik olarak göndermek için bu saldırıları mümkündür.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="9ac3c-107">Olarak da bilinen bu yararlanma biçiminde olan bir *tek tıklamayla saldırı* veya *arabası oturumu* saldırı yararlanır çünkü kullanıcı daha önce oturum kimliği doğrulanmış.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="9ac3c-108">CSRF saldırı örneği:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="9ac3c-109">İçinde bir kullanıcı oturum açtığında `www.good-banking-site.com` forms kimlik doğrulaması kullanma.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="9ac3c-110">Sunucusu kullanıcının kimliğini doğrular ve bir kimlik doğrulama tanımlama bilgisi içeren yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="9ac3c-111">Site bir geçerli bir kimlik doğrulama tanımlama bilgisi ile aldığı herhangi bir istek güvendiği için saldırılara karşı savunmasız durumdadır.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="9ac3c-112">Kullanıcı bir kötü amaçlı sitesini ziyaret `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="9ac3c-113">Kötü niyetli site `www.bad-crook-site.com`, aşağıdakine benzer bir HTML formuna içerir:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="9ac3c-114">Dikkat formun `action` gönderileri savunmasız sitesine, kötü niyetli site uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="9ac3c-115">CSRF "siteler arası" parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="9ac3c-116">Kullanıcının Gönder düğmesini seçer.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-116">The user selects the submit button.</span></span> <span data-ttu-id="9ac3c-117">Tarayıcı istekte bulunur ve otomatik olarak istenen etki alanı için kimlik doğrulama tanımlama bilgisi ekler `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="9ac3c-118">İstek üzerinde çalıştığı `www.good-banking-site.com` kullanıcının kimlik doğrulaması bağlamı sunucusuyla ve kimliği doğrulanmış bir kullanıcıyı gerçekleştirmek için izin verilen herhangi bir eylem gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="9ac3c-119">Kullanıcının seçebileceği formunun düğme senaryo ek olarak, kötü niyetli site yüklenemedi:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="9ac3c-120">Otomatik olarak formu gönderdiği bir betiği çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="9ac3c-121">Form gönderme bir AJAX isteği gönderin.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="9ac3c-122">CSS kullanarak form gizleyin.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-122">Hide the form using CSS.</span></span>

<span data-ttu-id="9ac3c-123">Bu alternatif senaryoların herhangi bir eylem veya kötü niyetli site ilk kez ziyaret dışındaki kullanıcı girişi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="9ac3c-124">HTTPS kullanarak CSRF saldırı engellemez.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="9ac3c-125">Kötü niyetli site göndermek için bir `https://www.good-banking-site.com/` kolayca güvenli olmayan bir istek gönderebilir gibi istek.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="9ac3c-126">Bazı saldırıları, bu durumda bir resim etiketi eylemi gerçekleştirmek için kullanılabilir GET isteklerine yanıt veren uç noktaları hedefleyin.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="9ac3c-127">Bu tür bir saldırı görüntüleri izin verir, ancak JavaScript block forum siteleri yaygındır.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="9ac3c-128">GET isteklerinde değişkenleri veya kaynaklar nerede değiştirilir, durum değişikliği uygulamaları kötü amaçlı saldırılara karşı savunmasızdır.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="9ac3c-129">**Durum değişikliği GET istekleri güvenli değil. Hiçbir zaman bir GET isteği durumu değiştirmek iyi bir uygulamadır.**</span><span class="sxs-lookup"><span data-stu-id="9ac3c-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="9ac3c-130">CSRF saldırılarına karşı olduğundan kimlik doğrulaması için tanımlama bilgilerini kullanan web uygulamaları desteklenir:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="9ac3c-131">Tarayıcılar, bir web uygulaması tarafından verilen tanımlama bilgilerini depolar.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="9ac3c-132">Depolanan bir tanımlama bilgisi kimliği doğrulanmış kullanıcılar için oturum tanımlama bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="9ac3c-133">Tarayıcılar, her isteği istek uygulamasına bir tarayıcıdan nasıl oluşturulduğunu bağımsız olarak tüm tanımlama bilgilerini ilişkili bir etki alanını web uygulaması ile gönderin.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="9ac3c-134">Ancak, CSRF saldırıları için sınırlı olmayan tanımlama bilgilerini kötüye.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="9ac3c-135">Örneğin, temel ve Özet kimlik doğrulaması ayrıca savunmasız.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="9ac3c-136">Bir kullanıcı, temel veya Özet kimlik doğrulaması ile oturum açtıktan sonra tarayıcı kadar oturum kimlik bilgileri otomatik olarak gönderir.&dagger; sona erer.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="9ac3c-137">&dagger;Bu bağlamda *oturumu* istemci-tarafı oturumu sırasında kullanıcı kimliğinin ifade eder.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="9ac3c-138">Bu sunucu tarafı oturum ilgisi olmayan veya [ASP.NET Core oturumu ara yazılım](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="9ac3c-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="9ac3c-139">Kullanıcılar, CSRF güvenlik açıklarına karşı önlemler alarak önleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="9ac3c-140">Bunları kullanarak bittiğinde, web uygulamaları dışına oturum açın.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="9ac3c-141">Düzenli aralıklarla açık tarayıcı tanımlama.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="9ac3c-142">Ancak, CSRF güvenlik açıkları, temelde web uygulaması, son kullanıcı ile ilgili bir sorun değildir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="9ac3c-143">Kimlik doğrulaması temelleri</span><span class="sxs-lookup"><span data-stu-id="9ac3c-143">Authentication fundamentals</span></span>

<span data-ttu-id="9ac3c-144">Tanımlama bilgisi tabanlı kimlik doğrulaması, kimlik doğrulama popüler bir biçimidir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="9ac3c-145">Belirteç tabanlı kimlik doğrulama sistemleri popülerliği, özellikle tek sayfalık uygulamalar için (Spa'lar) büyüyor.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="9ac3c-146">Tanımlama bilgisi tabanlı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="9ac3c-146">Cookie-based authentication</span></span>

<span data-ttu-id="9ac3c-147">Bir kullanıcı, kullanıcı adı ve parolasını kullanarak kimliğini doğrular, kimlik doğrulama ve yetkilendirme için kullanılan bir kimlik doğrulama anahtarı içeren bir belirteç, verilen.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="9ac3c-148">Belirteç, her istemci isteğe eşlik eden bir tanımlama bilgisi şekilde depolanır.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="9ac3c-149">Oluşturma ve bu tanımlama bilgisi doğrulama tanımlama bilgisi kimlik doğrulaması ara yazılımı tarafından gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="9ac3c-150">[Ara yazılım](xref:fundamentals/middleware/index) şifrelenmiş bir tanımlama bilgisine kullanıcı asıl serileştirir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="9ac3c-151">Sonraki istekler, ara yazılım tanımlama bilgisini doğrular, asıl yeniden oluşturur ve sorumlusuna atar [kullanıcı](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) özelliği [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="9ac3c-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="9ac3c-152">Belirteç tabanlı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="9ac3c-152">Token-based authentication</span></span>

<span data-ttu-id="9ac3c-153">Bir kullanıcının kimliği doğrulandığında, bir belirteç (antiforgery bir belirteç değil) verilen.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="9ac3c-154">Kullanıcı bilgileri'biçimindeki belirteç içeren [talep](/dotnet/framework/security/claims-based-identity-model) veya uygulama uygulamayı korunan kullanıcı durumunu gösteren bir başvuru belirteci.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="9ac3c-155">Bir kullanıcı kimlik doğrulaması gerektiren bir kaynağa erişmeye çalıştığında, belirteç bir ek yetkilendirme üst bilgisinde taşıyıcı belirteç biçimi ile uygulamaya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="9ac3c-156">Bu uygulama durum bilgisiz hale getirir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-156">This makes the app stateless.</span></span> <span data-ttu-id="9ac3c-157">Sonraki her istek için sunucu tarafı doğrulama isteği belirteci geçirildi.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="9ac3c-158">Bu belirteci değil *şifrelenmiş*; sahip *kodlanmış*.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="9ac3c-159">Sunucuda, belirteç bilgilerini erişmek için kodu.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="9ac3c-160">Belirtecin sonraki istekleri göndermek için belirteç tarayıcının yerel depolama alanında depolayın.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="9ac3c-161">Belirteç tarayıcının yerel depolama alanında saklanıyorsa CSRF güvenlik açığı hakkında endişelenmeyin.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="9ac3c-162">Belirteç bir tanımlama bilgisinde depolandığında CSRF bir konudur.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="9ac3c-163">Bir etki alanında barındırılan birden fazla uygulama</span><span class="sxs-lookup"><span data-stu-id="9ac3c-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="9ac3c-164">Paylaşılan barındırma ortamları oturumu ele geçirme, oturum açma CSRF ve diğer saldırılara karşı savunmasız.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="9ac3c-165">Ancak `example1.contoso.net` ve `example2.contoso.net` farklı konaklar altında konaklar arasında örtük güven ilişkisi yoktur `*.contoso.net` etki alanı.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="9ac3c-166">Bu örtük güven ilişkisi (AJAX isteklerini yöneten bir aynı çıkış noktası ilkeleri mutlaka HTTP tanımlama bilgileri için geçerli değildir) birbirlerinin tanımlama etkilemek potansiyel olarak güvenilmeyen konakları sağlar.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="9ac3c-167">Aynı etki alanında barındırılan uygulamalar arasında güvenilir tanımlama bilgilerini açığından yararlanma saldırıları, etki alanları kullanılmadığı engellenebilir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="9ac3c-168">Her uygulama kendi etki alanı üzerinde barındırıldığında yararlanmak için örtük bir tanımlama bilgisi güven ilişkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="9ac3c-169">ASP.NET Core antiforgery yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9ac3c-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="9ac3c-170">ASP.NET Core uygulayan antiforgery kullanarak [ASP.NET Core veri koruma](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="9ac3c-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="9ac3c-171">Veri koruma yığınından bir sunucu grubunda çalışacak şekilde yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="9ac3c-172">Bkz: [veri korumayı yapılandırma](xref:security/data-protection/configuration/overview) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="9ac3c-173">ASP.NET Core 2.0 veya sonraki sürümlerde, [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) antiforgery belirteçleri HTML form öğelerini yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="9ac3c-174">Bir Razor dosyasında aşağıdaki biçimlendirme antiforgery belirteçleri otomatik olarak oluşturur:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="9ac3c-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) formun yöntemi GET değilse, varsayılan olarak antiforgery belirteçleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="9ac3c-176">Otomatik olarak oluşturulmasını antiforgery belirteçleri için bir HTML form öğelerini olur olduğunda `<form>` etiketinde `method="post"` özniteliği ve aşağıdakilerden birini geçerliyse:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="9ac3c-177">Eylem özniteliktir boş (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="9ac3c-177">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="9ac3c-178">Eylem öznitelik verilmiyorsa (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="9ac3c-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="9ac3c-179">HTML form öğelerini için antiforgery belirteçleri otomatik olarak oluşturulmasını devre dışı bırakılabilir:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="9ac3c-180">Antiforgery belirteçleri ile açıkça devre dışı `asp-antiforgery` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="9ac3c-181">Form öğesi kabul etiket Yardımcıları etiket Yardımcısı kullanarak genişletme [! çevirme sembol](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="9ac3c-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="9ac3c-182">Kaldırma `FormTagHelper` görünümü.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="9ac3c-183">`FormTagHelper` Razor görünüme aşağıdaki yönerge ekleyerek bir görünümden kaldırılabilir:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="9ac3c-184">[Razor sayfaları](xref:razor-pages/index) XSRF/CSRF otomatik olarak korunur.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-184">[Razor Pages](xref:razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="9ac3c-185">Daha fazla bilgi için [XSRF/CSRF ve Razor sayfaları](xref:razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="9ac3c-185">For more information, see [XSRF/CSRF and Razor Pages](xref:razor-pages/index#xsrf).</span></span>

<span data-ttu-id="9ac3c-186">CSRF saldırılarına karşı savunma için en yaygın yaklaşımı *Eşitleyici belirteci deseni* (STP).</span><span class="sxs-lookup"><span data-stu-id="9ac3c-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="9ac3c-187">Kullanıcı form verilerini içeren bir sayfa istediğinde STP kullanılır:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="9ac3c-188">Sunucu, istemci için geçerli kullanıcının kimliğiyle ilişkili bir belirteç gönderir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="9ac3c-189">İstemci belirteci doğrulama sunucusuna geri gönderir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="9ac3c-190">Sunucu kimliği doğrulanmış kullanıcının kimliğine eşleşmeyen bir belirteç alırsa, istek reddedilir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="9ac3c-191">Belirteç benzersiz ve tahmin edilemez.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="9ac3c-192">Belirteç, bir dizi isteğin doğru sıralama emin olmak için de kullanılabilir (örneğin, istek sırası sağlama: sayfa 1 &ndash; sayfa 2 &ndash; sayfa 3).</span><span class="sxs-lookup"><span data-stu-id="9ac3c-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="9ac3c-193">Tüm ASP.NET Core MVC ve Razor sayfaları şablonları formlarında antiforgery belirteçleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="9ac3c-194">Örnekleri görüntüle çiftiyle antiforgery belirteçleri oluşturun:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="9ac3c-195">Açıkça bir antiforgery belirtecine ekleme bir `<form>` etiket Yardımcıları ile HTML Yardımcısını kullanmadan öğesi [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="9ac3c-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="9ac3c-196">Her önceki örneklerin, ASP.NET Core aşağıdakine benzer bir gizli form alanının ekler:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="9ac3c-197">ASP.NET Core içeren üç [filtreleri](xref:mvc/controllers/filters) antiforgery belirteçleri ile çalışmak için:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="9ac3c-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="9ac3c-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="9ac3c-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="9ac3c-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="9ac3c-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="9ac3c-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="9ac3c-201">Antiforgery seçenekleri</span><span class="sxs-lookup"><span data-stu-id="9ac3c-201">Antiforgery options</span></span>

<span data-ttu-id="9ac3c-202">Özelleştirme [antiforgery seçenekleri](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

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

| <span data-ttu-id="9ac3c-203">Seçenek</span><span class="sxs-lookup"><span data-stu-id="9ac3c-203">Option</span></span> | <span data-ttu-id="9ac3c-204">Açıklama</span><span class="sxs-lookup"><span data-stu-id="9ac3c-204">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="9ac3c-205">Tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="9ac3c-205">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="9ac3c-206">Antiforgery tanımlama bilgisi oluşturmak için kullanılan ayarları belirler.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-206">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="9ac3c-207">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="9ac3c-207">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="9ac3c-208">Tanımlama bilgisinin etki alanı.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-208">The domain of the cookie.</span></span> <span data-ttu-id="9ac3c-209">Varsayılan olarak `null`.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-209">Defaults to `null`.</span></span> <span data-ttu-id="9ac3c-210">Bu özellik artık kullanılmıyor ve gelecekte yayımlanacak bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-210">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="9ac3c-211">Önerilen Cookie.Domain alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-211">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="9ac3c-212">CookieName</span><span class="sxs-lookup"><span data-stu-id="9ac3c-212">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="9ac3c-213">Tanımlama bilgisinin adı.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-213">The name of the cookie.</span></span> <span data-ttu-id="9ac3c-214">Ayarlı değil, system ile başlayan bir benzersiz ad oluşturursa [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="9ac3c-214">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="9ac3c-215">Bu özellik artık kullanılmıyor ve gelecekte yayımlanacak bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-215">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="9ac3c-216">Önerilen Cookie.Name alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-216">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="9ac3c-217">CookiePath</span><span class="sxs-lookup"><span data-stu-id="9ac3c-217">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="9ac3c-218">Yolu tanımlama bilgisinde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-218">The path set on the cookie.</span></span> <span data-ttu-id="9ac3c-219">Bu özellik artık kullanılmıyor ve gelecekte yayımlanacak bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-219">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="9ac3c-220">Önerilen Cookie.Path alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-220">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="9ac3c-221">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="9ac3c-221">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="9ac3c-222">Antiforgery sistem tarafından görünümlerde antiforgery belirteçleri işlemek için kullanılan gizli form alanının adı.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-222">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="9ac3c-223">HeaderName</span><span class="sxs-lookup"><span data-stu-id="9ac3c-223">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="9ac3c-224">Antiforgery sistemi tarafından kullanılan üstbilginin adı.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-224">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="9ac3c-225">Varsa `null`, sistem yalnızca form verileri olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-225">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="9ac3c-226">İçindeki requireSSL öğesini</span><span class="sxs-lookup"><span data-stu-id="9ac3c-226">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="9ac3c-227">SSL antiforgery sistem tarafından gerekli olup olmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-227">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="9ac3c-228">Varsa `true`, SSL olmayan istekler başarısız.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-228">If `true`, non-SSL requests fail.</span></span> <span data-ttu-id="9ac3c-229">Varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-229">Defaults to `false`.</span></span> <span data-ttu-id="9ac3c-230">Bu özellik artık kullanılmıyor ve gelecekte yayımlanacak bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-230">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="9ac3c-231">Önerilen alternatif Cookie.SecurePolicy ayarlamaktır.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-231">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="9ac3c-232">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="9ac3c-232">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="9ac3c-233">Nesil engellenip engellenmeyeceğini belirtir `X-Frame-Options` başlığı.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-233">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="9ac3c-234">Varsayılan olarak, başlığı "SAMEORIGIN" bir değerle oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-234">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="9ac3c-235">Varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-235">Defaults to `false`.</span></span> |

<span data-ttu-id="9ac3c-236">Daha fazla bilgi için [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="9ac3c-236">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="9ac3c-237">İle IAntiforgery antiforgery özellikleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9ac3c-237">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="9ac3c-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) antiforgery özellikleri yapılandırmak için API sağlar.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="9ac3c-239">`IAntiforgery` içinde istenen `Configure` yöntemi `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-239">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="9ac3c-240">Aşağıdaki örnek uygulamanın giriş sayfasından ara yazılım antiforgery bir belirteç oluşturun ve yanıt olarak bir tanımlama bilgisi (Bu konunun ilerleyen bölümlerinde açıklanan varsayılan Angular adlandırma kuralını kullanarak) olarak göndermek için kullanır:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-240">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

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

### <a name="require-antiforgery-validation"></a><span data-ttu-id="9ac3c-241">Antiforgery doğrulama gerektir</span><span class="sxs-lookup"><span data-stu-id="9ac3c-241">Require antiforgery validation</span></span>

<span data-ttu-id="9ac3c-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) tek bir eylem, bir denetleyici uygulanabilir bir eylem filtresi veya genel olarak.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="9ac3c-243">Geçerli bir antiforgery belirteç isteği eklemediği sürece bu filtre uygulanmış olan eylemler için yapılan istekleri engellenir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-243">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

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

<span data-ttu-id="9ac3c-244">`ValidateAntiForgeryToken` Özniteliği, bu düzenler, HTTP GET istekleri dahil olmak üzere istekleri eylem yöntemleri için bir belirteç gerektirir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-244">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="9ac3c-245">Varsa `ValidateAntiForgeryToken` özniteliği, uygulamanın denetleyicilerinde uygulandığında, ile kılınabilir `IgnoreAntiforgeryToken` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-245">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="9ac3c-246">ASP.NET Core antiforgery belirtecinin GET istekleri için otomatik olarak eklenmesini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-246">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="9ac3c-247">Yalnızca güvenli olmayan HTTP yöntemleri için antiforgery belirteçleri otomatik olarak doğrulama</span><span class="sxs-lookup"><span data-stu-id="9ac3c-247">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="9ac3c-248">ASP.NET Core uygulamaları için Güvenli HTTP yöntemleri (GET, HEAD, SEÇENEKLERİNİ ve izleme) antiforgery belirteçleri oluşturma.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-248">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="9ac3c-249">Geniş çapta uygulamak yerine `ValidateAntiForgeryToken` özniteliği ve ile geçersiz kılma `IgnoreAntiforgeryToken` öznitelikleri [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) özniteliği kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-249">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="9ac3c-250">Bu öznitelik için aynı şekilde çalışır `ValidateAntiForgeryToken` aşağıdaki HTTP yöntemleri kullanılarak yapılan istekler için belirteçleri gerektirmeyen dışında öznitelik:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-250">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="9ac3c-251">AL</span><span class="sxs-lookup"><span data-stu-id="9ac3c-251">GET</span></span>
* <span data-ttu-id="9ac3c-252">HEAD</span><span class="sxs-lookup"><span data-stu-id="9ac3c-252">HEAD</span></span>
* <span data-ttu-id="9ac3c-253">SEÇENEKLER</span><span class="sxs-lookup"><span data-stu-id="9ac3c-253">OPTIONS</span></span>
* <span data-ttu-id="9ac3c-254">TRACE</span><span class="sxs-lookup"><span data-stu-id="9ac3c-254">TRACE</span></span>

<span data-ttu-id="9ac3c-255">Kullanılmasını öneririz `AutoValidateAntiforgeryToken` problem API olmayan senaryolar için.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-255">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="9ac3c-256">Bu işlem sonrası eylemler varsayılan olarak korumalıdır sağlar.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-256">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="9ac3c-257">Sürece varsayılan olarak, antiforgery belirteçleri yoksaymak için alternatif olan `ValidateAntiForgeryToken` tek tek eylem yöntemlerine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-257">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="9ac3c-258">Bu büyük olasılıkla kalma sonrası eylem yöntemi için bu senaryoda yanlışlıkla uygulama CSRF saldırılarına karşı savunmasız bırakır korumasız.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-258">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="9ac3c-259">Tüm gönderileri antiforgery belirteci göndermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-259">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="9ac3c-260">API'leri tanımlama bilgisi olmayan belirtecinin bir parçası olarak göndermek için otomatik bir mekanizma yoktur.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-260">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="9ac3c-261">Uygulama, büyük olasılıkla istemci kodu uygulamasının bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-261">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="9ac3c-262">Bazı örnekler aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-262">Some examples are shown below:</span></span>

<span data-ttu-id="9ac3c-263">Örnek sınıf düzeyi:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-263">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="9ac3c-264">Genel örnek:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-264">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="9ac3c-265">Geçersiz kılma genel veya denetleyici antiforgery öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="9ac3c-265">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="9ac3c-266">[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtre, belirli bir eylem (veya denetleyici) için bir antiforgery belirteci gereksinimini ortadan kaldırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-266">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="9ac3c-267">Uygulandığında, bu filtresi geçersiz kılmaları `ValidateAntiForgeryToken` ve `AutoValidateAntiforgeryToken` (genel olarak veya bir denetleyicisinde) daha yüksek bir düzeyde belirtilen filtreler.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-267">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

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

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="9ac3c-268">Belirteçlerin kimlik doğrulamasından sonra Yenile</span><span class="sxs-lookup"><span data-stu-id="9ac3c-268">Refresh tokens after authentication</span></span>

<span data-ttu-id="9ac3c-269">Kullanıcı, kullanıcı bir görünüm veya Razor sayfaları sayfasına yönlendirerek doğrulandıktan sonra belirteçleri yenilenmesi.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-269">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="9ac3c-270">JavaScript ve AJAX Spa'lar</span><span class="sxs-lookup"><span data-stu-id="9ac3c-270">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="9ac3c-271">Geleneksel HTML tabanlı uygulamalarda antiforgery belirteçleri gizli form alanları kullanarak sunucuya geçirilir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-271">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="9ac3c-272">JavaScript tabanlı modern uygulamalar ve Spa'lar, çok sayıda istek program aracılığıyla yapılır.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-272">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="9ac3c-273">Bu AJAX istekleri (veya istek üst bilgilerini tanımlama bilgileri gibi) başka teknikler belirteç göndermek için kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-273">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="9ac3c-274">Tanımlama bilgileri kimlik doğrulama belirteçlerinizi depolamak ve sunucuda API isteklerinin kimliğini doğrulamak için kullanılıyorsa, CSRF olası bir sorundur.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-274">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="9ac3c-275">Yerel depolama belirteç depolamak için kullanılır, çünkü her istek ile sunucu için yerel depolama alanından değerleri otomatik olarak gönderilmez CSRF güvenlik açığı azaltılması gereken.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-275">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="9ac3c-276">Bu nedenle, istemci ve istek üst bilgisi önerilen bir yaklaşım olarak belirteç gönderme antiforgery belirteç saklamak için yerel depolama ile.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-276">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="9ac3c-277">JavaScript</span><span class="sxs-lookup"><span data-stu-id="9ac3c-277">JavaScript</span></span>

<span data-ttu-id="9ac3c-278">Görünümler ile JavaScript kullanarak belirteç hizmeti aracılığıyla görünümü içinde oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-278">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="9ac3c-279">Ekleme [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) görüntüleyebileceği ve çağırabileceği uygulamasına service [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="9ac3c-279">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="9ac3c-280">Bu yaklaşım, doğrudan sunucudan tanımlama bilgilerini ayarlama ya da istemciden okumadan altyapıdaki gereğini ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-280">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="9ac3c-281">Yukarıdaki örnekte AJAX posta üst bilgisi için gizli bir alan değerini okumak için JavaScript kullanır.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-281">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="9ac3c-282">JavaScript, ayrıca tanımlama bilgilerini belirteçlerinde erişebilir ve tanımlama bilgisinin içeriği belirtecin değeri ile bir başlığı oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-282">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="9ac3c-283">Betik varsayılarak istekleri belirteç adı verilen bir üst bilgisinde göndermek için `X-CSRF-TOKEN`, aranacak antiforgery hizmetini yapılandırma `X-CSRF-TOKEN` üst bilgi:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-283">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="9ac3c-284">Aşağıdaki örnek, uygun üst bilgisiyle bir AJAX isteği yapmak için JavaScript kullanır:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-284">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

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

### <a name="angularjs"></a><span data-ttu-id="9ac3c-285">AngularJS</span><span class="sxs-lookup"><span data-stu-id="9ac3c-285">AngularJS</span></span>

<span data-ttu-id="9ac3c-286">AngularJS adresine CSRF kuralını kullanır.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-286">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="9ac3c-287">Sunucu adı ile bir tanımlama bilgisi gönderirse `XSRF-TOKEN`, AngularJS `$http` hizmeti sunucusuna bir istek gönderdiğinde bu tanımlama bilgisi değeri için bir başlık ekler.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-287">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="9ac3c-288">Bu işlemi otomatik olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-288">This process is automatic.</span></span> <span data-ttu-id="9ac3c-289">Üst bilgi, açıkça ayarlanmış olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-289">The header doesn't need to be set explicitly.</span></span> <span data-ttu-id="9ac3c-290">Üst bilgi adı: `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-290">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="9ac3c-291">Sunucu, bu başlığı algılamak ve içeriğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-291">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="9ac3c-292">Bu kural ile ASP.NET Core API'si işler:</span><span class="sxs-lookup"><span data-stu-id="9ac3c-292">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="9ac3c-293">Bir belirteç olarak adlandırılan bir tanımlama bilgisinde sağlamak için uygulamanızı yapılandırma `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-293">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="9ac3c-294">Adlı bir üst bilgisini antiforgery hizmetinin yapılandırma `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-294">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="9ac3c-295">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9ac3c-295">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="9ac3c-296">Antiforgery genişletme</span><span class="sxs-lookup"><span data-stu-id="9ac3c-296">Extend antiforgery</span></span>

<span data-ttu-id="9ac3c-297">[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) türü tarafından gidiş dönüşü ek veriler her belirteç, CSRF önleme sisteminin davranışını genişletmek geliştiricilerin kullanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-297">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="9ac3c-298">[GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) yöntemi her çağrıldığında bir alan belirteç oluşturulur ve dönüş değeri içinde oluşturulan belirtecin katıştırılır.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-298">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="9ac3c-299">Bir uygulayan bir zaman damgası, nonce veya başka bir değer döndürür ve sonra çağrı [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) belirteç doğrulandığında, bu verileri doğrulamak için.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-299">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="9ac3c-300">Bu bilgiyi içer gerekmez. Bu nedenle istemcinin kullanıcı adını oluşturulan belirteçlere zaten eklenir.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-300">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="9ac3c-301">Bir belirteç yok, ancak ek veriler içeriyorsa `IAntiForgeryAdditionalDataProvider` olan yapılandırılmış, ek veriler doğrulanmış değil.</span><span class="sxs-lookup"><span data-stu-id="9ac3c-301">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9ac3c-302">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9ac3c-302">Additional resources</span></span>

* <span data-ttu-id="9ac3c-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) üzerinde [Web uygulaması güvenlik projesi açın](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="9ac3c-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
* <xref:host-and-deploy/web-farm>
