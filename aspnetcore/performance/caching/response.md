---
title: ASP.NET Core yanıt önbelleğe alma
author: rick-anderson
description: Önbelleğe alma daha düşük bant genişliği gereksinimlerine yanıt kullanmayı öğrenin ve ASP.NET Core uygulamaları performansını artırın.
ms.author: riande
ms.date: 01/07/2018
uid: performance/caching/response
ms.openlocfilehash: 5fbcaddff6e53d01a19ba8a7455c719feb614326
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098954"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="7738a-103">ASP.NET Core yanıt önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="7738a-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="7738a-104">Tarafından [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7738a-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="7738a-105">Razor sayfaları yanıt önbelleğe alma, ASP.NET Core 2.1 veya üstü.</span><span class="sxs-lookup"><span data-stu-id="7738a-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="7738a-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7738a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7738a-107">Yanıt önbelleğe alma, bir istemci ya da proxy web sunucusunda yapar istek sayısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="7738a-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="7738a-108">Yanıtları önbelleğe alma de azaltır çalışmanın bir yanıtı oluşturmak için web sunucusu gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="7738a-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="7738a-109">Yanıt önbelleğe alma, istemci proxy ve önbellek yanıtları Ara yazılımıyla nasıl istediğinizi belirtmek üstbilgileri tarafından denetlenir.</span><span class="sxs-lookup"><span data-stu-id="7738a-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="7738a-110">[ResponseCache özniteliği](#responsecache-attribute) yanıt üst bilgileri, hangi istemcilerin yanıtları önbelleğe alırken dikkate, önbelleğe alma ayarını katılır.</span><span class="sxs-lookup"><span data-stu-id="7738a-110">The [ResponseCache attribute](#responsecache-attribute) participates in setting response caching headers, which clients may honor when caching responses.</span></span> <span data-ttu-id="7738a-111">[Yanıtları önbelleğe alma ara yazılımı](xref:performance/caching/middleware) sunucusunda önbellek yanıtları için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7738a-111">[Response Caching Middleware](xref:performance/caching/middleware) can be used to cache responses on the server.</span></span> <span data-ttu-id="7738a-112">Ara yazılım kullanabilirsiniz `ResponseCache` özniteliği sunucu tarafı önbelleğe alma davranışını etkilemek için özellikleri.</span><span class="sxs-lookup"><span data-stu-id="7738a-112">The middleware can use `ResponseCache` attribute properties to influence server-side caching behavior.</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="7738a-113">HTTP tabanlı yanıt önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="7738a-113">HTTP-based response caching</span></span>

<span data-ttu-id="7738a-114">[HTTP 1.1 önbelleğe alma belirtimi](https://tools.ietf.org/html/rfc7234) Internet önbelleklerini nasıl hareket etmesi gerektiğini açıklar.</span><span class="sxs-lookup"><span data-stu-id="7738a-114">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="7738a-115">Önbelleğe alma işlemi için kullanılan birincil HTTP üst bilgisi [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), önbellek belirtmek için kullanılan *yönergeleri*.</span><span class="sxs-lookup"><span data-stu-id="7738a-115">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="7738a-116">Yönergeleri aşamalarından istemcilerden sunucularına isteklerde ve yanıtı sunucudan istemcilere geri aşamalarından yaptıkça önbelleğe alma davranışını denetleme.</span><span class="sxs-lookup"><span data-stu-id="7738a-116">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="7738a-117">İsteklerin ve yanıtların proxy sunucular taşıyın ve proxy sunucuları HTTP 1.1 önbelleğe alma belirtime uygun de gerekir.</span><span class="sxs-lookup"><span data-stu-id="7738a-117">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="7738a-118">Ortak `Cache-Control` yönergeleri, aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7738a-118">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="7738a-119">Yönergesi</span><span class="sxs-lookup"><span data-stu-id="7738a-119">Directive</span></span>                                                       | <span data-ttu-id="7738a-120">Eylem</span><span class="sxs-lookup"><span data-stu-id="7738a-120">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="7738a-121">public</span><span class="sxs-lookup"><span data-stu-id="7738a-121">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="7738a-122">Bir önbellek yanıt depolayabilir.</span><span class="sxs-lookup"><span data-stu-id="7738a-122">A cache may store the response.</span></span> |
| [<span data-ttu-id="7738a-123">private</span><span class="sxs-lookup"><span data-stu-id="7738a-123">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="7738a-124">Yanıt tarafından paylaşılan bir önbellek depolanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7738a-124">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="7738a-125">Özel bir önbellek depolayın ve yanıt yeniden kullanın.</span><span class="sxs-lookup"><span data-stu-id="7738a-125">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="7738a-126">Maksimum yaş</span><span class="sxs-lookup"><span data-stu-id="7738a-126">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="7738a-127">İstemci, yaş, belirtilen sayıda saniye büyük bir yanıtı kabul edilmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="7738a-127">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="7738a-128">Örnekler: `max-age=60` (60 saniye) `max-age=2592000` (1 ay)</span><span class="sxs-lookup"><span data-stu-id="7738a-128">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="7738a-129">önbellek yok</span><span class="sxs-lookup"><span data-stu-id="7738a-129">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="7738a-130">**İsteklerinde**: Bir önbellekte depolanmış bir yanıt isteği karşılamak için kullanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="7738a-130">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="7738a-131">Not: İçin istemci yanıt kaynak sunucuya yeniden oluşturur ve ara yazılım önbelleğinde depolanan yanıtta güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="7738a-131">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="7738a-132">**Yanıtlar üzerinde**: Kaynak sunucu doğrulaması olmadan bir sonraki istek için yanıt kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="7738a-132">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="7738a-133">No-store</span><span class="sxs-lookup"><span data-stu-id="7738a-133">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="7738a-134">**İsteklerinde**: Bir önbellek isteği depolamamayı gerekir.</span><span class="sxs-lookup"><span data-stu-id="7738a-134">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="7738a-135">**Yanıtlar üzerinde**: Bir önbellek yanıt herhangi bir bölümünü depolamamayı gerekir.</span><span class="sxs-lookup"><span data-stu-id="7738a-135">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="7738a-136">Önbelleğe alma bir rol oynar diğer önbellek üst bilgileri aşağıdaki tabloda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7738a-136">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="7738a-137">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="7738a-137">Header</span></span>                                                     | <span data-ttu-id="7738a-138">İşlev</span><span class="sxs-lookup"><span data-stu-id="7738a-138">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="7738a-139">Geçerlilik süresi</span><span class="sxs-lookup"><span data-stu-id="7738a-139">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="7738a-140">Yanıt beri geçen saniye sayısı süre miktarı tahmin oluşturulan veya kaynak sunucuda başarıyla doğrulandı.</span><span class="sxs-lookup"><span data-stu-id="7738a-140">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="7738a-141">Süre sonu</span><span class="sxs-lookup"><span data-stu-id="7738a-141">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="7738a-142">Sonra yanıt eski olarak değerlendirmeden tarih.</span><span class="sxs-lookup"><span data-stu-id="7738a-142">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="7738a-143">Pragması</span><span class="sxs-lookup"><span data-stu-id="7738a-143">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="7738a-144">HTTP/1.0 ile uyumluluk ayarı için geriye doğru önbellekleri için mevcut `no-cache` davranışı.</span><span class="sxs-lookup"><span data-stu-id="7738a-144">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="7738a-145">Varsa `Cache-Control` üstbilgisi mevcutsa, `Pragma` üstbilgi göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="7738a-145">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="7738a-146">değişiklik</span><span class="sxs-lookup"><span data-stu-id="7738a-146">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="7738a-147">Önbelleğe alınan yanıt sürece tüm gönderilmemesini gerekir belirtir, `Vary` önbelleğe alınan yanıtın özgün istek ve yeni istek üst bilgi alanları eşleştir.</span><span class="sxs-lookup"><span data-stu-id="7738a-147">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="7738a-148">HTTP tabanlı önbelleğe alma gizliliğinize Cache-Control yönergeleri istek</span><span class="sxs-lookup"><span data-stu-id="7738a-148">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="7738a-149">[Cache-Control üst bilgisi için HTTP 1.1 önbelleğe alma belirtimi](https://tools.ietf.org/html/rfc7234#section-5.2) geçerli kabul etmenin bir önbellek gerektirir `Cache-Control` istemci tarafından gönderilen başlığı.</span><span class="sxs-lookup"><span data-stu-id="7738a-149">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="7738a-150">Bir istemci isteği ile yapabilen bir `no-cache` üstbilgi değeri ve zorla her istek için yeni bir yanıt oluşturmak için sunucu.</span><span class="sxs-lookup"><span data-stu-id="7738a-150">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="7738a-151">Her zaman istemci uygularken `Cache-Control` HTTP önbelleğe alma amacı gerçekleştiriliyorsa, istek üst bilgilerini mantıklı.</span><span class="sxs-lookup"><span data-stu-id="7738a-151">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="7738a-152">Altında bir resmi belirtimi, önbelleğe alma, istemcileri ve proxy sunucuları, ağ üzerinden isteklerini karşılamak gecikme süresi ve ağ yükünü azaltmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="7738a-152">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="7738a-153">Mutlaka, kaynak sunucu üzerindeki yükü denetlemek için bir yol değil.</span><span class="sxs-lookup"><span data-stu-id="7738a-153">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="7738a-154">Kullanırken bu önbelleğe alma davranışı üzerinde Geliştirici denetimi olan [yanıt önbelleğe alma ara yazılımı](xref:performance/caching/middleware) çünkü resmi belirtimi önbelleğe alma ara yazılımı uyar.</span><span class="sxs-lookup"><span data-stu-id="7738a-154">There's no developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="7738a-155">[Ara yazılım geliştirmeler planlı](https://github.com/aspnet/AspNetCore/issues/2612) bir isteğin yoksaymak için bir ara yazılım yapılandırmak için bir fırsat `Cache-Control` önbelleğe alınan yanıt hizmet verirken başlığı.</span><span class="sxs-lookup"><span data-stu-id="7738a-155">[Planned enhancements to the middleware](https://github.com/aspnet/AspNetCore/issues/2612) are an opportunity to configure the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="7738a-156">Planlanan geliştirmeleri daha iyi denetim sunucu iş yükü için bir fırsat sağlar.</span><span class="sxs-lookup"><span data-stu-id="7738a-156">Planned enhancements provide an opportunity to better control server load.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="7738a-157">ASP.NET core'da diğer önbelleğe alma teknolojisini</span><span class="sxs-lookup"><span data-stu-id="7738a-157">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="7738a-158">Bellek içi önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="7738a-158">In-memory caching</span></span>

<span data-ttu-id="7738a-159">Bellek içi caching, önbelleğe alınmış verileri depolamak için sunucu belleği kullanır.</span><span class="sxs-lookup"><span data-stu-id="7738a-159">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="7738a-160">Bu tür bir önbelleğe alma, tek veya birden fazla sunucu kullanmak için uygundur *Yapışkan oturumlar*.</span><span class="sxs-lookup"><span data-stu-id="7738a-160">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="7738a-161">Yapışkan oturumlar, istemciden gelen isteklere her zaman aynı sunucuya işleme yönlendirildiğinden anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="7738a-161">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="7738a-162">Daha fazla bilgi için [bellek içi önbelleğe alma](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="7738a-162">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="7738a-163">Dağıtılmış önbellek</span><span class="sxs-lookup"><span data-stu-id="7738a-163">Distributed Cache</span></span>

<span data-ttu-id="7738a-164">Uygulamayı bir bulut veya sunucu grubunda barındırıldığında bellek verilerini depolamak için Dağıtılmış bir önbellek kullanın.</span><span class="sxs-lookup"><span data-stu-id="7738a-164">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="7738a-165">Önbelleği istekleri işleyen sunucular arasında paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="7738a-165">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="7738a-166">Bir istemci, istemci için önbelleğe alınan verileri kullanılabilir ise gruptaki hiçbir sunucu tarafından işlenen bir istek gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7738a-166">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="7738a-167">ASP.NET Core, SQL Server ve dağıtılmış Redis önbellekleri sunar.</span><span class="sxs-lookup"><span data-stu-id="7738a-167">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="7738a-168">Daha fazla bilgi için bkz. <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="7738a-168">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="7738a-169">Önbellek etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="7738a-169">Cache Tag Helper</span></span>

<span data-ttu-id="7738a-170">Önbellek etiketi Yardımcısı ile bir MVC görünümü ya da bir Razor sayfası içeriği önbelleğe alabilir.</span><span class="sxs-lookup"><span data-stu-id="7738a-170">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="7738a-171">Önbellek etiketi Yardımcısı, verileri depolamak için bellek içi önbelleğe alma kullanır.</span><span class="sxs-lookup"><span data-stu-id="7738a-171">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="7738a-172">Daha fazla bilgi için [önbellek etiketi Yardımcısı, ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="7738a-172">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="7738a-173">Dağıtılmış önbellek etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="7738a-173">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="7738a-174">Dağıtılmış önbellek etiketi Yardımcısı ile bir MVC görünümü ya da dağıtılmış bir bulut ya da web grubu senaryoları Razor sayfası içeriği önbelleğe alabilir.</span><span class="sxs-lookup"><span data-stu-id="7738a-174">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="7738a-175">Dağıtılmış önbellek etiketi Yardımcısı, verileri depolamak için SQL Server veya Redis kullanır.</span><span class="sxs-lookup"><span data-stu-id="7738a-175">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="7738a-176">Daha fazla bilgi için [dağıtılmış önbellek etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="7738a-176">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="7738a-177">ResponseCache özniteliği</span><span class="sxs-lookup"><span data-stu-id="7738a-177">ResponseCache attribute</span></span>

<span data-ttu-id="7738a-178">[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) yanıt önbelleğe alma uygun üstbilgileri ayarlamak için gerekli parametreleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="7738a-178">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="7738a-179">Kimlik doğrulamasından geçen istemcilerin bilgilerini içeren içerik için önbelleğe alma devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="7738a-179">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="7738a-180">Önbelleğe alma yalnızca bir kullanıcının kimlik veya bir kullanıcının oturum açtığı göre değişmeyen içerik için etkinleştirilmiş olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="7738a-180">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="7738a-181">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) saklı yanıt sorgu anahtarları ait belirtilen liste değerleriyle göre değişir.</span><span class="sxs-lookup"><span data-stu-id="7738a-181">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="7738a-182">Tek bir değeri `*` ara yazılım değişir, yanıtların tümü tarafından istek sorgu dizesi parametreleri sağlanır.</span><span class="sxs-lookup"><span data-stu-id="7738a-182">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="7738a-183">`VaryByQueryKeys` ASP.NET Core 1.1 veya sonraki bir sürümü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="7738a-183">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="7738a-184">[Yanıtları önbelleğe alma ara yazılımı](xref:performance/caching/middleware) ayarlamak için etkinleştirilmelidir `VaryByQueryKeys` özelliği; Aksi takdirde, çalışma zamanı özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7738a-184">[Response Caching Middleware](xref:performance/caching/middleware) must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="7738a-185">Karşılık gelen bir HTTP üst bilgisi için hiç `VaryByQueryKeys` özelliği.</span><span class="sxs-lookup"><span data-stu-id="7738a-185">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="7738a-186">Özelliği, yanıt önbelleğe alma ara yazılımı tarafından işlenen bir HTTP özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="7738a-186">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="7738a-187">Önbelleğe alınan yanıt verecek ara yazılımı için sorgu dizesi ve sorgu dizesi değerini, önceki bir istek eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="7738a-187">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="7738a-188">Örneğin, istekler ve sonuçları aşağıdaki tabloda gösterilen bir dizi göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="7738a-188">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="7738a-189">İstek</span><span class="sxs-lookup"><span data-stu-id="7738a-189">Request</span></span>                          | <span data-ttu-id="7738a-190">Sonuç</span><span class="sxs-lookup"><span data-stu-id="7738a-190">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="7738a-191">Sunucudan döndürülen</span><span class="sxs-lookup"><span data-stu-id="7738a-191">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="7738a-192">Ara yazılım döndürülen</span><span class="sxs-lookup"><span data-stu-id="7738a-192">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="7738a-193">Sunucudan döndürülen</span><span class="sxs-lookup"><span data-stu-id="7738a-193">Returned from server</span></span>     |

<span data-ttu-id="7738a-194">İlk istek sunucu tarafından döndürülen ve Ara yazılımında önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="7738a-194">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="7738a-195">Önceki istek sorgu dizesini eşleştiği için ikinci isteği ara yazılımı tarafından döndürülür.</span><span class="sxs-lookup"><span data-stu-id="7738a-195">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="7738a-196">Sorgu dizesi değerini, önceki bir istek eşleşmediği için üçüncü isteği ara yazılım önbellekte değil.</span><span class="sxs-lookup"><span data-stu-id="7738a-196">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="7738a-197">`ResponseCacheAttribute` Yapılandırmak ve oluşturmak için kullanılır (aracılığıyla `IFilterFactory`) bir [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="7738a-197">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="7738a-198">`ResponseCacheFilter` Uygun HTTP üst bilgilerini ve yanıt özellikleri güncelleştirme işlemlerini gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="7738a-198">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="7738a-199">Filtresi:</span><span class="sxs-lookup"><span data-stu-id="7738a-199">The filter:</span></span>

* <span data-ttu-id="7738a-200">Mevcut üst bilgileri için kaldırır `Vary`, `Cache-Control`, ve `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="7738a-200">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="7738a-201">Kullanıma uygun üstbilgileri kümesinde özelliklerine göre Yazar `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="7738a-201">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="7738a-202">HTTP özelliği, önbelleğe alma yanıt güncelleştirmeleri `VaryByQueryKeys` ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="7738a-202">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="7738a-203">değişiklik</span><span class="sxs-lookup"><span data-stu-id="7738a-203">Vary</span></span>

<span data-ttu-id="7738a-204">Bu üst bilginin ne zaman yalnızca yazılır `VaryByHeader` özelliği ayarlanmış.</span><span class="sxs-lookup"><span data-stu-id="7738a-204">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="7738a-205">Ayarlanmış `Vary` özelliğinin değeri.</span><span class="sxs-lookup"><span data-stu-id="7738a-205">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="7738a-206">Aşağıdaki örnek kullanımları `VaryByHeader` özelliği:</span><span class="sxs-lookup"><span data-stu-id="7738a-206">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="7738a-207">Yanıt Üstbilgileri tarayıcınızın ağ araçları ile görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7738a-207">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="7738a-208">Aşağıdaki resimde gösterilmektedir Edge üzerinde çıkışını F12 **ağ** sekmesinde `About2` eylem yöntemine yenilenir:</span><span class="sxs-lookup"><span data-stu-id="7738a-208">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![About2 eylem yöntemi çağrıldığında F12 çıkış Ağ sekmesinde kenar](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="7738a-210">NoStore ve Location.None</span><span class="sxs-lookup"><span data-stu-id="7738a-210">NoStore and Location.None</span></span>

<span data-ttu-id="7738a-211">`NoStore` diğer özelliklerin çoğu geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="7738a-211">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="7738a-212">Bu özelliği ayarlandığında `true`, `Cache-Control` üstbilgi ayarlandığında `no-store`.</span><span class="sxs-lookup"><span data-stu-id="7738a-212">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="7738a-213">Varsa `Location` ayarlanır `None`:</span><span class="sxs-lookup"><span data-stu-id="7738a-213">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="7738a-214">`Cache-Control` ayarlanır `no-store,no-cache`.</span><span class="sxs-lookup"><span data-stu-id="7738a-214">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="7738a-215">`Pragma` ayarlanır `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="7738a-215">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="7738a-216">Varsa `NoStore` olduğu `false` ve `Location` olduğu `None`, `Cache-Control` ve `Pragma` ayarlandığından `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="7738a-216">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="7738a-217">Genellikle ayarlanabilir `NoStore` için `true` hata sayfalarında.</span><span class="sxs-lookup"><span data-stu-id="7738a-217">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="7738a-218">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7738a-218">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="7738a-219">Bu, şu olur:</span><span class="sxs-lookup"><span data-stu-id="7738a-219">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="7738a-220">Konum ve süresi</span><span class="sxs-lookup"><span data-stu-id="7738a-220">Location and Duration</span></span>

<span data-ttu-id="7738a-221">Önbelleğe alma, etkinleştirme `Duration` pozitif bir değere ayarlanması gerekir ve `Location` olmalıdır `Any` (varsayılan) veya `Client`.</span><span class="sxs-lookup"><span data-stu-id="7738a-221">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="7738a-222">Bu durumda, `Cache-Control` üstbilgi arkasından konum değeri ayarlandığında `max-age` yanıtın.</span><span class="sxs-lookup"><span data-stu-id="7738a-222">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="7738a-223">`Location`ın seçenekleri `Any` ve `Client` küçültmesini `Cache-Control` üstbilgi değerlerini `public` ve `private`sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="7738a-223">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="7738a-224">Daha önce belirtildiği gibi ayarı `Location` için `None` hem de ayarlar `Cache-Control` ve `Pragma` üstbilgileri `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="7738a-224">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="7738a-225">Ayarlayarak üstbilgileri gösteren bir örnek aşağıda üretilir `Duration` varsayılan bırakarak `Location` değeri:</span><span class="sxs-lookup"><span data-stu-id="7738a-225">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="7738a-226">Bu, aşağıdaki üst bilgi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="7738a-226">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="7738a-227">Önbellek profilleri</span><span class="sxs-lookup"><span data-stu-id="7738a-227">Cache profiles</span></span>

<span data-ttu-id="7738a-228">Çoğaltma yerine `ResponseCache` birçok denetleyici eylem özniteliklerinin, önbellek profilleri ayarları MVC'de ayarlarken seçeneği olarak yapılandırılabilir `ConfigureServices` yönteminde `Startup`.</span><span class="sxs-lookup"><span data-stu-id="7738a-228">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="7738a-229">Başvurulan önbellek profilinde bulunan değerleri tarafından varsayılan olarak kullanılır `ResponseCache` öznitelik ve öznitelik üzerinde belirtilen herhangi bir özelliği tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="7738a-229">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="7738a-230">Önbellek profili ayarlama:</span><span class="sxs-lookup"><span data-stu-id="7738a-230">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="7738a-231">Önbellek profili başvuruyor:</span><span class="sxs-lookup"><span data-stu-id="7738a-231">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="7738a-232">`ResponseCache` Özniteliği, hem Eylemler (yöntem) ve denetleyicilerine (sınıflar) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="7738a-232">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="7738a-233">Yöntem düzeyindeki öznitelikler, sınıf düzeyi özniteliklerinde belirtilen ayarları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="7738a-233">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="7738a-234">60 saniye olarak belirlendi süre önbellek profili yöntemi düzeyinde bir öznitelik başvuruyor ancak yukarıdaki örnekte, 30 saniye süre sınıf düzeyinde bir öznitelik belirtir.</span><span class="sxs-lookup"><span data-stu-id="7738a-234">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="7738a-235">Sonuçta elde edilen üst bilgi:</span><span class="sxs-lookup"><span data-stu-id="7738a-235">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="7738a-236">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7738a-236">Additional resources</span></span>

* [<span data-ttu-id="7738a-237">Yanıtları depolama önbelleklerinde</span><span class="sxs-lookup"><span data-stu-id="7738a-237">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="7738a-238">Önbellek denetimi</span><span class="sxs-lookup"><span data-stu-id="7738a-238">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
