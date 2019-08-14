---
title: ASP.NET Core 'de yanıt sıkıştırması
author: guardrex
description: Yanıt sıkıştırması ve ASP.NET Core uygulamalarında yanıt sıkıştırma ara yazılımı kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/09/2019
uid: performance/response-compression
ms.openlocfilehash: e320e87179f9f1b9773a55c380684a3f3f712632
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993469"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="b7760-103">ASP.NET Core 'de yanıt sıkıştırması</span><span class="sxs-lookup"><span data-stu-id="b7760-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="b7760-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b7760-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b7760-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b7760-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b7760-106">Ağ bant genişliği sınırlı bir kaynaktır.</span><span class="sxs-lookup"><span data-stu-id="b7760-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="b7760-107">Yanıt boyutunu azaltmak genellikle önemli ölçüde önemli ölçüde bir uygulamanın yanıt hızını artırır.</span><span class="sxs-lookup"><span data-stu-id="b7760-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="b7760-108">Yük boyutlarını azaltmanın bir yolu, uygulamanın yanıtlarını sıkıştırmaktır.</span><span class="sxs-lookup"><span data-stu-id="b7760-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="b7760-109">Yanıt sıkıştırma ara yazılımı ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="b7760-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="b7760-110">IIS, Apache veya NGINX 'te sunucu tabanlı yanıt sıkıştırma teknolojilerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="b7760-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="b7760-111">Ara yazılım performansı büyük olasılıkla sunucu modülleriyle eşleşmemelidir.</span><span class="sxs-lookup"><span data-stu-id="b7760-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="b7760-112">[Http. sys Server](xref:fundamentals/servers/httpsys) Server ve [Kestrel](xref:fundamentals/servers/kestrel) Server şu anda yerleşik sıkıştırma desteği sunmaz.</span><span class="sxs-lookup"><span data-stu-id="b7760-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="b7760-113">Şu durumlarda yanıt sıkıştırma ara yazılımını kullanın:</span><span class="sxs-lookup"><span data-stu-id="b7760-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="b7760-114">Aşağıdaki sunucu tabanlı sıkıştırma teknolojileri kullanılamıyor:</span><span class="sxs-lookup"><span data-stu-id="b7760-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="b7760-115">IIS dinamik sıkıştırma modülü</span><span class="sxs-lookup"><span data-stu-id="b7760-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="b7760-116">Apache mod_deflate modülü</span><span class="sxs-lookup"><span data-stu-id="b7760-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="b7760-117">NGINX sıkıştırma ve açma</span><span class="sxs-lookup"><span data-stu-id="b7760-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="b7760-118">Doğrudan barındırma:</span><span class="sxs-lookup"><span data-stu-id="b7760-118">Hosting directly on:</span></span>
  * <span data-ttu-id="b7760-119">[Http. sys sunucusu](xref:fundamentals/servers/httpsys) (eski adıyla WebListener)</span><span class="sxs-lookup"><span data-stu-id="b7760-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="b7760-120">Kestrel sunucusu</span><span class="sxs-lookup"><span data-stu-id="b7760-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="b7760-121">Yanıt sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="b7760-121">Response compression</span></span>

