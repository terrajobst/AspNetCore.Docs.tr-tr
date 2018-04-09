---
title: ASP.NET Core çıkış noktaları arası istekleri (CORS) etkinleştir
author: rick-anderson
description: Bilgi nasıl CORS izin verme veya reddetme bir ASP.NET Core uygulamasında cross-origin istekleri için bir standart olarak.
manager: wpickett
ms.author: riande
ms.date: 05/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cors
ms.openlocfilehash: 3c5d0840426c7ed52353a7832a1a1959027121de
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="f5e6d-103">ASP.NET Core çıkış noktaları arası istekleri (CORS) etkinleştir</span><span class="sxs-lookup"><span data-stu-id="f5e6d-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="f5e6d-104">Tarafından [CAN Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), ve [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f5e6d-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f5e6d-105">Tarayıcı güvenlik bir web sayfası AJAX istekleri başka bir etki alanına yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="f5e6d-106">Bu kısıtlama adlı *kaynak aynı ilke*ve kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önler.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="f5e6d-107">Ancak, bazen web API'nize çıkış noktaları arası isteklerde diğer sitelere izin vermek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="f5e6d-108">[Kaynak kaynak paylaşma arası](http://www.w3.org/TR/cors/) (CORS) olan W3C standart bir kaynak aynı ilke hafifletin sunucusunun sağlar.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="f5e6d-109">CORS kullanarak, bir sunucu açıkça bazı cross-origin istekleri başkalarının reddetme çalışırken izin verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="f5e6d-110">CORS daha güvenli ve önceki teknikleri daha esnek gibi [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="f5e6d-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="f5e6d-111">Bu konuda, bir ASP.NET Core uygulamada CORS'yi etkinleştirmeniz gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="f5e6d-112">"Aynı kaynak" nedir?</span><span class="sxs-lookup"><span data-stu-id="f5e6d-112">What is "same origin"?</span></span>

<span data-ttu-id="f5e6d-113">Aynı düzenleri, konakları ve bağlantı noktaları varsa iki URL'leri aynı kaynağa sahip.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="f5e6d-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="f5e6d-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="f5e6d-115">Bu iki URL'leri aynı kaynağa sahip:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="f5e6d-116">Bu URL'leri iki farklı çıkış önceki daha vardır:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="f5e6d-117">`http://example.net` -Farklı bir etki alanı</span><span class="sxs-lookup"><span data-stu-id="f5e6d-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="f5e6d-118">`http://www.example.com/foo.html` -Farklı bir alt etki alanı</span><span class="sxs-lookup"><span data-stu-id="f5e6d-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="f5e6d-119">`https://example.com/foo.html` -Farklı düzeni</span><span class="sxs-lookup"><span data-stu-id="f5e6d-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="f5e6d-120">`http://example.com:9000/foo.html` -Farklı bir bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="f5e6d-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="f5e6d-121">Internet Explorer bağlantı noktası kaynakları karşılaştırma göz önünde değil.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="setting-up-cors"></a><span data-ttu-id="f5e6d-122">CORS ayarlama</span><span class="sxs-lookup"><span data-stu-id="f5e6d-122">Setting up CORS</span></span>

<span data-ttu-id="f5e6d-123">Ayarlamak için uygulamanız için CORS eklemek `Microsoft.AspNetCore.Cors` projenize paket.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

