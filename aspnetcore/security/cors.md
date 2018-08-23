---
title: ASP.NET core'da çıkış noktaları arası istekleri (CORS) etkinleştirme
author: rick-anderson
description: Bilgi nasıl CORS izin verme veya reddetme ASP.NET Core uygulaması çıkış noktaları arası istekleri için standart olarak.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "41757312"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="174af-103">ASP.NET core'da çıkış noktaları arası istekleri (CORS) etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="174af-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="174af-104">Tarafından [Mike Wasson](https://github.com/mikewasson), [Shayne boyer'ın](https://twitter.com/spboyer), ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="174af-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="174af-105">Tarayıcı Güvenliği, bir web sayfası, başka bir etki alanına AJAX istekleri yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="174af-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="174af-106">Bu kısıtlama adlı *aynı çıkış noktası İlkesi*ve kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önler.</span><span class="sxs-lookup"><span data-stu-id="174af-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="174af-107">Ancak, bazen, web API'nize çıkış noktaları arası isteklerde diğer sitelere izin vermek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="174af-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="174af-108">[Kaynağın kaynak paylaşımını çapraz](http://www.w3.org/TR/cors/) (CORS) olan gevşek bir aynı çıkış noktası ilkesi izin veren bir W3C standart.</span><span class="sxs-lookup"><span data-stu-id="174af-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="174af-109">CORS kullanarak, bir sunucu açıkça bazı çıkış noktaları arası istekleri izin verirken diğerlerini.</span><span class="sxs-lookup"><span data-stu-id="174af-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="174af-110">CORS güvenli ve önceki teknikler daha esnek gibi [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="174af-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="174af-111">Bu konuda, bir ASP.NET Core uygulamada CORS'yi etkinleştirme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="174af-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="174af-112">"Aynı kaynak" nedir?</span><span class="sxs-lookup"><span data-stu-id="174af-112">What is "same origin"?</span></span>

<span data-ttu-id="174af-113">Aynı düzeni, konaklar ve bağlantı noktaları varsa iki URL aynı kaynağa sahip.</span><span class="sxs-lookup"><span data-stu-id="174af-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="174af-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="174af-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="174af-115">Bu iki URL aynı kaynağa sahip:</span><span class="sxs-lookup"><span data-stu-id="174af-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="174af-116">Bu URL'ler önceki değerinden farklı kaynakları iki vardır:</span><span class="sxs-lookup"><span data-stu-id="174af-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="174af-117">`http://example.net` -Farklı bir etki alanı</span><span class="sxs-lookup"><span data-stu-id="174af-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="174af-118">`http://www.example.com/foo.html` -Farklı bir alt etki alanı</span><span class="sxs-lookup"><span data-stu-id="174af-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="174af-119">`https://example.com/foo.html` -Farklı düzeni</span><span class="sxs-lookup"><span data-stu-id="174af-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="174af-120">`http://example.com:9000/foo.html` -Farklı bir bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="174af-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="174af-121">Internet Explorer kaynakları karşılaştırılırken, bağlantı noktası dikkate almaz.</span><span class="sxs-lookup"><span data-stu-id="174af-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="enable-cors"></a><span data-ttu-id="174af-122">CORS'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="174af-122">Enable CORS</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="174af-123">Ayarlamak için uygulamanız için CORS ekleme `Microsoft.AspNetCore.Cors` paketini projenize.</span><span class="sxs-lookup"><span data-stu-id="174af-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

::: moniker-end

<span data-ttu-id="174af-124">Çağrı [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="174af-124">Call [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="174af-125">CORS ile Ara yazılım etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="174af-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="174af-126">CORS etkinleştirmek için CORS ara yazılım istek kullanarak işlem hattı ekleyin `UseCors` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="174af-126">To enable CORS, add the CORS middleware to the request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="174af-127">CORS ara yazılım tanımlı uç nokta uygulamanızda çıkış noktaları arası istekleri desteklemek istediğiniz gelmelidir (örneğin, önce yapılan tüm çağrıların `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="174af-127">The CORS middleware must precede any defined endpoints in your app where you want to support cross-origin requests (For example, before any call to `UseMvc`).</span></span>

<span data-ttu-id="174af-128">Çıkış noktaları arası ilke CORS ara yazılımı kullanarak eklerken belirtilebilir [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="174af-128">A cross-origin policy can be specified when adding the CORS middleware using the [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) class.</span></span> <span data-ttu-id="174af-129">Bunu yapmanın iki yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="174af-129">There are two ways to do this.</span></span> <span data-ttu-id="174af-130">İlk çağırmaktır `UseCors` bir lambda ile:</span><span class="sxs-lookup"><span data-stu-id="174af-130">The first is to call `UseCors` with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="174af-131">**Not:** URL'nin sonunda bir eğik çizgi belirtilmelidir (`/`).</span><span class="sxs-lookup"><span data-stu-id="174af-131">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="174af-132">URL ile sona ererse `/`, karşılaştırma döndüreceği `false` ve üst bilgi döndürülür.</span><span class="sxs-lookup"><span data-stu-id="174af-132">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="174af-133">Lambda alan bir `CorsPolicyBuilder` nesne.</span><span class="sxs-lookup"><span data-stu-id="174af-133">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="174af-134">Bir listesi bulacaksınız [yapılandırma seçenekleri](#cors-policy-options) bu konuda.</span><span class="sxs-lookup"><span data-stu-id="174af-134">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="174af-135">Bu örnekte, çıkış noktaları arası istekleri ilkeyi sağlayan `http://example.com` ve diğer kaynak.</span><span class="sxs-lookup"><span data-stu-id="174af-135">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="174af-136">Yöntem çağrıları zincirleyebilirsiniz şekilde CorsPolicyBuilder fluent API'sini sahiptir:</span><span class="sxs-lookup"><span data-stu-id="174af-136">CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="174af-137">İkinci bir veya daha fazla adlandırılmış CORS ilkelerini tanımlama ve ardından ilkeyi çalışma zamanında adına göre seçmek için bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="174af-137">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="174af-138">Bu örnek, "AllowSpecificOrigin" adlı CORS ilkesi ekler.</span><span class="sxs-lookup"><span data-stu-id="174af-138">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="174af-139">İlkeyi seçmek için adına geçirmek `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="174af-139">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="174af-140">Mvc'de CORS'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="174af-140">Enabling CORS in MVC</span></span>

<span data-ttu-id="174af-141">Alternatif olarak, MVC her eylem, denetleyici başına veya genel olarak tüm denetleyicileri için belirli CORS uygulamak için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="174af-141">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="174af-142">MVC, CORS öğesini etkinleştirmek üzere kullanırken aynı CORS Hizmetleri kullanılır ancak CORS ara yazılımı değil.</span><span class="sxs-lookup"><span data-stu-id="174af-142">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="174af-143">Eylem başına</span><span class="sxs-lookup"><span data-stu-id="174af-143">Per action</span></span>

<span data-ttu-id="174af-144">Belirtmek için özel bir eylem için CORS ilkesinin ekleyin `[EnableCors]` eyleme özniteliği.</span><span class="sxs-lookup"><span data-stu-id="174af-144">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="174af-145">İlke adı belirtin.</span><span class="sxs-lookup"><span data-stu-id="174af-145">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="174af-146">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="174af-146">Per controller</span></span>

<span data-ttu-id="174af-147">Belirtmek için belirli bir denetleyicinin CORS ilkesi ekleme `[EnableCors]` özniteliği için denetleyici sınıfı.</span><span class="sxs-lookup"><span data-stu-id="174af-147">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="174af-148">İlke adı belirtin.</span><span class="sxs-lookup"><span data-stu-id="174af-148">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="174af-149">Genel olarak</span><span class="sxs-lookup"><span data-stu-id="174af-149">Globally</span></span>

<span data-ttu-id="174af-150">CORS genel olarak tüm denetleyicilerinin ekleyerek etkinleştirebilirsiniz `CorsAuthorizationFilterFactory` genel filtre koleksiyonuna filtre:</span><span class="sxs-lookup"><span data-stu-id="174af-150">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="174af-151">Öncelik sırası: eylem, denetleyici, genel.</span><span class="sxs-lookup"><span data-stu-id="174af-151">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="174af-152">Eylem düzeyinde ilkeler denetleyici düzeyinde ilkeler üzerinde önceliklidir ve denetleyici düzeyinde ilkeler genel ilkelere göre önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="174af-152">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="174af-153">CORS devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="174af-153">Disable CORS</span></span>

<span data-ttu-id="174af-154">CORS bir denetleyici veya eylem için devre dışı bırakmak için `[DisableCors]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="174af-154">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="174af-155">CORS ilkesi seçenekleri</span><span class="sxs-lookup"><span data-stu-id="174af-155">CORS policy options</span></span>

<span data-ttu-id="174af-156">Bu bölümde, CORS ilke ayarlayabileceğiniz çeşitli seçenekler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="174af-156">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="174af-157">İzin verilen çıkış noktaları ayarlama</span><span class="sxs-lookup"><span data-stu-id="174af-157">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="174af-158">İzin verilen HTTP yöntemleri Ayarla</span><span class="sxs-lookup"><span data-stu-id="174af-158">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="174af-159">İzin verilen istek üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="174af-159">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="174af-160">İfşa edilen yanıt üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="174af-160">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="174af-161">Çıkış noktaları arası istekleri kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="174af-161">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="174af-162">Denetim öncesi sona erme saati ayarla</span><span class="sxs-lookup"><span data-stu-id="174af-162">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="174af-163">Bazı seçenekleri okumak yardımcı olabilecek [CORS nasıl çalıştığını](#how-cors-works) ilk.</span><span class="sxs-lookup"><span data-stu-id="174af-163">For some options, it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="174af-164">İzin verilen çıkış noktaları ayarlama</span><span class="sxs-lookup"><span data-stu-id="174af-164">Set the allowed origins</span></span>

<span data-ttu-id="174af-165">Bir veya daha fazla belirli kaynaklara izin veren:</span><span class="sxs-lookup"><span data-stu-id="174af-165">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="174af-166">Tüm kaynaklara izin veren:</span><span class="sxs-lookup"><span data-stu-id="174af-166">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="174af-167">Her türlü kaynağa gelen isteklere izin vermeden önce dikkatlice düşünün.</span><span class="sxs-lookup"><span data-stu-id="174af-167">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="174af-168">Bu, tam anlamıyla herhangi bir Web API AJAX çağrıları yapabileceğini anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="174af-168">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="174af-169">İzin verilen HTTP yöntemleri Ayarla</span><span class="sxs-lookup"><span data-stu-id="174af-169">Set the allowed HTTP methods</span></span>

<span data-ttu-id="174af-170">Tüm HTTP yöntemleri izin vermek için:</span><span class="sxs-lookup"><span data-stu-id="174af-170">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="174af-171">Bu, uçuş öncesi istekleri ve erişim-denetim-Allow-Methods üstbilgi etkiler.</span><span class="sxs-lookup"><span data-stu-id="174af-171">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="174af-172">İzin verilen istek üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="174af-172">Set the allowed request headers</span></span>

<span data-ttu-id="174af-173">CORS denetim öncesi isteğinin HTTP üst bilgileri uygulama tarafından ayarlanıp listeleme, bir Access-Control-Request-Headers üstbilgisi içerebilir (Malum "yazar, istek üst bilgileri").</span><span class="sxs-lookup"><span data-stu-id="174af-173">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="174af-174">Beyaz liste belirli üst bilgileri için:</span><span class="sxs-lookup"><span data-stu-id="174af-174">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="174af-175">İzin vermek için tüm istek üst bilgilerini yazar:</span><span class="sxs-lookup"><span data-stu-id="174af-175">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="174af-176">Tarayıcılar nasıl bunlar Access-Control-Request-Headers kümesinde tamamen tutarlı değil.</span><span class="sxs-lookup"><span data-stu-id="174af-176">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="174af-177">Üst bilgileri için herhangi bir şey dışında ayarlarsanız "\*", "kabul", "content-type" ve "Başlangıç" yanı sıra, desteklemek istediğiniz tüm özel üst en az içermelidir.</span><span class="sxs-lookup"><span data-stu-id="174af-177">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="174af-178">İfşa edilen yanıt üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="174af-178">Set the exposed response headers</span></span>

<span data-ttu-id="174af-179">Varsayılan olarak, tüm yanıt üstbilgilerini uygulama tarayıcı ortaya çıkarmıyor.</span><span class="sxs-lookup"><span data-stu-id="174af-179">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="174af-180">(Bkz [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Varsayılan olarak kullanılabilir olan yanıt üstbilgilerini şunlardır:</span><span class="sxs-lookup"><span data-stu-id="174af-180">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="174af-181">Önbellek denetimi</span><span class="sxs-lookup"><span data-stu-id="174af-181">Cache-Control</span></span>

* <span data-ttu-id="174af-182">İçerik dili</span><span class="sxs-lookup"><span data-stu-id="174af-182">Content-Language</span></span>

* <span data-ttu-id="174af-183">İçerik Türü</span><span class="sxs-lookup"><span data-stu-id="174af-183">Content-Type</span></span>

* <span data-ttu-id="174af-184">Süre sonu</span><span class="sxs-lookup"><span data-stu-id="174af-184">Expires</span></span>

* <span data-ttu-id="174af-185">Son değiştirilme</span><span class="sxs-lookup"><span data-stu-id="174af-185">Last-Modified</span></span>

* <span data-ttu-id="174af-186">Pragması</span><span class="sxs-lookup"><span data-stu-id="174af-186">Pragma</span></span>

<span data-ttu-id="174af-187">CORS spec bu çağrıları *basit yanıt üstbilgilerini*.</span><span class="sxs-lookup"><span data-stu-id="174af-187">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="174af-188">Diğer üst bilgileri uygulama kullanılabilir hale getirmek için:</span><span class="sxs-lookup"><span data-stu-id="174af-188">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="174af-189">Çıkış noktaları arası istekleri kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="174af-189">Credentials in cross-origin requests</span></span>

<span data-ttu-id="174af-190">Özel işleme bir CORS isteğinde kimlik bilgilerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="174af-190">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="174af-191">Varsayılan olarak, tarayıcının çıkış noktaları arası istek ile herhangi bir kimlik bilgisi göndermez.</span><span class="sxs-lookup"><span data-stu-id="174af-191">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="174af-192">Kimlik bilgileri, tanımlama bilgilerinin yanı sıra HTTP kimlik doğrulama düzenleri içerir.</span><span class="sxs-lookup"><span data-stu-id="174af-192">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="174af-193">Kimlik bilgileriyle bir çıkış noktaları arası istek göndermek için istemci XMLHttpRequest.withCredentials true olarak ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="174af-193">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="174af-194">Kullanarak doğrudan XMLHttpRequest:</span><span class="sxs-lookup"><span data-stu-id="174af-194">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="174af-195">JQuery içinde:</span><span class="sxs-lookup"><span data-stu-id="174af-195">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="174af-196">Ayrıca, sunucunun kimlik bilgilerini izin vermeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="174af-196">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="174af-197">Çıkış noktaları arası kimlik bilgilerine izin vermek için:</span><span class="sxs-lookup"><span data-stu-id="174af-197">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="174af-198">Artık HTTP yanıtı sunucunun bir çıkış noktaları arası istek için kimlik bilgilerini sağlayan tarayıcı belirten bir erişim-denetim-Allow-Credentials üst bilgisi içerir.</span><span class="sxs-lookup"><span data-stu-id="174af-198">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="174af-199">Kimlik bilgilerini tarayıcı gönderir, ancak yanıt geçerli erişim-denetim-Allow-Credentials üst bilgi içermeyen, tarayıcı uygulamaya yanıt kullanıma olmaz ve AJAX isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="174af-199">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="174af-200">Çıkış noktaları arası kimlik bilgilerine izin verirken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="174af-200">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="174af-201">Başka bir etki alanındaki bir Web sitesi kullanıcı bilgisi olmadan kullanıcı adına uygulamasında oturum açmış kullanıcının kimlik bilgilerini gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="174af-201">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="174af-202">CORS belirtimi Ayrıca bu ayar durumları için kaynakları `"*"` (tüm kaynaklar) geçersiz, `Access-Control-Allow-Credentials` başlığı.</span><span class="sxs-lookup"><span data-stu-id="174af-202">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="174af-203">Denetim öncesi sona erme saati ayarla</span><span class="sxs-lookup"><span data-stu-id="174af-203">Set the preflight expiration time</span></span>

<span data-ttu-id="174af-204">Denetim öncesi isteğin yanıtını önbelleğe ne kadar süreyle erişim-denetim-Max-Age üstbilgisini belirtir.</span><span class="sxs-lookup"><span data-stu-id="174af-204">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="174af-205">Bu üstbilginin ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="174af-205">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="174af-206">CORS nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="174af-206">How CORS works</span></span>

<span data-ttu-id="174af-207">Bu bölümde, HTTP iletileri düzeyinde bir CORS isteğinde ne açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="174af-207">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="174af-208">CORS ilkesinin doğru yapılandırılmış ve beklenmeyen davranışlar ortaya çıktığında hata ayıklaması CORS nasıl çalıştığını anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="174af-208">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="174af-209">CORS belirtimi çıkış noktaları arası istekleri etkinleştirme birkaç yeni HTTP üst bilgilerini ortaya çıkarır.</span><span class="sxs-lookup"><span data-stu-id="174af-209">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="174af-210">Bir tarayıcı CORS destekliyorsa, bu üstbilgileri çıkış noktaları arası istekleri için otomatik olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="174af-210">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="174af-211">Özel JavaScript kodu, CORS'yi etkinleştirmek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="174af-211">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="174af-212">Çıkış noktaları arası isteğinin bir örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="174af-212">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="174af-213">`Origin` Üst bilgi isteği yapan site etki alanı sağlar:</span><span class="sxs-lookup"><span data-stu-id="174af-213">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="174af-214">Sunucu isteği izin veriyorsa, yanıtta Access-Control-Allow-Origin üstbilgisini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="174af-214">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="174af-215">Bu üst bilgi değeri eşleşen kaynak üst bilgisi istek veya joker karakter değeri "\*", yani her türlü kaynağa izin verilir:</span><span class="sxs-lookup"><span data-stu-id="174af-215">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="174af-216">Access-Control-Allow-Origin üst bilgi yanıtı içermiyorsa, AJAX isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="174af-216">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="174af-217">Özellikle, tarayıcının isteği izin vermiyor.</span><span class="sxs-lookup"><span data-stu-id="174af-217">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="174af-218">Sunucunun başarılı bir yanıt döndürürse bile, tarayıcı yanıtı istemci uygulama tarafından kullanılabilir yapmaz.</span><span class="sxs-lookup"><span data-stu-id="174af-218">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="174af-219">Denetim öncesi isteği</span><span class="sxs-lookup"><span data-stu-id="174af-219">Preflight Requests</span></span>

<span data-ttu-id="174af-220">Bazı CORS istekleri için tarayıcı kaynak gerçek isteği göndermeden önce bir "Denetim öncesi isteği" adlı ek bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="174af-220">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="174af-221">Aşağıdaki koşullar doğruysa, tarayıcının denetim öncesi isteği atlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="174af-221">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="174af-222">İstek yöntemini GET, HEAD veya sonrası, olduğundan ve</span><span class="sxs-lookup"><span data-stu-id="174af-222">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="174af-223">Uygulama dışındaki içeriği kabul et, Accept-Language, dil, tüm istek üst bilgilerini ayarlayıp ayarlamadığını Content-Type veya son-Event-ID ve</span><span class="sxs-lookup"><span data-stu-id="174af-223">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="174af-224">Content-Type üst bilgisi (varsa ayarlayın) aşağıdakilerden biridir:</span><span class="sxs-lookup"><span data-stu-id="174af-224">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="174af-225">Application/x-www-form-urlencoded işlemek</span><span class="sxs-lookup"><span data-stu-id="174af-225">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="174af-226">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="174af-226">multipart/form-data</span></span>

  * <span data-ttu-id="174af-227">metin/düz</span><span class="sxs-lookup"><span data-stu-id="174af-227">text/plain</span></span>

<span data-ttu-id="174af-228">İstek üst bilgileri hakkında kural, uygulama XMLHttpRequest nesnesinde setRequestHeader çağırarak ayarlar üst bilgileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="174af-228">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="174af-229">(Bu "Yazar istek üstbilgilerini" CORS belirtimi çağırır.) Kural, kullanıcı aracısı, konak veya Content-Length gibi tarayıcı ayarlayabilirsiniz üstbilgi için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="174af-229">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="174af-230">Denetim öncesi isteğinin bir örneği aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="174af-230">Here is an example of a preflight request:</span></span>

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

<span data-ttu-id="174af-231">Uçuş öncesi isteğinin HTTP OPTIONS yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="174af-231">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="174af-232">Bu, iki özel üst bilgileri içerir:</span><span class="sxs-lookup"><span data-stu-id="174af-232">It includes two special headers:</span></span>

* <span data-ttu-id="174af-233">Access-Control-Request-Method: fiili istek için kullanılacak HTTP yöntemi.</span><span class="sxs-lookup"><span data-stu-id="174af-233">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="174af-234">Access-Control-Request-Headers: Uygulama gerçek istek üzerinde ayarlanan istek üst bilgilerini içeren bir liste.</span><span class="sxs-lookup"><span data-stu-id="174af-234">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="174af-235">(Yeniden, bu tarayıcı ayarlar üst bilgiler dahil değildir.)</span><span class="sxs-lookup"><span data-stu-id="174af-235">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="174af-236">Sunucunun isteği izin varsayılarak bir yanıt örneği, şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="174af-236">Here is an example response, assuming that the server allows the request:</span></span>

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

<span data-ttu-id="174af-237">Yanıt, izin verilen yöntemleri listeleyen bir erişim-denetim-Allow-Methods üst bilgisi ve isteğe bağlı olarak izin verilen üstbilgileri listeleyen bir Access-Control-izin ver-Headers üstbilgisi içeriyor.</span><span class="sxs-lookup"><span data-stu-id="174af-237">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="174af-238">Denetim öncesi isteği başarıyla sonuçlanırsa, tarayıcı daha önce açıklandığı gibi gerçek bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="174af-238">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
