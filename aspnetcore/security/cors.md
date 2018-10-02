---
title: ASP.NET core'da çıkış noktaları arası istekleri (CORS) etkinleştirme
author: rick-anderson
description: Bilgi nasıl CORS izin verme veya reddetme ASP.NET Core uygulaması çıkış noktaları arası istekleri için standart olarak.
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2018
uid: security/cors
ms.openlocfilehash: cfbf24edb1dae76f676d51738b0d57266688d53e
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045594"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="5f4ec-103">ASP.NET core'da çıkış noktaları arası istekleri (CORS) etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="5f4ec-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="5f4ec-104">Tarafından [Mike Wasson](https://github.com/mikewasson), [Shayne boyer'ın](https://twitter.com/spboyer), ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5f4ec-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="5f4ec-105">Tarayıcı güvenlik, bir web sayfası web sayfada sunulandan daha farklı bir etki alanı istekleri yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="5f4ec-106">Bu kısıtlama adlı *aynı çıkış noktası İlkesi*.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="5f4ec-107">Aynı kaynak İlkesi, kötü niyetli site başka bir siteden hassas verileri okumasını önler.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="5f4ec-108">Bazı durumlarda, uygulamanız için diğer sitelere çıkış noktaları arası isteklerde izin vermek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="5f4ec-109">[Kaynağın kaynak paylaşımını çapraz](https://www.w3.org/TR/cors/) (CORS) olan gevşek bir aynı çıkış noktası ilkesi izin veren bir W3C standart.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="5f4ec-110">CORS kullanarak, bir sunucu açıkça bazı çıkış noktaları arası istekleri izin verirken diğerlerini.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="5f4ec-111">CORS güvenli ve önceki teknikleri, daha esnek gibi [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="5f4ec-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="5f4ec-112">Bu konuda, bir ASP.NET Core uygulamada CORS'yi etkinleştirme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="5f4ec-113">Aynı kaynak</span><span class="sxs-lookup"><span data-stu-id="5f4ec-113">Same origin</span></span>

<span data-ttu-id="5f4ec-114">İki URL aynı düzenleri, konaklar ve bağlantı noktaları varsa aynı kaynağa sahip ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="5f4ec-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="5f4ec-115">Bu iki URL aynı kaynağa sahip:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="5f4ec-116">Bu URL'ler, önceki iki URL değerinden farklı çıkış noktaları vardır:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="5f4ec-117">`https://example.net` &ndash; Farklı bir etki alanı</span><span class="sxs-lookup"><span data-stu-id="5f4ec-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="5f4ec-118">`https://www.example.com/foo.html` &ndash; Farklı alt etki alanı</span><span class="sxs-lookup"><span data-stu-id="5f4ec-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="5f4ec-119">`http://example.com/foo.html` &ndash; Farklı düzeni</span><span class="sxs-lookup"><span data-stu-id="5f4ec-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="5f4ec-120">`https://example.com:9000/foo.html` &ndash; Farklı bir bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="5f4ec-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="5f4ec-121">Internet Explorer kaynakları karşılaştırılırken, bağlantı noktası dikkate almaz.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="5f4ec-122">CORS Hizmetleri'ne kaydetme</span><span class="sxs-lookup"><span data-stu-id="5f4ec-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5f4ec-123">Başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) veya paket başvurusu ekleme [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) paket.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="5f4ec-124">Başvuru [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) veya paket başvurusu ekleme [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) paket.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5f4ec-125">Paket başvurusu ekleme [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) paket.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="5f4ec-126">Çağrı <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> içinde `Startup.ConfigureServices` uygulamanın hizmet kapsayıcıya CORS Hizmetleri eklemek için:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="5f4ec-127">CORS'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="5f4ec-127">Enable CORS</span></span>

<span data-ttu-id="5f4ec-128">CORS Hizmetleri kaydolduktan sonra aşağıdaki yaklaşımlardan birini CORS ASP.NET Core uygulamanızı etkinleştirmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="5f4ec-129">[CORS ara yazılımı](#enable-cors-with-cors-middleware) &ndash; uygulamak CORS ilkelerini genel ara yazılım aracılığıyla.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="5f4ec-130">[CORS mvc'de](#enable-cors-in-mvc) &ndash; uygulamak CORS ilkelerini eylem veya denetleyici başına.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="5f4ec-131">CORS ara yazılımı kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="5f4ec-132">CORS ara yazılımı ile CORS'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="5f4ec-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="5f4ec-133">CORS ara yazılımı, uygulamaya çıkış noktaları arası istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="5f4ec-134">CORS ara yazılım istek işleme ardışık etkinleştirmek için çağrı <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> uzantı yönteminde `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="5f4ec-135">CORS ara yazılımı gelmelidir tanımlanmış tüm uç noktalar uygulamanızda çıkış noktaları arası istekleri desteklemek istediğiniz (örneğin, çağırmadan önce `UseMvc` MVC/Razor sayfaları ara yazılımı için).</span><span class="sxs-lookup"><span data-stu-id="5f4ec-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="5f4ec-136">A *çıkış noktaları arası ilke* CORS ara yazılımı kullanarak eklerken belirtilebilir <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="5f4ec-137">CORS ilkesini tanımlamak için iki yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="5f4ec-138">Çağrı `UseCors` bir lambda ile:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="5f4ec-139">Lambda alan bir <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> nesne.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="5f4ec-140">[Yapılandırma seçenekleri](#cors-policy-options), gibi `WithOrigins`, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="5f4ec-141">Önceki örnekte, çıkış noktaları arası istekleri ilkeyi sağlar `https://example.com` ve diğer kaynak.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="5f4ec-142">URL'nin sonunda bir eğik çizgi belirtilmelidir (`/`).</span><span class="sxs-lookup"><span data-stu-id="5f4ec-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="5f4ec-143">URL ile sona ererse `/`, karşılaştırma döndürür `false` ve üst bilgi döndürülür.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="5f4ec-144">`CorsPolicyBuilder` Yöntem çağrıları zincirleyebilirsiniz şekilde fluent API'sini sahiptir:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="5f4ec-145">Bir veya daha fazla adlandırılmış CORS ilkelerini tanımlama ve çalışma zamanında adıyla ilkeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="5f4ec-146">Aşağıdaki örnekte adlı bir kullanıcı tanımlı CORS ilkesinin ekler *AllowSpecificOrigin*.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="5f4ec-147">İlkeyi seçmek için adına geçirmek `UseCors`:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="5f4ec-148">Mvc'de CORS'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="5f4ec-148">Enable CORS in MVC</span></span>

<span data-ttu-id="5f4ec-149">Alternatif olarak, MVC eylem veya denetleyici başına belirli CORS ilkelerini uygulamak için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="5f4ec-150">MVC, CORS'yi etkinleştirmek için kullanılırken kaydedilen CORS Hizmetleri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="5f4ec-151">CORS ara yazılımı kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="5f4ec-152">Eylem başına</span><span class="sxs-lookup"><span data-stu-id="5f4ec-152">Per action</span></span>

<span data-ttu-id="5f4ec-153">Belirli bir eylem için CORS ilkesinin belirtmek için [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) eyleme özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="5f4ec-154">İlke adı belirtin.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="5f4ec-155">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="5f4ec-155">Per controller</span></span>

<span data-ttu-id="5f4ec-156">CORS ilkesini belirli bir denetleyicinin belirtmek için [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) özniteliği için denetleyici sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="5f4ec-157">İlke adı belirtin.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="5f4ec-158">Öncelik sırası şöyledir:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-158">The precedence order is:</span></span>

1. <span data-ttu-id="5f4ec-159">Eylem</span><span class="sxs-lookup"><span data-stu-id="5f4ec-159">action</span></span>
1. <span data-ttu-id="5f4ec-160">denetleyici</span><span class="sxs-lookup"><span data-stu-id="5f4ec-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="5f4ec-161">CORS devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="5f4ec-161">Disable CORS</span></span>

<span data-ttu-id="5f4ec-162">CORS bir denetleyici veya eylem için devre dışı bırakmak için [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) özniteliği:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="5f4ec-163">CORS ilkesi seçenekleri</span><span class="sxs-lookup"><span data-stu-id="5f4ec-163">CORS policy options</span></span>

<span data-ttu-id="5f4ec-164">Bu bölümde, CORS ilke ayarlayabileceğiniz çeşitli seçenekler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-164">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="5f4ec-165">İzin verilen çıkış noktaları ayarlama</span><span class="sxs-lookup"><span data-stu-id="5f4ec-165">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="5f4ec-166">İzin verilen HTTP yöntemleri Ayarla</span><span class="sxs-lookup"><span data-stu-id="5f4ec-166">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="5f4ec-167">İzin verilen istek üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="5f4ec-167">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="5f4ec-168">İfşa edilen yanıt üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="5f4ec-168">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="5f4ec-169">Çıkış noktaları arası istekleri kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="5f4ec-169">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="5f4ec-170">Denetim öncesi sona erme saati ayarla</span><span class="sxs-lookup"><span data-stu-id="5f4ec-170">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="5f4ec-171">Bazı seçenekleri okumak yardımcı olabilecek [CORS nasıl çalıştığını](#how-cors-works) ilk bölümü.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-171">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="5f4ec-172">İzin verilen çıkış noktaları ayarlama</span><span class="sxs-lookup"><span data-stu-id="5f4ec-172">Set the allowed origins</span></span>

<span data-ttu-id="5f4ec-173">Bir veya daha fazla belirli kaynaklara izin veren çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-173">To allow one or more specific origins, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

<span data-ttu-id="5f4ec-174">Tüm kaynaklara izin veren çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-174">To allow all origins, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

<span data-ttu-id="5f4ec-175">Her türlü kaynağa gelen isteklere izin vermeden önce dikkatlice düşünün.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-175">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="5f4ec-176">Her türlü kaynağa gelen isteklere izin vererek anlamına *herhangi bir Web sitesinde* çıkış noktaları arası istekleri uygulamanıza yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-176">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

<span data-ttu-id="5f4ec-177">Bu ayar etkiler [istekleri ve Access-Control-Allow-Origin üst bilgisi ön kontrol](#preflight-requests) (Bu konunun ilerleyen bölümlerinde açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="5f4ec-177">This setting affects [preflight requests and the Access-Control-Allow-Origin header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="5f4ec-178">İzin verilen HTTP yöntemleri Ayarla</span><span class="sxs-lookup"><span data-stu-id="5f4ec-178">Set the allowed HTTP methods</span></span>

<span data-ttu-id="5f4ec-179">Tüm HTTP yöntemleri izin vermek için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-179">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

<span data-ttu-id="5f4ec-180">Bu ayar etkiler [istekleri ve erişim-denetim-Allow-Methods üst bilgisi ön kontrol](#preflight-requests) (Bu konunun ilerleyen bölümlerinde açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="5f4ec-180">This setting affects [preflight requests and the Access-Control-Allow-Methods header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="5f4ec-181">İzin verilen istek üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="5f4ec-181">Set the allowed request headers</span></span>

<span data-ttu-id="5f4ec-182">Adlı bir CORS isteğinde gönderilecek belirli üstbilgilere izin için *yazar, istek üst bilgilerini*, çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> ve izin verilen üstbilgileri belirtin:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-182">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="5f4ec-183">Tüm istek üstbilgilerini Yazar izin vermek için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-183">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="5f4ec-184">Bu ayar etkiler [istekleri ve Access-Control-Request-Headers üstbilgisi ön kontrol](#preflight-requests) (Bu konunun ilerleyen bölümlerinde açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="5f4ec-184">This setting affects [preflight requests and the Access-Control-Request-Headers header](#preflight-requests) (described later in this topic).</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5f4ec-185">CORS ara yazılımı ilke eşleşmesi için belirli üst bilgileri tarafından belirtilen `WithHeaders` gönderilen üst bilgiler, yalnızca mümkündür `Access-Control-Request-Headers` belirtilen üst bilgilerin tam olarak eşleşmesi `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-185">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="5f4ec-186">Örneğin, şu şekilde yapılandırılmış bir uygulama göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-186">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="5f4ec-187">CORS ara yazılımı, çünkü bir denetim öncesi isteği şu istek üst bilgisi ile azalma `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) içinde listelenmiyor `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-187">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="5f4ec-188">Uygulama döndürür bir *200 Tamam* yanıt geri CORS üst bilgileri göndermez ancak.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-188">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="5f4ec-189">Bu nedenle, tarayıcının çıkış noktaları arası istek çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-189">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="5f4ec-190">CORS ara yazılımı, her zaman dört üst bilgilerinde sağlar `Access-Control-Request-Headers` CorsPolicy.Headers içinde yapılandırılan değerlere bakılmaksızın gönderilecek.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-190">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="5f4ec-191">Üst bilgi bu liste aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-191">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="5f4ec-192">Örneğin, şu şekilde yapılandırılmış bir uygulama göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-192">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="5f4ec-193">CORS ara yazılımın yanıt başarıyla bir denetim öncesi isteği şu istek üst bilgisi için çünkü `Content-Language` her zaman izin verilenler listesinde olur:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-193">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="5f4ec-194">İfşa edilen yanıt üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="5f4ec-194">Set the exposed response headers</span></span>

<span data-ttu-id="5f4ec-195">Varsayılan olarak, tüm yanıt üstbilgilerini uygulamaya tarayıcı ortaya çıkarmıyor.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-195">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="5f4ec-196">Daha fazla bilgi için [W3C çıkış noktaları arası kaynak paylaşımı (terminolojisi): basit yanıt üst bilgisi](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="5f4ec-196">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="5f4ec-197">Varsayılan olarak kullanılabilir olan yanıt üstbilgilerini şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-197">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="5f4ec-198">Bu üst CORS belirtimi çağırır *basit yanıt üstbilgilerini*.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-198">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="5f4ec-199">Diğer üst bilgileri uygulama için kullanılabilir hale getirmek için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-199">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="5f4ec-200">Çıkış noktaları arası istekleri kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="5f4ec-200">Credentials in cross-origin requests</span></span>

<span data-ttu-id="5f4ec-201">Özel işleme bir CORS isteğinde kimlik bilgilerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-201">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="5f4ec-202">Varsayılan olarak, tarayıcının çıkış noktaları arası istek ile kimlik bilgileri göndermez.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-202">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="5f4ec-203">Tanımlama bilgileri ve kimlik doğrulama düzeni HTTP kimlik bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-203">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="5f4ec-204">İstemci kimlik bilgileriyle bir çıkış noktaları arası istek göndermek için ayarlamalısınız `XMLHttpRequest.withCredentials` için `true`.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-204">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="5f4ec-205">Kullanarak `XMLHttpRequest` doğrudan:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-205">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="5f4ec-206">JQuery içinde:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-206">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="5f4ec-207">Ayrıca, sunucunun kimlik bilgilerini izin vermeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-207">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="5f4ec-208">Çıkış noktaları arası kimlik bilgilerini sağlamak için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-208">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

<span data-ttu-id="5f4ec-209">HTTP yanıtı içeren bir `Access-Control-Allow-Credentials` sunucu çıkış noktaları arası istek için kimlik bilgilerini sağlayan tarayıcı söyler. başlığı.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-209">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="5f4ec-210">Tarayıcı bilgilerini gönderir, ancak yanıt geçerli bir içermez `Access-Control-Allow-Credentials` üst bilgi, tarayıcı olmayan uygulama yanıtı kullanımına sunun ve çıkış noktaları arası istek başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-210">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="5f4ec-211">Çıkış noktaları arası kimlik bilgilerine izin verirken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-211">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="5f4ec-212">Başka bir etki alanındaki bir Web sitesi kullanıcı bilgisi olmadan kullanıcı adına uygulamasında oturum açmış kullanıcının kimlik bilgilerini gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-212">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="5f4ec-213">CORS belirtimi Ayrıca bu ayar durumları için kaynakları `"*"` (tüm kaynaklar) geçersiz, `Access-Control-Allow-Credentials` başlığı.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-213">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="5f4ec-214">Denetim öncesi isteği</span><span class="sxs-lookup"><span data-stu-id="5f4ec-214">Preflight requests</span></span>

<span data-ttu-id="5f4ec-215">Bazı CORS istekleri için tarayıcı, gerçek isteği yapmadan önce ek bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-215">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="5f4ec-216">Bu istek adında bir *denetim öncesi isteği*.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-216">This request is called a *preflight request*.</span></span> <span data-ttu-id="5f4ec-217">Aşağıdaki koşullar doğruysa, tarayıcının denetim öncesi isteği atlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-217">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="5f4ec-218">İstek yöntemini GET, HEAD veya POST ' dir.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-218">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="5f4ec-219">Uygulama dışında istek üst bilgilerini ayarlayıp ayarlamadığını `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, veya `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-219">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="5f4ec-220">`Content-Type` Üst bilgi, ayarla, aşağıdaki değerlerden birine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-220">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="5f4ec-221">İstek üstbilgileri kural kümesi çağırarak uygulama ayarlar üst bilgileri istemci isteği uygulandığı için `setRequestHeader` üzerinde `XMLHttpRequest` nesne.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-221">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="5f4ec-222">Bu üst CORS belirtimi çağırır *yazar, istek üst bilgilerini*.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-222">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="5f4ec-223">Tarayıcı ayarlayabilir, gibi üstbilgileri kuralı uygulanmaz `User-Agent`, `Host`, veya `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-223">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="5f4ec-224">Denetim öncesi isteğinin bir örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-224">The following is an example of a preflight request:</span></span>

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="5f4ec-225">Uçuş öncesi isteğinin HTTP OPTIONS yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-225">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="5f4ec-226">Bu, iki özel üst bilgileri içerir:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-226">It includes two special headers:</span></span>

* <span data-ttu-id="5f4ec-227">`Access-Control-Request-Method`: Fiili istek için kullanılacak HTTP yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-227">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="5f4ec-228">`Access-Control-Request-Headers`: Uygulamayı gerçek istekte ayarlar istek üst bilgilerini bir listesi.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-228">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="5f4ec-229">Daha önce belirtildiği gibi bu tarayıcı ayarlar, aşağıdaki gibi üst bilgiler içermez `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-229">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="5f4ec-230">CORS denetim öncesi isteği içerebilir bir `Access-Control-Request-Headers` üst bilgi sunucuya gerçek isteğiyle gönderilen üst bilgiler gösterir.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-230">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="5f4ec-231">Özel üst bilgiler izin vermek için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-231">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="5f4ec-232">Tüm istek üstbilgilerini Yazar izin vermek için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-232">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="5f4ec-233">Bunlar nasıl kümesinde tamamen tutarlı olmayan tarayıcıları `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-233">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="5f4ec-234">Üst bilgileri için herhangi bir şey dışında ayarlarsanız `"*"` (veya <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), en az içermelidir `Accept`, `Content-Type`, ve `Origin`, artı, desteklemek istediğiniz tüm özel üst.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-234">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="5f4ec-235">(Sunucu isteği izin varsayılarak) denetim öncesi isteği için bir örnek yanıt aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-235">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="5f4ec-236">Yanıt içeren bir `Access-Control-Allow-Methods` izin verilen yöntemleri listeleyen üst bilgi ve isteğe bağlı olarak bir `Access-Control-Allow-Headers` izin verilen üstbilgileri listeleyen üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-236">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="5f4ec-237">Denetim öncesi isteği başarıyla sonuçlanırsa, tarayıcı gerçek isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-237">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="5f4ec-238">Denetim öncesi isteği reddedilirse, uygulamayı döndürür bir *200 Tamam* yanıt geri CORS üst bilgileri göndermez ancak.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-238">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="5f4ec-239">Bu nedenle, tarayıcının çıkış noktaları arası istek çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-239">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="5f4ec-240">Denetim öncesi sona erme saati ayarla</span><span class="sxs-lookup"><span data-stu-id="5f4ec-240">Set the preflight expiration time</span></span>

<span data-ttu-id="5f4ec-241">`Access-Control-Max-Age` Üstbilgisini belirtir ne kadar süreyle denetim öncesi isteğin yanıtını önbelleğe alınabilir.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-241">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="5f4ec-242">Bu başlık ayarlamak için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-242">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="5f4ec-243">CORS nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="5f4ec-243">How CORS works</span></span>

<span data-ttu-id="5f4ec-244">Bu bölümde, HTTP iletileri düzeyinde bir CORS isteğinde ne açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-244">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="5f4ec-245">CORS ilkesinin doğru yapılandırılmış ve beklenmeyen davranışlar ortaya çıktığında hata ayıklaması CORS nasıl çalıştığını anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-245">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="5f4ec-246">CORS belirtimi çıkış noktaları arası istekleri etkinleştirme birkaç yeni HTTP üst bilgilerini ortaya çıkarır.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-246">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="5f4ec-247">Bir tarayıcı CORS destekliyorsa, bu üstbilgileri çıkış noktaları arası istekleri için otomatik olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-247">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="5f4ec-248">Özel JavaScript kodu, CORS'yi etkinleştirmek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-248">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="5f4ec-249">Çıkış noktaları arası isteğinin bir örneği verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-249">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="5f4ec-250">`Origin` Üst bilgi isteği yapan site etki alanı sağlar:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-250">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="5f4ec-251">Sunucu isteği izin veriyorsa, bu ayarlar `Access-Control-Allow-Origin` yanıt üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-251">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="5f4ec-252">Bu üst bilgi değeri ya da eşleşen `Origin` istekteki üstbilgi veya joker karakter değeri `"*"`, yani her türlü kaynağa izin verilir:</span><span class="sxs-lookup"><span data-stu-id="5f4ec-252">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="5f4ec-253">Yanıt içermiyorsa `Access-Control-Allow-Origin` üst bilgi çıkış noktaları arası istek başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-253">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="5f4ec-254">Özellikle, tarayıcının isteği izin vermiyor.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-254">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="5f4ec-255">Sunucunun başarılı bir yanıt döndürürse bile, tarayıcı yanıtı istemci uygulamasının kullanımına yapmaz.</span><span class="sxs-lookup"><span data-stu-id="5f4ec-255">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5f4ec-256">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5f4ec-256">Additional resources</span></span>

* [<span data-ttu-id="5f4ec-257">Çıkış noktaları arası kaynak paylaşımı (CORS)</span><span class="sxs-lookup"><span data-stu-id="5f4ec-257">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
