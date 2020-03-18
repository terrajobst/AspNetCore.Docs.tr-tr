---
title: ASP.NET Core 'de çıkış noktaları arası Istekleri (CORS) etkinleştirme
author: rick-anderson
description: ASP.NET Core uygulamasında çapraz kaynak isteklerini izin vermek veya reddetmek için CORS 'nin nasıl standart olduğunu öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: 09cc296ebf605907371619124cac00883beb6abb
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511580"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="723e0-103">ASP.NET Core 'de çıkış noktaları arası Istekleri (CORS) etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="723e0-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="723e0-104">Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="723e0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="723e0-105">Bu makalede, ASP.NET Core uygulamasında CORS 'nin nasıl etkinleştirileceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="723e0-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="723e0-106">Tarayıcı güvenliği, bir Web sayfasının Web sayfasını sunduğundan farklı bir etki alanına istek yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="723e0-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="723e0-107">Bu kısıtlamaya *aynı-Origin ilkesi*adı verilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="723e0-108">Aynı-kaynak ilkesi, kötü niyetli bir sitenin gizli verileri başka bir siteden okumasını engeller.</span><span class="sxs-lookup"><span data-stu-id="723e0-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="723e0-109">Bazen diğer sitelerin uygulamanıza çapraz çıkış istekleri yapmasına izin vermek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723e0-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="723e0-110">Daha fazla bilgi için bkz. [MOZILLA CORS makalesi](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="723e0-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="723e0-111">[Çapraz kaynak kaynak paylaşımı](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="723e0-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="723e0-112">, Bir sunucunun aynı kaynak ilkeyi rahat bir şekilde sağlamasına izin veren bir W3C standardıdır.</span><span class="sxs-lookup"><span data-stu-id="723e0-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="723e0-113">Güvenlik özelliği **değil** , CORS güvenliği.</span><span class="sxs-lookup"><span data-stu-id="723e0-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="723e0-114">CORS 'nin izin vermesini sağlayan bir API daha güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="723e0-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="723e0-115">Daha fazla bilgi için bkz. [CORS çalışma](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="723e0-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="723e0-116">Bir sunucunun bazı çapraz kaynak isteklerine, diğerlerini reddetirken açık olarak izin almasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="723e0-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="723e0-117">, [JSONP](/dotnet/framework/wcf/samples/jsonp)gibi önceki tekniklerin daha güvenli ve daha esnektir.</span><span class="sxs-lookup"><span data-stu-id="723e0-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="723e0-118">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="723e0-118">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="723e0-119">Aynı kaynak</span><span class="sxs-lookup"><span data-stu-id="723e0-119">Same origin</span></span>

<span data-ttu-id="723e0-120">Özdeş şemaları, konakları ve bağlantı noktalarına sahip olmaları durumunda iki URL aynı kaynağa sahiptir ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="723e0-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="723e0-121">Bu iki URL aynı kaynağa sahiptir:</span><span class="sxs-lookup"><span data-stu-id="723e0-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="723e0-122">Bu URL 'Ler, önceki iki URL 'den farklı kaynaklardan farklıdır:</span><span class="sxs-lookup"><span data-stu-id="723e0-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="723e0-123">Farklı etki alanı &ndash; `https://example.net`</span><span class="sxs-lookup"><span data-stu-id="723e0-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="723e0-124">Farklı alt etki alanı &ndash; `https://www.example.com/foo.html`</span><span class="sxs-lookup"><span data-stu-id="723e0-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="723e0-125">Farklı &ndash; düzeni `http://example.com/foo.html`</span><span class="sxs-lookup"><span data-stu-id="723e0-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="723e0-126">Farklı bağlantı noktası &ndash; `https://example.com:9000/foo.html`</span><span class="sxs-lookup"><span data-stu-id="723e0-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="723e0-127">Internet Explorer, kaynakları karşılaştırırken bağlantı noktasını dikkate almaz.</span><span class="sxs-lookup"><span data-stu-id="723e0-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="723e0-128">Adlandırılmış ilke ve ara yazılım ile CORS</span><span class="sxs-lookup"><span data-stu-id="723e0-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="723e0-129">CORS ara yazılımı, çıkış noktaları arası istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="723e0-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="723e0-130">Aşağıdaki kod, belirtilen kaynağa sahip tüm uygulama için CORS 'yi sunar:</span><span class="sxs-lookup"><span data-stu-id="723e0-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="723e0-131">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="723e0-131">The preceding code:</span></span>

* <span data-ttu-id="723e0-132">İlke adını "\_Myallowspecifickaynaklarına" olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="723e0-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="723e0-133">İlke adı rastgele olur.</span><span class="sxs-lookup"><span data-stu-id="723e0-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="723e0-134">CORS 'yi sağlayan <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> uzantısı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="723e0-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="723e0-135">[Lambda ifadesiyle](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="723e0-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="723e0-136">Lambda bir <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> nesnesi alır.</span><span class="sxs-lookup"><span data-stu-id="723e0-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="723e0-137">`WithOrigins`gibi [yapılandırma seçenekleri](#cors-policy-options), bu makalenin ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="723e0-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="723e0-138"><xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> yöntemi çağrısı, uygulamanın hizmet kapsayıcısına CORS Hizmetleri ekler:</span><span class="sxs-lookup"><span data-stu-id="723e0-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="723e0-139">Daha fazla bilgi için bu belgedeki [CORS ilke seçenekleri](#cpo) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="723e0-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="723e0-140"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> yöntemi aşağıdaki kodda gösterildiği gibi yöntemleri zincirleyebilir:</span><span class="sxs-lookup"><span data-stu-id="723e0-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="723e0-141">Not: URL sonunda eğik çizgi (`/` **) bulunmamalıdır.**</span><span class="sxs-lookup"><span data-stu-id="723e0-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="723e0-142">URL `/`ile sonlandığında, karşılaştırma `false` döndürür ve üst bilgi döndürülmez.</span><span class="sxs-lookup"><span data-stu-id="723e0-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a><span data-ttu-id="723e0-143">CORS ilkelerini tüm uç noktalara Uygula</span><span class="sxs-lookup"><span data-stu-id="723e0-143">Apply CORS policies to all endpoints</span></span>

<span data-ttu-id="723e0-144">Aşağıdaki kod, CORS ara yazılımı aracılığıyla CORS ilkelerini tüm uygulama uç noktalarına uygular:</span><span class="sxs-lookup"><span data-stu-id="723e0-144">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code omitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code omitted.
}
```

> [!WARNING]
> <span data-ttu-id="723e0-145">Endpoint Routing ile CORS ara yazılımı, `UseRouting` ve `UseEndpoints`çağrıları arasında yürütülecek şekilde yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="723e0-145">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="723e0-146">Yanlış yapılandırma, ara yazılımın doğru çalışmayı durdurmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="723e0-146">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

<span data-ttu-id="723e0-147">Bu sayfa/denetleyici/eylem düzeyinde CORS ilkesini uygulamak için [Razor Pages, denetleyiciler ve eylem YÖNTEMLERINDE CORS 'Yi etkinleştirin](#ecors) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="723e0-147">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="723e0-148">Önceki kodun test edilmesine ilişkin yönergeler için bkz. [Test CORS](#test) .</span><span class="sxs-lookup"><span data-stu-id="723e0-148">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="723e0-149">Uç nokta yönlendirme ile CORS 'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="723e0-149">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="723e0-150">Uç nokta yönlendirme ile CORS, `RequireCors` uzantı yöntemleri kullanılarak uç nokta temelinde etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-150">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="723e0-151">Benzer şekilde, CORS tüm denetleyiciler için de etkinleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="723e0-151">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="723e0-152">CORS 'yi özniteliklerle etkinleştir</span><span class="sxs-lookup"><span data-stu-id="723e0-152">Enable CORS with attributes</span></span>

<span data-ttu-id="723e0-153">[&lbrack;enablecors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) ÖZNITELIĞI, CORS 'yi küresel olarak uygulamaya bir alternatif sağlar.</span><span class="sxs-lookup"><span data-stu-id="723e0-153">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="723e0-154">`[EnableCors]` özniteliği, tüm bitiş noktaları yerine, seçilen bitiş noktaları için CORS 'yi mümkün.</span><span class="sxs-lookup"><span data-stu-id="723e0-154">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="723e0-155">İlke belirtmek için varsayılan ilkeyi ve `[EnableCors("{Policy String}")]` belirtmek için `[EnableCors]` kullanın.</span><span class="sxs-lookup"><span data-stu-id="723e0-155">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="723e0-156">`[EnableCors]` özniteliği şu şekilde uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="723e0-156">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="723e0-157">Razor sayfası `PageModel`</span><span class="sxs-lookup"><span data-stu-id="723e0-157">Razor Page `PageModel`</span></span>
* <span data-ttu-id="723e0-158">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="723e0-158">Controller</span></span>
* <span data-ttu-id="723e0-159">Denetleyici eylemi yöntemi</span><span class="sxs-lookup"><span data-stu-id="723e0-159">Controller action method</span></span>

<span data-ttu-id="723e0-160">`[EnableCors]` özniteliğiyle, denetleyici/sayfa-model/eyleme farklı ilkeler uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723e0-160">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="723e0-161">`[EnableCors]` özniteliği bir denetleyiciler/sayfa modeli/eylem yöntemine uygulandığında ve bir ara yazılım içinde CORS etkinleştirildiğinde her iki ilke de uygulanır.</span><span class="sxs-lookup"><span data-stu-id="723e0-161">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="723e0-162">İlkelerin birleştirilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-162">We recommend against combining policies.</span></span> <span data-ttu-id="723e0-163">Aynı uygulamada değil, `[EnableCors]` özniteliği veya ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="723e0-163">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="723e0-164">Aşağıdaki kod her bir yönteme farklı bir ilke uygular:</span><span class="sxs-lookup"><span data-stu-id="723e0-164">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="723e0-165">Aşağıdaki kod, bir CORS varsayılan ilkesi ve `"AnotherPolicy"`adlı bir ilke oluşturur:</span><span class="sxs-lookup"><span data-stu-id="723e0-165">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="723e0-166">CORS 'yi devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="723e0-166">Disable CORS</span></span>

<span data-ttu-id="723e0-167">[&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) özniteliği denetleyici/sayfa modeli/eylem için CORS 'yi devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="723e0-167">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="723e0-168">CORS ilke seçenekleri</span><span class="sxs-lookup"><span data-stu-id="723e0-168">CORS policy options</span></span>

<span data-ttu-id="723e0-169">Bu bölümde, bir CORS ilkesinde ayarlanmakta olabilecek çeşitli seçenekler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="723e0-169">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="723e0-170">İzin verilen kaynakları ayarla</span><span class="sxs-lookup"><span data-stu-id="723e0-170">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="723e0-171">İzin verilen HTTP yöntemlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-171">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="723e0-172">İzin verilen istek üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-172">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="723e0-173">Gösterilen yanıt üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-173">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="723e0-174">Kaynaklar arası isteklerde kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="723e0-174">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="723e0-175">Ön kontrol sona erme süresini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-175">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="723e0-176"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> `Startup.ConfigureServices`olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="723e0-176"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="723e0-177">Bazı seçenekler için, ilk olarak [CORS 'Nin nasıl çalıştığı](#how-cors) bölümü okumanız yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-177">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="723e0-178">İzin verilen kaynakları ayarla</span><span class="sxs-lookup"><span data-stu-id="723e0-178">Set the allowed origins</span></span>

<span data-ttu-id="723e0-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; tüm kaynaklardan gelen CORS isteklerinin herhangi bir düzene (`http` veya `https`) Izin verir.</span><span class="sxs-lookup"><span data-stu-id="723e0-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="723e0-180">*herhangi bir Web sitesi* uygulamaya çapraz kaynak istekleri yapabildiğinden `AllowAnyOrigin` güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="723e0-180">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="723e0-181">`AllowAnyOrigin` ve `AllowCredentials` belirtme, güvenli olmayan bir yapılandırmadır ve siteler arası istek elde edilmesine yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-181">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="723e0-182">Bir uygulama her iki yöntemle yapılandırıldığında, CORS hizmeti geçersiz bir CORS yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="723e0-182">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

<span data-ttu-id="723e0-183">`AllowAnyOrigin`, ön kontrol isteklerini ve `Access-Control-Allow-Origin` üst bilgisini etkiler.</span><span class="sxs-lookup"><span data-stu-id="723e0-183">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="723e0-184">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="723e0-184">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="723e0-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash;, kaynağın <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> özelliğini, kaynağa izin verilip verilmediğini değerlendirirken, kaynağın, yapılandırılan bir joker karakterle eşleştiğinden emin olan bir işlev olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="723e0-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="723e0-186">İzin verilen HTTP yöntemlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-186">Set the allowed HTTP methods</span></span>

<span data-ttu-id="723e0-187"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="723e0-187"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="723e0-188">Herhangi bir HTTP yöntemine izin verir:</span><span class="sxs-lookup"><span data-stu-id="723e0-188">Allows any HTTP method:</span></span>
* <span data-ttu-id="723e0-189">Ön kontrol isteklerini ve `Access-Control-Allow-Methods` üst bilgisini etkiler.</span><span class="sxs-lookup"><span data-stu-id="723e0-189">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="723e0-190">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="723e0-190">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="723e0-191">İzin verilen istek üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-191">Set the allowed request headers</span></span>

<span data-ttu-id="723e0-192">Belirli başlıkların, *Yazar istek üstbilgileri*ADLı bir CORS isteğinde gönderilmesine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> çağırın ve izin verilen üst bilgileri belirtin:</span><span class="sxs-lookup"><span data-stu-id="723e0-192">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="723e0-193">Tüm yazar isteği üst bilgilerine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="723e0-193">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="723e0-194">Bu ayar, ön kontrol isteklerini ve `Access-Control-Request-Headers` üst bilgisini etkiler.</span><span class="sxs-lookup"><span data-stu-id="723e0-194">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="723e0-195">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="723e0-195">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>


<span data-ttu-id="723e0-196">`WithHeaders` tarafından belirtilen belirli başlıklarıyla eşleşen bir CORS ara yazılım ilkesi, yalnızca `Access-Control-Request-Headers` gönderilen üstbilgiler `WithHeaders`belirtilen üstbilgilere tam olarak eşleşiyorsa mümkündür.</span><span class="sxs-lookup"><span data-stu-id="723e0-196">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="723e0-197">Örneğin, aşağıdaki gibi yapılandırılmış bir uygulamayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="723e0-197">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="723e0-198">CORS ara yazılımı, `Content-Language` ([Headernames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) `WithHeaders`listede olmadığından, şu istek üstbilgisiyle bir ön kontrol isteğini reddeder:</span><span class="sxs-lookup"><span data-stu-id="723e0-198">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="723e0-199">Uygulama *200 ok* yanıtı DÖNDÜRÜYOR ancak CORS üst bilgilerini geri göndermez.</span><span class="sxs-lookup"><span data-stu-id="723e0-199">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="723e0-200">Bu nedenle tarayıcı, çıkış noktaları arası isteği denemez.</span><span class="sxs-lookup"><span data-stu-id="723e0-200">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="723e0-201">Gösterilen yanıt üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-201">Set the exposed response headers</span></span>

<span data-ttu-id="723e0-202">Varsayılan olarak tarayıcı, tüm yanıt üst bilgilerini uygulamaya sunmaz.</span><span class="sxs-lookup"><span data-stu-id="723e0-202">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="723e0-203">Daha fazla bilgi için bkz. [W3C çıkış noktaları arası kaynak paylaşımı (terminoloji): basit yanıt üst bilgisi](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="723e0-203">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="723e0-204">Varsayılan olarak kullanılabilen yanıt üstbilgileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="723e0-204">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="723e0-205">CORS belirtimi, bu üst bilgiler *basit yanıt üst bilgilerini*çağırır.</span><span class="sxs-lookup"><span data-stu-id="723e0-205">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="723e0-206">Diğer üst bilgileri uygulama için kullanılabilir hale getirmek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="723e0-206">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="723e0-207">Kaynaklar arası isteklerde kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="723e0-207">Credentials in cross-origin requests</span></span>

<span data-ttu-id="723e0-208">Kimlik bilgileri CORS isteğinde özel işleme gerektirir.</span><span class="sxs-lookup"><span data-stu-id="723e0-208">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="723e0-209">Varsayılan olarak tarayıcı, kimlik bilgilerini bir çapraz kaynak isteğiyle göndermez.</span><span class="sxs-lookup"><span data-stu-id="723e0-209">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="723e0-210">Kimlik bilgileri, tanımlama bilgileri ve HTTP kimlik doğrulama düzenlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="723e0-210">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="723e0-211">Bir çapraz kaynak isteğiyle kimlik bilgilerini göndermek için, istemcisinin `true``XMLHttpRequest.withCredentials` ayarlaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="723e0-211">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="723e0-212">`XMLHttpRequest` doğrudan kullanarak:</span><span class="sxs-lookup"><span data-stu-id="723e0-212">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="723e0-213">JQuery kullanarak:</span><span class="sxs-lookup"><span data-stu-id="723e0-213">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="723e0-214">[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)'sini kullanma:</span><span class="sxs-lookup"><span data-stu-id="723e0-214">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="723e0-215">Sunucu kimlik bilgilerine izin vermelidir.</span><span class="sxs-lookup"><span data-stu-id="723e0-215">The server must allow the credentials.</span></span> <span data-ttu-id="723e0-216">Çıkış noktaları arası kimlik bilgilerine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="723e0-216">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="723e0-217">HTTP yanıtı, tarayıcıya sunucunun bir çapraz kaynak isteği için kimlik bilgileri verdiğini bildiren bir `Access-Control-Allow-Credentials` üst bilgisi içerir.</span><span class="sxs-lookup"><span data-stu-id="723e0-217">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="723e0-218">Tarayıcı kimlik bilgilerini gönderirse ancak yanıt geçerli bir `Access-Control-Allow-Credentials` üst bilgisi içermiyorsa, tarayıcı uygulamaya yanıtı kullanıma sunmaz ve çapraz kaynak isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="723e0-218">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="723e0-219">Çıkış noktaları arası kimlik bilgilerine izin vermek bir güvenlik riskidir.</span><span class="sxs-lookup"><span data-stu-id="723e0-219">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="723e0-220">Başka bir etki alanındaki Web sitesi, kullanıcının bilgisi olmadan kullanıcı adına, oturum açmış bir kullanıcının kimlik bilgilerini uygulamaya gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-220">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="723e0-221">CORS belirtimi Ayrıca, `Access-Control-Allow-Credentials` üst bilgisi varsa, `"*"` (tüm kaynaklar) için çıkış ayarının geçersiz olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="723e0-221">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="723e0-222">Ön kontrol istekleri</span><span class="sxs-lookup"><span data-stu-id="723e0-222">Preflight requests</span></span>

<span data-ttu-id="723e0-223">Bazı CORS istekleri için, tarayıcı gerçek isteği yapmadan önce ek bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="723e0-223">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="723e0-224">Bu isteğe bir *ön kontrol isteği*denir.</span><span class="sxs-lookup"><span data-stu-id="723e0-224">This request is called a *preflight request*.</span></span> <span data-ttu-id="723e0-225">Aşağıdaki koşullar doğruysa tarayıcı, ön kontrol isteğini atlayabilir:</span><span class="sxs-lookup"><span data-stu-id="723e0-225">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="723e0-226">İstek yöntemi al, HEAD veya POST.</span><span class="sxs-lookup"><span data-stu-id="723e0-226">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="723e0-227">Uygulama `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`veya `Last-Event-ID`dışındaki istek üst bilgilerini ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="723e0-227">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="723e0-228">Eğer ayarlandıysa `Content-Type` üst bilgisi aşağıdaki değerlerden birine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="723e0-228">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="723e0-229">İstemci isteği için ayarlanan istek üst bilgileri kuralı, `XMLHttpRequest` nesnesine `setRequestHeader` çağırarak uygulamanın ayarladığı üst bilgiler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="723e0-229">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="723e0-230">CORS belirtimi, bu üst bilgiler *Yazar istek üst bilgilerini*çağırır.</span><span class="sxs-lookup"><span data-stu-id="723e0-230">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="723e0-231">Kural, tarayıcının ayarlayabilmesi için `User-Agent`, `Host`veya `Content-Length`gibi üstbilgilere uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="723e0-231">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="723e0-232">Aşağıda bir ön denetim isteğine örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="723e0-232">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="723e0-233">Uçuş öncesi isteği HTTP SEÇENEKLERI yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="723e0-233">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="723e0-234">İki özel üst bilgi içerir:</span><span class="sxs-lookup"><span data-stu-id="723e0-234">It includes two special headers:</span></span>

* <span data-ttu-id="723e0-235">`Access-Control-Request-Method`: gerçek istek için kullanılacak HTTP yöntemi.</span><span class="sxs-lookup"><span data-stu-id="723e0-235">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="723e0-236">`Access-Control-Request-Headers`: uygulamanın gerçek istekte ayarladığı istek üst bilgilerinin bir listesi.</span><span class="sxs-lookup"><span data-stu-id="723e0-236">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="723e0-237">Daha önce belirtildiği gibi, bu, tarayıcının ayarladığı `User-Agent`gibi üst bilgileri içermez.</span><span class="sxs-lookup"><span data-stu-id="723e0-237">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="723e0-238">CORS ön kontrol isteği, sunucuya gerçek istekle gönderilen üstbilgileri belirten bir `Access-Control-Request-Headers` üst bilgisi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-238">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="723e0-239">Belirli üstbilgilere izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="723e0-239">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="723e0-240">Tüm yazar isteği üst bilgilerine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="723e0-240">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="723e0-241">Tarayıcılar, `Access-Control-Request-Headers`nasıl ayarlandıklarından tamamen tutarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="723e0-241">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="723e0-242">Üst bilgileri `"*"` dışında bir şeye ayarlarsanız (veya <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>kullanıyorsanız), en az `Accept`, `Content-Type`ve `Origin`, ayrıca desteklemek istediğiniz tüm özel üstbilgileri dahil etmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="723e0-242">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="723e0-243">Aşağıda, ön kontrol isteğine örnek bir yanıt verilmiştir (sunucunun isteğe izin verdiği varsayıldığında):</span><span class="sxs-lookup"><span data-stu-id="723e0-243">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="723e0-244">Yanıt, izin verilen üst bilgileri listeleyen, izin verilen yöntemleri ve isteğe bağlı olarak bir `Access-Control-Allow-Headers` üstbilgisini listeleyen bir `Access-Control-Allow-Methods` üst bilgisi içerir.</span><span class="sxs-lookup"><span data-stu-id="723e0-244">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="723e0-245">Ön kontrol isteği başarılı olursa, tarayıcı gerçek isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="723e0-245">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="723e0-246">Ön kontrol isteği reddedilirse, uygulama *200 ok* yanıtı döndürür ancak CORS üst bilgilerini geri göndermez.</span><span class="sxs-lookup"><span data-stu-id="723e0-246">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="723e0-247">Bu nedenle tarayıcı, çıkış noktaları arası isteği denemez.</span><span class="sxs-lookup"><span data-stu-id="723e0-247">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="723e0-248">Ön kontrol sona erme süresini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-248">Set the preflight expiration time</span></span>

<span data-ttu-id="723e0-249">`Access-Control-Max-Age` üstbilgisi, ön kontrol isteğine olan yanıtın ne kadar süreyle önbelleğe alınacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="723e0-249">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="723e0-250">Bu üstbilgiyi ayarlamak için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="723e0-250">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="723e0-251">CORS nasıl kullanılır?</span><span class="sxs-lookup"><span data-stu-id="723e0-251">How CORS works</span></span>

<span data-ttu-id="723e0-252">Bu bölümde, HTTP iletileri düzeyindeki bir [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) isteğinde ne olacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="723e0-252">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="723e0-253">CORS bir güvenlik özelliği **değil** .</span><span class="sxs-lookup"><span data-stu-id="723e0-253">CORS is **not** a security feature.</span></span> <span data-ttu-id="723e0-254">CORS, bir sunucunun aynı kaynaklı ilkeyi rahat bir şekilde sağlamasına izin veren bir W3C standardıdır.</span><span class="sxs-lookup"><span data-stu-id="723e0-254">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="723e0-255">Örneğin, kötü niyetli bir aktör sitenize karşı [siteler arası komut dosyalarını (XSS) engelliyor](xref:security/cross-site-scripting) ve bilgileri çalmak için CORS özellikli sitesine bir siteler arası istek yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-255">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="723e0-256">CORS 'nin izin vermesini sağlayan API 'niz daha güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="723e0-256">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="723e0-257">CORS 'yi zorlamak için istemciye (tarayıcı) sahiptir.</span><span class="sxs-lookup"><span data-stu-id="723e0-257">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="723e0-258">Sunucu isteği yürütür ve yanıtı döndürür. Bu, bir hatayı döndüren ve yanıtı engelleyen istemcdir.</span><span class="sxs-lookup"><span data-stu-id="723e0-258">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="723e0-259">Örneğin, aşağıdaki araçlardan herhangi birinde sunucu yanıtı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="723e0-259">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="723e0-260">Fiddler</span><span class="sxs-lookup"><span data-stu-id="723e0-260">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="723e0-261">Postman</span><span class="sxs-lookup"><span data-stu-id="723e0-261">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="723e0-262">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="723e0-262">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="723e0-263">Adres çubuğuna URL girerek bir Web tarayıcısı.</span><span class="sxs-lookup"><span data-stu-id="723e0-263">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="723e0-264">Bir sunucu, tarayıcıların çapraz kaynak [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) veya [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) isteği çalıştırmasına izin vermesinin yasaktır bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="723e0-264">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="723e0-265">Tarayıcılar (CORS olmadan), çapraz kaynak istekleri yapamıyor.</span><span class="sxs-lookup"><span data-stu-id="723e0-265">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="723e0-266">CORS 'den önce, bu kısıtlamayı aşmak için [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="723e0-266">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="723e0-267">JSONP XHR kullanmaz, yanıtı almak için `<script>` etiketini kullanır.</span><span class="sxs-lookup"><span data-stu-id="723e0-267">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="723e0-268">Betiklerin, çapraz kaynak olarak yüklenmesine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-268">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="723e0-269">[CORS belirtimi](https://www.w3.org/TR/cors/) , çıkış noktaları arası istekleri etkinleştiren bırkaç yeni http üst bilgisi sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="723e0-269">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="723e0-270">Bir tarayıcı CORS 'yi destekliyorsa, bu üst bilgileri, çıkış noktaları arası istekler için otomatik olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="723e0-270">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="723e0-271">CORS 'yi etkinleştirmek için özel JavaScript kodu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="723e0-271">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="723e0-272">Aşağıda, bir çapraz kaynak isteğine bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="723e0-272">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="723e0-273">`Origin` üstbilgisi, isteği yapan sitenin etki alanını sağlar.</span><span class="sxs-lookup"><span data-stu-id="723e0-273">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="723e0-274">`Origin` üst bilgisi gereklidir ve konaktan farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="723e0-274">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="723e0-275">Sunucu isteğe izin veriyorsa, yanıtta `Access-Control-Allow-Origin` üst bilgisini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="723e0-275">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="723e0-276">Bu üstbilginin değeri, istekten `Origin` üst bilgisiyle eşleşir veya `"*"`joker değerdir, yani herhangi bir kaynağa izin verilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-276">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="723e0-277">Yanıt `Access-Control-Allow-Origin` üst bilgisini içermiyorsa, çapraz kaynak isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="723e0-277">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="723e0-278">Özellikle, tarayıcı isteğe izin vermez.</span><span class="sxs-lookup"><span data-stu-id="723e0-278">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="723e0-279">Sunucu başarılı bir yanıt döndürse bile tarayıcı, yanıtı istemci uygulama için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="723e0-279">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="723e0-280">Test CORS</span><span class="sxs-lookup"><span data-stu-id="723e0-280">Test CORS</span></span>

<span data-ttu-id="723e0-281">CORS 'yi sınamak için:</span><span class="sxs-lookup"><span data-stu-id="723e0-281">To test CORS:</span></span>

1. <span data-ttu-id="723e0-282">[BIR API projesi oluşturun](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="723e0-282">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="723e0-283">Alternatif olarak, [örneği de indirebilirsiniz](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="723e0-283">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="723e0-284">Bu belgedeki yaklaşımlardan birini kullanarak CORS 'yi etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="723e0-284">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="723e0-285">Örnek:</span><span class="sxs-lookup"><span data-stu-id="723e0-285">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="723e0-286">`WithOrigins("https://localhost:<port>");`, yalnızca [indirme örnek koduna](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)benzer bir örnek uygulamanın test edilmesi için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="723e0-286">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="723e0-287">Web uygulaması projesi (Razor Pages veya MVC) oluşturun.</span><span class="sxs-lookup"><span data-stu-id="723e0-287">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="723e0-288">Örnek Razor Pages kullanır.</span><span class="sxs-lookup"><span data-stu-id="723e0-288">The sample uses Razor Pages.</span></span> <span data-ttu-id="723e0-289">Web uygulamasını, API projesiyle aynı çözümde oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723e0-289">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="723e0-290">Aşağıdaki Vurgulanan kodu *Index. cshtml* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="723e0-290">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="723e0-291">Yukarıdaki kodda `url: 'https://<web app>.azurewebsites.net/api/values/1',`, dağıtılan uygulamanın URL 'siyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="723e0-291">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="723e0-292">API projesini dağıtın.</span><span class="sxs-lookup"><span data-stu-id="723e0-292">Deploy the API project.</span></span> <span data-ttu-id="723e0-293">Örneğin, [Azure 'a dağıtın](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="723e0-293">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="723e0-294">Razor Pages veya MVC uygulamasını masaüstünden çalıştırın ve **Test** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="723e0-294">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="723e0-295">Hata iletilerini gözden geçirmek için F12 araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="723e0-295">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="723e0-296">`WithOrigins` 'ten localhost kaynağını kaldırın ve uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="723e0-296">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="723e0-297">Alternatif olarak, istemci uygulamasını farklı bir bağlantı noktasıyla çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="723e0-297">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="723e0-298">Örneğin, Visual Studio 'dan çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="723e0-298">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="723e0-299">İstemci uygulamasıyla test edin.</span><span class="sxs-lookup"><span data-stu-id="723e0-299">Test with the client app.</span></span> <span data-ttu-id="723e0-300">CORS hataları bir hata döndürüyor, ancak hata iletisi JavaScript için kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="723e0-300">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="723e0-301">Hatayı görmek için F12 araçlarındaki konsol sekmesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="723e0-301">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="723e0-302">Tarayıcıya bağlı olarak, aşağıdakine benzer bir hata alırsınız (F12 araçları konsolunda):</span><span class="sxs-lookup"><span data-stu-id="723e0-302">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="723e0-303">Microsoft Edge 'i kullanarak:</span><span class="sxs-lookup"><span data-stu-id="723e0-303">Using Microsoft Edge:</span></span>

     <span data-ttu-id="723e0-304">**SEC7120: [CORS] kaynak `https://localhost:44375`, `https://webapi.azurewebsites.net/api/values/1` konumundaki çapraz kaynak kaynağı için Access-Control-Allow-Origin yanıt üstbilgisinde `https://localhost:44375` bulamadı**</span><span class="sxs-lookup"><span data-stu-id="723e0-304">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="723e0-305">Chrome kullanarak:</span><span class="sxs-lookup"><span data-stu-id="723e0-305">Using Chrome:</span></span>

     <span data-ttu-id="723e0-306">**`https://localhost:44375` kaynaktan `https://webapi.azurewebsites.net/api/values/1` konumundaki XMLHttpRequest erişimi CORS ilkesi tarafından engellendi: İstenen kaynakta hiçbir ' erişim-denetim-Izin-Origin ' üst bilgisi yok.**</span><span class="sxs-lookup"><span data-stu-id="723e0-306">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="723e0-307">CORS özellikli uç noktalar [Fiddler](https://www.telerik.com/fiddler) veya [Postman](https://www.getpostman.com/)gibi bir araçla test edilebilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-307">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="723e0-308">Bir araç kullanırken, `Origin` üstbilgisi tarafından belirtilen isteğin kaynağı isteği alan konaktan farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="723e0-308">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="723e0-309">İstek, `Origin` üst bilgisinin değerine göre *Çıkış dışı* değilse:</span><span class="sxs-lookup"><span data-stu-id="723e0-309">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="723e0-310">CORS ara yazılımı için isteği işleme gereksinimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="723e0-310">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="723e0-311">CORS üstbilgileri yanıtta döndürülmedi.</span><span class="sxs-lookup"><span data-stu-id="723e0-311">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="723e0-312">IIS 'de CORS</span><span class="sxs-lookup"><span data-stu-id="723e0-312">CORS in IIS</span></span>

<span data-ttu-id="723e0-313">IIS 'ye dağıtım yaparken, sunucu anonim erişime izin verecek şekilde yapılandırılmamışsa CORS, Windows kimlik doğrulamasından önce çalıştırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="723e0-313">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="723e0-314">Bu senaryoyu desteklemek için, [IIS CORS modülünün](https://www.iis.net/downloads/microsoft/iis-cors-module) uygulama için yüklenmiş ve yapılandırılmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="723e0-314">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="723e0-315">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="723e0-315">Additional resources</span></span>

* [<span data-ttu-id="723e0-316">Çıkış noktaları arası kaynak paylaşımı (CORS)</span><span class="sxs-lookup"><span data-stu-id="723e0-316">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="723e0-317">IIS CORS modülünü kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="723e0-317">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="723e0-318">Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="723e0-318">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="723e0-319">Bu makalede, ASP.NET Core uygulamasında CORS 'nin nasıl etkinleştirileceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="723e0-319">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="723e0-320">Tarayıcı güvenliği, bir Web sayfasının Web sayfasını sunduğundan farklı bir etki alanına istek yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="723e0-320">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="723e0-321">Bu kısıtlamaya *aynı-Origin ilkesi*adı verilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-321">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="723e0-322">Aynı-kaynak ilkesi, kötü niyetli bir sitenin gizli verileri başka bir siteden okumasını engeller.</span><span class="sxs-lookup"><span data-stu-id="723e0-322">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="723e0-323">Bazen diğer sitelerin uygulamanıza çapraz çıkış istekleri yapmasına izin vermek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723e0-323">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="723e0-324">Daha fazla bilgi için bkz. [MOZILLA CORS makalesi](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="723e0-324">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="723e0-325">[Çapraz kaynak kaynak paylaşımı](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="723e0-325">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="723e0-326">, Bir sunucunun aynı kaynak ilkeyi rahat bir şekilde sağlamasına izin veren bir W3C standardıdır.</span><span class="sxs-lookup"><span data-stu-id="723e0-326">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="723e0-327">Güvenlik özelliği **değil** , CORS güvenliği.</span><span class="sxs-lookup"><span data-stu-id="723e0-327">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="723e0-328">CORS 'nin izin vermesini sağlayan bir API daha güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="723e0-328">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="723e0-329">Daha fazla bilgi için bkz. [CORS çalışma](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="723e0-329">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="723e0-330">Bir sunucunun bazı çapraz kaynak isteklerine, diğerlerini reddetirken açık olarak izin almasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="723e0-330">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="723e0-331">, [JSONP](/dotnet/framework/wcf/samples/jsonp)gibi önceki tekniklerin daha güvenli ve daha esnektir.</span><span class="sxs-lookup"><span data-stu-id="723e0-331">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="723e0-332">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="723e0-332">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="723e0-333">Aynı kaynak</span><span class="sxs-lookup"><span data-stu-id="723e0-333">Same origin</span></span>

<span data-ttu-id="723e0-334">Özdeş şemaları, konakları ve bağlantı noktalarına sahip olmaları durumunda iki URL aynı kaynağa sahiptir ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="723e0-334">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="723e0-335">Bu iki URL aynı kaynağa sahiptir:</span><span class="sxs-lookup"><span data-stu-id="723e0-335">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="723e0-336">Bu URL 'Ler, önceki iki URL 'den farklı kaynaklardan farklıdır:</span><span class="sxs-lookup"><span data-stu-id="723e0-336">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="723e0-337">Farklı etki alanı &ndash; `https://example.net`</span><span class="sxs-lookup"><span data-stu-id="723e0-337">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="723e0-338">Farklı alt etki alanı &ndash; `https://www.example.com/foo.html`</span><span class="sxs-lookup"><span data-stu-id="723e0-338">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="723e0-339">Farklı &ndash; düzeni `http://example.com/foo.html`</span><span class="sxs-lookup"><span data-stu-id="723e0-339">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="723e0-340">Farklı bağlantı noktası &ndash; `https://example.com:9000/foo.html`</span><span class="sxs-lookup"><span data-stu-id="723e0-340">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="723e0-341">Internet Explorer, kaynakları karşılaştırırken bağlantı noktasını dikkate almaz.</span><span class="sxs-lookup"><span data-stu-id="723e0-341">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="723e0-342">Adlandırılmış ilke ve ara yazılım ile CORS</span><span class="sxs-lookup"><span data-stu-id="723e0-342">CORS with named policy and middleware</span></span>

<span data-ttu-id="723e0-343">CORS ara yazılımı, çıkış noktaları arası istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="723e0-343">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="723e0-344">Aşağıdaki kod, belirtilen kaynağa sahip tüm uygulama için CORS 'yi sunar:</span><span class="sxs-lookup"><span data-stu-id="723e0-344">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="723e0-345">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="723e0-345">The preceding code:</span></span>

* <span data-ttu-id="723e0-346">İlke adını "\_Myallowspecifickaynaklarına" olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="723e0-346">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="723e0-347">İlke adı rastgele olur.</span><span class="sxs-lookup"><span data-stu-id="723e0-347">The policy name is arbitrary.</span></span>
* <span data-ttu-id="723e0-348">CORS 'yi sağlayan <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> uzantısı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="723e0-348">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="723e0-349">[Lambda ifadesiyle](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="723e0-349">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="723e0-350">Lambda bir <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> nesnesi alır.</span><span class="sxs-lookup"><span data-stu-id="723e0-350">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="723e0-351">`WithOrigins`gibi [yapılandırma seçenekleri](#cors-policy-options), bu makalenin ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="723e0-351">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="723e0-352"><xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> yöntemi çağrısı, uygulamanın hizmet kapsayıcısına CORS Hizmetleri ekler:</span><span class="sxs-lookup"><span data-stu-id="723e0-352">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="723e0-353">Daha fazla bilgi için bu belgedeki [CORS ilke seçenekleri](#cpo) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="723e0-353">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="723e0-354"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> yöntemi aşağıdaki kodda gösterildiği gibi yöntemleri zincirleyebilir:</span><span class="sxs-lookup"><span data-stu-id="723e0-354">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="723e0-355">Not: URL sonunda eğik çizgi (`/` **) bulunmamalıdır.**</span><span class="sxs-lookup"><span data-stu-id="723e0-355">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="723e0-356">URL `/`ile sonlandığında, karşılaştırma `false` döndürür ve üst bilgi döndürülmez.</span><span class="sxs-lookup"><span data-stu-id="723e0-356">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="723e0-357">Aşağıdaki kod, CORS ara yazılımı aracılığıyla CORS ilkelerini tüm uygulama uç noktalarına uygular:</span><span class="sxs-lookup"><span data-stu-id="723e0-357">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors();

    app.UseHttpsRedirection();
    app.UseMvc();
}
```
<span data-ttu-id="723e0-358">Note: `UseMvc`önce `UseCors` çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="723e0-358">Note: `UseCors` must be called before `UseMvc`.</span></span>

<span data-ttu-id="723e0-359">Bu sayfa/denetleyici/eylem düzeyinde CORS ilkesini uygulamak için [Razor Pages, denetleyiciler ve eylem YÖNTEMLERINDE CORS 'Yi etkinleştirin](#ecors) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="723e0-359">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="723e0-360">Önceki kodun test edilmesine ilişkin yönergeler için bkz. [Test CORS](#test) .</span><span class="sxs-lookup"><span data-stu-id="723e0-360">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="723e0-361">CORS 'yi özniteliklerle etkinleştir</span><span class="sxs-lookup"><span data-stu-id="723e0-361">Enable CORS with attributes</span></span>

<span data-ttu-id="723e0-362">[&lbrack;enablecors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) ÖZNITELIĞI, CORS 'yi küresel olarak uygulamaya bir alternatif sağlar.</span><span class="sxs-lookup"><span data-stu-id="723e0-362">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="723e0-363">`[EnableCors]` özniteliği, tüm bitiş noktaları yerine, seçilen bitiş noktaları için CORS 'yi mümkün.</span><span class="sxs-lookup"><span data-stu-id="723e0-363">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="723e0-364">İlke belirtmek için varsayılan ilkeyi ve `[EnableCors("{Policy String}")]` belirtmek için `[EnableCors]` kullanın.</span><span class="sxs-lookup"><span data-stu-id="723e0-364">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="723e0-365">`[EnableCors]` özniteliği şu şekilde uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="723e0-365">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="723e0-366">Razor sayfası `PageModel`</span><span class="sxs-lookup"><span data-stu-id="723e0-366">Razor Page `PageModel`</span></span>
* <span data-ttu-id="723e0-367">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="723e0-367">Controller</span></span>
* <span data-ttu-id="723e0-368">Denetleyici eylemi yöntemi</span><span class="sxs-lookup"><span data-stu-id="723e0-368">Controller action method</span></span>

<span data-ttu-id="723e0-369">`[EnableCors]` özniteliğiyle, denetleyici/sayfa-model/eyleme farklı ilkeler uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723e0-369">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="723e0-370">`[EnableCors]` özniteliği bir denetleyiciler/sayfa modeli/eylem yöntemine uygulandığında ve bir ara yazılım içinde CORS etkinleştirildiğinde her iki ilke de uygulanır.</span><span class="sxs-lookup"><span data-stu-id="723e0-370">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="723e0-371">İlkelerin birleştirilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-371">We recommend against combining policies.</span></span> <span data-ttu-id="723e0-372">Aynı uygulamada değil, `[EnableCors]` özniteliği veya ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="723e0-372">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="723e0-373">Aşağıdaki kod her bir yönteme farklı bir ilke uygular:</span><span class="sxs-lookup"><span data-stu-id="723e0-373">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="723e0-374">Aşağıdaki kod, bir CORS varsayılan ilkesi ve `"AnotherPolicy"`adlı bir ilke oluşturur:</span><span class="sxs-lookup"><span data-stu-id="723e0-374">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="723e0-375">CORS 'yi devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="723e0-375">Disable CORS</span></span>

<span data-ttu-id="723e0-376">[&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) özniteliği denetleyici/sayfa modeli/eylem için CORS 'yi devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="723e0-376">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="723e0-377">CORS ilke seçenekleri</span><span class="sxs-lookup"><span data-stu-id="723e0-377">CORS policy options</span></span>

<span data-ttu-id="723e0-378">Bu bölümde, bir CORS ilkesinde ayarlanmakta olabilecek çeşitli seçenekler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="723e0-378">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="723e0-379">İzin verilen kaynakları ayarla</span><span class="sxs-lookup"><span data-stu-id="723e0-379">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="723e0-380">İzin verilen HTTP yöntemlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-380">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="723e0-381">İzin verilen istek üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-381">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="723e0-382">Gösterilen yanıt üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-382">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="723e0-383">Kaynaklar arası isteklerde kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="723e0-383">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="723e0-384">Ön kontrol sona erme süresini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-384">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="723e0-385"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> `Startup.ConfigureServices`olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="723e0-385"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="723e0-386">Bazı seçenekler için, ilk olarak [CORS 'Nin nasıl çalıştığı](#how-cors) bölümü okumanız yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-386">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="723e0-387">İzin verilen kaynakları ayarla</span><span class="sxs-lookup"><span data-stu-id="723e0-387">Set the allowed origins</span></span>

<span data-ttu-id="723e0-388"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; tüm kaynaklardan gelen CORS isteklerinin herhangi bir düzene (`http` veya `https`) Izin verir.</span><span class="sxs-lookup"><span data-stu-id="723e0-388"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="723e0-389">*herhangi bir Web sitesi* uygulamaya çapraz kaynak istekleri yapabildiğinden `AllowAnyOrigin` güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="723e0-389">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="723e0-390">`AllowAnyOrigin` ve `AllowCredentials` belirtme, güvenli olmayan bir yapılandırmadır ve siteler arası istek elde edilmesine yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-390">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="723e0-391">Güvenli bir uygulama için, istemcinin sunucu kaynaklarına erişim yetkisi olması gerekiyorsa, kaynakların tam bir listesini belirtin.</span><span class="sxs-lookup"><span data-stu-id="723e0-391">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

<span data-ttu-id="723e0-392">`AllowAnyOrigin`, ön kontrol isteklerini ve `Access-Control-Allow-Origin` üst bilgisini etkiler.</span><span class="sxs-lookup"><span data-stu-id="723e0-392">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="723e0-393">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="723e0-393">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="723e0-394"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash;, kaynağın <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> özelliğini, kaynağa izin verilip verilmediğini değerlendirirken, kaynağın, yapılandırılan bir joker karakterle eşleştiğinden emin olan bir işlev olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="723e0-394"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="723e0-395">İzin verilen HTTP yöntemlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-395">Set the allowed HTTP methods</span></span>

<span data-ttu-id="723e0-396"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="723e0-396"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="723e0-397">Herhangi bir HTTP yöntemine izin verir:</span><span class="sxs-lookup"><span data-stu-id="723e0-397">Allows any HTTP method:</span></span>
* <span data-ttu-id="723e0-398">Ön kontrol isteklerini ve `Access-Control-Allow-Methods` üst bilgisini etkiler.</span><span class="sxs-lookup"><span data-stu-id="723e0-398">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="723e0-399">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="723e0-399">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="723e0-400">İzin verilen istek üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-400">Set the allowed request headers</span></span>

<span data-ttu-id="723e0-401">Belirli başlıkların, *Yazar istek üstbilgileri*ADLı bir CORS isteğinde gönderilmesine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> çağırın ve izin verilen üst bilgileri belirtin:</span><span class="sxs-lookup"><span data-stu-id="723e0-401">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="723e0-402">Tüm yazar isteği üst bilgilerine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="723e0-402">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="723e0-403">Bu ayar, ön kontrol isteklerini ve `Access-Control-Request-Headers` üst bilgisini etkiler.</span><span class="sxs-lookup"><span data-stu-id="723e0-403">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="723e0-404">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="723e0-404">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="723e0-405">CORS ara yazılımı, CorsPolicy. Headers ' de yapılandırılan değerlere bakılmaksızın `Access-Control-Request-Headers` her zaman dört üst bilgilerin gönderilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="723e0-405">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="723e0-406">Bu üst bilgi listesi şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="723e0-406">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="723e0-407">Örneğin, aşağıdaki gibi yapılandırılmış bir uygulamayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="723e0-407">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="723e0-408">`Content-Language` her zaman beyaz listeye alındığından, CORS ara yazılımı aşağıdaki istek üstbilgisiyle bir ön kontrol isteğine başarıyla yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="723e0-408">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="723e0-409">Gösterilen yanıt üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-409">Set the exposed response headers</span></span>

<span data-ttu-id="723e0-410">Varsayılan olarak tarayıcı, tüm yanıt üst bilgilerini uygulamaya sunmaz.</span><span class="sxs-lookup"><span data-stu-id="723e0-410">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="723e0-411">Daha fazla bilgi için bkz. [W3C çıkış noktaları arası kaynak paylaşımı (terminoloji): basit yanıt üst bilgisi](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="723e0-411">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="723e0-412">Varsayılan olarak kullanılabilen yanıt üstbilgileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="723e0-412">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="723e0-413">CORS belirtimi, bu üst bilgiler *basit yanıt üst bilgilerini*çağırır.</span><span class="sxs-lookup"><span data-stu-id="723e0-413">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="723e0-414">Diğer üst bilgileri uygulama için kullanılabilir hale getirmek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="723e0-414">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="723e0-415">Kaynaklar arası isteklerde kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="723e0-415">Credentials in cross-origin requests</span></span>

<span data-ttu-id="723e0-416">Kimlik bilgileri CORS isteğinde özel işleme gerektirir.</span><span class="sxs-lookup"><span data-stu-id="723e0-416">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="723e0-417">Varsayılan olarak tarayıcı, kimlik bilgilerini bir çapraz kaynak isteğiyle göndermez.</span><span class="sxs-lookup"><span data-stu-id="723e0-417">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="723e0-418">Kimlik bilgileri, tanımlama bilgileri ve HTTP kimlik doğrulama düzenlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="723e0-418">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="723e0-419">Bir çapraz kaynak isteğiyle kimlik bilgilerini göndermek için, istemcisinin `true``XMLHttpRequest.withCredentials` ayarlaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="723e0-419">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="723e0-420">`XMLHttpRequest` doğrudan kullanarak:</span><span class="sxs-lookup"><span data-stu-id="723e0-420">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="723e0-421">JQuery kullanarak:</span><span class="sxs-lookup"><span data-stu-id="723e0-421">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="723e0-422">[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)'sini kullanma:</span><span class="sxs-lookup"><span data-stu-id="723e0-422">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="723e0-423">Sunucu kimlik bilgilerine izin vermelidir.</span><span class="sxs-lookup"><span data-stu-id="723e0-423">The server must allow the credentials.</span></span> <span data-ttu-id="723e0-424">Çıkış noktaları arası kimlik bilgilerine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="723e0-424">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="723e0-425">HTTP yanıtı, tarayıcıya sunucunun bir çapraz kaynak isteği için kimlik bilgileri verdiğini bildiren bir `Access-Control-Allow-Credentials` üst bilgisi içerir.</span><span class="sxs-lookup"><span data-stu-id="723e0-425">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="723e0-426">Tarayıcı kimlik bilgilerini gönderirse ancak yanıt geçerli bir `Access-Control-Allow-Credentials` üst bilgisi içermiyorsa, tarayıcı uygulamaya yanıtı kullanıma sunmaz ve çapraz kaynak isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="723e0-426">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="723e0-427">Çıkış noktaları arası kimlik bilgilerine izin vermek bir güvenlik riskidir.</span><span class="sxs-lookup"><span data-stu-id="723e0-427">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="723e0-428">Başka bir etki alanındaki Web sitesi, kullanıcının bilgisi olmadan kullanıcı adına, oturum açmış bir kullanıcının kimlik bilgilerini uygulamaya gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-428">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="723e0-429">CORS belirtimi Ayrıca, `Access-Control-Allow-Credentials` üst bilgisi varsa, `"*"` (tüm kaynaklar) için çıkış ayarının geçersiz olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="723e0-429">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="723e0-430">Ön kontrol istekleri</span><span class="sxs-lookup"><span data-stu-id="723e0-430">Preflight requests</span></span>

<span data-ttu-id="723e0-431">Bazı CORS istekleri için, tarayıcı gerçek isteği yapmadan önce ek bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="723e0-431">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="723e0-432">Bu isteğe bir *ön kontrol isteği*denir.</span><span class="sxs-lookup"><span data-stu-id="723e0-432">This request is called a *preflight request*.</span></span> <span data-ttu-id="723e0-433">Aşağıdaki koşullar doğruysa tarayıcı, ön kontrol isteğini atlayabilir:</span><span class="sxs-lookup"><span data-stu-id="723e0-433">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="723e0-434">İstek yöntemi al, HEAD veya POST.</span><span class="sxs-lookup"><span data-stu-id="723e0-434">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="723e0-435">Uygulama `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`veya `Last-Event-ID`dışındaki istek üst bilgilerini ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="723e0-435">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="723e0-436">Eğer ayarlandıysa `Content-Type` üst bilgisi aşağıdaki değerlerden birine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="723e0-436">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="723e0-437">İstemci isteği için ayarlanan istek üst bilgileri kuralı, `XMLHttpRequest` nesnesine `setRequestHeader` çağırarak uygulamanın ayarladığı üst bilgiler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="723e0-437">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="723e0-438">CORS belirtimi, bu üst bilgiler *Yazar istek üst bilgilerini*çağırır.</span><span class="sxs-lookup"><span data-stu-id="723e0-438">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="723e0-439">Kural, tarayıcının ayarlayabilmesi için `User-Agent`, `Host`veya `Content-Length`gibi üstbilgilere uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="723e0-439">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="723e0-440">Aşağıda bir ön denetim isteğine örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="723e0-440">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="723e0-441">Uçuş öncesi isteği HTTP SEÇENEKLERI yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="723e0-441">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="723e0-442">İki özel üst bilgi içerir:</span><span class="sxs-lookup"><span data-stu-id="723e0-442">It includes two special headers:</span></span>

* <span data-ttu-id="723e0-443">`Access-Control-Request-Method`: gerçek istek için kullanılacak HTTP yöntemi.</span><span class="sxs-lookup"><span data-stu-id="723e0-443">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="723e0-444">`Access-Control-Request-Headers`: uygulamanın gerçek istekte ayarladığı istek üst bilgilerinin bir listesi.</span><span class="sxs-lookup"><span data-stu-id="723e0-444">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="723e0-445">Daha önce belirtildiği gibi, bu, tarayıcının ayarladığı `User-Agent`gibi üst bilgileri içermez.</span><span class="sxs-lookup"><span data-stu-id="723e0-445">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="723e0-446">CORS ön kontrol isteği, sunucuya gerçek istekle gönderilen üstbilgileri belirten bir `Access-Control-Request-Headers` üst bilgisi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-446">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="723e0-447">Belirli üstbilgilere izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="723e0-447">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="723e0-448">Tüm yazar isteği üst bilgilerine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="723e0-448">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="723e0-449">Tarayıcılar, `Access-Control-Request-Headers`nasıl ayarlandıklarından tamamen tutarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="723e0-449">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="723e0-450">Üst bilgileri `"*"` dışında bir şeye ayarlarsanız (veya <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>kullanıyorsanız), en az `Accept`, `Content-Type`ve `Origin`, ayrıca desteklemek istediğiniz tüm özel üstbilgileri dahil etmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="723e0-450">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="723e0-451">Aşağıda, ön kontrol isteğine örnek bir yanıt verilmiştir (sunucunun isteğe izin verdiği varsayıldığında):</span><span class="sxs-lookup"><span data-stu-id="723e0-451">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="723e0-452">Yanıt, izin verilen üst bilgileri listeleyen, izin verilen yöntemleri ve isteğe bağlı olarak bir `Access-Control-Allow-Headers` üstbilgisini listeleyen bir `Access-Control-Allow-Methods` üst bilgisi içerir.</span><span class="sxs-lookup"><span data-stu-id="723e0-452">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="723e0-453">Ön kontrol isteği başarılı olursa, tarayıcı gerçek isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="723e0-453">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="723e0-454">Ön kontrol isteği reddedilirse, uygulama *200 ok* yanıtı döndürür ancak CORS üst bilgilerini geri göndermez.</span><span class="sxs-lookup"><span data-stu-id="723e0-454">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="723e0-455">Bu nedenle tarayıcı, çıkış noktaları arası isteği denemez.</span><span class="sxs-lookup"><span data-stu-id="723e0-455">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="723e0-456">Ön kontrol sona erme süresini ayarlama</span><span class="sxs-lookup"><span data-stu-id="723e0-456">Set the preflight expiration time</span></span>

<span data-ttu-id="723e0-457">`Access-Control-Max-Age` üstbilgisi, ön kontrol isteğine olan yanıtın ne kadar süreyle önbelleğe alınacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="723e0-457">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="723e0-458">Bu üstbilgiyi ayarlamak için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="723e0-458">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="723e0-459">CORS nasıl kullanılır?</span><span class="sxs-lookup"><span data-stu-id="723e0-459">How CORS works</span></span>

<span data-ttu-id="723e0-460">Bu bölümde, HTTP iletileri düzeyindeki bir [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) isteğinde ne olacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="723e0-460">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="723e0-461">CORS bir güvenlik özelliği **değil** .</span><span class="sxs-lookup"><span data-stu-id="723e0-461">CORS is **not** a security feature.</span></span> <span data-ttu-id="723e0-462">CORS, bir sunucunun aynı kaynaklı ilkeyi rahat bir şekilde sağlamasına izin veren bir W3C standardıdır.</span><span class="sxs-lookup"><span data-stu-id="723e0-462">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="723e0-463">Örneğin, kötü niyetli bir aktör sitenize karşı [siteler arası komut dosyalarını (XSS) engelliyor](xref:security/cross-site-scripting) ve bilgileri çalmak için CORS özellikli sitesine bir siteler arası istek yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-463">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="723e0-464">CORS 'nin izin vermesini sağlayan API 'niz daha güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="723e0-464">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="723e0-465">CORS 'yi zorlamak için istemciye (tarayıcı) sahiptir.</span><span class="sxs-lookup"><span data-stu-id="723e0-465">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="723e0-466">Sunucu isteği yürütür ve yanıtı döndürür. Bu, bir hatayı döndüren ve yanıtı engelleyen istemcdir.</span><span class="sxs-lookup"><span data-stu-id="723e0-466">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="723e0-467">Örneğin, aşağıdaki araçlardan herhangi birinde sunucu yanıtı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="723e0-467">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="723e0-468">Fiddler</span><span class="sxs-lookup"><span data-stu-id="723e0-468">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="723e0-469">Postman</span><span class="sxs-lookup"><span data-stu-id="723e0-469">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="723e0-470">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="723e0-470">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="723e0-471">Adres çubuğuna URL girerek bir Web tarayıcısı.</span><span class="sxs-lookup"><span data-stu-id="723e0-471">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="723e0-472">Bir sunucu, tarayıcıların çapraz kaynak [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) veya [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) isteği çalıştırmasına izin vermesinin yasaktır bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="723e0-472">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="723e0-473">Tarayıcılar (CORS olmadan), çapraz kaynak istekleri yapamıyor.</span><span class="sxs-lookup"><span data-stu-id="723e0-473">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="723e0-474">CORS 'den önce, bu kısıtlamayı aşmak için [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="723e0-474">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="723e0-475">JSONP XHR kullanmaz, yanıtı almak için `<script>` etiketini kullanır.</span><span class="sxs-lookup"><span data-stu-id="723e0-475">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="723e0-476">Betiklerin, çapraz kaynak olarak yüklenmesine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-476">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="723e0-477">[CORS belirtimi](https://www.w3.org/TR/cors/) , çıkış noktaları arası istekleri etkinleştiren bırkaç yeni http üst bilgisi sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="723e0-477">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="723e0-478">Bir tarayıcı CORS 'yi destekliyorsa, bu üst bilgileri, çıkış noktaları arası istekler için otomatik olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="723e0-478">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="723e0-479">CORS 'yi etkinleştirmek için özel JavaScript kodu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="723e0-479">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="723e0-480">Aşağıda, bir çapraz kaynak isteğine bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="723e0-480">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="723e0-481">`Origin` üstbilgisi, isteği yapan sitenin etki alanını sağlar.</span><span class="sxs-lookup"><span data-stu-id="723e0-481">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="723e0-482">`Origin` üst bilgisi gereklidir ve konaktan farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="723e0-482">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="723e0-483">Sunucu isteğe izin veriyorsa, yanıtta `Access-Control-Allow-Origin` üst bilgisini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="723e0-483">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="723e0-484">Bu üstbilginin değeri, istekten `Origin` üst bilgisiyle eşleşir veya `"*"`joker değerdir, yani herhangi bir kaynağa izin verilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-484">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="723e0-485">Yanıt `Access-Control-Allow-Origin` üst bilgisini içermiyorsa, çapraz kaynak isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="723e0-485">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="723e0-486">Özellikle, tarayıcı isteğe izin vermez.</span><span class="sxs-lookup"><span data-stu-id="723e0-486">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="723e0-487">Sunucu başarılı bir yanıt döndürse bile tarayıcı, yanıtı istemci uygulama için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="723e0-487">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="723e0-488">Test CORS</span><span class="sxs-lookup"><span data-stu-id="723e0-488">Test CORS</span></span>

<span data-ttu-id="723e0-489">CORS 'yi sınamak için:</span><span class="sxs-lookup"><span data-stu-id="723e0-489">To test CORS:</span></span>

1. <span data-ttu-id="723e0-490">[BIR API projesi oluşturun](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="723e0-490">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="723e0-491">Alternatif olarak, [örneği de indirebilirsiniz](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="723e0-491">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="723e0-492">Bu belgedeki yaklaşımlardan birini kullanarak CORS 'yi etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="723e0-492">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="723e0-493">Örnek:</span><span class="sxs-lookup"><span data-stu-id="723e0-493">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="723e0-494">`WithOrigins("https://localhost:<port>");`, yalnızca [indirme örnek koduna](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)benzer bir örnek uygulamanın test edilmesi için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="723e0-494">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="723e0-495">Web uygulaması projesi (Razor Pages veya MVC) oluşturun.</span><span class="sxs-lookup"><span data-stu-id="723e0-495">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="723e0-496">Örnek Razor Pages kullanır.</span><span class="sxs-lookup"><span data-stu-id="723e0-496">The sample uses Razor Pages.</span></span> <span data-ttu-id="723e0-497">Web uygulamasını, API projesiyle aynı çözümde oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723e0-497">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="723e0-498">Aşağıdaki Vurgulanan kodu *Index. cshtml* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="723e0-498">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="723e0-499">Yukarıdaki kodda `url: 'https://<web app>.azurewebsites.net/api/values/1',`, dağıtılan uygulamanın URL 'siyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="723e0-499">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="723e0-500">API projesini dağıtın.</span><span class="sxs-lookup"><span data-stu-id="723e0-500">Deploy the API project.</span></span> <span data-ttu-id="723e0-501">Örneğin, [Azure 'a dağıtın](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="723e0-501">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="723e0-502">Razor Pages veya MVC uygulamasını masaüstünden çalıştırın ve **Test** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="723e0-502">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="723e0-503">Hata iletilerini gözden geçirmek için F12 araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="723e0-503">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="723e0-504">`WithOrigins` 'ten localhost kaynağını kaldırın ve uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="723e0-504">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="723e0-505">Alternatif olarak, istemci uygulamasını farklı bir bağlantı noktasıyla çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="723e0-505">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="723e0-506">Örneğin, Visual Studio 'dan çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="723e0-506">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="723e0-507">İstemci uygulamasıyla test edin.</span><span class="sxs-lookup"><span data-stu-id="723e0-507">Test with the client app.</span></span> <span data-ttu-id="723e0-508">CORS hataları bir hata döndürüyor, ancak hata iletisi JavaScript için kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="723e0-508">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="723e0-509">Hatayı görmek için F12 araçlarındaki konsol sekmesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="723e0-509">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="723e0-510">Tarayıcıya bağlı olarak, aşağıdakine benzer bir hata alırsınız (F12 araçları konsolunda):</span><span class="sxs-lookup"><span data-stu-id="723e0-510">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="723e0-511">Microsoft Edge 'i kullanarak:</span><span class="sxs-lookup"><span data-stu-id="723e0-511">Using Microsoft Edge:</span></span>

     <span data-ttu-id="723e0-512">**SEC7120: [CORS] kaynak `https://localhost:44375`, `https://webapi.azurewebsites.net/api/values/1` konumundaki çapraz kaynak kaynağı için Access-Control-Allow-Origin yanıt üstbilgisinde `https://localhost:44375` bulamadı**</span><span class="sxs-lookup"><span data-stu-id="723e0-512">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="723e0-513">Chrome kullanarak:</span><span class="sxs-lookup"><span data-stu-id="723e0-513">Using Chrome:</span></span>

     <span data-ttu-id="723e0-514">**`https://localhost:44375` kaynaktan `https://webapi.azurewebsites.net/api/values/1` konumundaki XMLHttpRequest erişimi CORS ilkesi tarafından engellendi: İstenen kaynakta hiçbir ' erişim-denetim-Izin-Origin ' üst bilgisi yok.**</span><span class="sxs-lookup"><span data-stu-id="723e0-514">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="723e0-515">CORS özellikli uç noktalar [Fiddler](https://www.telerik.com/fiddler) veya [Postman](https://www.getpostman.com/)gibi bir araçla test edilebilir.</span><span class="sxs-lookup"><span data-stu-id="723e0-515">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="723e0-516">Bir araç kullanırken, `Origin` üstbilgisi tarafından belirtilen isteğin kaynağı isteği alan konaktan farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="723e0-516">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="723e0-517">İstek, `Origin` üst bilgisinin değerine göre *Çıkış dışı* değilse:</span><span class="sxs-lookup"><span data-stu-id="723e0-517">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="723e0-518">CORS ara yazılımı için isteği işleme gereksinimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="723e0-518">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="723e0-519">CORS üstbilgileri yanıtta döndürülmedi.</span><span class="sxs-lookup"><span data-stu-id="723e0-519">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="723e0-520">IIS 'de CORS</span><span class="sxs-lookup"><span data-stu-id="723e0-520">CORS in IIS</span></span>

<span data-ttu-id="723e0-521">IIS 'ye dağıtım yaparken, sunucu anonim erişime izin verecek şekilde yapılandırılmamışsa CORS, Windows kimlik doğrulamasından önce çalıştırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="723e0-521">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="723e0-522">Bu senaryoyu desteklemek için, [IIS CORS modülünün](https://www.iis.net/downloads/microsoft/iis-cors-module) uygulama için yüklenmiş ve yapılandırılmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="723e0-522">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="723e0-523">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="723e0-523">Additional resources</span></span>

* [<span data-ttu-id="723e0-524">Çıkış noktaları arası kaynak paylaşımı (CORS)</span><span class="sxs-lookup"><span data-stu-id="723e0-524">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="723e0-525">IIS CORS modülünü kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="723e0-525">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end