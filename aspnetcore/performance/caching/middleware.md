---
title: ASP.NET Core 'de yanıt önbelleğe alma ara yazılımı
author: guardrex
description: ASP.NET Core 'de yanıt önbelleğe alma ara yazılımını yapılandırmayı ve kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/08/2019
uid: performance/caching/middleware
ms.openlocfilehash: 6371f42b100f70c6042064a6372c7b9e41fd5c73
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914989"
---
# <a name="response-caching-middleware-in-aspnet-core"></a><span data-ttu-id="8915c-103">ASP.NET Core 'de yanıt önbelleğe alma ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="8915c-103">Response Caching Middleware in ASP.NET Core</span></span>

<span data-ttu-id="8915c-104">[Luke Latham](https://github.com/guardrex) ve [John Luo](https://github.com/JunTaoLuo) tarafından</span><span class="sxs-lookup"><span data-stu-id="8915c-104">By [Luke Latham](https://github.com/guardrex) and [John Luo](https://github.com/JunTaoLuo)</span></span>

<span data-ttu-id="8915c-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8915c-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8915c-106">Bu makalede, yanıt önbelleğe alma ara yazılımı ASP.NET Core bir uygulamada nasıl yapılandırılacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="8915c-106">This article explains how to configure Response Caching Middleware in an ASP.NET Core app.</span></span> <span data-ttu-id="8915c-107">Ara yazılım, yanıtların önbelleklenebilir olup olmadığını belirler, yanıtları depolar ve önbellekten yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="8915c-107">The middleware determines when responses are cacheable, stores responses, and serves responses from cache.</span></span> <span data-ttu-id="8915c-108">HTTP önbelleğine giriş ve [[Responsecache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) özniteliği için bkz. [Yanıt önbelleği](xref:performance/caching/response).</span><span class="sxs-lookup"><span data-stu-id="8915c-108">For an introduction to HTTP caching and the [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute, see [Response Caching](xref:performance/caching/response).</span></span>

## <a name="configuration"></a><span data-ttu-id="8915c-109">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8915c-109">Configuration</span></span>

<span data-ttu-id="8915c-110">Microsoft. [aspnetcore. app metapackage](xref:fundamentals/metapackage-app) kullanın veya [Microsoft. Aspnetcore. responsecaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8915c-110">Use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package.</span></span>

<span data-ttu-id="8915c-111">' `Startup.ConfigureServices`De, yanıt önbelleğe alma ara yazılımını hizmet koleksiyonuna ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8915c-111">In `Startup.ConfigureServices`, add the Response Caching Middleware to the service collection:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

::: moniker-end

<span data-ttu-id="8915c-112">Uygulamayı, içindeki `Startup.Configure`istek işleme işlem hattına bir ara <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> yazılım ekleyen uzantı yöntemiyle ara yazılımı kullanacak şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="8915c-112">Configure the app to use the middleware with the <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> extension method, which adds the middleware to the request processing pipeline in `Startup.Configure`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=16)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

::: moniker-end

<span data-ttu-id="8915c-113">Örnek uygulama, sonraki isteklerde önbelleğe almayı denetlemek için üstbilgiler ekler:</span><span class="sxs-lookup"><span data-stu-id="8915c-113">The sample app adds headers to control caching on subsequent requests:</span></span>

* <span data-ttu-id="8915c-114">[Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; 10 saniyeye kadar önbelleklenebilir yanıtları önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="8915c-114">[Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; Caches cacheable responses for up to 10 seconds.</span></span>
* <span data-ttu-id="8915c-115">[Farklılık gösterir](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; Yalnızca [sonrakiisteklerinüstbilgisiözgünistektenbiriyleeşleşiyorsaönbelleğealınmışbiryanıta`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) yönelik ara yazılımı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="8915c-115">[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; Configures the middleware to serve a cached response only if the [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) header of subsequent requests matches that of the original request.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](middleware/samples_snippets/3.x/AddHeaders.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

::: moniker-end

<span data-ttu-id="8915c-116">Yanıt önbelleğe alma ara yazılımı yalnızca 200 (Tamam) durum kodu ile sonuçlanan sunucu yanıtlarını önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="8915c-116">Response Caching Middleware only caches server responses that result in a 200 (OK) status code.</span></span> <span data-ttu-id="8915c-117">[Hata sayfaları](xref:fundamentals/error-handling)dahil diğer tüm yanıtlar, ara yazılım tarafından yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="8915c-117">Any other responses, including [error pages](xref:fundamentals/error-handling), are ignored by the middleware.</span></span>

> [!WARNING]
> <span data-ttu-id="8915c-118">Kimliği doğrulanmış istemciler için içerik içeren yanıtların, ara yazılımın bu yanıtları depolamasını ve hizmet vermek için önbelleğe alınamaz olarak işaretlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8915c-118">Responses containing content for authenticated clients must be marked as not cacheable to prevent the middleware from storing and serving those responses.</span></span> <span data-ttu-id="8915c-119">Bir yanıtın önbelleklenmesini nasıl belirlediği hakkında bilgi için bkz. [önbelleğe alma koşulları](#conditions-for-caching) .</span><span class="sxs-lookup"><span data-stu-id="8915c-119">See [Conditions for caching](#conditions-for-caching) for details on how the middleware determines if a response is cacheable.</span></span>

## <a name="options"></a><span data-ttu-id="8915c-120">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="8915c-120">Options</span></span>

<span data-ttu-id="8915c-121">Yanıt önbelleğe alma seçenekleri aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8915c-121">Response caching options are shown in the following table.</span></span>

| <span data-ttu-id="8915c-122">Seçenek</span><span class="sxs-lookup"><span data-stu-id="8915c-122">Option</span></span> | <span data-ttu-id="8915c-123">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8915c-123">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | <span data-ttu-id="8915c-124">Yanıt gövdesi için bayt cinsinden en büyük önbelleklenebilir boyut.</span><span class="sxs-lookup"><span data-stu-id="8915c-124">The largest cacheable size for the response body in bytes.</span></span> <span data-ttu-id="8915c-125">Varsayılan değer `64 * 1024 * 1024` (64 MB).</span><span class="sxs-lookup"><span data-stu-id="8915c-125">The default value is `64 * 1024 * 1024` (64 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | <span data-ttu-id="8915c-126">Yanıt önbelleği ara yazılımı için bayt cinsinden boyut sınırı.</span><span class="sxs-lookup"><span data-stu-id="8915c-126">The size limit for the response cache middleware in bytes.</span></span> <span data-ttu-id="8915c-127">Varsayılan değer `100 * 1024 * 1024` (100 MB).</span><span class="sxs-lookup"><span data-stu-id="8915c-127">The default value is `100 * 1024 * 1024` (100 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | <span data-ttu-id="8915c-128">Yanıtların büyük/küçük harfe duyarlı yollarda önbelleğe alınıp alınmayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="8915c-128">Determines if responses are cached on case-sensitive paths.</span></span> <span data-ttu-id="8915c-129">Varsayılan değer `false` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="8915c-129">The default value is `false`.</span></span> |

<span data-ttu-id="8915c-130">Aşağıdaki örnek, şu şekilde bir ara yazılım yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="8915c-130">The following example configures the middleware to:</span></span>

* <span data-ttu-id="8915c-131">Gövde boyutu 1.024 bayttan küçük veya buna eşit olan önbellek yanıtları.</span><span class="sxs-lookup"><span data-stu-id="8915c-131">Cache responses with a body size smaller than or equal to 1,024 bytes.</span></span>
* <span data-ttu-id="8915c-132">Yanıtları büyük/küçük harfe duyarlı yollarla depolayın.</span><span class="sxs-lookup"><span data-stu-id="8915c-132">Store the responses by case-sensitive paths.</span></span> <span data-ttu-id="8915c-133">Örneğin, `/page1` ve `/Page1` ayrı olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="8915c-133">For example, `/page1` and `/Page1` are stored separately.</span></span>

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a><span data-ttu-id="8915c-134">VaryByQueryKeys</span><span class="sxs-lookup"><span data-stu-id="8915c-134">VaryByQueryKeys</span></span>

<span data-ttu-id="8915c-135">MVC/web API denetleyicilerini veya Razor Pages sayfa modellerini kullanırken, [[Responsecache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) özniteliği, yanıt önbelleğe alma için uygun üst bilgileri ayarlamak için gereken parametreleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="8915c-135">When using MVC / web API controllers or Razor Pages page models, the [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute specifies the parameters necessary for setting the appropriate headers for response caching.</span></span> <span data-ttu-id="8915c-136">`[ResponseCache]` Yalnızca<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>ara yazılım gerektiren özniteliğin tek parametresi, gerçek bir http üst bilgisine karşılık gelmiyor.</span><span class="sxs-lookup"><span data-stu-id="8915c-136">The only parameter of the `[ResponseCache]` attribute that strictly requires the middleware is <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, which doesn't correspond to an actual HTTP header.</span></span> <span data-ttu-id="8915c-137">Daha fazla bilgi için bkz. <xref:performance/caching/response#responsecache-attribute>.</span><span class="sxs-lookup"><span data-stu-id="8915c-137">For more information, see <xref:performance/caching/response#responsecache-attribute>.</span></span>

<span data-ttu-id="8915c-138">`[ResponseCache]` Özniteliği kullanmıyorsanız, yanıt önbelleğe alma ile `VaryByQueryKeys`değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="8915c-138">When not using the `[ResponseCache]` attribute, response caching can be varied with `VaryByQueryKeys`.</span></span> <span data-ttu-id="8915c-139">Doğrudan HttpContext. Features içinden kullanın: [](xref:Microsoft.AspNetCore.Http.HttpContext.Features) <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature></span><span class="sxs-lookup"><span data-stu-id="8915c-139">Use the <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> directly from the [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):</span></span>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

<span data-ttu-id="8915c-140">Tek bir değerin `*` ' de `VaryByQueryKeys` değerine eşit olması, önbelleğin tüm istek sorgu parametrelerine göre değişiklik gösterir.</span><span class="sxs-lookup"><span data-stu-id="8915c-140">Using a single value equal to `*` in `VaryByQueryKeys` varies the cache by all request query parameters.</span></span>

## <a name="http-headers-used-by-response-caching-middleware"></a><span data-ttu-id="8915c-141">Yanıt önbelleğe alma ara yazılımı tarafından kullanılan HTTP üstbilgileri</span><span class="sxs-lookup"><span data-stu-id="8915c-141">HTTP headers used by Response Caching Middleware</span></span>

<span data-ttu-id="8915c-142">Aşağıdaki tabloda, yanıt önbelleğini etkileyen HTTP üstbilgileri hakkında bilgi verilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8915c-142">The following table provides information on HTTP headers that affect response caching.</span></span>

| <span data-ttu-id="8915c-143">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="8915c-143">Header</span></span> | <span data-ttu-id="8915c-144">Ayrıntılar</span><span class="sxs-lookup"><span data-stu-id="8915c-144">Details</span></span> |
| ------ | ------- |
| `Authorization` | <span data-ttu-id="8915c-145">Üst bilgi varsa yanıt önbelleğe alınmaz.</span><span class="sxs-lookup"><span data-stu-id="8915c-145">The response isn't cached if the header exists.</span></span> |
| `Cache-Control` | <span data-ttu-id="8915c-146">Ara yazılım yalnızca `public` önbellek yönergesi ile işaretlenmiş önbelleğe alma yanıtlarını dikkate alır.</span><span class="sxs-lookup"><span data-stu-id="8915c-146">The middleware only considers caching responses marked with the `public` cache directive.</span></span> <span data-ttu-id="8915c-147">Aşağıdaki parametrelerle önbelleğe alma denetimi:</span><span class="sxs-lookup"><span data-stu-id="8915c-147">Control caching with the following parameters:</span></span><ul><li><span data-ttu-id="8915c-148">Maksimum yaş</span><span class="sxs-lookup"><span data-stu-id="8915c-148">max-age</span></span></li><li><span data-ttu-id="8915c-149">en fazla-eski&#8224;</span><span class="sxs-lookup"><span data-stu-id="8915c-149">max-stale&#8224;</span></span></li><li><span data-ttu-id="8915c-150">en az-yeni</span><span class="sxs-lookup"><span data-stu-id="8915c-150">min-fresh</span></span></li><li><span data-ttu-id="8915c-151">yeniden doğrulama gerekir</span><span class="sxs-lookup"><span data-stu-id="8915c-151">must-revalidate</span></span></li><li><span data-ttu-id="8915c-152">önbellek yok</span><span class="sxs-lookup"><span data-stu-id="8915c-152">no-cache</span></span></li><li><span data-ttu-id="8915c-153">mağaza yok</span><span class="sxs-lookup"><span data-stu-id="8915c-153">no-store</span></span></li><li><span data-ttu-id="8915c-154">yalnızca-if-önbelleğe alındı</span><span class="sxs-lookup"><span data-stu-id="8915c-154">only-if-cached</span></span></li><li><span data-ttu-id="8915c-155">özel</span><span class="sxs-lookup"><span data-stu-id="8915c-155">private</span></span></li><li><span data-ttu-id="8915c-156">public</span><span class="sxs-lookup"><span data-stu-id="8915c-156">public</span></span></li><li><span data-ttu-id="8915c-157">s-maxage</span><span class="sxs-lookup"><span data-stu-id="8915c-157">s-maxage</span></span></li><li><span data-ttu-id="8915c-158">ara sunucu-yeniden doğrulama&#8225;</span><span class="sxs-lookup"><span data-stu-id="8915c-158">proxy-revalidate&#8225;</span></span></li></ul><span data-ttu-id="8915c-159">&#8224;İçin `max-stale`hiçbir sınır belirtilmemişse, ara yazılım hiçbir eylemde bulunmaz.</span><span class="sxs-lookup"><span data-stu-id="8915c-159">&#8224;If no limit is specified to `max-stale`, the middleware takes no action.</span></span><br><span data-ttu-id="8915c-160">&#8225;`proxy-revalidate`, ile `must-revalidate`aynı etkiye sahiptir.</span><span class="sxs-lookup"><span data-stu-id="8915c-160">&#8225;`proxy-revalidate` has the same effect as `must-revalidate`.</span></span><br><br><span data-ttu-id="8915c-161">Daha fazla bilgi için bkz [. RFC 7231: Cache-Control yönergeleri](https://tools.ietf.org/html/rfc7234#section-5.2.1)iste.</span><span class="sxs-lookup"><span data-stu-id="8915c-161">For more information, see [RFC 7231: Request Cache-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span></span> |
| `Pragma` | <span data-ttu-id="8915c-162">İstekteki `Pragma: no-cache` bir üst bilgi, ile `Cache-Control: no-cache`aynı etkiyi üretir.</span><span class="sxs-lookup"><span data-stu-id="8915c-162">A `Pragma: no-cache` header in the request produces the same effect as `Cache-Control: no-cache`.</span></span> <span data-ttu-id="8915c-163">Bu üst bilgi, varsa `Cache-Control` başlıktaki ilgili yönergeler tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="8915c-163">This header is overridden by the relevant directives in the `Cache-Control` header, if present.</span></span> <span data-ttu-id="8915c-164">HTTP/1.0 ile geriye dönük uyumluluk için değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="8915c-164">Considered for backward compatibility with HTTP/1.0.</span></span> |
| `Set-Cookie` | <span data-ttu-id="8915c-165">Üst bilgi varsa yanıt önbelleğe alınmaz.</span><span class="sxs-lookup"><span data-stu-id="8915c-165">The response isn't cached if the header exists.</span></span> <span data-ttu-id="8915c-166">İstek işleme ardışık düzeninde bir veya daha fazla tanımlama bilgisi ayarlayan herhangi bir ara yazılım, yanıt önbelleğe alma ara hattının yanıtı önbelleğe almasını önler (örneğin, [tanımlama bilgisi tabanlı TempData sağlayıcısı](xref:fundamentals/app-state#tempdata)).</span><span class="sxs-lookup"><span data-stu-id="8915c-166">Any middleware in the request processing pipeline that sets one or more cookies prevents the Response Caching Middleware from caching the response (for example, the [cookie-based TempData provider](xref:fundamentals/app-state#tempdata)).</span></span>  |
| `Vary` | <span data-ttu-id="8915c-167">`Vary` Üst bilgi, başka bir üst bilgi tarafından önbelleğe alınan yanıtı değiştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8915c-167">The `Vary` header is used to vary the cached response by another header.</span></span> <span data-ttu-id="8915c-168">Örneğin, üst bilgi `Vary: Accept-Encoding` `Accept-Encoding: gzip` ve `Accept-Encoding: text/plain` ayrı ayrı istekler için yanıtları önbelleğe alan üstbilgiyi ekleyerek kodlamaya göre yanıtları önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="8915c-168">For example, cache responses by encoding by including the `Vary: Accept-Encoding` header, which caches responses for requests with headers `Accept-Encoding: gzip` and `Accept-Encoding: text/plain` separately.</span></span> <span data-ttu-id="8915c-169">Üstbilgi değeri `*` olan bir yanıt hiçbir şekilde depolanmaz.</span><span class="sxs-lookup"><span data-stu-id="8915c-169">A response with a header value of `*` is never stored.</span></span> |
| `Expires` | <span data-ttu-id="8915c-170">Bu üstbilginin eski olduğu bir yanıt, diğer `Cache-Control` üstbilgiler tarafından geçersiz kılınmadıkça depolanmaz veya alınamaz.</span><span class="sxs-lookup"><span data-stu-id="8915c-170">A response deemed stale by this header isn't stored or retrieved unless overridden by other `Cache-Control` headers.</span></span> |
| `If-None-Match` | <span data-ttu-id="8915c-171">Tam yanıt, değer değilse `*` önbellekten, `ETag` yanıtın ise belirtilen değerlerden hiçbiriyle eşleşmez.</span><span class="sxs-lookup"><span data-stu-id="8915c-171">The full response is served from cache if the value isn't `*` and the `ETag` of the response doesn't match any of the values provided.</span></span> <span data-ttu-id="8915c-172">Aksi takdirde, 304 (değiştirilmez) yanıtı sunulur.</span><span class="sxs-lookup"><span data-stu-id="8915c-172">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| `If-Modified-Since` | <span data-ttu-id="8915c-173">`If-None-Match` Üst bilgi yoksa, önbelleğe alınmış yanıt tarihi verilen değerden daha yeniyse önbellekten tam bir yanıt sunulur.</span><span class="sxs-lookup"><span data-stu-id="8915c-173">If the `If-None-Match` header isn't present, a full response is served from cache if the cached response date is newer than the value provided.</span></span> <span data-ttu-id="8915c-174">Aksi takdirde, *304 olarak değiştirilmemiş* bir yanıt sunulur.</span><span class="sxs-lookup"><span data-stu-id="8915c-174">Otherwise, a *304 - Not Modified* response is served.</span></span> |
| `Date` | <span data-ttu-id="8915c-175">Önbellekten hizmet verirken, `Date` özgün yanıtta sağlanmadıysa üst bilgi ara yazılım tarafından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8915c-175">When serving from cache, the `Date` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Content-Length` | <span data-ttu-id="8915c-176">Önbellekten hizmet verirken, `Content-Length` özgün yanıtta sağlanmadıysa üst bilgi ara yazılım tarafından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8915c-176">When serving from cache, the `Content-Length` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Age` | <span data-ttu-id="8915c-177">Özgün yanıtta gönderilen üstbilgi yok sayılır. `Age`</span><span class="sxs-lookup"><span data-stu-id="8915c-177">The `Age` header sent in the original response is ignored.</span></span> <span data-ttu-id="8915c-178">Ara yazılım, önbelleğe alınmış bir yanıta hizmet verirken yeni bir değeri hesaplar.</span><span class="sxs-lookup"><span data-stu-id="8915c-178">The middleware computes a new value when serving a cached response.</span></span> |

## <a name="caching-respects-request-cache-control-directives"></a><span data-ttu-id="8915c-179">İstekleri önbelleğe alma isteği Cache-Control yönergeleri</span><span class="sxs-lookup"><span data-stu-id="8915c-179">Caching respects request Cache-Control directives</span></span>

<span data-ttu-id="8915c-180">Ara yazılım, [HTTP 1,1 önbelleğe alma belirtiminin](https://tools.ietf.org/html/rfc7234#section-5.2)kurallarına uyar.</span><span class="sxs-lookup"><span data-stu-id="8915c-180">The middleware respects the rules of the [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234#section-5.2).</span></span> <span data-ttu-id="8915c-181">Kurallar, istemci tarafından gönderilen geçerli `Cache-Control` bir üst bilgiyi kabul etmek için bir önbellek gerektirir.</span><span class="sxs-lookup"><span data-stu-id="8915c-181">The rules require a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="8915c-182">Belirtim altında, bir istemci bir `no-cache` üst bilgi değeri ile istek yapabilir ve sunucuyu her istek için yeni bir yanıt oluşturmaya zorlayabilir.</span><span class="sxs-lookup"><span data-stu-id="8915c-182">Under the specification, a client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span> <span data-ttu-id="8915c-183">Şu anda, ara yazılım resmi önbelleğe alma belirtimine bağlı olduğundan, ara yazılım kullanılırken bu önbelleğe alma davranışı üzerinde geliştirici denetimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="8915c-183">Currently, there's no developer control over this caching behavior when using the middleware because the middleware adheres to the official caching specification.</span></span>

<span data-ttu-id="8915c-184">Önbelleğe alma davranışı üzerinde daha fazla denetim için ASP.NET Core diğer önbelleğe alma özelliklerine göz atın.</span><span class="sxs-lookup"><span data-stu-id="8915c-184">For more control over caching behavior, explore other caching features of ASP.NET Core.</span></span> <span data-ttu-id="8915c-185">Aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="8915c-185">See the following topics:</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a><span data-ttu-id="8915c-186">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="8915c-186">Troubleshooting</span></span>

<span data-ttu-id="8915c-187">Önbelleğe alma davranışı beklenmiyorsa, yanıtların önbelleklenmesini ve önbellekten sunulduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="8915c-187">If caching behavior isn't as expected, confirm that responses are cacheable and capable of being served from the cache.</span></span> <span data-ttu-id="8915c-188">İsteğin gelen üst bilgilerini ve yanıtın giden üst bilgilerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="8915c-188">Examine the request's incoming headers and the response's outgoing headers.</span></span> <span data-ttu-id="8915c-189">Hata ayıklamaya yardımcı olmak için [günlük kaydını](xref:fundamentals/logging/index) etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="8915c-189">Enable [logging](xref:fundamentals/logging/index) to help with debugging.</span></span>

<span data-ttu-id="8915c-190">Önbelleğe alma davranışını test ederken ve sorun giderirken, bir tarayıcı, istenmeyen yollarla önbelleğe almayı etkileyen istek üst bilgilerini ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="8915c-190">When testing and troubleshooting caching behavior, a browser may set request headers that affect caching in undesirable ways.</span></span> <span data-ttu-id="8915c-191">Örneğin, bir tarayıcı `Cache-Control` üstbilgiyi bir sayfa yenileyene veya `max-age=0` olarak `no-cache` ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="8915c-191">For example, a browser may set the `Cache-Control` header to `no-cache` or `max-age=0` when refreshing a page.</span></span> <span data-ttu-id="8915c-192">Aşağıdaki araçlar, istek üst bilgilerini açık bir şekilde ayarlayabilir ve önbelleğe alma testi için tercih edilir:</span><span class="sxs-lookup"><span data-stu-id="8915c-192">The following tools can explicitly set request headers and are preferred for testing caching:</span></span>

* [<span data-ttu-id="8915c-193">Fiddler</span><span class="sxs-lookup"><span data-stu-id="8915c-193">Fiddler</span></span>](https://www.telerik.com/fiddler)
* [<span data-ttu-id="8915c-194">Postman</span><span class="sxs-lookup"><span data-stu-id="8915c-194">Postman</span></span>](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a><span data-ttu-id="8915c-195">Önbelleğe alma koşulları</span><span class="sxs-lookup"><span data-stu-id="8915c-195">Conditions for caching</span></span>

* <span data-ttu-id="8915c-196">İstek, 200 (Tamam) durum kodu ile bir sunucu yanıtı ile sonuçlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8915c-196">The request must result in a server response with a 200 (OK) status code.</span></span>
* <span data-ttu-id="8915c-197">İstek yöntemi GET veya HEAD olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8915c-197">The request method must be GET or HEAD.</span></span>
* <span data-ttu-id="8915c-198">' `Startup.Configure`De, yanıt önbelleğe alma ara yazılımı, önbelleğe alma gerektiren ara yazılımlar için yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="8915c-198">In `Startup.Configure`, Response Caching Middleware must be placed before middleware that require caching.</span></span> <span data-ttu-id="8915c-199">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="8915c-199">For more information, see <xref:fundamentals/middleware/index>.</span></span>
* <span data-ttu-id="8915c-200">`Authorization` Üst bilgi mevcut olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="8915c-200">The `Authorization` header must not be present.</span></span>
* <span data-ttu-id="8915c-201">`Cache-Control`üst bilgi parametreleri geçerli olmalıdır ve yanıtın işaretlenmiş `public` ve işaretlenmemiş `private`olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8915c-201">`Cache-Control` header parameters must be valid, and the response must be marked `public` and not marked `private`.</span></span>
* <span data-ttu-id="8915c-202">Üst bilgi mevcut olmadığında üst bilgi mevcut olmamalıdır, `Cache-Control` üstbilgi mevcut olduğunda üstbilgiyi geçersiz kılar `Pragma`. `Pragma: no-cache` `Cache-Control`</span><span class="sxs-lookup"><span data-stu-id="8915c-202">The `Pragma: no-cache` header must not be present if the `Cache-Control` header isn't present, as the `Cache-Control` header overrides the `Pragma` header when present.</span></span>
* <span data-ttu-id="8915c-203">`Set-Cookie` Üst bilgi mevcut olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="8915c-203">The `Set-Cookie` header must not be present.</span></span>
* <span data-ttu-id="8915c-204">`Vary`üst bilgi parametreleri geçerli ve eşit `*`olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8915c-204">`Vary` header parameters must be valid and not equal to `*`.</span></span>
* <span data-ttu-id="8915c-205">`Content-Length` Üst bilgi değeri (ayarlandıysa), yanıt gövdesinin boyutuyla aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8915c-205">The `Content-Length` header value (if set) must match the size of the response body.</span></span>
* <span data-ttu-id="8915c-206"><xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> Kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="8915c-206">The <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> isn't used.</span></span>
* <span data-ttu-id="8915c-207">Yanıtın `Expires` üst bilgi `max-age` ve ve `s-maxage` önbellek yönergeleri tarafından belirtilen eski olmaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8915c-207">The response must not be stale as specified by the `Expires` header and the `max-age` and `s-maxage` cache directives.</span></span>
* <span data-ttu-id="8915c-208">Yanıt arabelleğe alma başarılı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8915c-208">Response buffering must be successful.</span></span> <span data-ttu-id="8915c-209">Yanıtın boyutu yapılandırılan veya varsayılan <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>değerden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8915c-209">The size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>.</span></span> <span data-ttu-id="8915c-210">Yanıtın gövde boyutu yapılandırılan veya varsayılan <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>değerden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8915c-210">The body size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.</span></span>
* <span data-ttu-id="8915c-211">Yanıt, [RFC 7234](https://tools.ietf.org/html/rfc7234) belirtimlerine göre önbelleklenebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8915c-211">The response must be cacheable according to the [RFC 7234](https://tools.ietf.org/html/rfc7234) specifications.</span></span> <span data-ttu-id="8915c-212">Örneğin, `no-store` yönerge istek veya yanıt üst bilgisi alanlarında mevcut olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="8915c-212">For example, the `no-store` directive must not exist in request or response header fields.</span></span> <span data-ttu-id="8915c-213">Bkz *. Bölüm 3: Ayrıntılar için* [RFC 7234](https://tools.ietf.org/html/rfc7234) önbelleklerinde yanıtları depolama.</span><span class="sxs-lookup"><span data-stu-id="8915c-213">See *Section 3: Storing Responses in Caches* of [RFC 7234](https://tools.ietf.org/html/rfc7234) for details.</span></span>

> [!NOTE]
> <span data-ttu-id="8915c-214">Siteler arası istek sahteciliği (CSRF) saldırılarını önlemeye yönelik güvenli belirteçler oluşturmaya yönelik antiforgery sistemi, `Cache-Control` yanıtların önbelleğe alınmaması için ve `Pragma` üst `no-cache` bilgilerini olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8915c-214">The Antiforgery system for generating secure tokens to prevent Cross-Site Request Forgery (CSRF) attacks sets the `Cache-Control` and `Pragma` headers to `no-cache` so that responses aren't cached.</span></span> <span data-ttu-id="8915c-215">HTML form öğeleri için antiforgery belirteçlerini devre dışı bırakma hakkında daha fazla bilgi için <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>bkz.</span><span class="sxs-lookup"><span data-stu-id="8915c-215">For information on how to disable antiforgery tokens for HTML form elements, see <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8915c-216">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8915c-216">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
