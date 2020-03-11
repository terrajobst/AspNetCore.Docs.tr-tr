---
title: ASP.NET Core 'de çıkış noktaları arası Istekleri (CORS) etkinleştirme
author: rick-anderson
description: ASP.NET Core uygulamasında çapraz kaynak isteklerini izin vermek veya reddetmek için CORS 'nin nasıl standart olduğunu öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: e0e0e1abf1ecaa12038b3ee1bdaa384d979be254
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666252"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="b0b0c-103">ASP.NET Core 'de çıkış noktaları arası Istekleri (CORS) etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="b0b0c-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="b0b0c-104">Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b0b0c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b0b0c-105">Bu makalede, ASP.NET Core uygulamasında CORS 'nin nasıl etkinleştirileceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="b0b0c-106">Tarayıcı güvenliği, bir Web sayfasının Web sayfasını sunduğundan farklı bir etki alanına istek yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="b0b0c-107">Bu kısıtlamaya *aynı-Origin ilkesi*adı verilir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="b0b0c-108">Aynı-kaynak ilkesi, kötü niyetli bir sitenin gizli verileri başka bir siteden okumasını engeller.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="b0b0c-109">Bazen diğer sitelerin uygulamanıza çapraz çıkış istekleri yapmasına izin vermek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="b0b0c-110">Daha fazla bilgi için bkz. [MOZILLA CORS makalesi](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="b0b0c-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="b0b0c-111">[Çapraz kaynak kaynak paylaşımı](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="b0b0c-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="b0b0c-112">, Bir sunucunun aynı kaynak ilkeyi rahat bir şekilde sağlamasına izin veren bir W3C standardıdır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="b0b0c-113">Güvenlik özelliği **değil** , CORS güvenliği.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="b0b0c-114">CORS 'nin izin vermesini sağlayan bir API daha güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="b0b0c-115">Daha fazla bilgi için bkz. [CORS çalışma](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="b0b0c-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="b0b0c-116">Bir sunucunun bazı çapraz kaynak isteklerine, diğerlerini reddetirken açık olarak izin almasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="b0b0c-117">, [JSONP](/dotnet/framework/wcf/samples/jsonp)gibi önceki tekniklerin daha güvenli ve daha esnektir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="b0b0c-118">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b0b0c-118">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="b0b0c-119">Aynı kaynak</span><span class="sxs-lookup"><span data-stu-id="b0b0c-119">Same origin</span></span>

<span data-ttu-id="b0b0c-120">Özdeş şemaları, konakları ve bağlantı noktalarına sahip olmaları durumunda iki URL aynı kaynağa sahiptir ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="b0b0c-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="b0b0c-121">Bu iki URL aynı kaynağa sahiptir:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="b0b0c-122">Bu URL 'Ler, önceki iki URL 'den farklı kaynaklardan farklıdır:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="b0b0c-123">Farklı etki alanı &ndash; `https://example.net`</span><span class="sxs-lookup"><span data-stu-id="b0b0c-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="b0b0c-124">Farklı alt etki alanı &ndash; `https://www.example.com/foo.html`</span><span class="sxs-lookup"><span data-stu-id="b0b0c-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="b0b0c-125">Farklı &ndash; düzeni `http://example.com/foo.html`</span><span class="sxs-lookup"><span data-stu-id="b0b0c-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="b0b0c-126">Farklı bağlantı noktası &ndash; `https://example.com:9000/foo.html`</span><span class="sxs-lookup"><span data-stu-id="b0b0c-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="b0b0c-127">Internet Explorer, kaynakları karşılaştırırken bağlantı noktasını dikkate almaz.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="b0b0c-128">Adlandırılmış ilke ve ara yazılım ile CORS</span><span class="sxs-lookup"><span data-stu-id="b0b0c-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="b0b0c-129">CORS ara yazılımı, çıkış noktaları arası istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="b0b0c-130">Aşağıdaki kod, belirtilen kaynağa sahip tüm uygulama için CORS 'yi sunar:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="b0b0c-131">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-131">The preceding code:</span></span>

* <span data-ttu-id="b0b0c-132">İlke adını "\_Myallowspecifickaynaklarına" olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="b0b0c-133">İlke adı rastgele olur.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="b0b0c-134">CORS 'yi sağlayan <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> uzantısı yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="b0b0c-135">[Lambda ifadesiyle](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="b0b0c-136">Lambda bir <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> nesnesi alır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="b0b0c-137">`WithOrigins`gibi [yapılandırma seçenekleri](#cors-policy-options), bu makalenin ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="b0b0c-138"><xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> yöntemi çağrısı, uygulamanın hizmet kapsayıcısına CORS Hizmetleri ekler:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="b0b0c-139">Daha fazla bilgi için bu belgedeki [CORS ilke seçenekleri](#cpo) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="b0b0c-140"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> yöntemi aşağıdaki kodda gösterildiği gibi yöntemleri zincirleyebilir:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="b0b0c-141">Not: URL sonunda eğik çizgi (`/` **) bulunmamalıdır.**</span><span class="sxs-lookup"><span data-stu-id="b0b0c-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="b0b0c-142">URL `/`ile sonlandığında, karşılaştırma `false` döndürür ve üst bilgi döndürülmez.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a><span data-ttu-id="b0b0c-143">CORS ilkelerini tüm uç noktalara Uygula</span><span class="sxs-lookup"><span data-stu-id="b0b0c-143">Apply CORS policies to all endpoints</span></span>

<span data-ttu-id="b0b0c-144">Aşağıdaki kod, CORS ara yazılımı aracılığıyla CORS ilkelerini tüm uygulama uç noktalarına uygular:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-144">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
> <span data-ttu-id="b0b0c-145">Endpoint Routing ile CORS ara yazılımı, `UseRouting` ve `UseEndpoints`çağrıları arasında yürütülecek şekilde yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-145">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="b0b0c-146">Yanlış yapılandırma, ara yazılımın doğru çalışmayı durdurmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-146">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="b0b0c-147">Aşağıdaki kod, CORS ara yazılımı aracılığıyla CORS ilkelerini tüm uygulama uç noktalarına uygular:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-147">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="b0b0c-148">Note: `UseMvc`önce `UseCors` çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-148">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="b0b0c-149">Bu sayfa/denetleyici/eylem düzeyinde CORS ilkesini uygulamak için [Razor Pages, denetleyiciler ve eylem YÖNTEMLERINDE CORS 'Yi etkinleştirin](#ecors) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-149">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="b0b0c-150">Önceki kodun test edilmesine ilişkin yönergeler için bkz. [Test CORS](#test) .</span><span class="sxs-lookup"><span data-stu-id="b0b0c-150">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="b0b0c-151">Uç nokta yönlendirme ile CORS 'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="b0b0c-151">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="b0b0c-152">Uç nokta yönlendirme ile CORS, `RequireCors` uzantı yöntemleri kullanılarak uç nokta temelinde etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-152">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="b0b0c-153">Benzer şekilde, CORS tüm denetleyiciler için de etkinleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-153">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="b0b0c-154">CORS 'yi özniteliklerle etkinleştir</span><span class="sxs-lookup"><span data-stu-id="b0b0c-154">Enable CORS with attributes</span></span>

<span data-ttu-id="b0b0c-155">[&lbrack;enablecors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) ÖZNITELIĞI, CORS 'yi küresel olarak uygulamaya bir alternatif sağlar.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-155">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="b0b0c-156">`[EnableCors]` özniteliği, tüm bitiş noktaları yerine, seçilen bitiş noktaları için CORS 'yi mümkün.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-156">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="b0b0c-157">İlke belirtmek için varsayılan ilkeyi ve `[EnableCors("{Policy String}")]` belirtmek için `[EnableCors]` kullanın.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-157">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="b0b0c-158">`[EnableCors]` özniteliği şu şekilde uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-158">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="b0b0c-159">Razor sayfası `PageModel`</span><span class="sxs-lookup"><span data-stu-id="b0b0c-159">Razor Page `PageModel`</span></span>
* <span data-ttu-id="b0b0c-160">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="b0b0c-160">Controller</span></span>
* <span data-ttu-id="b0b0c-161">Denetleyici eylemi yöntemi</span><span class="sxs-lookup"><span data-stu-id="b0b0c-161">Controller action method</span></span>

<span data-ttu-id="b0b0c-162">`[EnableCors]` özniteliğiyle, denetleyici/sayfa-model/eyleme farklı ilkeler uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-162">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="b0b0c-163">`[EnableCors]` özniteliği bir denetleyiciler/sayfa modeli/eylem yöntemine uygulandığında ve bir ara yazılım içinde CORS etkinleştirildiğinde her iki ilke de uygulanır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-163">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="b0b0c-164">İlkelerin birleştirilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-164">We recommend against combining policies.</span></span> <span data-ttu-id="b0b0c-165">Aynı uygulamada değil, `[EnableCors]` özniteliği veya ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-165">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="b0b0c-166">Aşağıdaki kod her bir yönteme farklı bir ilke uygular:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-166">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="b0b0c-167">Aşağıdaki kod, bir CORS varsayılan ilkesi ve `"AnotherPolicy"`adlı bir ilke oluşturur:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-167">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="b0b0c-168">CORS 'yi devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="b0b0c-168">Disable CORS</span></span>

<span data-ttu-id="b0b0c-169">[&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) özniteliği denetleyici/sayfa modeli/eylem için CORS 'yi devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-169">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="b0b0c-170">CORS ilke seçenekleri</span><span class="sxs-lookup"><span data-stu-id="b0b0c-170">CORS policy options</span></span>

<span data-ttu-id="b0b0c-171">Bu bölümde, bir CORS ilkesinde ayarlanmakta olabilecek çeşitli seçenekler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-171">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="b0b0c-172">İzin verilen kaynakları ayarla</span><span class="sxs-lookup"><span data-stu-id="b0b0c-172">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="b0b0c-173">İzin verilen HTTP yöntemlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="b0b0c-173">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="b0b0c-174">İzin verilen istek üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="b0b0c-174">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="b0b0c-175">Gösterilen yanıt üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="b0b0c-175">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="b0b0c-176">Kaynaklar arası isteklerde kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="b0b0c-176">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="b0b0c-177">Ön kontrol sona erme süresini ayarlama</span><span class="sxs-lookup"><span data-stu-id="b0b0c-177">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="b0b0c-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> `Startup.ConfigureServices`olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b0b0c-179">Bazı seçenekler için, ilk olarak [CORS 'Nin nasıl çalıştığı](#how-cors) bölümü okumanız yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-179">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="b0b0c-180">İzin verilen kaynakları ayarla</span><span class="sxs-lookup"><span data-stu-id="b0b0c-180">Set the allowed origins</span></span>

<span data-ttu-id="b0b0c-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; tüm kaynaklardan gelen CORS isteklerinin herhangi bir düzene (`http` veya `https`) Izin verir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="b0b0c-182">*herhangi bir Web sitesi* uygulamaya çapraz kaynak istekleri yapabildiğinden `AllowAnyOrigin` güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-182">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="b0b0c-183">`AllowAnyOrigin` ve `AllowCredentials` belirtme, güvenli olmayan bir yapılandırmadır ve siteler arası istek elde edilmesine yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-183">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="b0b0c-184">Bir uygulama her iki yöntemle yapılandırıldığında, CORS hizmeti geçersiz bir CORS yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-184">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="b0b0c-185">`AllowAnyOrigin` ve `AllowCredentials` belirtme, güvenli olmayan bir yapılandırmadır ve siteler arası istek elde edilmesine yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-185">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="b0b0c-186">Güvenli bir uygulama için, istemcinin sunucu kaynaklarına erişim yetkisi olması gerekiyorsa, kaynakların tam bir listesini belirtin.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-186">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="b0b0c-187">`AllowAnyOrigin`, ön kontrol isteklerini ve `Access-Control-Allow-Origin` üst bilgisini etkiler.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-187">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="b0b0c-188">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b0b0c-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash;, kaynağın <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> özelliğini, kaynağa izin verilip verilmediğini değerlendirirken, kaynağın, yapılandırılan bir joker karakterle eşleştiğinden emin olan bir işlev olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="b0b0c-190">İzin verilen HTTP yöntemlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="b0b0c-190">Set the allowed HTTP methods</span></span>

<span data-ttu-id="b0b0c-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="b0b0c-192">Herhangi bir HTTP yöntemine izin verir:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-192">Allows any HTTP method:</span></span>
* <span data-ttu-id="b0b0c-193">Ön kontrol isteklerini ve `Access-Control-Allow-Methods` üst bilgisini etkiler.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-193">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="b0b0c-194">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-194">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="b0b0c-195">İzin verilen istek üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="b0b0c-195">Set the allowed request headers</span></span>

<span data-ttu-id="b0b0c-196">Belirli başlıkların, *Yazar istek üstbilgileri*ADLı bir CORS isteğinde gönderilmesine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> çağırın ve izin verilen üst bilgileri belirtin:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-196">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="b0b0c-197">Tüm yazar isteği üst bilgilerine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-197">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="b0b0c-198">Bu ayar, ön kontrol isteklerini ve `Access-Control-Request-Headers` üst bilgisini etkiler.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-198">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="b0b0c-199">Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-199">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b0b0c-200">`WithHeaders` tarafından belirtilen belirli başlıklarıyla eşleşen bir CORS ara yazılım ilkesi, yalnızca `Access-Control-Request-Headers` gönderilen üstbilgiler `WithHeaders`belirtilen üstbilgilere tam olarak eşleşiyorsa mümkündür.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-200">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="b0b0c-201">Örneğin, aşağıdaki gibi yapılandırılmış bir uygulamayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="b0b0c-202">CORS ara yazılımı, `Content-Language` ([Headernames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) `WithHeaders`listede olmadığından, şu istek üstbilgisiyle bir ön kontrol isteğini reddeder:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-202">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="b0b0c-203">Uygulama *200 ok* yanıtı DÖNDÜRÜYOR ancak CORS üst bilgilerini geri göndermez.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-203">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="b0b0c-204">Bu nedenle tarayıcı, çıkış noktaları arası isteği denemez.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-204">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b0b0c-205">CORS ara yazılımı, CorsPolicy. Headers ' de yapılandırılan değerlere bakılmaksızın `Access-Control-Request-Headers` her zaman dört üst bilgilerin gönderilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-205">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="b0b0c-206">Bu üst bilgi listesi şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-206">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="b0b0c-207">Örneğin, aşağıdaki gibi yapılandırılmış bir uygulamayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-207">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="b0b0c-208">`Content-Language` her zaman beyaz listeye alındığından, CORS ara yazılımı aşağıdaki istek üstbilgisiyle bir ön kontrol isteğine başarıyla yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-208">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="b0b0c-209">Gösterilen yanıt üst bilgilerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="b0b0c-209">Set the exposed response headers</span></span>

<span data-ttu-id="b0b0c-210">Varsayılan olarak tarayıcı, tüm yanıt üst bilgilerini uygulamaya sunmaz.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-210">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="b0b0c-211">Daha fazla bilgi için bkz. [W3C çıkış noktaları arası kaynak paylaşımı (terminoloji): basit yanıt üst bilgisi](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="b0b0c-211">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="b0b0c-212">Varsayılan olarak kullanılabilen yanıt üstbilgileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-212">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="b0b0c-213">CORS belirtimi, bu üst bilgiler *basit yanıt üst bilgilerini*çağırır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-213">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="b0b0c-214">Diğer üst bilgileri uygulama için kullanılabilir hale getirmek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-214">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="b0b0c-215">Kaynaklar arası isteklerde kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="b0b0c-215">Credentials in cross-origin requests</span></span>

<span data-ttu-id="b0b0c-216">Kimlik bilgileri CORS isteğinde özel işleme gerektirir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-216">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="b0b0c-217">Varsayılan olarak tarayıcı, kimlik bilgilerini bir çapraz kaynak isteğiyle göndermez.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-217">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="b0b0c-218">Kimlik bilgileri, tanımlama bilgileri ve HTTP kimlik doğrulama düzenlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-218">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="b0b0c-219">Bir çapraz kaynak isteğiyle kimlik bilgilerini göndermek için, istemcisinin `true``XMLHttpRequest.withCredentials` ayarlaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-219">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="b0b0c-220">`XMLHttpRequest` doğrudan kullanarak:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-220">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="b0b0c-221">JQuery kullanarak:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-221">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="b0b0c-222">[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)'sini kullanma:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-222">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="b0b0c-223">Sunucu kimlik bilgilerine izin vermelidir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-223">The server must allow the credentials.</span></span> <span data-ttu-id="b0b0c-224">Çıkış noktaları arası kimlik bilgilerine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-224">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="b0b0c-225">HTTP yanıtı, tarayıcıya sunucunun bir çapraz kaynak isteği için kimlik bilgileri verdiğini bildiren bir `Access-Control-Allow-Credentials` üst bilgisi içerir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-225">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="b0b0c-226">Tarayıcı kimlik bilgilerini gönderirse ancak yanıt geçerli bir `Access-Control-Allow-Credentials` üst bilgisi içermiyorsa, tarayıcı uygulamaya yanıtı kullanıma sunmaz ve çapraz kaynak isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-226">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="b0b0c-227">Çıkış noktaları arası kimlik bilgilerine izin vermek bir güvenlik riskidir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-227">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="b0b0c-228">Başka bir etki alanındaki Web sitesi, kullanıcının bilgisi olmadan kullanıcı adına, oturum açmış bir kullanıcının kimlik bilgilerini uygulamaya gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-228">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="b0b0c-229">CORS belirtimi Ayrıca, `Access-Control-Allow-Credentials` üst bilgisi varsa, `"*"` (tüm kaynaklar) için çıkış ayarının geçersiz olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-229">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="b0b0c-230">Ön kontrol istekleri</span><span class="sxs-lookup"><span data-stu-id="b0b0c-230">Preflight requests</span></span>

<span data-ttu-id="b0b0c-231">Bazı CORS istekleri için, tarayıcı gerçek isteği yapmadan önce ek bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-231">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="b0b0c-232">Bu isteğe bir *ön kontrol isteği*denir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-232">This request is called a *preflight request*.</span></span> <span data-ttu-id="b0b0c-233">Aşağıdaki koşullar doğruysa tarayıcı, ön kontrol isteğini atlayabilir:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-233">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="b0b0c-234">İstek yöntemi al, HEAD veya POST.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-234">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="b0b0c-235">Uygulama `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`veya `Last-Event-ID`dışındaki istek üst bilgilerini ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-235">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="b0b0c-236">Eğer ayarlandıysa `Content-Type` üst bilgisi aşağıdaki değerlerden birine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-236">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="b0b0c-237">İstemci isteği için ayarlanan istek üst bilgileri kuralı, `XMLHttpRequest` nesnesine `setRequestHeader` çağırarak uygulamanın ayarladığı üst bilgiler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-237">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="b0b0c-238">CORS belirtimi, bu üst bilgiler *Yazar istek üst bilgilerini*çağırır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-238">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="b0b0c-239">Kural, tarayıcının ayarlayabilmesi için `User-Agent`, `Host`veya `Content-Length`gibi üstbilgilere uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-239">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="b0b0c-240">Aşağıda bir ön denetim isteğine örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-240">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="b0b0c-241">Uçuş öncesi isteği HTTP SEÇENEKLERI yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-241">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="b0b0c-242">İki özel üst bilgi içerir:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-242">It includes two special headers:</span></span>

* <span data-ttu-id="b0b0c-243">`Access-Control-Request-Method`: gerçek istek için kullanılacak HTTP yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-243">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="b0b0c-244">`Access-Control-Request-Headers`: uygulamanın gerçek istekte ayarladığı istek üst bilgilerinin bir listesi.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-244">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="b0b0c-245">Daha önce belirtildiği gibi, bu, tarayıcının ayarladığı `User-Agent`gibi üst bilgileri içermez.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-245">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="b0b0c-246">CORS ön kontrol isteği, sunucuya gerçek istekle gönderilen üstbilgileri belirten bir `Access-Control-Request-Headers` üst bilgisi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-246">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="b0b0c-247">Belirli üstbilgilere izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-247">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="b0b0c-248">Tüm yazar isteği üst bilgilerine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-248">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="b0b0c-249">Tarayıcılar, `Access-Control-Request-Headers`nasıl ayarlandıklarından tamamen tutarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-249">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="b0b0c-250">Üst bilgileri `"*"` dışında bir şeye ayarlarsanız (veya <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>kullanıyorsanız), en az `Accept`, `Content-Type`ve `Origin`, ayrıca desteklemek istediğiniz tüm özel üstbilgileri dahil etmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-250">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="b0b0c-251">Aşağıda, ön kontrol isteğine örnek bir yanıt verilmiştir (sunucunun isteğe izin verdiği varsayıldığında):</span><span class="sxs-lookup"><span data-stu-id="b0b0c-251">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="b0b0c-252">Yanıt, izin verilen üst bilgileri listeleyen, izin verilen yöntemleri ve isteğe bağlı olarak bir `Access-Control-Allow-Headers` üstbilgisini listeleyen bir `Access-Control-Allow-Methods` üst bilgisi içerir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-252">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="b0b0c-253">Ön kontrol isteği başarılı olursa, tarayıcı gerçek isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-253">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="b0b0c-254">Ön kontrol isteği reddedilirse, uygulama *200 ok* yanıtı döndürür ancak CORS üst bilgilerini geri göndermez.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-254">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="b0b0c-255">Bu nedenle tarayıcı, çıkış noktaları arası isteği denemez.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-255">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="b0b0c-256">Ön kontrol sona erme süresini ayarlama</span><span class="sxs-lookup"><span data-stu-id="b0b0c-256">Set the preflight expiration time</span></span>

<span data-ttu-id="b0b0c-257">`Access-Control-Max-Age` üstbilgisi, ön kontrol isteğine olan yanıtın ne kadar süreyle önbelleğe alınacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-257">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="b0b0c-258">Bu üstbilgiyi ayarlamak için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-258">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="b0b0c-259">CORS nasıl kullanılır?</span><span class="sxs-lookup"><span data-stu-id="b0b0c-259">How CORS works</span></span>

<span data-ttu-id="b0b0c-260">Bu bölümde, HTTP iletileri düzeyindeki bir [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) isteğinde ne olacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-260">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="b0b0c-261">CORS bir güvenlik özelliği **değil** .</span><span class="sxs-lookup"><span data-stu-id="b0b0c-261">CORS is **not** a security feature.</span></span> <span data-ttu-id="b0b0c-262">CORS, bir sunucunun aynı kaynaklı ilkeyi rahat bir şekilde sağlamasına izin veren bir W3C standardıdır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-262">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="b0b0c-263">Örneğin, kötü niyetli bir aktör sitenize karşı [siteler arası komut dosyalarını (XSS) engelliyor](xref:security/cross-site-scripting) ve bilgileri çalmak için CORS özellikli sitesine bir siteler arası istek yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-263">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="b0b0c-264">CORS 'nin izin vermesini sağlayan API 'niz daha güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-264">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="b0b0c-265">CORS 'yi zorlamak için istemciye (tarayıcı) sahiptir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-265">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="b0b0c-266">Sunucu isteği yürütür ve yanıtı döndürür. Bu, bir hatayı döndüren ve yanıtı engelleyen istemcdir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-266">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="b0b0c-267">Örneğin, aşağıdaki araçlardan herhangi birinde sunucu yanıtı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-267">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="b0b0c-268">Fiddler</span><span class="sxs-lookup"><span data-stu-id="b0b0c-268">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="b0b0c-269">Postman</span><span class="sxs-lookup"><span data-stu-id="b0b0c-269">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="b0b0c-270">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="b0b0c-270">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="b0b0c-271">Adres çubuğuna URL girerek bir Web tarayıcısı.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-271">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="b0b0c-272">Bir sunucu, tarayıcıların çapraz kaynak [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) veya [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) isteği çalıştırmasına izin vermesinin yasaktır bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-272">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="b0b0c-273">Tarayıcılar (CORS olmadan), çapraz kaynak istekleri yapamıyor.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-273">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="b0b0c-274">CORS 'den önce, bu kısıtlamayı aşmak için [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-274">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="b0b0c-275">JSONP XHR kullanmaz, yanıtı almak için `<script>` etiketini kullanır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-275">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="b0b0c-276">Betiklerin, çapraz kaynak olarak yüklenmesine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-276">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="b0b0c-277">[CORS belirtimi](https://www.w3.org/TR/cors/) , çıkış noktaları arası istekleri etkinleştiren bırkaç yeni http üst bilgisi sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-277">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="b0b0c-278">Bir tarayıcı CORS 'yi destekliyorsa, bu üst bilgileri, çıkış noktaları arası istekler için otomatik olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-278">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="b0b0c-279">CORS 'yi etkinleştirmek için özel JavaScript kodu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-279">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="b0b0c-280">Aşağıda, bir çapraz kaynak isteğine bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-280">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="b0b0c-281">`Origin` üstbilgisi, isteği yapan sitenin etki alanını sağlar.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-281">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="b0b0c-282">`Origin` üst bilgisi gereklidir ve konaktan farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-282">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="b0b0c-283">Sunucu isteğe izin veriyorsa, yanıtta `Access-Control-Allow-Origin` üst bilgisini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-283">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="b0b0c-284">Bu üstbilginin değeri, istekten `Origin` üst bilgisiyle eşleşir veya `"*"`joker değerdir, yani herhangi bir kaynağa izin verilir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-284">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="b0b0c-285">Yanıt `Access-Control-Allow-Origin` üst bilgisini içermiyorsa, çapraz kaynak isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-285">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="b0b0c-286">Özellikle, tarayıcı isteğe izin vermez.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-286">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="b0b0c-287">Sunucu başarılı bir yanıt döndürse bile tarayıcı, yanıtı istemci uygulama için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-287">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="b0b0c-288">Test CORS</span><span class="sxs-lookup"><span data-stu-id="b0b0c-288">Test CORS</span></span>

<span data-ttu-id="b0b0c-289">CORS 'yi sınamak için:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-289">To test CORS:</span></span>

1. <span data-ttu-id="b0b0c-290">[BIR API projesi oluşturun](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="b0b0c-290">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="b0b0c-291">Alternatif olarak, [örneği de indirebilirsiniz](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="b0b0c-291">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="b0b0c-292">Bu belgedeki yaklaşımlardan birini kullanarak CORS 'yi etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-292">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="b0b0c-293">Örnek:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-293">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="b0b0c-294">`WithOrigins("https://localhost:<port>");`, yalnızca [indirme örnek koduna](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)benzer bir örnek uygulamanın test edilmesi için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-294">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="b0b0c-295">Web uygulaması projesi (Razor Pages veya MVC) oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-295">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="b0b0c-296">Örnek Razor Pages kullanır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-296">The sample uses Razor Pages.</span></span> <span data-ttu-id="b0b0c-297">Web uygulamasını, API projesiyle aynı çözümde oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-297">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="b0b0c-298">Aşağıdaki Vurgulanan kodu *Index. cshtml* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-298">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="b0b0c-299">Yukarıdaki kodda `url: 'https://<web app>.azurewebsites.net/api/values/1',`, dağıtılan uygulamanın URL 'siyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-299">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="b0b0c-300">API projesini dağıtın.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-300">Deploy the API project.</span></span> <span data-ttu-id="b0b0c-301">Örneğin, [Azure 'a dağıtın](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="b0b0c-301">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="b0b0c-302">Razor Pages veya MVC uygulamasını masaüstünden çalıştırın ve **Test** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-302">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="b0b0c-303">Hata iletilerini gözden geçirmek için F12 araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-303">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="b0b0c-304">`WithOrigins` 'ten localhost kaynağını kaldırın ve uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-304">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="b0b0c-305">Alternatif olarak, istemci uygulamasını farklı bir bağlantı noktasıyla çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-305">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="b0b0c-306">Örneğin, Visual Studio 'dan çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-306">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="b0b0c-307">İstemci uygulamasıyla test edin.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-307">Test with the client app.</span></span> <span data-ttu-id="b0b0c-308">CORS hataları bir hata döndürüyor, ancak hata iletisi JavaScript için kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-308">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="b0b0c-309">Hatayı görmek için F12 araçlarındaki konsol sekmesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-309">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="b0b0c-310">Tarayıcıya bağlı olarak, aşağıdakine benzer bir hata alırsınız (F12 araçları konsolunda):</span><span class="sxs-lookup"><span data-stu-id="b0b0c-310">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="b0b0c-311">Microsoft Edge 'i kullanarak:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-311">Using Microsoft Edge:</span></span>

     <span data-ttu-id="b0b0c-312">**SEC7120: [CORS] kaynak `https://localhost:44375`, `https://webapi.azurewebsites.net/api/values/1` konumundaki çapraz kaynak kaynağı için Access-Control-Allow-Origin yanıt üstbilgisinde `https://localhost:44375` bulamadı**</span><span class="sxs-lookup"><span data-stu-id="b0b0c-312">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="b0b0c-313">Chrome kullanarak:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-313">Using Chrome:</span></span>

     <span data-ttu-id="b0b0c-314">**`https://localhost:44375` kaynaktan `https://webapi.azurewebsites.net/api/values/1` konumundaki XMLHttpRequest erişimi CORS ilkesi tarafından engellendi: İstenen kaynakta hiçbir ' erişim-denetim-Izin-Origin ' üst bilgisi yok.**</span><span class="sxs-lookup"><span data-stu-id="b0b0c-314">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="b0b0c-315">CORS özellikli uç noktalar [Fiddler](https://www.telerik.com/fiddler) veya [Postman](https://www.getpostman.com/)gibi bir araçla test edilebilir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-315">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="b0b0c-316">Bir araç kullanırken, `Origin` üstbilgisi tarafından belirtilen isteğin kaynağı isteği alan konaktan farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-316">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="b0b0c-317">İstek, `Origin` üst bilgisinin değerine göre *Çıkış dışı* değilse:</span><span class="sxs-lookup"><span data-stu-id="b0b0c-317">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="b0b0c-318">CORS ara yazılımı için isteği işleme gereksinimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-318">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="b0b0c-319">CORS üstbilgileri yanıtta döndürülmedi.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-319">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="b0b0c-320">IIS 'de CORS</span><span class="sxs-lookup"><span data-stu-id="b0b0c-320">CORS in IIS</span></span>

<span data-ttu-id="b0b0c-321">IIS 'ye dağıtım yaparken, sunucu anonim erişime izin verecek şekilde yapılandırılmamışsa CORS, Windows kimlik doğrulamasından önce çalıştırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-321">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="b0b0c-322">Bu senaryoyu desteklemek için, [IIS CORS modülünün](https://www.iis.net/downloads/microsoft/iis-cors-module) uygulama için yüklenmiş ve yapılandırılmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b0b0c-322">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0b0c-323">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b0b0c-323">Additional resources</span></span>

* [<span data-ttu-id="b0b0c-324">Çıkış noktaları arası kaynak paylaşımı (CORS)</span><span class="sxs-lookup"><span data-stu-id="b0b0c-324">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="b0b0c-325">IIS CORS modülünü kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="b0b0c-325">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)
