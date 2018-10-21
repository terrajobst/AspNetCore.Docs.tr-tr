---
title: ASP.NET Core yanıt önbelleğe alma
author: rick-anderson
description: Önbelleğe alma daha düşük bant genişliği gereksinimlerine yanıt kullanmayı öğrenin ve ASP.NET Core uygulamaları performansını artırın.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: 4bf61502738d70760679ec98c8f2f303eca9d504
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477495"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="f11cb-103">ASP.NET Core yanıt önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="f11cb-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="f11cb-104">Tarafından [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f11cb-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="f11cb-105">Razor sayfaları yanıt önbelleğe alma, ASP.NET Core 2.1 veya üstü.</span><span class="sxs-lookup"><span data-stu-id="f11cb-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="f11cb-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f11cb-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f11cb-107">Yanıt önbelleğe alma, bir istemci ya da proxy web sunucusunda yapar istek sayısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="f11cb-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="f11cb-108">Yanıtları önbelleğe alma de azaltır çalışmanın bir yanıtı oluşturmak için web sunucusu gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="f11cb-109">Yanıt önbelleğe alma, istemci proxy ve önbellek yanıtları Ara yazılımıyla nasıl istediğinizi belirtmek üstbilgileri tarafından denetlenir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="f11cb-110">Web sunucusu eklediğinizde, yanıtları önbelleğe alabilir [yanıt önbelleğe alma ara yazılımı](xref:performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="f11cb-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="f11cb-111">HTTP tabanlı yanıt önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="f11cb-111">HTTP-based response caching</span></span>

<span data-ttu-id="f11cb-112">[HTTP 1.1 önbelleğe alma belirtimi](https://tools.ietf.org/html/rfc7234) Internet önbelleklerini nasıl hareket etmesi gerektiğini açıklar.</span><span class="sxs-lookup"><span data-stu-id="f11cb-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="f11cb-113">Önbelleğe alma işlemi için kullanılan birincil HTTP üst bilgisi [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), önbellek belirtmek için kullanılan *yönergeleri*.</span><span class="sxs-lookup"><span data-stu-id="f11cb-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="f11cb-114">Yönergeleri aşamalarından istemcilerden sunucularına isteklerde ve yanıtı sunucudan istemcilere geri aşamalarından yaptıkça önbelleğe alma davranışını denetleme.</span><span class="sxs-lookup"><span data-stu-id="f11cb-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="f11cb-115">İsteklerin ve yanıtların proxy sunucular taşıyın ve proxy sunucuları HTTP 1.1 önbelleğe alma belirtime uygun de gerekir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="f11cb-116">Ortak `Cache-Control` yönergeleri, aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="f11cb-117">Yönergesi</span><span class="sxs-lookup"><span data-stu-id="f11cb-117">Directive</span></span>                                                       | <span data-ttu-id="f11cb-118">Eylem</span><span class="sxs-lookup"><span data-stu-id="f11cb-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="f11cb-119">public</span><span class="sxs-lookup"><span data-stu-id="f11cb-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="f11cb-120">Bir önbellek yanıt depolayabilir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="f11cb-121">private</span><span class="sxs-lookup"><span data-stu-id="f11cb-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="f11cb-122">Yanıt tarafından paylaşılan bir önbellek depolanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="f11cb-123">Özel bir önbellek depolayın ve yanıt yeniden kullanın.</span><span class="sxs-lookup"><span data-stu-id="f11cb-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="f11cb-124">Maksimum yaş</span><span class="sxs-lookup"><span data-stu-id="f11cb-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="f11cb-125">İstemci, yaş, belirtilen sayıda saniye büyük bir yanıtı kabul edilmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="f11cb-126">Örnekler: `max-age=60` (60 saniye) `max-age=2592000` (1 ay)</span><span class="sxs-lookup"><span data-stu-id="f11cb-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="f11cb-127">önbellek yok</span><span class="sxs-lookup"><span data-stu-id="f11cb-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="f11cb-128">**İsteklerinde**: önbellek isteği karşılamak için saklı bir yanıt kullanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="f11cb-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="f11cb-129">Not: Yeniden için istemci kaynak sunucu yanıtı oluşturur ve önbelleğinde depolanan yanıtta ara yazılım güncelleştirmeleri.</span><span class="sxs-lookup"><span data-stu-id="f11cb-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="f11cb-130">**Yanıtlar üzerinde**: kaynak sunucu doğrulaması olmadan bir sonraki istek için yanıt kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="f11cb-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="f11cb-131">No-store</span><span class="sxs-lookup"><span data-stu-id="f11cb-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="f11cb-132">**İsteklerinde**: önbellek isteği depolamamayı gerekir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="f11cb-133">**Yanıtlar üzerinde**: önbellek yanıt herhangi bir bölümünü depolamamayı gerekir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="f11cb-134">Önbelleğe alma bir rol oynar diğer önbellek üst bilgileri aşağıdaki tabloda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="f11cb-135">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="f11cb-135">Header</span></span>                                                     | <span data-ttu-id="f11cb-136">İşlev</span><span class="sxs-lookup"><span data-stu-id="f11cb-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="f11cb-137">Geçerlilik süresi</span><span class="sxs-lookup"><span data-stu-id="f11cb-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="f11cb-138">Yanıt beri geçen saniye sayısı süre miktarı tahmin oluşturulan veya kaynak sunucuda başarıyla doğrulandı.</span><span class="sxs-lookup"><span data-stu-id="f11cb-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="f11cb-139">Süre sonu</span><span class="sxs-lookup"><span data-stu-id="f11cb-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="f11cb-140">Sonra yanıt eski olarak değerlendirmeden tarih.</span><span class="sxs-lookup"><span data-stu-id="f11cb-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="f11cb-141">Pragması</span><span class="sxs-lookup"><span data-stu-id="f11cb-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="f11cb-142">HTTP/1.0 ile uyumluluk ayarı için geriye doğru önbellekleri için mevcut `no-cache` davranışı.</span><span class="sxs-lookup"><span data-stu-id="f11cb-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="f11cb-143">Varsa `Cache-Control` üstbilgisi mevcutsa, `Pragma` üstbilgi göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="f11cb-144">değişiklik</span><span class="sxs-lookup"><span data-stu-id="f11cb-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="f11cb-145">Önbelleğe alınan yanıt sürece tüm gönderilmemesini gerekir belirtir, `Vary` önbelleğe alınan yanıtın özgün istek ve yeni istek üst bilgi alanları eşleştir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="f11cb-146">HTTP tabanlı önbelleğe alma gizliliğinize Cache-Control yönergeleri istek</span><span class="sxs-lookup"><span data-stu-id="f11cb-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="f11cb-147">[Cache-Control üst bilgisi için HTTP 1.1 önbelleğe alma belirtimi](https://tools.ietf.org/html/rfc7234#section-5.2) geçerli kabul etmenin bir önbellek gerektirir `Cache-Control` istemci tarafından gönderilen başlığı.</span><span class="sxs-lookup"><span data-stu-id="f11cb-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="f11cb-148">Bir istemci isteği ile yapabilen bir `no-cache` üstbilgi değeri ve zorla her istek için yeni bir yanıt oluşturmak için sunucu.</span><span class="sxs-lookup"><span data-stu-id="f11cb-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="f11cb-149">Her zaman istemci uygularken `Cache-Control` HTTP önbelleğe alma amacı gerçekleştiriliyorsa, istek üst bilgilerini mantıklı.</span><span class="sxs-lookup"><span data-stu-id="f11cb-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="f11cb-150">Altında bir resmi belirtimi, önbelleğe alma, istemcileri ve proxy sunucuları, ağ üzerinden isteklerini karşılamak gecikme süresi ve ağ yükünü azaltmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="f11cb-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="f11cb-151">Mutlaka, kaynak sunucu üzerindeki yükü denetlemek için bir yol değil.</span><span class="sxs-lookup"><span data-stu-id="f11cb-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="f11cb-152">Kullanırken bu önbelleğe alma davranışı üzerinde geçerli bir geliştirici denetimi yoktur olan [yanıt önbelleğe alma ara yazılımı](xref:performance/caching/middleware) çünkü resmi belirtimi önbelleğe alma ara yazılımı uyar.</span><span class="sxs-lookup"><span data-stu-id="f11cb-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="f11cb-153">[Ara yazılıma gelecekteki iyileştirmeler](https://github.com/aspnet/ResponseCaching/issues/96) bir isteğin yoksaymak için bir ara yazılım yapılandırma izin verecek `Cache-Control` önbelleğe alınan yanıt hizmet verirken başlığı.</span><span class="sxs-lookup"><span data-stu-id="f11cb-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="f11cb-154">Ara yazılım kullandığınızda bu, sunucunuzdaki yük daha iyi denetim için bir fırsat sunar.</span><span class="sxs-lookup"><span data-stu-id="f11cb-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="f11cb-155">ASP.NET core'da diğer önbelleğe alma teknolojisini</span><span class="sxs-lookup"><span data-stu-id="f11cb-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="f11cb-156">Bellek içi önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="f11cb-156">In-memory caching</span></span>

<span data-ttu-id="f11cb-157">Bellek içi caching, önbelleğe alınmış verileri depolamak için sunucu belleği kullanır.</span><span class="sxs-lookup"><span data-stu-id="f11cb-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="f11cb-158">Bu tür bir önbelleğe alma, tek veya birden fazla sunucu kullanmak için uygundur *Yapışkan oturumlar*.</span><span class="sxs-lookup"><span data-stu-id="f11cb-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="f11cb-159">Yapışkan oturumlar, istemciden gelen isteklere her zaman aynı sunucuya işleme yönlendirildiğinden anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="f11cb-160">Daha fazla bilgi için [bellek içi önbelleğe alma](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="f11cb-160">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="f11cb-161">Dağıtılmış önbellek</span><span class="sxs-lookup"><span data-stu-id="f11cb-161">Distributed Cache</span></span>

<span data-ttu-id="f11cb-162">Uygulamayı bir bulut veya sunucu grubunda barındırıldığında bellek verilerini depolamak için Dağıtılmış bir önbellek kullanın.</span><span class="sxs-lookup"><span data-stu-id="f11cb-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="f11cb-163">Önbelleği istekleri işleyen sunucular arasında paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="f11cb-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="f11cb-164">Bir istemci, istemci için önbelleğe alınan verileri kullanılabilir ise gruptaki hiçbir sunucu tarafından işlenen bir istek gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f11cb-164">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="f11cb-165">ASP.NET Core, SQL Server ve dağıtılmış Redis önbellekleri sunar.</span><span class="sxs-lookup"><span data-stu-id="f11cb-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="f11cb-166">Daha fazla bilgi için [dağıtılmış Önbellekle çalışma](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="f11cb-166">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="f11cb-167">Önbellek etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="f11cb-167">Cache Tag Helper</span></span>

<span data-ttu-id="f11cb-168">Önbellek etiketi Yardımcısı ile bir MVC görünümü ya da bir Razor sayfası içeriği önbelleğe alabilir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="f11cb-169">Önbellek etiketi Yardımcısı, verileri depolamak için bellek içi önbelleğe alma kullanır.</span><span class="sxs-lookup"><span data-stu-id="f11cb-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="f11cb-170">Daha fazla bilgi için [önbellek etiketi Yardımcısı, ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="f11cb-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="f11cb-171">Dağıtılmış önbellek etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="f11cb-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="f11cb-172">Dağıtılmış önbellek etiketi Yardımcısı ile bir MVC görünümü ya da dağıtılmış bir bulut ya da web grubu senaryoları Razor sayfası içeriği önbelleğe alabilir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="f11cb-173">Dağıtılmış önbellek etiketi Yardımcısı, verileri depolamak için SQL Server veya Redis kullanır.</span><span class="sxs-lookup"><span data-stu-id="f11cb-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="f11cb-174">Daha fazla bilgi için [dağıtılmış önbellek etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="f11cb-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="f11cb-175">ResponseCache özniteliği</span><span class="sxs-lookup"><span data-stu-id="f11cb-175">ResponseCache attribute</span></span>

<span data-ttu-id="f11cb-176">[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) yanıt önbelleğe alma uygun üstbilgileri ayarlamak için gerekli parametreleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-176">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="f11cb-177">Kimlik doğrulamasından geçen istemcilerin bilgilerini içeren içerik için önbelleğe alma devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="f11cb-177">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="f11cb-178">Önbelleğe alma yalnızca bir kullanıcının kimlik veya bir kullanıcının oturum açtığı göre değişmeyen içerik için etkinleştirilmiş olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f11cb-178">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="f11cb-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) saklı yanıt sorgu anahtarları ait belirtilen liste değerleriyle göre değişir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="f11cb-180">Tek bir değeri `*` ara yazılım değişir, yanıtların tümü tarafından istek sorgu dizesi parametreleri sağlanır.</span><span class="sxs-lookup"><span data-stu-id="f11cb-180">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="f11cb-181">`VaryByQueryKeys` ASP.NET Core 1.1 veya sonraki bir sürümü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-181">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="f11cb-182">Ayarlanacak yanıt önbelleğe alma ara yazılımı etkinleştirilmelidir `VaryByQueryKeys` özelliği; Aksi takdirde, çalışma zamanı özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f11cb-182">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="f11cb-183">Karşılık gelen bir HTTP üst bilgisi için hiç `VaryByQueryKeys` özelliği.</span><span class="sxs-lookup"><span data-stu-id="f11cb-183">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="f11cb-184">Özelliği, yanıt önbelleğe alma ara yazılımı tarafından işlenen bir HTTP özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-184">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="f11cb-185">Önbelleğe alınan yanıt verecek ara yazılımı için sorgu dizesi ve sorgu dizesi değerini, önceki bir istek eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-185">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="f11cb-186">Örneğin, istekler ve sonuçları aşağıdaki tabloda gösterilen bir dizi göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="f11cb-186">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="f11cb-187">İstek</span><span class="sxs-lookup"><span data-stu-id="f11cb-187">Request</span></span>                          | <span data-ttu-id="f11cb-188">Sonuç</span><span class="sxs-lookup"><span data-stu-id="f11cb-188">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="f11cb-189">Sunucudan döndürülen</span><span class="sxs-lookup"><span data-stu-id="f11cb-189">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="f11cb-190">Ara yazılım döndürülen</span><span class="sxs-lookup"><span data-stu-id="f11cb-190">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="f11cb-191">Sunucudan döndürülen</span><span class="sxs-lookup"><span data-stu-id="f11cb-191">Returned from server</span></span>     |

<span data-ttu-id="f11cb-192">İlk istek sunucu tarafından döndürülen ve Ara yazılımında önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="f11cb-192">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="f11cb-193">Önceki istek sorgu dizesini eşleştiği için ikinci isteği ara yazılımı tarafından döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f11cb-193">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="f11cb-194">Sorgu dizesi değerini, önceki bir istek eşleşmediği için üçüncü isteği ara yazılım önbellekte değil.</span><span class="sxs-lookup"><span data-stu-id="f11cb-194">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="f11cb-195">`ResponseCacheAttribute` Yapılandırmak ve oluşturmak için kullanılır (aracılığıyla `IFilterFactory`) bir [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="f11cb-195">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="f11cb-196">`ResponseCacheFilter` Uygun HTTP üst bilgilerini ve yanıt özellikleri güncelleştirme işlemlerini gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-196">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="f11cb-197">Filtresi:</span><span class="sxs-lookup"><span data-stu-id="f11cb-197">The filter:</span></span>

* <span data-ttu-id="f11cb-198">Mevcut üst bilgileri için kaldırır `Vary`, `Cache-Control`, ve `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="f11cb-198">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="f11cb-199">Kullanıma uygun üstbilgileri kümesinde özelliklerine göre Yazar `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="f11cb-199">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="f11cb-200">HTTP özelliği, önbelleğe alma yanıt güncelleştirmeleri `VaryByQueryKeys` ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="f11cb-200">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="f11cb-201">değişiklik</span><span class="sxs-lookup"><span data-stu-id="f11cb-201">Vary</span></span>

<span data-ttu-id="f11cb-202">Bu üst bilginin ne zaman yalnızca yazılır `VaryByHeader` özelliği ayarlanmış.</span><span class="sxs-lookup"><span data-stu-id="f11cb-202">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="f11cb-203">Ayarlanmış `Vary` özelliğinin değeri.</span><span class="sxs-lookup"><span data-stu-id="f11cb-203">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="f11cb-204">Aşağıdaki örnek kullanımları `VaryByHeader` özelliği:</span><span class="sxs-lookup"><span data-stu-id="f11cb-204">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="f11cb-205">Yanıt Üstbilgileri tarayıcınızın ağ araçları ile görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f11cb-205">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="f11cb-206">Aşağıdaki resimde gösterilmektedir Edge üzerinde çıkışını F12 **ağ** sekmesinde `About2` eylem yöntemine yenilenir:</span><span class="sxs-lookup"><span data-stu-id="f11cb-206">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![About2 eylem yöntemi çağrıldığında F12 çıkış Ağ sekmesinde kenar](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="f11cb-208">NoStore ve Location.None</span><span class="sxs-lookup"><span data-stu-id="f11cb-208">NoStore and Location.None</span></span>

<span data-ttu-id="f11cb-209">`NoStore` diğer özelliklerin çoğu geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="f11cb-209">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="f11cb-210">Bu özelliği ayarlandığında `true`, `Cache-Control` üstbilgi ayarlandığında `no-store`.</span><span class="sxs-lookup"><span data-stu-id="f11cb-210">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="f11cb-211">Varsa `Location` ayarlanır `None`:</span><span class="sxs-lookup"><span data-stu-id="f11cb-211">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="f11cb-212">`Cache-Control` ayarlanır `no-store,no-cache`.</span><span class="sxs-lookup"><span data-stu-id="f11cb-212">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="f11cb-213">`Pragma` ayarlanır `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="f11cb-213">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="f11cb-214">Varsa `NoStore` olduğu `false` ve `Location` olduğu `None`, `Cache-Control` ve `Pragma` ayarlandığından `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="f11cb-214">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="f11cb-215">Genellikle ayarlanabilir `NoStore` için `true` hata sayfalarında.</span><span class="sxs-lookup"><span data-stu-id="f11cb-215">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="f11cb-216">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f11cb-216">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="f11cb-217">Bu, şu olur:</span><span class="sxs-lookup"><span data-stu-id="f11cb-217">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="f11cb-218">Konum ve süresi</span><span class="sxs-lookup"><span data-stu-id="f11cb-218">Location and Duration</span></span>

<span data-ttu-id="f11cb-219">Önbelleğe alma, etkinleştirme `Duration` pozitif bir değere ayarlanması gerekir ve `Location` olmalıdır `Any` (varsayılan) veya `Client`.</span><span class="sxs-lookup"><span data-stu-id="f11cb-219">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="f11cb-220">Bu durumda, `Cache-Control` üstbilgi arkasından konum değeri ayarlandığında `max-age` yanıtın.</span><span class="sxs-lookup"><span data-stu-id="f11cb-220">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="f11cb-221">`Location`ın seçenekleri `Any` ve `Client` küçültmesini `Cache-Control` üstbilgi değerlerini `public` ve `private`sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="f11cb-221">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="f11cb-222">Daha önce belirtildiği gibi ayarı `Location` için `None` hem de ayarlar `Cache-Control` ve `Pragma` üstbilgileri `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="f11cb-222">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="f11cb-223">Ayarlayarak üstbilgileri gösteren bir örnek aşağıda üretilir `Duration` varsayılan bırakarak `Location` değeri:</span><span class="sxs-lookup"><span data-stu-id="f11cb-223">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="f11cb-224">Bu, aşağıdaki üst bilgi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="f11cb-224">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="f11cb-225">Önbellek profilleri</span><span class="sxs-lookup"><span data-stu-id="f11cb-225">Cache profiles</span></span>

<span data-ttu-id="f11cb-226">Çoğaltma yerine `ResponseCache` birçok denetleyici eylem özniteliklerinin, önbellek profilleri ayarları MVC'de ayarlarken seçeneği olarak yapılandırılabilir `ConfigureServices` yönteminde `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f11cb-226">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="f11cb-227">Başvurulan önbellek profilinde bulunan değerleri tarafından varsayılan olarak kullanılır `ResponseCache` öznitelik ve öznitelik üzerinde belirtilen herhangi bir özelliği tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="f11cb-227">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="f11cb-228">Önbellek profili ayarlama:</span><span class="sxs-lookup"><span data-stu-id="f11cb-228">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f11cb-229">Önbellek profili başvuruyor:</span><span class="sxs-lookup"><span data-stu-id="f11cb-229">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="f11cb-230">`ResponseCache` Özniteliği, hem Eylemler (yöntem) ve denetleyicilerine (sınıflar) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-230">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="f11cb-231">Yöntem düzeyindeki öznitelikler, sınıf düzeyi özniteliklerinde belirtilen ayarları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="f11cb-231">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="f11cb-232">60 saniye olarak belirlendi süre önbellek profili yöntemi düzeyinde bir öznitelik başvuruyor ancak yukarıdaki örnekte, 30 saniye süre sınıf düzeyinde bir öznitelik belirtir.</span><span class="sxs-lookup"><span data-stu-id="f11cb-232">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="f11cb-233">Sonuçta elde edilen üst bilgi:</span><span class="sxs-lookup"><span data-stu-id="f11cb-233">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="f11cb-234">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f11cb-234">Additional resources</span></span>

* [<span data-ttu-id="f11cb-235">Yanıtları depolama önbelleklerinde</span><span class="sxs-lookup"><span data-stu-id="f11cb-235">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="f11cb-236">Önbellek denetimi</span><span class="sxs-lookup"><span data-stu-id="f11cb-236">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="f11cb-237">Belleğe yüklenmiş önbellek</span><span class="sxs-lookup"><span data-stu-id="f11cb-237">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="f11cb-238">Dağıtılmış önbellekle çalışma</span><span class="sxs-lookup"><span data-stu-id="f11cb-238">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="f11cb-239">Değişiklik belirteçleri ile değişiklikleri algılama</span><span class="sxs-lookup"><span data-stu-id="f11cb-239">Detect changes with change tokens</span></span>](xref:fundamentals/change-tokens)
* [<span data-ttu-id="f11cb-240">Yanıtları Önbelleğe Alma Ara Yazılımı</span><span class="sxs-lookup"><span data-stu-id="f11cb-240">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="f11cb-241">Önbellek Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="f11cb-241">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="f11cb-242">Dağıtılmış Önbellek Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="f11cb-242">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
