---
title: ASP.NET Core 'de çıkış noktaları arası Istekleri (CORS) etkinleştirme
author: rick-anderson
description: ASP.NET Core uygulamasında çapraz kaynak isteklerini izin vermek veya reddetmek için CORS 'nin nasıl standart olduğunu öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/13/2019
uid: security/cors
ms.openlocfilehash: 3a51d365626c858ad48298a1108e37eba9050fe7
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391301"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="689cd-103">ASP.NET Core 'de çıkış noktaları arası Istekleri (CORS) etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="689cd-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="689cd-104">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="689cd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="689cd-105">Bu makalede, ASP.NET Core uygulamasında CORS 'nin nasıl etkinleştirileceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="689cd-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="689cd-106">Tarayıcı güvenliği, bir Web sayfasının Web sayfasını sunduğundan farklı bir etki alanına istek yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="689cd-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="689cd-107">Bu kısıtlamaya *aynı-Origin ilkesi*adı verilir.</span><span class="sxs-lookup"><span data-stu-id="689cd-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="689cd-108">Aynı-kaynak ilkesi, kötü niyetli bir sitenin gizli verileri başka bir siteden okumasını engeller.</span><span class="sxs-lookup"><span data-stu-id="689cd-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="689cd-109">Bazen diğer sitelerin uygulamanıza çapraz çıkış istekleri yapmasına izin vermek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="689cd-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="689cd-110">Daha fazla bilgi için bkz. [MOZILLA CORS makalesi](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="689cd-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="689cd-111">[Çapraz kaynak kaynak paylaşımı](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="689cd-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="689cd-112">, Bir sunucunun aynı kaynak ilkeyi rahat bir şekilde sağlamasına izin veren bir W3C standardıdır.</span><span class="sxs-lookup"><span data-stu-id="689cd-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="689cd-113">Güvenlik özelliği **değil** , CORS güvenliği.</span><span class="sxs-lookup"><span data-stu-id="689cd-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="689cd-114">CORS 'nin izin vermesini sağlayan bir API daha güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="689cd-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="689cd-115">Daha fazla bilgi için bkz. [CORS çalışma](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="689cd-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="689cd-116">Bir sunucunun bazı çapraz kaynak isteklerine, diğerlerini reddetirken açık olarak izin almasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="689cd-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="689cd-117">, [JSONP](/dotnet/framework/wcf/samples/jsonp)gibi önceki tekniklerin daha güvenli ve daha esnektir.</span><span class="sxs-lookup"><span data-stu-id="689cd-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="689cd-118">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="689cd-118">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="689cd-119">Aynı kaynak</span><span class="sxs-lookup"><span data-stu-id="689cd-119">Same origin</span></span>

<span data-ttu-id="689cd-120">Özdeş şemaları, konakları ve bağlantı noktalarına sahip olmaları durumunda iki URL aynı kaynağa sahiptir ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="689cd-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="689cd-121">Bu iki URL aynı kaynağa sahiptir:</span><span class="sxs-lookup"><span data-stu-id="689cd-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="689cd-122">Bu URL 'Ler, önceki iki URL 'den farklı kaynaklardan farklıdır:</span><span class="sxs-lookup"><span data-stu-id="689cd-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="689cd-123">`https://example.net` &ndash; farklı etki alanı</span><span class="sxs-lookup"><span data-stu-id="689cd-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="689cd-124">`https://www.example.com/foo.html` &ndash; farklı alt etki alanı</span><span class="sxs-lookup"><span data-stu-id="689cd-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="689cd-125">`http://example.com/foo.html` &ndash; farklı düzen</span><span class="sxs-lookup"><span data-stu-id="689cd-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="689cd-126">`https://example.com:9000/foo.html` &ndash; farklı bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="689cd-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="689cd-127">Internet Explorer, kaynakları karşılaştırırken bağlantı noktasını dikkate almaz.</span><span class="sxs-lookup"><span data-stu-id="689cd-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="689cd-128">Adlandırılmış ilke ve ara yazılım ile CORS</span><span class="sxs-lookup"><span data-stu-id="689cd-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="689cd-129">CORS ara yazılımı, çıkış noktaları arası istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="689cd-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="689cd-130">Aşağıdaki kod, belirtilen kaynağa sahip tüm uygulama için CORS 'yi sunar:</span><span class="sxs-lookup"><span data-stu-id="689cd-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="689cd-131">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="689cd-131">The preceding code:</span></span>

* <span data-ttu-id="689cd-132">İlke adını "\_Myallowspecifickaynaklarına" olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="689cd-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="689cd-133">İlke adı rastgele olur.</span><span class="sxs-lookup"><span data-stu-id="689cd-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="689cd-134">CORS 'yi sağlayan <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> genişletme yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="689cd-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="689cd-135">[Lambda ifadesiyle](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="689cd-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="689cd-136">Lambda <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> nesnesi alır.</span><span class="sxs-lookup"><span data-stu-id="689cd-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="689cd-137">@No__t-1 gibi [yapılandırma seçenekleri](#cors-policy-options), bu makalenin ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="689cd-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="689cd-138">@No__t-0 Yöntem çağrısı, uygulamanın hizmet kapsayıcısına CORS Hizmetleri ekler:</span><span class="sxs-lookup"><span data-stu-id="689cd-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="689cd-139">Daha fazla bilgi için bu belgedeki [CORS ilke seçenekleri](#cpo) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="689cd-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="689cd-140">@No__t-0 yöntemi, aşağıdaki kodda gösterildiği gibi yöntemleri zincirleyebilir:</span><span class="sxs-lookup"><span data-stu-id="689cd-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="689cd-141">Not: URL sonunda eğik çizgi (`/` **) bulunmamalıdır.**</span><span class="sxs-lookup"><span data-stu-id="689cd-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="689cd-142">URL `/` ile sonlandığında karşılaştırma, `false` döndürür ve hiçbir üst bilgi döndürülmez.</span><span class="sxs-lookup"><span data-stu-id="689cd-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a><span data-ttu-id="689cd-143">CORS ilkelerini tüm uç noktalara Uygula</span><span class="sxs-lookup"><span data-stu-id="689cd-143">Apply CORS policies to all endpoints</span></span>

<span data-ttu-id="689cd-144">Aşağıdaki kod, CORS ara yazılımı aracılığıyla CORS ilkelerini tüm uygulama uç noktalarına uygular:</span><span class="sxs-lookup"><span data-stu-id="689cd-144">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code ommitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code ommited.
}
```

> [!WARNING]
> <span data-ttu-id="689cd-145">Endpoint Routing ile CORS ara yazılımı `UseRouting` ve `UseEndpoints` çağrıları arasında yürütülecek şekilde yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="689cd-145">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="689cd-146">Yanlış yapılandırma, ara yazılımın doğru çalışmayı durdurmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="689cd-146">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="689cd-147">Aşağıdaki kod, CORS ara yazılımı aracılığıyla CORS ilkelerini tüm uygulama uç noktalarına uygular:</span><span class="sxs-lookup"><span data-stu-id="689cd-147">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="689cd-148">Note: `UseCors` ' dan önce `UseMvc` ' den önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="689cd-148">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="689cd-149">Bu sayfa/denetleyici/eylem düzeyinde CORS ilkesini uygulamak için [Razor Pages, denetleyiciler ve eylem YÖNTEMLERINDE CORS 'Yi etkinleştirin](#ecors) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="689cd-149">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="689cd-150">Önceki kodun test edilmesine ilişkin yönergeler için bkz. [Test CORS](#test) .</span><span class="sxs-lookup"><span data-stu-id="689cd-150">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="689cd-151">Uç nokta yönlendirme ile CORS 'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="689cd-151">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="689cd-152">Uç nokta yönlendirme ile, CORS, `RequireCors` uzantı yöntemleri kümesi kullanılarak uç nokta temelinde etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="689cd-152">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="689cd-153">Benzer şekilde, CORS tüm denetleyiciler için de etkinleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="689cd-153">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="689cd-154">CORS 'yi özniteliklerle etkinleştir</span><span class="sxs-lookup"><span data-stu-id="689cd-154">Enable CORS with attributes</span></span>

<span data-ttu-id="689cd-155">[@No__t-1EnableCors @ no__t-2](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) ÖZNITELIĞI, CORS 'yi küresel olarak uygulamaya bir alternatif sağlar.</span><span class="sxs-lookup"><span data-stu-id="689cd-155">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="689cd-156">@No__t-0 özniteliği, tüm bitiş noktaları yerine, seçilen bitiş noktaları için CORS 'yi mümkün.</span><span class="sxs-lookup"><span data-stu-id="689cd-156">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="689cd-157">Varsayılan ilkeyi belirtmek için `[EnableCors]` ve bir ilke belirtmek için-1 @no__t kullanın.</span><span class="sxs-lookup"><span data-stu-id="689cd-157">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="689cd-158">@No__t-0 özniteliği için uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="689cd-158">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="689cd-159">Razor sayfası `PageModel`</span><span class="sxs-lookup"><span data-stu-id="689cd-159">Razor Page `PageModel`</span></span>
* <span data-ttu-id="689cd-160">Kumandasını</span><span class="sxs-lookup"><span data-stu-id="689cd-160">Controller</span></span>
* <span data-ttu-id="689cd-161">Denetleyici eylemi yöntemi</span><span class="sxs-lookup"><span data-stu-id="689cd-161">Controller action method</span></span>

<span data-ttu-id="689cd-162">@No__t-0 özniteliğiyle denetleyici/sayfa-model/eylem 'e farklı ilkeler uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="689cd-162">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="689cd-163">@No__t-0 özniteliği bir denetleyiciler/sayfa modeli/eylem yöntemine uygulandığında ve bir ara yazılım içinde CORS etkinleştirildiğinde, her iki ilke de uygulanır.</span><span class="sxs-lookup"><span data-stu-id="689cd-163">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="689cd-164">İlkelerin birleştirilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="689cd-164">We recommend against combining policies.</span></span> <span data-ttu-id="689cd-165">Aynı uygulamada değil `[EnableCors]` özniteliğini veya ara yazılımını kullanın.</span><span class="sxs-lookup"><span data-stu-id="689cd-165">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="689cd-166">Aşağıdaki kod her bir yönteme farklı bir ilke uygular:</span><span class="sxs-lookup"><span data-stu-id="689cd-166">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="689cd-167">Aşağıdaki kod, bir CORS varsayılan ilkesi ve `"AnotherPolicy"` adlı bir ilke oluşturur:</span><span class="sxs-lookup"><span data-stu-id="689cd-167">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="689cd-168">CORS 'yi devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="689cd-168">Disable CORS</span></span>

<span data-ttu-id="689cd-169">[@No__t-1DisableCors @ no__t-2](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) özniteliği denetleyici/sayfa modeli/eylem için CORS 'yi devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="689cd-169">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="689cd-170">CORS ilke seçenekleri</span><span class="sxs-lookup"><span data-stu-id="689cd-170">CORS policy options</span></span>

<span data-ttu-id="689cd-171">Bu bölümde, bir CORS ilkesinde ayarlanmakta olabilecek çeşitli seçenekler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="689cd-171">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="689cd-172">İzin verilen kaynakları ayarla</span><span class="sxs-lookup"><span data-stu-id="689cd-172">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="689cd-173">İzin verilen HTTP yöntemlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="689cd-173">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="689cd-174">İzin verilen istek üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="689cd-174">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="689cd-175">Gösterilen yanıt üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="689cd-175">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="689cd-176">Kaynaklar arası isteklerde kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="689cd-176">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="689cd-177">Ön kontrol sona erme süresini ayarlama</span><span class="sxs-lookup"><span data-stu-id="689cd-177">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="689cd-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> `Startup.ConfigureServices` ' de çağrılır.</span><span class="sxs-lookup"><span data-stu-id="689cd-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="689cd-179">Bazı seçenekler için, ilk olarak [CORS 'Nin nasıl çalıştığı](#how-cors) bölümü okumanız yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="689cd-179">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="689cd-180">İzin verilen kaynakları ayarla</span><span class="sxs-lookup"><span data-stu-id="689cd-180">Set the allowed origins</span></span>

<span data-ttu-id="689cd-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; herhangi bir düzen (`http` veya `https`) olan tüm kaynaklardan gelen CORS isteklerine Izin verir.</span><span class="sxs-lookup"><span data-stu-id="689cd-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="689cd-182">*herhangi bir Web sitesi* uygulamaya çapraz kaynak istekleri yapabildiğinden `AllowAnyOrigin` güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="689cd-182">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="689cd-183">@No__t-0 ve `AllowCredentials` belirtildiğinde, güvenli olmayan bir yapılandırmadır ve siteler arası istek elde edilmesine yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="689cd-183">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="689cd-184">Bir uygulama her iki yöntemle yapılandırıldığında, CORS hizmeti geçersiz bir CORS yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="689cd-184">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="689cd-185">@No__t-0 ve `AllowCredentials` belirtildiğinde, güvenli olmayan bir yapılandırmadır ve siteler arası istek elde edilmesine yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="689cd-185">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="689cd-186">Güvenli bir uygulama için, istemcinin sunucu kaynaklarına erişim yetkisi olması gerekiyorsa, kaynakların tam bir listesini belirtin.</span><span class="sxs-lookup"><span data-stu-id="689cd-186">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="689cd-187">`AllowAnyOrigin`, ön kontrol isteklerini ve `Access-Control-Allow-Origin` üst bilgisini etkiler.</span><span class="sxs-lookup"><span data-stu-id="689cd-187">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="689cd-188">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="689cd-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="689cd-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash;, kaynağın <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> özelliğini, kaynağa izin verilip verilmediğini değerlendirirken, kaynağın, yapılandırılan bir joker karakterle eşleşmesini sağlayan bir işlev olacak şekilde ayarlar.</span><span class="sxs-lookup"><span data-stu-id="689cd-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="689cd-190">İzin verilen HTTP yöntemlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="689cd-190">Set the allowed HTTP methods</span></span>

<span data-ttu-id="689cd-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="689cd-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="689cd-192">Herhangi bir HTTP yöntemine izin verir:</span><span class="sxs-lookup"><span data-stu-id="689cd-192">Allows any HTTP method:</span></span>
* <span data-ttu-id="689cd-193">Ön kontrol isteklerini ve `Access-Control-Allow-Methods` üst bilgisini etkiler.</span><span class="sxs-lookup"><span data-stu-id="689cd-193">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="689cd-194">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="689cd-194">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="689cd-195">İzin verilen istek üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="689cd-195">Set the allowed request headers</span></span>

<span data-ttu-id="689cd-196">Belirli başlıkların, *Yazar istek üstbilgileri*ADLı bir CORS isteğinde gönderilmesine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> ' i çağırın ve izin verilen üst bilgileri belirtin:</span><span class="sxs-lookup"><span data-stu-id="689cd-196">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="689cd-197">Tüm yazar isteği üst bilgilerine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*> ' ı çağırın:</span><span class="sxs-lookup"><span data-stu-id="689cd-197">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="689cd-198">Bu ayar, ön kontrol isteklerini ve `Access-Control-Request-Headers` üst bilgisini etkiler.</span><span class="sxs-lookup"><span data-stu-id="689cd-198">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="689cd-199">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="689cd-199">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="689cd-200">@No__t-0 tarafından belirtilen belirli başlıklarıyla eşleşen bir CORS ara yazılım ilkesi, yalnızca `Access-Control-Request-Headers` ' de gönderilen üstbilgiler `WithHeaders` ' de belirtilen üstbilgileriyle tam olarak eşleşiyorsa mümkündür.</span><span class="sxs-lookup"><span data-stu-id="689cd-200">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="689cd-201">Örneğin, aşağıdaki gibi yapılandırılmış bir uygulamayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="689cd-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="689cd-202">CORS ara yazılımı, `Content-Language` ([Headernames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) `WithHeaders` ' de listelenmediğinden, aşağıdaki istek üstbilgisiyle bir ön kontrol isteğini reddeder:</span><span class="sxs-lookup"><span data-stu-id="689cd-202">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="689cd-203">Uygulama *200 ok* yanıtı DÖNDÜRÜYOR ancak CORS üst bilgilerini geri göndermez.</span><span class="sxs-lookup"><span data-stu-id="689cd-203">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="689cd-204">Bu nedenle tarayıcı, çıkış noktaları arası isteği denemez.</span><span class="sxs-lookup"><span data-stu-id="689cd-204">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="689cd-205">CORS ara yazılımı, CorsPolicy. Headers içinde yapılandırılan değerlere bakılmaksızın `Access-Control-Request-Headers` ' daki dört üstbilgiyi her zaman sağlar.</span><span class="sxs-lookup"><span data-stu-id="689cd-205">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="689cd-206">Bu üst bilgi listesi şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="689cd-206">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="689cd-207">Örneğin, aşağıdaki gibi yapılandırılmış bir uygulamayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="689cd-207">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="689cd-208">CORS ara yazılımı, her zaman beyaz listeye @no__t için aşağıdaki istek üstbilgisiyle bir ön kontrol isteğine başarıyla yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="689cd-208">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="689cd-209">Gösterilen yanıt üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="689cd-209">Set the exposed response headers</span></span>

<span data-ttu-id="689cd-210">Varsayılan olarak tarayıcı, tüm yanıt üst bilgilerini uygulamaya sunmaz.</span><span class="sxs-lookup"><span data-stu-id="689cd-210">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="689cd-211">Daha fazla bilgi için bkz. [W3C çıkış noktaları arası kaynak paylaşımı (terminoloji): basit yanıt üst bilgisi](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="689cd-211">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="689cd-212">Varsayılan olarak kullanılabilen yanıt üstbilgileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="689cd-212">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="689cd-213">CORS belirtimi, bu üst bilgiler *basit yanıt üst bilgilerini*çağırır.</span><span class="sxs-lookup"><span data-stu-id="689cd-213">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="689cd-214">Diğer üst bilgileri uygulama için kullanılabilir hale getirmek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*> ' ı çağırın:</span><span class="sxs-lookup"><span data-stu-id="689cd-214">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="689cd-215">Kaynaklar arası isteklerde kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="689cd-215">Credentials in cross-origin requests</span></span>

<span data-ttu-id="689cd-216">Kimlik bilgileri CORS isteğinde özel işleme gerektirir.</span><span class="sxs-lookup"><span data-stu-id="689cd-216">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="689cd-217">Varsayılan olarak tarayıcı, kimlik bilgilerini bir çapraz kaynak isteğiyle göndermez.</span><span class="sxs-lookup"><span data-stu-id="689cd-217">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="689cd-218">Kimlik bilgileri, tanımlama bilgileri ve HTTP kimlik doğrulama düzenlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="689cd-218">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="689cd-219">Bir çapraz kaynak isteğiyle kimlik bilgilerini göndermek için, istemcinin `XMLHttpRequest.withCredentials` ' ı `true` olarak ayarlaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="689cd-219">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="689cd-220">@No__t-0 kullanarak doğrudan:</span><span class="sxs-lookup"><span data-stu-id="689cd-220">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="689cd-221">JQuery kullanarak:</span><span class="sxs-lookup"><span data-stu-id="689cd-221">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="689cd-222">[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)'sini kullanma:</span><span class="sxs-lookup"><span data-stu-id="689cd-222">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="689cd-223">Sunucu kimlik bilgilerine izin vermelidir.</span><span class="sxs-lookup"><span data-stu-id="689cd-223">The server must allow the credentials.</span></span> <span data-ttu-id="689cd-224">Çıkış noktaları arası kimlik bilgilerine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*> ' ı çağırın:</span><span class="sxs-lookup"><span data-stu-id="689cd-224">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="689cd-225">HTTP yanıtı, tarayıcıya sunucunun bir çapraz kaynak isteği için kimlik bilgileri verdiğini bildiren `Access-Control-Allow-Credentials` üst bilgisini içerir.</span><span class="sxs-lookup"><span data-stu-id="689cd-225">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="689cd-226">Tarayıcı kimlik bilgilerini gönderirse ancak yanıt geçerli bir `Access-Control-Allow-Credentials` üstbilgisi içermiyorsa, tarayıcı uygulamaya yanıtı kullanıma sunmaz ve çapraz kaynak isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="689cd-226">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="689cd-227">Çıkış noktaları arası kimlik bilgilerine izin vermek bir güvenlik riskidir.</span><span class="sxs-lookup"><span data-stu-id="689cd-227">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="689cd-228">Başka bir etki alanındaki Web sitesi, kullanıcının bilgisi olmadan kullanıcı adına, oturum açmış bir kullanıcının kimlik bilgilerini uygulamaya gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="689cd-228">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="689cd-229">CORS belirtimi Ayrıca, `Access-Control-Allow-Credentials` üstbilgisi mevcutsa `"*"` ' a (tüm kaynaklar) çıkış ayarının geçersiz olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="689cd-229">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="689cd-230">Ön kontrol istekleri</span><span class="sxs-lookup"><span data-stu-id="689cd-230">Preflight requests</span></span>

<span data-ttu-id="689cd-231">Bazı CORS istekleri için, tarayıcı gerçek isteği yapmadan önce ek bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="689cd-231">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="689cd-232">Bu isteğe bir *ön kontrol isteği*denir.</span><span class="sxs-lookup"><span data-stu-id="689cd-232">This request is called a *preflight request*.</span></span> <span data-ttu-id="689cd-233">Aşağıdaki koşullar doğruysa tarayıcı, ön kontrol isteğini atlayabilir:</span><span class="sxs-lookup"><span data-stu-id="689cd-233">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="689cd-234">İstek yöntemi al, HEAD veya POST.</span><span class="sxs-lookup"><span data-stu-id="689cd-234">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="689cd-235">Uygulama `Accept`, `Accept-Language`, `Content-Language`, `Content-Type` veya `Last-Event-ID` dışındaki istek üst bilgilerini ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="689cd-235">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="689cd-236">@No__t-0 üstbilgisi, ayarlandıysa aşağıdaki değerlerden birine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="689cd-236">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="689cd-237">İstemci isteği için ayarlanan istek üstbilgileri kümesi kuralı, `XMLHttpRequest` nesnesinde `setRequestHeader` ' i çağırarak uygulamanın ayarladığı üst bilgiler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="689cd-237">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="689cd-238">CORS belirtimi, bu üst bilgiler *Yazar istek üst bilgilerini*çağırır.</span><span class="sxs-lookup"><span data-stu-id="689cd-238">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="689cd-239">Kural, tarayıcının ayarlayabilen `User-Agent`, `Host` veya `Content-Length` gibi üstbilgilere uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="689cd-239">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="689cd-240">Aşağıda bir ön denetim isteğine örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="689cd-240">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="689cd-241">Uçuş öncesi isteği HTTP SEÇENEKLERI yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="689cd-241">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="689cd-242">İki özel üst bilgi içerir:</span><span class="sxs-lookup"><span data-stu-id="689cd-242">It includes two special headers:</span></span>

* <span data-ttu-id="689cd-243">`Access-Control-Request-Method`: gerçek istek için kullanılacak HTTP yöntemi.</span><span class="sxs-lookup"><span data-stu-id="689cd-243">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="689cd-244">`Access-Control-Request-Headers`: uygulamanın gerçek istekte ayarladığı istek üst bilgilerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="689cd-244">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="689cd-245">Daha önce belirtildiği gibi, bu, tarayıcının ayarladığı `User-Agent` gibi üst bilgileri içermez.</span><span class="sxs-lookup"><span data-stu-id="689cd-245">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="689cd-246">CORS ön hazırlığı isteği, sunucuya gerçek istekle gönderilen üstbilgileri belirten bir `Access-Control-Request-Headers` üstbilgisi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="689cd-246">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="689cd-247">Belirli üstbilgilere izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> ' ı çağırın:</span><span class="sxs-lookup"><span data-stu-id="689cd-247">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="689cd-248">Tüm yazar isteği üst bilgilerine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*> ' ı çağırın:</span><span class="sxs-lookup"><span data-stu-id="689cd-248">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="689cd-249">Tarayıcılar, `Access-Control-Request-Headers` ' a nasıl ayarlandıklarından tamamen tutarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="689cd-249">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="689cd-250">Üst bilgileri `"*"` ' dan başka bir şeye ayarlarsanız (veya <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*> ' i kullanırsanız), en az `Accept`, `Content-Type` ve `Origin`, ayrıca desteklemek istediğiniz tüm özel üst bilgileri içermelidir.</span><span class="sxs-lookup"><span data-stu-id="689cd-250">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="689cd-251">Aşağıda, ön kontrol isteğine örnek bir yanıt verilmiştir (sunucunun isteğe izin verdiği varsayıldığında):</span><span class="sxs-lookup"><span data-stu-id="689cd-251">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="689cd-252">Yanıt, izin verilen üst bilgileri listeleyen bir `Access-Control-Allow-Methods` üst bilgisini ve isteğe bağlı olarak bir `Access-Control-Allow-Headers` üstbilgisini içerir.</span><span class="sxs-lookup"><span data-stu-id="689cd-252">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="689cd-253">Ön kontrol isteği başarılı olursa, tarayıcı gerçek isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="689cd-253">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="689cd-254">Ön kontrol isteği reddedilirse, uygulama *200 ok* yanıtı döndürür ancak CORS üst bilgilerini geri göndermez.</span><span class="sxs-lookup"><span data-stu-id="689cd-254">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="689cd-255">Bu nedenle tarayıcı, çıkış noktaları arası isteği denemez.</span><span class="sxs-lookup"><span data-stu-id="689cd-255">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="689cd-256">Ön kontrol sona erme süresini ayarlama</span><span class="sxs-lookup"><span data-stu-id="689cd-256">Set the preflight expiration time</span></span>

<span data-ttu-id="689cd-257">@No__t-0 üstbilgisi, ön kontrol isteğine olan yanıtın ne kadar süreyle önbelleğe alınacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="689cd-257">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="689cd-258">Bu üst bilgiyi ayarlamak için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*> ' ı çağırın:</span><span class="sxs-lookup"><span data-stu-id="689cd-258">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="689cd-259">CORS nasıl kullanılır?</span><span class="sxs-lookup"><span data-stu-id="689cd-259">How CORS works</span></span>

<span data-ttu-id="689cd-260">Bu bölümde, HTTP iletileri düzeyindeki bir [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) isteğinde ne olacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="689cd-260">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="689cd-261">CORS bir güvenlik özelliği **değil** .</span><span class="sxs-lookup"><span data-stu-id="689cd-261">CORS is **not** a security feature.</span></span> <span data-ttu-id="689cd-262">CORS, bir sunucunun aynı kaynaklı ilkeyi rahat bir şekilde sağlamasına izin veren bir W3C standardıdır.</span><span class="sxs-lookup"><span data-stu-id="689cd-262">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="689cd-263">Örneğin, kötü niyetli bir aktör sitenize karşı [siteler arası komut dosyalarını (XSS) engelliyor](xref:security/cross-site-scripting) ve bilgileri çalmak için CORS özellikli sitesine bir siteler arası istek yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="689cd-263">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="689cd-264">CORS 'nin izin vermesini sağlayan API 'niz daha güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="689cd-264">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="689cd-265">CORS 'yi zorlamak için istemciye (tarayıcı) sahiptir.</span><span class="sxs-lookup"><span data-stu-id="689cd-265">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="689cd-266">Sunucu isteği yürütür ve yanıtı döndürür. Bu, bir hatayı döndüren ve yanıtı engelleyen istemcdir.</span><span class="sxs-lookup"><span data-stu-id="689cd-266">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="689cd-267">Örneğin, aşağıdaki araçlardan herhangi birinde sunucu yanıtı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="689cd-267">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="689cd-268">Fiddler</span><span class="sxs-lookup"><span data-stu-id="689cd-268">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="689cd-269">Postman</span><span class="sxs-lookup"><span data-stu-id="689cd-269">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="689cd-270">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="689cd-270">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="689cd-271">Adres çubuğuna URL girerek bir Web tarayıcısı.</span><span class="sxs-lookup"><span data-stu-id="689cd-271">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="689cd-272">Bir sunucu, tarayıcıların çapraz kaynak [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) veya [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) isteği çalıştırmasına izin vermesinin yasaktır bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="689cd-272">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="689cd-273">Tarayıcılar (CORS olmadan), çapraz kaynak istekleri yapamıyor.</span><span class="sxs-lookup"><span data-stu-id="689cd-273">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="689cd-274">CORS 'den önce, bu kısıtlamayı aşmak için [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="689cd-274">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="689cd-275">JSONP XHR kullanmaz, yanıtı almak için `<script>` etiketini kullanır.</span><span class="sxs-lookup"><span data-stu-id="689cd-275">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="689cd-276">Betiklerin, çapraz kaynak olarak yüklenmesine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="689cd-276">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="689cd-277">[CORS belirtimi](https://www.w3.org/TR/cors/) , çıkış noktaları arası istekleri etkinleştiren bırkaç yeni http üst bilgisi sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="689cd-277">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="689cd-278">Bir tarayıcı CORS 'yi destekliyorsa, bu üst bilgileri, çıkış noktaları arası istekler için otomatik olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="689cd-278">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="689cd-279">CORS 'yi etkinleştirmek için özel JavaScript kodu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="689cd-279">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="689cd-280">Aşağıda, bir çapraz kaynak isteğine bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="689cd-280">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="689cd-281">@No__t-0 üstbilgisi, isteği yapan sitenin etki alanını sağlar.</span><span class="sxs-lookup"><span data-stu-id="689cd-281">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="689cd-282">@No__t-0 üst bilgisi gereklidir ve konaktan farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="689cd-282">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="689cd-283">Sunucu isteğe izin veriyorsa, yanıtta `Access-Control-Allow-Origin` üst bilgisini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="689cd-283">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="689cd-284">Bu üstbilginin değeri, istekten `Origin` üstbilgisiyle eşleşir veya `"*"` joker değeri, yani herhangi bir kaynağa izin verilir.</span><span class="sxs-lookup"><span data-stu-id="689cd-284">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="689cd-285">Yanıt `Access-Control-Allow-Origin` üst bilgisini içermiyorsa, çapraz kaynak isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="689cd-285">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="689cd-286">Özellikle, tarayıcı isteğe izin vermez.</span><span class="sxs-lookup"><span data-stu-id="689cd-286">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="689cd-287">Sunucu başarılı bir yanıt döndürse bile tarayıcı, yanıtı istemci uygulama için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="689cd-287">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="689cd-288">Test CORS</span><span class="sxs-lookup"><span data-stu-id="689cd-288">Test CORS</span></span>

<span data-ttu-id="689cd-289">CORS 'yi sınamak için:</span><span class="sxs-lookup"><span data-stu-id="689cd-289">To test CORS:</span></span>

1. <span data-ttu-id="689cd-290">[BIR API projesi oluşturun](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="689cd-290">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="689cd-291">Alternatif olarak, [örneği de indirebilirsiniz](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="689cd-291">Alternatively, you can [download the sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="689cd-292">Bu belgedeki yaklaşımlardan birini kullanarak CORS 'yi etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="689cd-292">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="689cd-293">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="689cd-293">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="689cd-294">`WithOrigins("https://localhost:<port>");` yalnızca [indirme örnek koduna](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)benzer bir örnek uygulamanın test edilmesi için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="689cd-294">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="689cd-295">Web uygulaması projesi (Razor Pages veya MVC) oluşturun.</span><span class="sxs-lookup"><span data-stu-id="689cd-295">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="689cd-296">Örnek Razor Pages kullanır.</span><span class="sxs-lookup"><span data-stu-id="689cd-296">The sample uses Razor Pages.</span></span> <span data-ttu-id="689cd-297">Web uygulamasını, API projesiyle aynı çözümde oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="689cd-297">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="689cd-298">Aşağıdaki Vurgulanan kodu *Index. cshtml* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="689cd-298">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="689cd-299">Yukarıdaki kodda `url: 'https://<web app>.azurewebsites.net/api/values/1',` ' ı dağıtılan uygulamanın URL 'siyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="689cd-299">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="689cd-300">API projesini dağıtın.</span><span class="sxs-lookup"><span data-stu-id="689cd-300">Deploy the API project.</span></span> <span data-ttu-id="689cd-301">Örneğin, [Azure 'a dağıtın](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="689cd-301">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="689cd-302">Razor Pages veya MVC uygulamasını masaüstünden çalıştırın ve **Test** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="689cd-302">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="689cd-303">Hata iletilerini gözden geçirmek için F12 araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="689cd-303">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="689cd-304">@No__t-0 ' dan localhost kaynağını kaldırın ve uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="689cd-304">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="689cd-305">Alternatif olarak, istemci uygulamasını farklı bir bağlantı noktasıyla çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="689cd-305">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="689cd-306">Örneğin, Visual Studio 'dan çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="689cd-306">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="689cd-307">İstemci uygulamasıyla test edin.</span><span class="sxs-lookup"><span data-stu-id="689cd-307">Test with the client app.</span></span> <span data-ttu-id="689cd-308">CORS hataları bir hata döndürüyor, ancak hata iletisi JavaScript için kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="689cd-308">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="689cd-309">Hatayı görmek için F12 araçlarındaki konsol sekmesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="689cd-309">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="689cd-310">Tarayıcıya bağlı olarak, aşağıdakine benzer bir hata alırsınız (F12 araçları konsolunda):</span><span class="sxs-lookup"><span data-stu-id="689cd-310">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="689cd-311">Microsoft Edge 'i kullanarak:</span><span class="sxs-lookup"><span data-stu-id="689cd-311">Using Microsoft Edge:</span></span>

     <span data-ttu-id="689cd-312">**SEC7120: [CORS] `https://localhost:44375` `https://webapi.azurewebsites.net/api/values/1` ' teki çıkış noktaları için erişim-denetimi-Izin-Origin yanıt üstbilgisinde `https://localhost:44375` bulamadı**</span><span class="sxs-lookup"><span data-stu-id="689cd-312">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="689cd-313">Chrome kullanarak:</span><span class="sxs-lookup"><span data-stu-id="689cd-313">Using Chrome:</span></span>

     <span data-ttu-id="689cd-314">**Kaynak `https://localhost:44375` ' den `https://webapi.azurewebsites.net/api/values/1` ' deki XMLHttpRequest erişimi CORS ilkesi tarafından engellendi: İstenen kaynakta ' erişim-Control-Allow-Origin ' üst bilgisi yok.**</span><span class="sxs-lookup"><span data-stu-id="689cd-314">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="689cd-315">CORS özellikli uç noktalar [Fiddler](https://www.telerik.com/fiddler) veya [Postman](https://www.getpostman.com/)gibi bir araçla test edilebilir.</span><span class="sxs-lookup"><span data-stu-id="689cd-315">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="689cd-316">Bir araç kullanırken, `Origin` üstbilgisi tarafından belirtilen isteğin kaynağı isteği alan konaktan farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="689cd-316">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="689cd-317">İstek, `Origin` üst bilgisinin değerine göre *çapraz kaynak* değilse:</span><span class="sxs-lookup"><span data-stu-id="689cd-317">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="689cd-318">CORS ara yazılımı için isteği işleme gereksinimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="689cd-318">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="689cd-319">CORS üstbilgileri yanıtta döndürülmedi.</span><span class="sxs-lookup"><span data-stu-id="689cd-319">CORS headers aren't returned in the response.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="689cd-320">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="689cd-320">Additional resources</span></span>

* [<span data-ttu-id="689cd-321">Çıkış noktaları arası kaynak paylaşımı (CORS)</span><span class="sxs-lookup"><span data-stu-id="689cd-321">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