<span data-ttu-id="b7760-122">Genellikle, yerel olarak sıkıştırılmamış herhangi bir yanıt, yanıt sıkıştırmasından faydalanabilir.</span><span class="sxs-lookup"><span data-stu-id="b7760-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="b7760-123">Yerel olarak sıkıştırılan yanıtlar genellikle şunları içerir: CSS, JavaScript, HTML, XML ve JSON.</span><span class="sxs-lookup"><span data-stu-id="b7760-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="b7760-124">PNG dosyaları gibi yerel olarak sıkıştırılan varlıkları sıkıştırmamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="b7760-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="b7760-125">Yerel olarak sıkıştırılan bir yanıtı daha fazla sıkıştırmaya çalışırsanız, boyut ve iletim süresi bakımından küçük bir ek azaltma büyük olasılıkla sıkıştırmayı işlemek için geçen süre kadar fazla gölgelenir.</span><span class="sxs-lookup"><span data-stu-id="b7760-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="b7760-126">150-1000 bayttan daha küçük dosyaları sıkıştırmayın (dosyanın içeriğine ve sıkıştırma verimliliğine bağlı olarak).</span><span class="sxs-lookup"><span data-stu-id="b7760-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="b7760-127">Küçük dosyaları sıkıştırma ek yükü sıkıştırılmamış dosyadan daha büyük bir sıkıştırılmış dosya üretebilir.</span><span class="sxs-lookup"><span data-stu-id="b7760-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="b7760-128">Bir istemci sıkıştırılmış içeriği işleyebilir, istemci, `Accept-Encoding` üst bilgiyi istekle birlikte göndererek yeteneklerini bilgilendirmelidir.</span><span class="sxs-lookup"><span data-stu-id="b7760-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="b7760-129">Bir sunucu sıkıştırılmış içerik gönderdiğinde, sıkıştırılan yanıtın kodlanmasının `Content-Encoding` üst bilgisine bilgi içermelidir.</span><span class="sxs-lookup"><span data-stu-id="b7760-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="b7760-130">Ara yazılım tarafından desteklenen içerik kodlama göstergeleri aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b7760-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="b7760-131">`Accept-Encoding`üst bilgi değerleri</span><span class="sxs-lookup"><span data-stu-id="b7760-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="b7760-132">Desteklenen ara yazılım</span><span class="sxs-lookup"><span data-stu-id="b7760-132">Middleware Supported</span></span> | <span data-ttu-id="b7760-133">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b7760-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="b7760-134">Evet (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="b7760-134">Yes (default)</span></span>        | [<span data-ttu-id="b7760-135">Brotli sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="b7760-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="b7760-136">Hayır</span><span class="sxs-lookup"><span data-stu-id="b7760-136">No</span></span>                   | [<span data-ttu-id="b7760-137">Sıkıştırılmış veri biçimini söndür</span><span class="sxs-lookup"><span data-stu-id="b7760-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="b7760-138">Hayır</span><span class="sxs-lookup"><span data-stu-id="b7760-138">No</span></span>                   | [<span data-ttu-id="b7760-139">W3C verimli XML değişim</span><span class="sxs-lookup"><span data-stu-id="b7760-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="b7760-140">Evet</span><span class="sxs-lookup"><span data-stu-id="b7760-140">Yes</span></span>                  | [<span data-ttu-id="b7760-141">Gzip dosya biçimi</span><span class="sxs-lookup"><span data-stu-id="b7760-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="b7760-142">Evet</span><span class="sxs-lookup"><span data-stu-id="b7760-142">Yes</span></span>                  | <span data-ttu-id="b7760-143">"Kodlama yok" tanımlayıcısı: Yanıt kodlanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="b7760-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="b7760-144">Hayır</span><span class="sxs-lookup"><span data-stu-id="b7760-144">No</span></span>                   | [<span data-ttu-id="b7760-145">Java arşivleri için ağ aktarım biçimi</span><span class="sxs-lookup"><span data-stu-id="b7760-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="b7760-146">Evet</span><span class="sxs-lookup"><span data-stu-id="b7760-146">Yes</span></span>                  | <span data-ttu-id="b7760-147">Tüm kullanılabilir içerik kodlamaları açıkça istenmedi</span><span class="sxs-lookup"><span data-stu-id="b7760-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="b7760-148">`Accept-Encoding`üst bilgi değerleri</span><span class="sxs-lookup"><span data-stu-id="b7760-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="b7760-149">Desteklenen ara yazılım</span><span class="sxs-lookup"><span data-stu-id="b7760-149">Middleware Supported</span></span> | <span data-ttu-id="b7760-150">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b7760-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="b7760-151">Hayır</span><span class="sxs-lookup"><span data-stu-id="b7760-151">No</span></span>                   | [<span data-ttu-id="b7760-152">Brotli sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="b7760-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="b7760-153">Hayır</span><span class="sxs-lookup"><span data-stu-id="b7760-153">No</span></span>                   | [<span data-ttu-id="b7760-154">Sıkıştırılmış veri biçimini söndür</span><span class="sxs-lookup"><span data-stu-id="b7760-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="b7760-155">Hayır</span><span class="sxs-lookup"><span data-stu-id="b7760-155">No</span></span>                   | [<span data-ttu-id="b7760-156">W3C verimli XML değişim</span><span class="sxs-lookup"><span data-stu-id="b7760-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="b7760-157">Evet (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="b7760-157">Yes (default)</span></span>        | [<span data-ttu-id="b7760-158">Gzip dosya biçimi</span><span class="sxs-lookup"><span data-stu-id="b7760-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="b7760-159">Evet</span><span class="sxs-lookup"><span data-stu-id="b7760-159">Yes</span></span>                  | <span data-ttu-id="b7760-160">"Kodlama yok" tanımlayıcısı: Yanıt kodlanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="b7760-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="b7760-161">Hayır</span><span class="sxs-lookup"><span data-stu-id="b7760-161">No</span></span>                   | [<span data-ttu-id="b7760-162">Java arşivleri için ağ aktarım biçimi</span><span class="sxs-lookup"><span data-stu-id="b7760-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="b7760-163">Evet</span><span class="sxs-lookup"><span data-stu-id="b7760-163">Yes</span></span>                  | <span data-ttu-id="b7760-164">Tüm kullanılabilir içerik kodlamaları açıkça istenmedi</span><span class="sxs-lookup"><span data-stu-id="b7760-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="b7760-165">Daha fazla bilgi için, [IANA resmi Içerik kodlama listesine](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)bakın.</span><span class="sxs-lookup"><span data-stu-id="b7760-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="b7760-166">Ara yazılım, özel `Accept-Encoding` üst bilgi değerleri için ek sıkıştırma sağlayıcıları eklemenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="b7760-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="b7760-167">Daha fazla bilgi için aşağıdaki [özel sağlayıcılar](#custom-providers) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b7760-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="b7760-168">Ara yazılım, sıkıştırma düzenlerini önceliklendirmek için istemci tarafından gönderildiğinde kalite değeri ( `q`qvalue,) ağırlığa yeniden davranıyor.</span><span class="sxs-lookup"><span data-stu-id="b7760-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="b7760-169">Daha fazla bilgi için bkz [. RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="b7760-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="b7760-170">Sıkıştırma algoritmaları, sıkıştırma hızı ve sıkıştırmanın verimliliği arasında bir zorunluluğunu getirir tabidir.</span><span class="sxs-lookup"><span data-stu-id="b7760-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="b7760-171">Bu bağlamdaki *verimlilik* , sıkıştırmadan sonra çıkışın boyutunu ifade eder.</span><span class="sxs-lookup"><span data-stu-id="b7760-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="b7760-172">En en uygun sıkıştırma, en *iyi* boyut ile elde edilir.</span><span class="sxs-lookup"><span data-stu-id="b7760-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="b7760-173">Sıkıştırılmış içerik isteme, gönderme, önbelleğe alma ve alma ile ilgili üstbilgiler aşağıdaki tabloda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b7760-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="b7760-174">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="b7760-174">Header</span></span>             | <span data-ttu-id="b7760-175">Rol</span><span class="sxs-lookup"><span data-stu-id="b7760-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="b7760-176">İstemci için kabul edilebilir içerik kodlama düzenlerini göstermek üzere istemciden sunucusuna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b7760-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="b7760-177">Yük içindeki içeriğin kodlamasını göstermek için sunucudan istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b7760-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="b7760-178">Sıkıştırma gerçekleştiğinde `Content-Length` , yanıt sıkıştırıldığında gövde içeriği değiştiği için başlık kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="b7760-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="b7760-179">Sıkıştırma gerçekleştiğinde `Content-MD5` , gövde içeriği değiştiğinden ve karma artık geçerli olmadığından başlık kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="b7760-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="b7760-180">İçeriğin MIME türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="b7760-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="b7760-181">Her yanıt, değerini `Content-Type`belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="b7760-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="b7760-182">Ara yazılım, yanıtın sıkıştırılıp sıkıştırılmadığını belirlemede bu değeri denetler.</span><span class="sxs-lookup"><span data-stu-id="b7760-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="b7760-183">Ara yazılım, kodlayamadığı bir dizi [varsayılan MIME türünü](#mime-types) belirtir, ancak MIME türlerini değiştirebilir veya ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b7760-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="b7760-184">Sunucu tarafından istemciler ve proxy 'ler `Accept-Encoding` için bir değere sahip olduğunda `Vary` , üst bilgi, isteğin `Accept-Encoding` üst bilgisinin değerine göre önbelleğe alma (değişiklik) gereken yanıtı istemciye veya proxy 'ye bildirir.</span><span class="sxs-lookup"><span data-stu-id="b7760-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="b7760-185">`Vary: Accept-Encoding` Üst bilgiyle içerik döndürmesinin sonucu, hem sıkıştırılmış hem de sıkıştırılmamış yanıtların ayrı olarak önbelleğe alınma sonucudur.</span><span class="sxs-lookup"><span data-stu-id="b7760-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="b7760-186">[Örnek uygulamayla](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples)yanıt sıkıştırma ara yazılımı 'nın özelliklerini gezin.</span><span class="sxs-lookup"><span data-stu-id="b7760-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="b7760-187">Örnek şunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="b7760-187">The sample illustrates:</span></span>

* <span data-ttu-id="b7760-188">Gzip ve özel sıkıştırma sağlayıcıları kullanılarak uygulama yanıtlarının sıkıştırılması.</span><span class="sxs-lookup"><span data-stu-id="b7760-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="b7760-189">Sıkıştırma için varsayılan MIME türleri listesine MIME türü ekleme.</span><span class="sxs-lookup"><span data-stu-id="b7760-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="b7760-190">Paket</span><span class="sxs-lookup"><span data-stu-id="b7760-190">Package</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b7760-191">Yanıt sıkıştırma ara yazılımı, ASP.NET Core uygulamalarında örtük olarak bulunan [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="b7760-191">Response Compression Middleware is provided by the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package, which is implicitly included in ASP.NET Core apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b7760-192">Ara yazılımı bir projeye eklemek için Microsoft. AspNetCore. [ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paketini içeren [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)öğesine bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b7760-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="b7760-193">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b7760-193">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b7760-194">Aşağıdaki kod, varsayılan MIME türleri ve sıkıştırma sağlayıcıları için yanıt sıkıştırma ara yazılımı 'nın nasıl etkinleştirileceğini gösterir ([Brotli](#brotli-compression-provider) ve [gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="b7760-194">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b7760-195">Aşağıdaki kod, varsayılan MIME türleri ve [gzip sıkıştırma sağlayıcısı](#gzip-compression-provider)Için yanıt sıkıştırma ara yazılımını nasıl etkinleştireceğinizi göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="b7760-195">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

::: moniker-end

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

<span data-ttu-id="b7760-196">Notlar:</span><span class="sxs-lookup"><span data-stu-id="b7760-196">Notes:</span></span>

* <span data-ttu-id="b7760-197">`app.UseResponseCompression`öğesinden önce `app.UseMvc`çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b7760-197">`app.UseResponseCompression` must be called before `app.UseMvc`.</span></span>
* <span data-ttu-id="b7760-198">`Accept-Encoding` İstek üst bilgisini ayarlamak ve yanıt üst bilgilerini, boyutunu ve gövdesini incelemek için [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)veya [Postman](https://www.getpostman.com/) gibi bir araç kullanın.</span><span class="sxs-lookup"><span data-stu-id="b7760-198">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="b7760-199">`Accept-Encoding` Üstbilgi olmadan örnek uygulamaya bir istek gönderir ve yanıtın sıkıştırılmamış olduğunu gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="b7760-199">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="b7760-200">`Content-Encoding` Ve`Vary` başlıkları yanıtta yok.</span><span class="sxs-lookup"><span data-stu-id="b7760-200">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Accept-Encoding üst bilgisi olmadan bir isteğin sonucunu gösteren Fiddler penceresi.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b7760-203">`Accept-Encoding: br` Üstbilgi (Brotli Compression) ile örnek uygulamaya bir istek gönderir ve yanıtın sıkıştırıldığını gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="b7760-203">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="b7760-204">`Content-Encoding` Ve`Vary` üstbilgileri yanıtta mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="b7760-204">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Accept-Encoding üst bilgisi ve br değeri olan bir isteğin sonucunu gösteren Fiddler penceresi.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b7760-208">`Accept-Encoding: gzip` Üstbilgi ile örnek uygulamaya bir istek gönderir ve yanıtın sıkıştırıldığını gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="b7760-208">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="b7760-209">`Content-Encoding` Ve`Vary` üstbilgileri yanıtta mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="b7760-209">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Accept-Encoding üst bilgisi ve gzip değeri ile bir isteğin sonucunu gösteren Fiddler penceresi.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="b7760-213">Sağlayıcılarla</span><span class="sxs-lookup"><span data-stu-id="b7760-213">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="b7760-214">Brotli sıkıştırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b7760-214">Brotli Compression Provider</span></span>

<span data-ttu-id="b7760-215">[Brotli sıkıştırılmış veri biçimiyle](https://tools.ietf.org/html/rfc7932)yanıtları sıkıştırmak içinöğesinikullanın.<xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider></span><span class="sxs-lookup"><span data-stu-id="b7760-215">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="b7760-216">Hiçbir sıkıştırma sağlayıcısı açıkça öğesine <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>eklenmemişse:</span><span class="sxs-lookup"><span data-stu-id="b7760-216">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="b7760-217">Brotli sıkıştırma sağlayıcısı, [gzip sıkıştırma sağlayıcısıyla](#gzip-compression-provider)birlikte, varsayılan olarak sıkıştırma sağlayıcılarının dizisine eklenir.</span><span class="sxs-lookup"><span data-stu-id="b7760-217">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="b7760-218">Brotli sıkıştırılmış veri biçimi istemci tarafından desteklenerek sıkıştırma varsayılan olarak Brotli Compression.</span><span class="sxs-lookup"><span data-stu-id="b7760-218">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="b7760-219">Brotli istemci tarafından desteklenmiyorsa, istemci gzip sıkıştırmasını destekliyorsa sıkıştırma varsayılan olarak gzip olur.</span><span class="sxs-lookup"><span data-stu-id="b7760-219">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="b7760-220">Herhangi bir sıkıştırma sağlayıcısı açıkça eklendiğinde, Brotoli sıkıştırma sağlayıcısının eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="b7760-220">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b7760-221">Sıkıştırma düzeyini ile <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b7760-221">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="b7760-222">Brotli sıkıştırma sağlayıcısı, en hızlı sıkıştırma düzeyi ([CompressionLevel. en hızlı](xref:System.IO.Compression.CompressionLevel)) olarak varsayılan olarak en verimli sıkıştırmayı oluşturmayabilir.</span><span class="sxs-lookup"><span data-stu-id="b7760-222">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="b7760-223">En verimli sıkıştırma isteniyorsa, ara yazılımı en uygun sıkıştırma için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="b7760-223">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="b7760-224">Sıkıştırma düzeyi</span><span class="sxs-lookup"><span data-stu-id="b7760-224">Compression Level</span></span> | <span data-ttu-id="b7760-225">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b7760-225">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="b7760-226">CompressionLevel. en hızlı</span><span class="sxs-lookup"><span data-stu-id="b7760-226">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="b7760-227">Elde edilen çıktı en iyi şekilde sıkıştırısa bile, sıkıştırma mümkün olduğunca hızlı tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="b7760-227">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="b7760-228">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="b7760-228">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="b7760-229">Sıkıştırma gerçekleştirilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="b7760-229">No compression should be performed.</span></span> |
| [<span data-ttu-id="b7760-230">CompressionLevel. optimum</span><span class="sxs-lookup"><span data-stu-id="b7760-230">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="b7760-231">Sıkıştırmanın tamamlanmasının daha uzun sürse bile yanıtlar en iyi şekilde sıkıştırılır.</span><span class="sxs-lookup"><span data-stu-id="b7760-231">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<BrotliCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

::: moniker-end

### <a name="gzip-compression-provider"></a><span data-ttu-id="b7760-232">Gzip sıkıştırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b7760-232">Gzip Compression Provider</span></span>

<span data-ttu-id="b7760-233">[Gzip dosya biçimiyle](https://tools.ietf.org/html/rfc1952)yanıtları sıkıştırmak içinöğesinikullanın.<xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider></span><span class="sxs-lookup"><span data-stu-id="b7760-233">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="b7760-234">Hiçbir sıkıştırma sağlayıcısı açıkça öğesine <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>eklenmemişse:</span><span class="sxs-lookup"><span data-stu-id="b7760-234">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="b7760-235">Gzip sıkıştırma sağlayıcısı varsayılan olarak, [Brotli sıkıştırma sağlayıcısıyla](#brotli-compression-provider)birlikte sıkıştırma sağlayıcılarının dizisine eklenir.</span><span class="sxs-lookup"><span data-stu-id="b7760-235">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="b7760-236">Brotli sıkıştırılmış veri biçimi istemci tarafından desteklenerek sıkıştırma varsayılan olarak Brotli Compression.</span><span class="sxs-lookup"><span data-stu-id="b7760-236">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="b7760-237">Brotli istemci tarafından desteklenmiyorsa, istemci gzip sıkıştırmasını destekliyorsa sıkıştırma varsayılan olarak gzip olur.</span><span class="sxs-lookup"><span data-stu-id="b7760-237">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="b7760-238">Gzip sıkıştırma sağlayıcısı, varsayılan olarak sıkıştırma sağlayıcılarının dizisine eklenir.</span><span class="sxs-lookup"><span data-stu-id="b7760-238">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="b7760-239">İstemci gzip sıkıştırmasını destekliyorsa, sıkıştırma varsayılan olarak gzip olur.</span><span class="sxs-lookup"><span data-stu-id="b7760-239">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="b7760-240">Herhangi bir sıkıştırma sağlayıcısı açık olarak eklendiğinde gzip sıkıştırma sağlayıcısının eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="b7760-240">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

<span data-ttu-id="b7760-241">Sıkıştırma düzeyini ile <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b7760-241">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="b7760-242">Gzip sıkıştırma sağlayıcısı, en hızlı sıkıştırma düzeyine ([CompressionLevel. en hızlı](xref:System.IO.Compression.CompressionLevel)) göre varsayılan olarak en verimli sıkıştırmayı oluşturmayabilir.</span><span class="sxs-lookup"><span data-stu-id="b7760-242">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="b7760-243">En verimli sıkıştırma isteniyorsa, ara yazılımı en uygun sıkıştırma için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="b7760-243">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="b7760-244">Sıkıştırma düzeyi</span><span class="sxs-lookup"><span data-stu-id="b7760-244">Compression Level</span></span> | <span data-ttu-id="b7760-245">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b7760-245">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="b7760-246">CompressionLevel. en hızlı</span><span class="sxs-lookup"><span data-stu-id="b7760-246">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="b7760-247">Elde edilen çıktı en iyi şekilde sıkıştırısa bile, sıkıştırma mümkün olduğunca hızlı tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="b7760-247">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="b7760-248">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="b7760-248">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="b7760-249">Sıkıştırma gerçekleştirilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="b7760-249">No compression should be performed.</span></span> |
| [<span data-ttu-id="b7760-250">CompressionLevel. optimum</span><span class="sxs-lookup"><span data-stu-id="b7760-250">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="b7760-251">Sıkıştırmanın tamamlanmasının daha uzun sürse bile yanıtlar en iyi şekilde sıkıştırılır.</span><span class="sxs-lookup"><span data-stu-id="b7760-251">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<GzipCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

### <a name="custom-providers"></a><span data-ttu-id="b7760-252">Özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="b7760-252">Custom providers</span></span>

<span data-ttu-id="b7760-253">İle <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>özel sıkıştırma uygulamaları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b7760-253">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="b7760-254">, <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> Bunun`ICompressionProvider` ürettiği içerik kodlamasını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="b7760-254">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="b7760-255">Ara yazılım bu bilgileri, isteğin `Accept-Encoding` üstbilgisinde belirtilen listeye göre sağlayıcıyı seçmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="b7760-255">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="b7760-256">İstemci, örnek uygulamayı kullanarak `Accept-Encoding: mycustomcompression` üst bilgiyle bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="b7760-256">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="b7760-257">Ara yazılım özel sıkıştırma uygulamasını kullanır ve bir `Content-Encoding: mycustomcompression` üst bilgiyle yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="b7760-257">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="b7760-258">Özel bir sıkıştırma uygulamasının çalışması için istemcinin özel kodlamayı açıp açabilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="b7760-258">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b7760-259">`Accept-Encoding: mycustomcompression` Üstbilgi ile örnek uygulamaya bir istek gönderir ve yanıt üst bilgilerini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="b7760-259">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="b7760-260">`Vary` Ve`Content-Encoding` üstbilgileri yanıtta mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="b7760-260">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="b7760-261">Yanıt gövdesi (gösterilmez) örnek tarafından sıkıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="b7760-261">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="b7760-262">Örnek `CustomCompressionProvider` sınıfında bir sıkıştırma uygulamanız yok.</span><span class="sxs-lookup"><span data-stu-id="b7760-262">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="b7760-263">Ancak örnek, bu tür bir sıkıştırma algoritmasını nerede uygulayacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="b7760-263">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Accept-Encoding üst bilgisi ve mycustomcompression değeri ile bir isteğin sonucunu gösteren Fiddler penceresi.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="b7760-266">MIME türleri</span><span class="sxs-lookup"><span data-stu-id="b7760-266">MIME types</span></span>

<span data-ttu-id="b7760-267">Ara yazılım, sıkıştırma için varsayılan bir MIME türleri kümesi belirtir:</span><span class="sxs-lookup"><span data-stu-id="b7760-267">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="b7760-268">MIME türlerini, yanıt sıkıştırma ara yazılım seçenekleriyle değiştirin veya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b7760-268">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="b7760-269">Gibi joker karakter MIME türlerinin `text/*` desteklenmediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="b7760-269">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="b7760-270">Örnek uygulama, için `image/svg+xml` bir MIME türü ekler ve ASP.NET Core başlık görüntüsünü (*Banner. SVG*) sıkıştırır ve sunar.</span><span class="sxs-lookup"><span data-stu-id="b7760-270">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="b7760-271">Güvenli protokolle sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="b7760-271">Compression with secure protocol</span></span>

<span data-ttu-id="b7760-272">Güvenli bağlantılar üzerinden sıkıştırılan yanıtlar, varsayılan olarak devre dışı `EnableForHttps` bırakılan seçeneğiyle denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="b7760-272">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="b7760-273">Dinamik olarak oluşturulan sayfalarla sıkıştırma kullanmak, SUA ve [ihlal](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları gibi güvenlik [](https://wikipedia.org/wiki/CRIME_(security_exploit)) sorunlarına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="b7760-273">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="b7760-274">Vary üst bilgisi ekleniyor</span><span class="sxs-lookup"><span data-stu-id="b7760-274">Adding the Vary header</span></span>

<span data-ttu-id="b7760-275">`Accept-Encoding` Üstbilgileri temel alarak yanıtları sıkıştırırken, büyük olasılıkla birden çok sıkıştırılmış yanıt ve sıkıştırılmamış bir sürüm vardır.</span><span class="sxs-lookup"><span data-stu-id="b7760-275">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="b7760-276">İstemci ve proxy 'nin birden çok sürümün var olduğunu ve depolanması gerektiğini bildirmek için, `Vary` üst bilgi bir `Accept-Encoding` değerle eklenir.</span><span class="sxs-lookup"><span data-stu-id="b7760-276">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="b7760-277">ASP.NET Core 2,0 veya sonraki bir sürümde, yanıt sıkıştırıldığında `Vary` ara yazılım üstbilgiyi otomatik olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="b7760-277">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="b7760-278">NGINX ters proxy 'nin arkasında ara yazılım sorunu</span><span class="sxs-lookup"><span data-stu-id="b7760-278">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="b7760-279">Bir istek NGINX tarafından proxy oluşturulduğunda, `Accept-Encoding` üst bilgi kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="b7760-279">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="b7760-280">`Accept-Encoding` Üstbilgiyi kaldırmak, ara yazılımın yanıtı sıkıştırmasını önler.</span><span class="sxs-lookup"><span data-stu-id="b7760-280">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="b7760-281">Daha fazla bilgi için bkz [. NGINX: Sıkıştırma ve açma](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="b7760-281">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="b7760-282">Bu sorun, [NGINX (ASPNET/basicara yazılım \#123) için geçişli sıkıştırmayı](https://github.com/aspnet/BasicMiddleware/issues/123)düzenleyerek izlenir.</span><span class="sxs-lookup"><span data-stu-id="b7760-282">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="b7760-283">IIS dinamik sıkıştırma ile çalışma</span><span class="sxs-lookup"><span data-stu-id="b7760-283">Working with IIS dynamic compression</span></span>

<span data-ttu-id="b7760-284">Bir uygulama için devre dışı bırakmak istediğiniz sunucu düzeyinde yapılandırılmış etkin bir IIS dinamik sıkıştırma modülünüzün varsa, modülü *Web. config* dosyasına ek olarak devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="b7760-284">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="b7760-285">Daha fazla bilgi için bkz. [IIS modüllerini devre dışı bırakma](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="b7760-285">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="b7760-286">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="b7760-286">Troubleshooting</span></span>

<span data-ttu-id="b7760-287">[Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)veya [Postman](https://www.getpostman.com/)gibi bir araç kullanarak `Accept-Encoding` istek üst bilgisini ayarlamanıza ve yanıt üst bilgilerini, boyutunu ve gövdesini incelemeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="b7760-287">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="b7760-288">Varsayılan olarak, yanıt sıkıştırma ara yazılımı aşağıdaki koşullara uyan yanıtları sıkıştırır:</span><span class="sxs-lookup"><span data-stu-id="b7760-288">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="b7760-289">Üst bilgi,,, veya oluşturduğunuz özel `br`bir sıkıştırma `*`sağlayıcısıyla eşleşen özel bir kodlama değeri `gzip`ile birlikte bulunur. `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="b7760-289">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="b7760-290">Değer `identity` , 0 (sıfır) olarak bir kalite değeri (qvalue, `q`) ayarı içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="b7760-290">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="b7760-291">MIME türü (`Content-Type`) ayarlanmalıdır ve <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>üzerinde yapılandırılmış bir MIME türüyle eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="b7760-291">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="b7760-292">İstek `Content-Range` üstbilgiyi içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="b7760-292">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="b7760-293">Yanıt sıkıştırma ara yazılımı seçeneklerinde güvenli Protokolü (https) yapılandırılmadığı takdirde istek güvenli olmayan protokol (http) kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b7760-293">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="b7760-294">*Güvenli içerik sıkıştırması etkinleştirildiğinde [yukarıda açıklanan](#compression-with-secure-protocol) tehlikede göz önünde yer.*</span><span class="sxs-lookup"><span data-stu-id="b7760-294">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="b7760-295">Üst bilgi, veya oluşturduğunuz özel bir sıkıştırma `gzip`sağlayıcısıyla `*`eşleşen özel bir kodlama değeriyle birlikte bulunur. `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="b7760-295">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="b7760-296">Değer `identity` , 0 (sıfır) olarak bir kalite değeri (qvalue, `q`) ayarı içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="b7760-296">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="b7760-297">MIME türü (`Content-Type`) ayarlanmalıdır ve <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>üzerinde yapılandırılmış bir MIME türüyle eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="b7760-297">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="b7760-298">İstek `Content-Range` üstbilgiyi içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="b7760-298">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="b7760-299">Yanıt sıkıştırma ara yazılımı seçeneklerinde güvenli Protokolü (https) yapılandırılmadığı takdirde istek güvenli olmayan protokol (http) kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b7760-299">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="b7760-300">*Güvenli içerik sıkıştırması etkinleştirildiğinde [yukarıda açıklanan](#compression-with-secure-protocol) tehlikede göz önünde yer.*</span><span class="sxs-lookup"><span data-stu-id="b7760-300">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="b7760-301">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b7760-301">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="b7760-302">Mozilla Geliştirici Ağı: Kabul etme-kodlama</span><span class="sxs-lookup"><span data-stu-id="b7760-302">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="b7760-303">RFC 7231 Bölüm 3.1.2.1: İçerik Işbirliği</span><span class="sxs-lookup"><span data-stu-id="b7760-303">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="b7760-304">RFC 7230 Bölüm 4.2.3: Gzip kodlaması</span><span class="sxs-lookup"><span data-stu-id="b7760-304">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="b7760-305">GZIP dosya biçimi belirtimi sürüm 4,3</span><span class="sxs-lookup"><span data-stu-id="b7760-305">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