<span data-ttu-id="f5e6d-124">CORS Hizmetleri içinde haline ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-124">Add the CORS services in Startup.cs:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="f5e6d-125">CORS Ara yazılımla etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f5e6d-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="f5e6d-126">Etkinleştirmek için tüm uygulamanız için CORS CORS ara yazılım, istek ardışık düzen kullanarak eklemeniz `UseCors` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-126">To enable CORS for your entire application add the CORS middleware to your request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="f5e6d-127">Not CORS Ara (örn. cross-origin istekleri desteklemek istediğiniz uygulamanızda tanımlanmış uç nokta gelmelidir</span><span class="sxs-lookup"><span data-stu-id="f5e6d-127">Note that the CORS middleware must precede any defined endpoints in your app that you want to support cross-origin requests (ex.</span></span> <span data-ttu-id="f5e6d-128">Tüm çağrıdan önce `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="f5e6d-128">before any call to `UseMvc`).</span></span>

<span data-ttu-id="f5e6d-129">CORS ara yazılımı kullanarak eklerken bir çıkış noktaları arası ilkesi belirtebilirsiniz `CorsPolicyBuilder` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-129">You can specify a cross-origin policy when adding the CORS middleware using the `CorsPolicyBuilder` class.</span></span> <span data-ttu-id="f5e6d-130">Bunu yapmanın iki yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-130">There are two ways to do this.</span></span> <span data-ttu-id="f5e6d-131">İlk UseCors ile bir lambda çağırmaktır:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-131">The first is to call UseCors with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="f5e6d-132">**Not:** URL eğik belirtilmelidir (`/`).</span><span class="sxs-lookup"><span data-stu-id="f5e6d-132">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="f5e6d-133">URL ile ererse `/`, karşılaştırma döndürülecek `false` ve üst bilgi döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-133">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="f5e6d-134">Lambda geçen bir `CorsPolicyBuilder` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-134">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="f5e6d-135">Bir listesini bulabilirsiniz [yapılandırma seçenekleri](#cors-policy-options) bu konuda daha sonra.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-135">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="f5e6d-136">Bu örnekte, ilke gelen çıkış noktaları arası isteklere izin verir. `http://example.com` ve başka bir kaynak yok.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-136">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="f5e6d-137">Yöntem çağrıları zincir şekilde CorsPolicyBuilder fluent API olduğuna dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-137">Note that CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="f5e6d-138">İkinci bir veya daha fazla adlandırılmış CORS ilkelerini tanımlamak ve ardından ilkeyi çalışma zamanında adına göre seçmek için bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-138">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="f5e6d-139">Bu örnekte "AllowSpecificOrigin" adlı bir CORS ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-139">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="f5e6d-140">İlkeyi seçmek için adına geçirmek `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-140">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="f5e6d-141">MVC CORS'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f5e6d-141">Enabling CORS in MVC</span></span>

<span data-ttu-id="f5e6d-142">MVC, eylem, denetleyici başına veya genel olarak tüm denetleyicileri için başına belirli CORS uygulamak için alternatif olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-142">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="f5e6d-143">MVC CORS'yi etkinleştirmeniz kullanırken aynı CORS Hizmetleri kullanılır, ancak CORS Ara değil.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-143">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="f5e6d-144">Her eylem</span><span class="sxs-lookup"><span data-stu-id="f5e6d-144">Per action</span></span>

<span data-ttu-id="f5e6d-145">Belirtmek için belirli bir eylemi için CORS ilkesinin ekleyin `[EnableCors]` özniteliği eylem.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-145">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="f5e6d-146">İlke adı belirtin.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-146">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="f5e6d-147">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="f5e6d-147">Per controller</span></span>

<span data-ttu-id="f5e6d-148">Belirtmek için belirli bir denetleyicinin CORS ilkesi ekleyin `[EnableCors]` özniteliği için denetleyici sınıfı.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-148">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="f5e6d-149">İlke adı belirtin.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-149">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="f5e6d-150">Genel</span><span class="sxs-lookup"><span data-stu-id="f5e6d-150">Globally</span></span>

<span data-ttu-id="f5e6d-151">CORS genel olarak tüm denetleyicilerinin ekleyerek etkinleştirebilirsiniz `CorsAuthorizationFilterFactory` genel filtre koleksiyonuna filtre:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-151">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="f5e6d-152">Öncelik sırası: eylem, denetleyici, genel.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-152">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="f5e6d-153">Eylem düzeyi ilkeleri denetleyicisi düzeyi ilkeleri göre önceliklidir ve denetleyici düzeyinde ilkeleri genel ilkelere göre önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-153">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="f5e6d-154">CORS devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="f5e6d-154">Disable CORS</span></span>

<span data-ttu-id="f5e6d-155">Denetleyici veya eylem için CORS devre dışı bırakmak için `[DisableCors]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-155">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="f5e6d-156">CORS ilkesi seçenekleri</span><span class="sxs-lookup"><span data-stu-id="f5e6d-156">CORS policy options</span></span>

<span data-ttu-id="f5e6d-157">Bu bölümde CORS ilke ayarlayabileceğiniz çeşitli seçenekler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-157">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="f5e6d-158">İzin verilen çıkış noktası ayarlama</span><span class="sxs-lookup"><span data-stu-id="f5e6d-158">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="f5e6d-159">İzin verilen HTTP yöntemleri Ayarla</span><span class="sxs-lookup"><span data-stu-id="f5e6d-159">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="f5e6d-160">İzin verilen istek üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="f5e6d-160">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="f5e6d-161">Sunulan yanıt üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="f5e6d-161">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="f5e6d-162">Cross-origin istekleri kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="f5e6d-162">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="f5e6d-163">Denetim öncesi sona erme süresini ayarlama</span><span class="sxs-lookup"><span data-stu-id="f5e6d-163">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="f5e6d-164">İçin bazı seçenekler okumak yararlı olabilir [nasıl CORS çalışır](#how-cors-works) ilk.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-164">For some options it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="f5e6d-165">İzin verilen çıkış noktası ayarlama</span><span class="sxs-lookup"><span data-stu-id="f5e6d-165">Set the allowed origins</span></span>

<span data-ttu-id="f5e6d-166">Bir veya daha fazla belirli kaynaklara izin veren:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-166">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="f5e6d-167">Tüm kaynaklara izin vermek için:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-167">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="f5e6d-168">Her türlü kaynağa gelen isteklere izin vermeden önce dikkatlice düşünün.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-168">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="f5e6d-169">Bu, tam anlamıyla herhangi bir Web apı'nize AJAX çağrıları yapabilirsiniz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-169">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="f5e6d-170">İzin verilen HTTP yöntemleri Ayarla</span><span class="sxs-lookup"><span data-stu-id="f5e6d-170">Set the allowed HTTP methods</span></span>

<span data-ttu-id="f5e6d-171">Tüm HTTP yöntemleri izin vermek için:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-171">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="f5e6d-172">Bu ön uçuş istekleri ve erişim-denetim-Allow-Methods üstbilgi etkiler.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-172">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="f5e6d-173">İzin verilen istek üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="f5e6d-173">Set the allowed request headers</span></span>

<span data-ttu-id="f5e6d-174">CORS denetim öncesi isteği uygulama tarafından ayarlanıp HTTP üst bilgileri listeleyen bir Access-Control-Request-Headers üstbilgisi içerebilir (sözde "yazar, istek üstbilgileri").</span><span class="sxs-lookup"><span data-stu-id="f5e6d-174">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="f5e6d-175">Beyaz liste belirli üstbilgileri:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-175">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="f5e6d-176">İzin vermek için tüm istek üstbilgileri Yazar:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-176">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="f5e6d-177">Tarayıcılar nasıl bunlar Access-Control-Request-Headers kümesinde tamamen tutarlı değil.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-177">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="f5e6d-178">Üstbilgi için herhangi bir şey dışında ayarlarsanız "\*", "kabul", "content-type" ve "kaynak" artı desteklemek istediğiniz tüm özel üstbilgileri en az içermelidir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-178">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="f5e6d-179">Sunulan yanıt üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="f5e6d-179">Set the exposed response headers</span></span>

<span data-ttu-id="f5e6d-180">Varsayılan olarak, tüm uygulama yanıt üstbilgilerinin tarayıcı açığa çıkarmıyor.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-180">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="f5e6d-181">(Bkz [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Varsayılan olarak kullanılabilir yanıt üstbilgilerini şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-181">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="f5e6d-182">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="f5e6d-182">Cache-Control</span></span>

* <span data-ttu-id="f5e6d-183">İçerik dili</span><span class="sxs-lookup"><span data-stu-id="f5e6d-183">Content-Language</span></span>

* <span data-ttu-id="f5e6d-184">İçerik Türü</span><span class="sxs-lookup"><span data-stu-id="f5e6d-184">Content-Type</span></span>

* <span data-ttu-id="f5e6d-185">Süre sonu</span><span class="sxs-lookup"><span data-stu-id="f5e6d-185">Expires</span></span>

* <span data-ttu-id="f5e6d-186">Son değiştiren</span><span class="sxs-lookup"><span data-stu-id="f5e6d-186">Last-Modified</span></span>

* <span data-ttu-id="f5e6d-187">Pragma</span><span class="sxs-lookup"><span data-stu-id="f5e6d-187">Pragma</span></span>

<span data-ttu-id="f5e6d-188">CORS spec bunlar çağırır *basit yanıt üstbilgilerini*.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-188">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="f5e6d-189">Diğer üstbilgileri uygulama kullanılabilir hale getirmek için:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-189">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="f5e6d-190">Cross-origin istekleri kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="f5e6d-190">Credentials in cross-origin requests</span></span>

<span data-ttu-id="f5e6d-191">Kimlik bilgileri bir CORS istek özel işleme gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-191">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="f5e6d-192">Varsayılan olarak, tarayıcı çıkış noktaları arası istek ile herhangi bir kimlik bilgisi göndermez.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-192">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="f5e6d-193">Kimlik bilgileri, tanımlama bilgilerinin yanı sıra HTTP kimlik doğrulama şemasını içerir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-193">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="f5e6d-194">Kimlik bilgileri ile bir çıkış noktaları arası istek göndermek için istemci XMLHttpRequest.withCredentials true olarak ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-194">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="f5e6d-195">XMLHttpRequest doğrudan kullanma:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-195">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="f5e6d-196">JQuery içinde:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-196">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="f5e6d-197">Ayrıca, sunucu kimlik bilgilerine izin vermeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-197">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="f5e6d-198">Çıkış noktaları arası kimlik bilgilerine izin vermek için:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-198">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="f5e6d-199">Artık HTTP yanıtı sunucunun bir çıkış noktaları arası istek için kimlik bilgilerini sağlayan tarayıcı söyler bir erişim-denetim-Allow-Credentials üstbilgisi içerecektir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-199">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="f5e6d-200">Kimlik bilgilerini tarayıcı gönderir, ancak yanıt geçerli erişim-denetim-Allow-Credentials üst bilgi içermeyen, tarayıcı uygulaması yanıtı kullanıma olmaz ve AJAX isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-200">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="f5e6d-201">Çıkış noktaları arası kimlik bilgilerine izin verirken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-201">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="f5e6d-202">Başka bir etki alanındaki bir Web sitesi uygulama kullanıcının adına kullanıcının bilgisi olmadan oturum açmış kullanıcının kimlik bilgileri gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-202">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="f5e6d-203">CORS belirtimi de bu ayar durumları kaynakları için "\*" (tüm kaynaklara) geçersiz durumunda `Access-Control-Allow-Credentials` başlığıdır mevcut.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-203">The CORS specification also states that setting origins to "\*" (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="f5e6d-204">Denetim öncesi sona erme süresini ayarlama</span><span class="sxs-lookup"><span data-stu-id="f5e6d-204">Set the preflight expiration time</span></span>

<span data-ttu-id="f5e6d-205">Ne kadar süreyle denetim öncesi isteğinin yanıtı önbelleğe erişim-denetim-Max-Age üstbilgisini belirtir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-205">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="f5e6d-206">Bu üst ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-206">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="f5e6d-207">CORS nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="f5e6d-207">How CORS works</span></span>

<span data-ttu-id="f5e6d-208">Bu bölümde, bir CORS İstek HTTP iletileri düzeyde neler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-208">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="f5e6d-209">Böylece CORS İlkesi doğru şekilde yapılandırılabilir CORS nasıl çalıştığını ve troubleshooted beklenmeyen davranışları ortaya çıktığında anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-209">It's important to understand how CORS works so that the CORS policy can be configured correctly and troubleshooted when unexpected behaviors occur.</span></span>

<span data-ttu-id="f5e6d-210">CORS belirtimi çıkış noktaları arası istekleri etkinleştirme birkaç yeni HTTP üstbilgileri tanıtır.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-210">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="f5e6d-211">Bir tarayıcı CORS destekliyorsa, bu üstbilgileri cross-origin istekleri için otomatik olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-211">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="f5e6d-212">Özel JavaScript kodu CORS etkinleştirmek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-212">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="f5e6d-213">Çıkış noktaları arası istek bir örneği burada verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-213">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="f5e6d-214">`Origin` Üstbilgisi isteği yapan sitenin etki alanını sağlar:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-214">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="f5e6d-215">Sunucu isteği izin veriyorsa, yanıtta Access-Control-Allow-Origin üstbilgisini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-215">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="f5e6d-216">Bu üstbilgi değerini istek kaynağı başlığından eşleşen veya joker karakter değeri "\*", yani her türlü kaynağa izin verilir:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-216">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="f5e6d-217">Access-Control-Allow-Origin üstbilgisi yanıt içermiyorsa, AJAX isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-217">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="f5e6d-218">Özellikle, tarayıcı istek izin vermez.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-218">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="f5e6d-219">Sunucu başarılı bir yanıt döndürürse bile, tarayıcı yanıtı istemci uygulamasının kullanımına yapmaz.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-219">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="f5e6d-220">Denetim öncesi isteği</span><span class="sxs-lookup"><span data-stu-id="f5e6d-220">Preflight Requests</span></span>

<span data-ttu-id="f5e6d-221">Bazı CORS istekler için tarayıcı kaynak gerçek isteği göndermeden önce bir "Denetim öncesi isteği" adlı bir ek istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-221">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="f5e6d-222">Aşağıdaki koşullar doğruysa, tarayıcı denetim öncesi isteği atlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-222">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="f5e6d-223">İstek yöntemini GET, HEAD veya POST, olduğundan ve</span><span class="sxs-lookup"><span data-stu-id="f5e6d-223">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="f5e6d-224">Uygulama kabul etme, Accept-Language, Content-Language, dışındaki tüm istek üstbilgileri ayarlamaz Content-Type veya son-olay-ID ve</span><span class="sxs-lookup"><span data-stu-id="f5e6d-224">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="f5e6d-225">Content-Type üstbilgisi (varsa ayarlayın) aşağıdakilerden biri:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-225">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="f5e6d-226">Uygulama/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="f5e6d-226">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="f5e6d-227">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="f5e6d-227">multipart/form-data</span></span>

  * <span data-ttu-id="f5e6d-228">Metin/düz</span><span class="sxs-lookup"><span data-stu-id="f5e6d-228">text/plain</span></span>

<span data-ttu-id="f5e6d-229">İstek üstbilgileri hakkında kural XMLHttpRequest nesnesinde setRequestHeader çağırarak uygulama ayarlar üstbilgileri uygulanır.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-229">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="f5e6d-230">(CORS belirtimi bu "Yazar istek üstbilgileri" çağırır.) Kural User-Agent, ana bilgisayar veya Content-Length gibi tarayıcı ayarlayabilirsiniz üstbilgileri uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-230">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="f5e6d-231">Bir denetim öncesi isteği bir örneği burada verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-231">Here is an example of a preflight request:</span></span>

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="f5e6d-232">Ön uçuş isteği HTTP OPTIONS yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-232">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="f5e6d-233">İki özel üstbilgi içerir:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-233">It includes two special headers:</span></span>

* <span data-ttu-id="f5e6d-234">Access-Control-Request-Method: fiili istek için kullanılacak HTTP yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-234">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="f5e6d-235">Access-Control-Request-Headers: Uygulama gerçek isteği ayarlama istek üstbilgileri listesi.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-235">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="f5e6d-236">(Yeniden, bu tarayıcı ayarlar üstbilgileri içermez.)</span><span class="sxs-lookup"><span data-stu-id="f5e6d-236">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="f5e6d-237">Sunucu isteği izin verdiğini varsayarak bir örnek yanıt şöyledir:</span><span class="sxs-lookup"><span data-stu-id="f5e6d-237">Here is an example response, assuming that the server allows the request:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="f5e6d-238">Yanıt, izin verilen yöntemleri listeleyen bir erişim-denetim-Allow-Methods üstbilgisi ve isteğe bağlı olarak izin verilen üstbilgileri listeler bir Access-Control-izin ver-Headers üstbilgisi içeriyor.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-238">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="f5e6d-239">Denetim öncesi isteği başarılı olursa, tarayıcı daha önce açıklandığı gibi gerçek isteğini gönderir.</span><span class="sxs-lookup"><span data-stu-id="f5e6d-239">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
