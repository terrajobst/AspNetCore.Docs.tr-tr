---
title: ASP.NET Core 'de çıkış noktaları arası Istekleri (CORS) etkinleştirme
author: rick-anderson
description: ASP.NET Core uygulamasında çapraz kaynak isteklerini izin vermek veya reddetmek için CORS 'nin nasıl standart olduğunu öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: a34b77ad799a00707048c923b82b48774ce91682
ms.sourcegitcommit: b1e480e1736b0fe0e4d8dce4a4cf5c8e47fc2101
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71108072"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="0fd04-103">ASP.NET Core 'de çıkış noktaları arası Istekleri (CORS) etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="0fd04-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="0fd04-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0fd04-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0fd04-105">Bu makalede, ASP.NET Core uygulamasında CORS 'nin nasıl etkinleştirileceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="0fd04-106">Tarayıcı güvenliği, bir Web sayfasının Web sayfasını sunduğundan farklı bir etki alanına istek yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="0fd04-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="0fd04-107">Bu kısıtlamaya *aynı-Origin ilkesi*adı verilir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="0fd04-108">Aynı-kaynak ilkesi, kötü niyetli bir sitenin gizli verileri başka bir siteden okumasını engeller.</span><span class="sxs-lookup"><span data-stu-id="0fd04-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="0fd04-109">Bazen diğer sitelerin uygulamanıza çapraz çıkış istekleri yapmasına izin vermek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0fd04-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="0fd04-110">Daha fazla bilgi için bkz. [MOZILLA CORS makalesi](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="0fd04-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="0fd04-111">[Çapraz kaynak kaynak paylaşımı](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="0fd04-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="0fd04-112">, Bir sunucunun aynı kaynak ilkeyi rahat bir şekilde sağlamasına izin veren bir W3C standardıdır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="0fd04-113">Güvenlik özelliği **değil** , CORS güvenliği.</span><span class="sxs-lookup"><span data-stu-id="0fd04-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="0fd04-114">CORS 'nin izin vermesini sağlayan bir API daha güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="0fd04-115">Daha fazla bilgi için bkz. [CORS çalışma](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="0fd04-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="0fd04-116">Bir sunucunun bazı çapraz kaynak isteklerine, diğerlerini reddetirken açık olarak izin almasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="0fd04-117">, [JSONP](/dotnet/framework/wcf/samples/jsonp)gibi önceki tekniklerin daha güvenli ve daha esnektir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="0fd04-118">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0fd04-118">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="0fd04-119">Aynı kaynak</span><span class="sxs-lookup"><span data-stu-id="0fd04-119">Same origin</span></span>

<span data-ttu-id="0fd04-120">Özdeş şemaları, konakları ve bağlantı noktalarına sahip olmaları durumunda iki URL aynı kaynağa sahiptir ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="0fd04-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="0fd04-121">Bu iki URL aynı kaynağa sahiptir:</span><span class="sxs-lookup"><span data-stu-id="0fd04-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="0fd04-122">Bu URL 'Ler, önceki iki URL 'den farklı kaynaklardan farklıdır:</span><span class="sxs-lookup"><span data-stu-id="0fd04-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="0fd04-123">`https://example.net`&ndash; Farklı etki alanı</span><span class="sxs-lookup"><span data-stu-id="0fd04-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="0fd04-124">`https://www.example.com/foo.html`&ndash; Farklı alt etki alanı</span><span class="sxs-lookup"><span data-stu-id="0fd04-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="0fd04-125">`http://example.com/foo.html`&ndash; Farklı düzen</span><span class="sxs-lookup"><span data-stu-id="0fd04-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="0fd04-126">`https://example.com:9000/foo.html`&ndash; Farklı bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="0fd04-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="0fd04-127">Internet Explorer, kaynakları karşılaştırırken bağlantı noktasını dikkate almaz.</span><span class="sxs-lookup"><span data-stu-id="0fd04-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="0fd04-128">Adlandırılmış ilke ve ara yazılım ile CORS</span><span class="sxs-lookup"><span data-stu-id="0fd04-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="0fd04-129">CORS ara yazılımı, çıkış noktaları arası istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="0fd04-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="0fd04-130">Aşağıdaki kod, belirtilen kaynağa sahip tüm uygulama için CORS 'yi sunar:</span><span class="sxs-lookup"><span data-stu-id="0fd04-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="0fd04-131">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="0fd04-131">The preceding code:</span></span>

* <span data-ttu-id="0fd04-132">İlke adını "\_myallowspecifickaynaklarına" olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0fd04-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="0fd04-133">İlke adı rastgele olur.</span><span class="sxs-lookup"><span data-stu-id="0fd04-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="0fd04-134">CORS 'yi sağlayan genişletme yöntemini çağırır. <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*></span><span class="sxs-lookup"><span data-stu-id="0fd04-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="0fd04-135"><xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> [Lambda ifadesi](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)ile çağrılar.</span><span class="sxs-lookup"><span data-stu-id="0fd04-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="0fd04-136">Lambda bir <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> nesnesi alır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="0fd04-137">Gibi [yapılandırma seçenekleri](#cors-policy-options) `WithOrigins`, bu makalenin ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="0fd04-138"><xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> Yöntem çağrısı, uygulamanın hizmet kapsayıcısına CORS Hizmetleri ekler:</span><span class="sxs-lookup"><span data-stu-id="0fd04-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="0fd04-139">Daha fazla bilgi için bu belgedeki [CORS ilke seçenekleri](#cpo) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="0fd04-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="0fd04-140"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> Yöntemi aşağıdaki kodda gösterildiği gibi yöntemleri zincirleyebilir:</span><span class="sxs-lookup"><span data-stu-id="0fd04-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="0fd04-141">Not: URL sonunda eğik çizgi (`/` **) bulunmamalıdır.**</span><span class="sxs-lookup"><span data-stu-id="0fd04-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="0fd04-142">URL ile `/`sonlandığında `false` karşılaştırma döndürülür ve üst bilgi döndürülmez.</span><span class="sxs-lookup"><span data-stu-id="0fd04-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0fd04-143">Aşağıdaki kod, CORS ara yazılımı aracılığıyla CORS ilkelerini tüm uygulama uç noktalarına uygular:</span><span class="sxs-lookup"><span data-stu-id="0fd04-143">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
> <span data-ttu-id="0fd04-144">Endpoint Routing ile CORS ara yazılımı, `UseRouting` ve `UseEndpoints`çağrıları arasında yürütülecek şekilde yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-144">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="0fd04-145">Yanlış yapılandırma, ara yazılımın doğru çalışmayı durdurmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="0fd04-145">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="0fd04-146">Aşağıdaki kod, CORS ara yazılımı aracılığıyla CORS ilkelerini tüm uygulama uç noktalarına uygular:</span><span class="sxs-lookup"><span data-stu-id="0fd04-146">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="0fd04-147">Note: `UseCors` öğesinden önce `UseMvc`çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-147">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="0fd04-148">Bu sayfa/denetleyici/eylem düzeyinde CORS ilkesini uygulamak için [Razor Pages, denetleyiciler ve eylem YÖNTEMLERINDE CORS 'Yi etkinleştirin](#ecors) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="0fd04-148">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="0fd04-149">Önceki kodun test edilmesine ilişkin yönergeler için bkz. [Test CORS](#test) .</span><span class="sxs-lookup"><span data-stu-id="0fd04-149">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="0fd04-150">Uç nokta yönlendirme ile CORS 'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="0fd04-150">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="0fd04-151">Uç nokta yönlendirme ile, CORS, uzantı yöntemleri `RequireCors` kümesi kullanılarak uç nokta temelinde etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-151">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="0fd04-152">Benzer şekilde, CORS tüm denetleyiciler için de etkinleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="0fd04-152">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="0fd04-153">CORS 'yi özniteliklerle etkinleştir</span><span class="sxs-lookup"><span data-stu-id="0fd04-153">Enable CORS with attributes</span></span>

<span data-ttu-id="0fd04-154">[ &lbrack;Enablecors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) özniteliği, CORS 'yi küresel olarak uygulamaya bir alternatif sağlar.</span><span class="sxs-lookup"><span data-stu-id="0fd04-154">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="0fd04-155">`[EnableCors]` Öznitelik, tüm bitiş noktaları yerine, seçilen bitiş noktaları için CORS 'yi mümkün.</span><span class="sxs-lookup"><span data-stu-id="0fd04-155">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="0fd04-156">Varsayılan `[EnableCors]` ilkeyi belirtmek ve `[EnableCors("{Policy String}")]` bir ilke belirlemek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="0fd04-156">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="0fd04-157">`[EnableCors]` Özniteliği öğesine uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="0fd04-157">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="0fd04-158">Razor sayfası`PageModel`</span><span class="sxs-lookup"><span data-stu-id="0fd04-158">Razor Page `PageModel`</span></span>
* <span data-ttu-id="0fd04-159">Kumandasını</span><span class="sxs-lookup"><span data-stu-id="0fd04-159">Controller</span></span>
* <span data-ttu-id="0fd04-160">Denetleyici eylemi yöntemi</span><span class="sxs-lookup"><span data-stu-id="0fd04-160">Controller action method</span></span>

<span data-ttu-id="0fd04-161">`[EnableCors]` Özniteliği ile denetleyici/sayfa-model/eylem 'e farklı ilkeler uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0fd04-161">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="0fd04-162">`[EnableCors]` Özniteliği bir denetleyiciler/sayfa modeli/eylem yöntemine uygulandığında ve bu işlem, ara yazılım içinde CORS 'yi etkinleştirmişse her iki ilke de uygulanır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-162">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="0fd04-163">İlkelerin birleştirilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-163">We recommend against combining policies.</span></span> <span data-ttu-id="0fd04-164">Aynı uygulamada değil, özniteliğiniveyaarayazılımınıkullanın.`[EnableCors]`</span><span class="sxs-lookup"><span data-stu-id="0fd04-164">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="0fd04-165">Aşağıdaki kod her bir yönteme farklı bir ilke uygular:</span><span class="sxs-lookup"><span data-stu-id="0fd04-165">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="0fd04-166">Aşağıdaki kod, bir CORS varsayılan ilkesi ve adlı `"AnotherPolicy"`bir ilke oluşturur:</span><span class="sxs-lookup"><span data-stu-id="0fd04-166">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="0fd04-167">CORS 'yi devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="0fd04-167">Disable CORS</span></span>

<span data-ttu-id="0fd04-168">Disablecors özniteliği denetleyici/sayfa modeli/eylem için CORS 'yi devre dışı bırakır. [ &lbrack;&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)</span><span class="sxs-lookup"><span data-stu-id="0fd04-168">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="0fd04-169">CORS ilke seçenekleri</span><span class="sxs-lookup"><span data-stu-id="0fd04-169">CORS policy options</span></span>

<span data-ttu-id="0fd04-170">Bu bölümde, bir CORS ilkesinde ayarlanmakta olabilecek çeşitli seçenekler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="0fd04-170">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="0fd04-171">İzin verilen kaynakları ayarla</span><span class="sxs-lookup"><span data-stu-id="0fd04-171">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="0fd04-172">İzin verilen HTTP yöntemlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="0fd04-172">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="0fd04-173">İzin verilen istek üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="0fd04-173">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="0fd04-174">Gösterilen yanıt üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="0fd04-174">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="0fd04-175">Kaynaklar arası isteklerde kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="0fd04-175">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="0fd04-176">Ön kontrol sona erme süresini ayarlama</span><span class="sxs-lookup"><span data-stu-id="0fd04-176">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="0fd04-177"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>içinde `Startup.ConfigureServices`çağırılır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-177"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0fd04-178">Bazı seçenekler için, ilk olarak [CORS 'Nin nasıl çalıştığı](#how-cors) bölümü okumanız yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-178">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="0fd04-179">İzin verilen kaynakları ayarla</span><span class="sxs-lookup"><span data-stu-id="0fd04-179">Set the allowed origins</span></span>

<span data-ttu-id="0fd04-180"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>Herhangi bir düzen (`http` veya `https`) ile tüm kaynaklardan gelen CORS isteklerine izin verir. &ndash;</span><span class="sxs-lookup"><span data-stu-id="0fd04-180"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="0fd04-181">`AllowAnyOrigin`, *herhangi bir Web sitesi* uygulamaya çapraz kaynak istekleri yapabildiğinden güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-181">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="0fd04-182">`AllowAnyOrigin` Ve`AllowCredentials` güvenli olmayan bir yapılandırma belirtip, siteler arası istek sahteciliği ile sonuçlanabilir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-182">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="0fd04-183">Bir uygulama her iki yöntemle yapılandırıldığında, CORS hizmeti geçersiz bir CORS yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="0fd04-183">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="0fd04-184">`AllowAnyOrigin` Ve`AllowCredentials` güvenli olmayan bir yapılandırma belirtip, siteler arası istek sahteciliği ile sonuçlanabilir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-184">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="0fd04-185">Güvenli bir uygulama için, istemcinin sunucu kaynaklarına erişim yetkisi olması gerekiyorsa, kaynakların tam bir listesini belirtin.</span><span class="sxs-lookup"><span data-stu-id="0fd04-185">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="0fd04-186">`AllowAnyOrigin`ön kontrol isteklerini ve `Access-Control-Allow-Origin` üstbilgiyi etkiler.</span><span class="sxs-lookup"><span data-stu-id="0fd04-186">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="0fd04-187">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="0fd04-187">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0fd04-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>Kaynağa izin verilip verilmediğini değerlendirirken, kaynağın bir yapılandırılmış joker karakterle eşleşmesini sağlayan bir işlev olarak ilkenin özelliğiniayarlar.<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> &ndash;</span><span class="sxs-lookup"><span data-stu-id="0fd04-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="0fd04-189">İzin verilen HTTP yöntemlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="0fd04-189">Set the allowed HTTP methods</span></span>

<span data-ttu-id="0fd04-190"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="0fd04-190"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="0fd04-191">Herhangi bir HTTP yöntemine izin verir:</span><span class="sxs-lookup"><span data-stu-id="0fd04-191">Allows any HTTP method:</span></span>
* <span data-ttu-id="0fd04-192">Ön kontrol isteklerini ve `Access-Control-Allow-Methods` üstbilgiyi etkiler.</span><span class="sxs-lookup"><span data-stu-id="0fd04-192">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="0fd04-193">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="0fd04-193">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="0fd04-194">İzin verilen istek üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="0fd04-194">Set the allowed request headers</span></span>

<span data-ttu-id="0fd04-195">Belirli başlıkların, *Yazar isteği üstbilgileri*adlı bir CORS isteğinde gönderilmesine izin vermek için, ' i çağırın <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> ve izin verilen üst bilgileri belirtin:</span><span class="sxs-lookup"><span data-stu-id="0fd04-195">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="0fd04-196">Tüm yazar isteği başlıklarına izin vermek için şunu <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="0fd04-196">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="0fd04-197">Bu ayar, `Access-Control-Request-Headers` ön kontrol isteklerini ve üstbilgiyi etkiler.</span><span class="sxs-lookup"><span data-stu-id="0fd04-197">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="0fd04-198">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="0fd04-198">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0fd04-199">Tarafından belirtilen belirli başlıklarıyla `WithHeaders` eşleşen bir CORS ara yazılım ilkesi, yalnızca ' de `WithHeaders`belirtilen üstbilgiler ile `Access-Control-Request-Headers` tam olarak eşleşiyorsa mümkündür.</span><span class="sxs-lookup"><span data-stu-id="0fd04-199">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="0fd04-200">Örneğin, aşağıdaki gibi yapılandırılmış bir uygulamayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="0fd04-200">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="0fd04-201">CORS ara yazılımı, ([headernames. contentlanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) `WithHeaders`içinde `Content-Language` listelenmediği için aşağıdaki istek üst bilgisi ile bir ön denetim isteğini reddettiğinde:</span><span class="sxs-lookup"><span data-stu-id="0fd04-201">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="0fd04-202">Uygulama *200 ok* yanıtı DÖNDÜRÜYOR ancak CORS üst bilgilerini geri göndermez.</span><span class="sxs-lookup"><span data-stu-id="0fd04-202">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="0fd04-203">Bu nedenle tarayıcı, çıkış noktaları arası isteği denemez.</span><span class="sxs-lookup"><span data-stu-id="0fd04-203">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="0fd04-204">CORS ara yazılımı her zaman CorsPolicy `Access-Control-Request-Headers` . Headers ' de yapılandırılan değerlerden bağımsız olarak, ' deki dört üst bilgilerin gönderilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-204">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="0fd04-205">Bu üst bilgi listesi şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="0fd04-205">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="0fd04-206">Örneğin, aşağıdaki gibi yapılandırılmış bir uygulamayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="0fd04-206">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="0fd04-207">CORS ara yazılımı, her zaman beyaz listelenmiş olduğundan `Content-Language` , aşağıdaki istek üstbilgisiyle bir ön kontrol isteğine başarıyla yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="0fd04-207">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="0fd04-208">Gösterilen yanıt üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="0fd04-208">Set the exposed response headers</span></span>

<span data-ttu-id="0fd04-209">Varsayılan olarak tarayıcı, tüm yanıt üst bilgilerini uygulamaya sunmaz.</span><span class="sxs-lookup"><span data-stu-id="0fd04-209">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="0fd04-210">Daha fazla bilgi için bkz [. W3C çıkış noktaları arası kaynak paylaşımı (terminoloji): Basit yanıt üst](https://www.w3.org/TR/cors/#simple-response-header)bilgisi.</span><span class="sxs-lookup"><span data-stu-id="0fd04-210">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="0fd04-211">Varsayılan olarak kullanılabilen yanıt üstbilgileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="0fd04-211">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="0fd04-212">CORS belirtimi, bu üst bilgiler *basit yanıt üst bilgilerini*çağırır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-212">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="0fd04-213">Diğer üst bilgileri uygulama için kullanılabilir hale getirmek için şunu <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="0fd04-213">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="0fd04-214">Kaynaklar arası isteklerde kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="0fd04-214">Credentials in cross-origin requests</span></span>

<span data-ttu-id="0fd04-215">Kimlik bilgileri CORS isteğinde özel işleme gerektirir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-215">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="0fd04-216">Varsayılan olarak tarayıcı, kimlik bilgilerini bir çapraz kaynak isteğiyle göndermez.</span><span class="sxs-lookup"><span data-stu-id="0fd04-216">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="0fd04-217">Kimlik bilgileri, tanımlama bilgileri ve HTTP kimlik doğrulama düzenlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-217">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="0fd04-218">Bir çapraz kaynak isteğiyle kimlik bilgilerini göndermek için, istemcisinin olarak `XMLHttpRequest.withCredentials` `true`ayarlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-218">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="0fd04-219">Doğrudan `XMLHttpRequest` kullanarak:</span><span class="sxs-lookup"><span data-stu-id="0fd04-219">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="0fd04-220">JQuery kullanarak:</span><span class="sxs-lookup"><span data-stu-id="0fd04-220">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="0fd04-221">[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)'sini kullanma:</span><span class="sxs-lookup"><span data-stu-id="0fd04-221">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="0fd04-222">Sunucu kimlik bilgilerine izin vermelidir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-222">The server must allow the credentials.</span></span> <span data-ttu-id="0fd04-223">Çıkış noktaları arası kimlik bilgilerine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>şunu arayın:</span><span class="sxs-lookup"><span data-stu-id="0fd04-223">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="0fd04-224">HTTP yanıtı, tarayıcıya sunucunun `Access-Control-Allow-Credentials` bir çapraz kaynak isteği için kimlik bilgileri verdiğini bildiren bir üst bilgi içerir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-224">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="0fd04-225">Tarayıcı kimlik bilgilerini gönderirse ancak yanıt geçerli `Access-Control-Allow-Credentials` bir üst bilgi içermiyorsa, tarayıcı uygulamaya yanıtı kullanıma sunmaz ve çapraz kaynak isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="0fd04-225">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="0fd04-226">Çıkış noktaları arası kimlik bilgilerine izin vermek bir güvenlik riskidir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-226">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="0fd04-227">Başka bir etki alanındaki Web sitesi, kullanıcının bilgisi olmadan kullanıcı adına, oturum açmış bir kullanıcının kimlik bilgilerini uygulamaya gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-227">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="0fd04-228">CORS belirtimi Ayrıca `"*"` `Access-Control-Allow-Credentials` üst bilgi varsa, çıkış (tüm kaynaklar) ayarının geçersiz olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-228">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="0fd04-229">Ön kontrol istekleri</span><span class="sxs-lookup"><span data-stu-id="0fd04-229">Preflight requests</span></span>

<span data-ttu-id="0fd04-230">Bazı CORS istekleri için, tarayıcı gerçek isteği yapmadan önce ek bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-230">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="0fd04-231">Bu isteğe bir *ön kontrol isteği*denir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-231">This request is called a *preflight request*.</span></span> <span data-ttu-id="0fd04-232">Aşağıdaki koşullar doğruysa tarayıcı, ön kontrol isteğini atlayabilir:</span><span class="sxs-lookup"><span data-stu-id="0fd04-232">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="0fd04-233">İstek yöntemi al, HEAD veya POST.</span><span class="sxs-lookup"><span data-stu-id="0fd04-233">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="0fd04-234">Uygulama `Accept` ,`Accept-Language` ,,`Last-Event-ID`veya dışındaki istek üst bilgilerini ayarlanmamış. `Content-Language` `Content-Type`</span><span class="sxs-lookup"><span data-stu-id="0fd04-234">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="0fd04-235">, Ayarlandıysa, aşağıdaki değerlerden birine sahip olan üstbilgi:`Content-Type`</span><span class="sxs-lookup"><span data-stu-id="0fd04-235">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="0fd04-236">İstemci isteği için ayarlanan istek üst bilgileri kuralı, uygulamanın, `setRequestHeader` `XMLHttpRequest` nesne üzerinde çağırarak ayarladığı üst bilgiler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-236">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="0fd04-237">CORS belirtimi, bu üst bilgiler *Yazar istek üst bilgilerini*çağırır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-237">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="0fd04-238">Kural `User-Agent` `Content-Length`, `Host`, veya gibi tarayıcının ayarlayabilmesi için, veya gibi bir üst bilgiye uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="0fd04-238">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="0fd04-239">Aşağıda bir ön denetim isteğine örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="0fd04-239">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="0fd04-240">Uçuş öncesi isteği HTTP SEÇENEKLERI yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-240">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="0fd04-241">İki özel üst bilgi içerir:</span><span class="sxs-lookup"><span data-stu-id="0fd04-241">It includes two special headers:</span></span>

* <span data-ttu-id="0fd04-242">`Access-Control-Request-Method`: Gerçek istek için kullanılacak HTTP yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0fd04-242">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="0fd04-243">`Access-Control-Request-Headers`: Uygulamanın gerçek istekte ayarladığı istek üst bilgilerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="0fd04-243">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="0fd04-244">Daha önce belirtildiği gibi, bu, tarayıcının ayarladığı `User-Agent`üst bilgileri içermez.</span><span class="sxs-lookup"><span data-stu-id="0fd04-244">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="0fd04-245">CORS ön kontrol isteği, sunucuya gerçek `Access-Control-Request-Headers` istekle gönderilen üstbilgileri belirten bir üst bilgi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-245">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="0fd04-246">Belirli üstbilgilere izin vermek için şunu <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>arayın:</span><span class="sxs-lookup"><span data-stu-id="0fd04-246">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="0fd04-247">Tüm yazar isteği başlıklarına izin vermek için şunu <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="0fd04-247">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="0fd04-248">Tarayıcılar, nasıl ayarlandıklarından `Access-Control-Request-Headers`tamamen tutarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-248">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="0fd04-249">Üst bilgileri ( `"*"` veya kullanımı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>) dışında bir şeye ayarlarsanız, desteklemek istediğiniz en az `Accept`, `Content-Type`, ve ve `Origin`Ek üstbilgileri dahil etmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="0fd04-249">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="0fd04-250">Aşağıda, ön kontrol isteğine örnek bir yanıt verilmiştir (sunucunun isteğe izin verdiği varsayıldığında):</span><span class="sxs-lookup"><span data-stu-id="0fd04-250">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="0fd04-251">Yanıt, izin verilen `Access-Control-Allow-Methods` üst bilgileri listeleyen, izin verilen yöntemleri ve isteğe `Access-Control-Allow-Headers` bağlı olarak bir üst bilgiyi listeleyen bir üst bilgi içerir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-251">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="0fd04-252">Ön kontrol isteği başarılı olursa, tarayıcı gerçek isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-252">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="0fd04-253">Ön kontrol isteği reddedilirse, uygulama *200 ok* yanıtı döndürür ancak CORS üst bilgilerini geri göndermez.</span><span class="sxs-lookup"><span data-stu-id="0fd04-253">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="0fd04-254">Bu nedenle tarayıcı, çıkış noktaları arası isteği denemez.</span><span class="sxs-lookup"><span data-stu-id="0fd04-254">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="0fd04-255">Ön kontrol sona erme süresini ayarlama</span><span class="sxs-lookup"><span data-stu-id="0fd04-255">Set the preflight expiration time</span></span>

<span data-ttu-id="0fd04-256">`Access-Control-Max-Age` Üst bilgi, ön kontrol isteğine olan yanıtın ne kadar süreyle önbelleğe alınacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-256">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="0fd04-257">Bu üstbilgiyi ayarlamak için şunu arayın <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="0fd04-257">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="0fd04-258">CORS nasıl kullanılır?</span><span class="sxs-lookup"><span data-stu-id="0fd04-258">How CORS works</span></span>

<span data-ttu-id="0fd04-259">Bu bölümde, HTTP iletileri düzeyindeki bir [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) isteğinde ne olacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-259">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="0fd04-260">CORS bir güvenlik özelliği **değil** .</span><span class="sxs-lookup"><span data-stu-id="0fd04-260">CORS is **not** a security feature.</span></span> <span data-ttu-id="0fd04-261">CORS, bir sunucunun aynı kaynaklı ilkeyi rahat bir şekilde sağlamasına izin veren bir W3C standardıdır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-261">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="0fd04-262">Örneğin, kötü niyetli bir aktör sitenize karşı [siteler arası komut dosyalarını (XSS) engelliyor](xref:security/cross-site-scripting) ve bilgileri çalmak için CORS özellikli sitesine bir siteler arası istek yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-262">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="0fd04-263">CORS 'nin izin vermesini sağlayan API 'niz daha güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-263">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="0fd04-264">CORS 'yi zorlamak için istemciye (tarayıcı) sahiptir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-264">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="0fd04-265">Sunucu isteği yürütür ve yanıtı döndürür. Bu, bir hatayı döndüren ve yanıtı engelleyen istemcdir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-265">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="0fd04-266">Örneğin, aşağıdaki araçlardan herhangi birinde sunucu yanıtı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="0fd04-266">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="0fd04-267">Fiddler</span><span class="sxs-lookup"><span data-stu-id="0fd04-267">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="0fd04-268">Postman</span><span class="sxs-lookup"><span data-stu-id="0fd04-268">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="0fd04-269">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="0fd04-269">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="0fd04-270">Adres çubuğuna URL girerek bir Web tarayıcısı.</span><span class="sxs-lookup"><span data-stu-id="0fd04-270">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="0fd04-271">Bir sunucu, tarayıcıların çapraz kaynak [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) veya [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) isteği çalıştırmasına izin vermesinin yasaktır bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="0fd04-271">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="0fd04-272">Tarayıcılar (CORS olmadan), çapraz kaynak istekleri yapamıyor.</span><span class="sxs-lookup"><span data-stu-id="0fd04-272">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="0fd04-273">CORS 'den önce, bu kısıtlamayı aşmak için [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-273">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="0fd04-274">JSONP XHR kullanmaz, yanıtı almak için `<script>` etiketini kullanır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-274">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="0fd04-275">Betiklerin, çapraz kaynak olarak yüklenmesine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-275">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="0fd04-276">[CORS belirtimi](https://www.w3.org/TR/cors/) , çıkış noktaları arası istekleri etkinleştiren bırkaç yeni http üst bilgisi sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="0fd04-276">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="0fd04-277">Bir tarayıcı CORS 'yi destekliyorsa, bu üst bilgileri, çıkış noktaları arası istekler için otomatik olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0fd04-277">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="0fd04-278">CORS 'yi etkinleştirmek için özel JavaScript kodu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-278">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="0fd04-279">Aşağıda, bir çapraz kaynak isteğine bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-279">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="0fd04-280">`Origin` Üst bilgi, isteği yapan sitenin etki alanını sağlar:</span><span class="sxs-lookup"><span data-stu-id="0fd04-280">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="0fd04-281">Sunucu isteğe izin veriyorsa, yanıttaki `Access-Control-Allow-Origin` üstbilgiyi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0fd04-281">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="0fd04-282">Bu üstbilginin değeri, istekten gelen `Origin` üstbilgiyle eşleşir veya joker değerdir `"*"`, yani herhangi bir kaynağa izin verilir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-282">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="0fd04-283">Yanıt `Access-Control-Allow-Origin` üstbilgiyi içermiyorsa, çapraz kaynak isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="0fd04-283">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="0fd04-284">Özellikle, tarayıcı isteğe izin vermez.</span><span class="sxs-lookup"><span data-stu-id="0fd04-284">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="0fd04-285">Sunucu başarılı bir yanıt döndürse bile tarayıcı, yanıtı istemci uygulama için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="0fd04-285">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="0fd04-286">Test CORS</span><span class="sxs-lookup"><span data-stu-id="0fd04-286">Test CORS</span></span>

<span data-ttu-id="0fd04-287">CORS 'yi sınamak için:</span><span class="sxs-lookup"><span data-stu-id="0fd04-287">To test CORS:</span></span>

1. <span data-ttu-id="0fd04-288">[BIR API projesi oluşturun](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="0fd04-288">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="0fd04-289">Alternatif olarak, [örneği de indirebilirsiniz](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="0fd04-289">Alternatively, you can [download the sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="0fd04-290">Bu belgedeki yaklaşımlardan birini kullanarak CORS 'yi etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="0fd04-290">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="0fd04-291">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0fd04-291">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="0fd04-292">`WithOrigins("https://localhost:<port>");`yalnızca [indirme örnek koduna](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)benzer bir örnek uygulamanın test edilmesi için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-292">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="0fd04-293">Web uygulaması projesi (Razor Pages veya MVC) oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0fd04-293">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="0fd04-294">Örnek Razor Pages kullanır.</span><span class="sxs-lookup"><span data-stu-id="0fd04-294">The sample uses Razor Pages.</span></span> <span data-ttu-id="0fd04-295">Web uygulamasını, API projesiyle aynı çözümde oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0fd04-295">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="0fd04-296">Aşağıdaki Vurgulanan kodu *Index. cshtml* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="0fd04-296">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="0fd04-297">Yukarıdaki kodda, dağıtılan uygulamanın URL `url: 'https://<web app>.azurewebsites.net/api/values/1',` 'siyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="0fd04-297">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="0fd04-298">API projesini dağıtın.</span><span class="sxs-lookup"><span data-stu-id="0fd04-298">Deploy the API project.</span></span> <span data-ttu-id="0fd04-299">Örneğin, [Azure 'a dağıtın](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="0fd04-299">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="0fd04-300">Razor Pages veya MVC uygulamasını masaüstünden çalıştırın ve **Test** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0fd04-300">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="0fd04-301">Hata iletilerini gözden geçirmek için F12 araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="0fd04-301">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="0fd04-302">Localhost kaynağını `WithOrigins` kaldırın ve uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="0fd04-302">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="0fd04-303">Alternatif olarak, istemci uygulamasını farklı bir bağlantı noktasıyla çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0fd04-303">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="0fd04-304">Örneğin, Visual Studio 'dan çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0fd04-304">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="0fd04-305">İstemci uygulamasıyla test edin.</span><span class="sxs-lookup"><span data-stu-id="0fd04-305">Test with the client app.</span></span> <span data-ttu-id="0fd04-306">CORS hataları bir hata döndürüyor, ancak hata iletisi JavaScript için kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="0fd04-306">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="0fd04-307">Hatayı görmek için F12 araçlarındaki konsol sekmesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="0fd04-307">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="0fd04-308">Tarayıcıya bağlı olarak, aşağıdakine benzer bir hata alırsınız (F12 araçları konsolunda):</span><span class="sxs-lookup"><span data-stu-id="0fd04-308">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="0fd04-309">Microsoft Edge 'i kullanarak:</span><span class="sxs-lookup"><span data-stu-id="0fd04-309">Using Microsoft Edge:</span></span>

     <span data-ttu-id="0fd04-310">**SEC7120: [CORS] kaynak `https://localhost:44375` , ' deki çıkış noktaları için erişim-denetim-izin-kaynak yanıt üstbilgisinde bulunamadı `https://localhost:44375``https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="0fd04-310">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="0fd04-311">Chrome kullanarak:</span><span class="sxs-lookup"><span data-stu-id="0fd04-311">Using Chrome:</span></span>

     <span data-ttu-id="0fd04-312">**`https://webapi.azurewebsites.net/api/values/1` Kaynak`https://localhost:44375` 'ten itibaren XMLHttpRequest erişimi, CORS ilkesi tarafından engellendi: İstenen kaynakta ' erişim-denetim-Izin-Origin ' üst bilgisi yok.**</span><span class="sxs-lookup"><span data-stu-id="0fd04-312">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0fd04-313">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0fd04-313">Additional resources</span></span>

* [<span data-ttu-id="0fd04-314">Çıkış noktaları arası kaynak paylaşımı (CORS)</span><span class="sxs-lookup"><span data-stu-id="0fd04-314">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
