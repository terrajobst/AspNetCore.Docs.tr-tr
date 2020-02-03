---
title: ASP.NET Core 'de yanıt sıkıştırması
author: guardrex
description: Yanıt sıkıştırması ve ASP.NET Core uygulamalarında yanıt sıkıştırma ara yazılımı kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
uid: performance/response-compression
ms.openlocfilehash: b8a84418a3258e9ac43b4eadd8564c0708590bce
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726969"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="9a906-103">ASP.NET Core 'de yanıt sıkıştırması</span><span class="sxs-lookup"><span data-stu-id="9a906-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="9a906-104">[Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="9a906-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9a906-105">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9a906-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9a906-106">Ağ bant genişliği sınırlı bir kaynaktır.</span><span class="sxs-lookup"><span data-stu-id="9a906-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="9a906-107">Yanıt boyutunu azaltmak genellikle önemli ölçüde önemli ölçüde bir uygulamanın yanıt hızını artırır.</span><span class="sxs-lookup"><span data-stu-id="9a906-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="9a906-108">Yük boyutlarını azaltmanın bir yolu, uygulamanın yanıtlarını sıkıştırmaktır.</span><span class="sxs-lookup"><span data-stu-id="9a906-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="9a906-109">Yanıt sıkıştırma ara yazılımı ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="9a906-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="9a906-110">IIS, Apache veya NGINX 'te sunucu tabanlı yanıt sıkıştırma teknolojilerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="9a906-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="9a906-111">Ara yazılım performansı büyük olasılıkla sunucu modülleriyle eşleşmemelidir.</span><span class="sxs-lookup"><span data-stu-id="9a906-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="9a906-112">[Http. sys Server](xref:fundamentals/servers/httpsys) Server ve [Kestrel](xref:fundamentals/servers/kestrel) Server şu anda yerleşik sıkıştırma desteği sunmaz.</span><span class="sxs-lookup"><span data-stu-id="9a906-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="9a906-113">Şu durumlarda yanıt sıkıştırma ara yazılımını kullanın:</span><span class="sxs-lookup"><span data-stu-id="9a906-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="9a906-114">Aşağıdaki sunucu tabanlı sıkıştırma teknolojileri kullanılamıyor:</span><span class="sxs-lookup"><span data-stu-id="9a906-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="9a906-115">IIS dinamik sıkıştırma modülü</span><span class="sxs-lookup"><span data-stu-id="9a906-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="9a906-116">Apache mod_deflate modülü</span><span class="sxs-lookup"><span data-stu-id="9a906-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="9a906-117">NGINX sıkıştırma ve açma</span><span class="sxs-lookup"><span data-stu-id="9a906-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="9a906-118">Doğrudan barındırma:</span><span class="sxs-lookup"><span data-stu-id="9a906-118">Hosting directly on:</span></span>
  * <span data-ttu-id="9a906-119">[Http. sys sunucusu](xref:fundamentals/servers/httpsys) (eski adıyla webListener)</span><span class="sxs-lookup"><span data-stu-id="9a906-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="9a906-120">Kestrel sunucusu</span><span class="sxs-lookup"><span data-stu-id="9a906-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="9a906-121">Yanıt sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="9a906-121">Response compression</span></span>

<span data-ttu-id="9a906-122">Genellikle, yerel olarak sıkıştırılmamış herhangi bir yanıt, yanıt sıkıştırmasından faydalanabilir.</span><span class="sxs-lookup"><span data-stu-id="9a906-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="9a906-123">Yerel olarak sıkıştırılan yanıtlar genellikle şunlardır: CSS, JavaScript, HTML, XML ve JSON.</span><span class="sxs-lookup"><span data-stu-id="9a906-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="9a906-124">PNG dosyaları gibi yerel olarak sıkıştırılan varlıkları sıkıştırmamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9a906-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="9a906-125">Yerel olarak sıkıştırılan bir yanıtı daha fazla sıkıştırmaya çalışırsanız, boyut ve iletim süresi bakımından küçük bir ek azaltma büyük olasılıkla sıkıştırmayı işlemek için geçen süre kadar fazla gölgelenir.</span><span class="sxs-lookup"><span data-stu-id="9a906-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="9a906-126">150-1000 bayttan daha küçük dosyaları sıkıştırmayın (dosyanın içeriğine ve sıkıştırma verimliliğine bağlı olarak).</span><span class="sxs-lookup"><span data-stu-id="9a906-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="9a906-127">Küçük dosyaları sıkıştırma ek yükü sıkıştırılmamış dosyadan daha büyük bir sıkıştırılmış dosya üretebilir.</span><span class="sxs-lookup"><span data-stu-id="9a906-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="9a906-128">Bir istemci sıkıştırılmış içeriği işleyebilişlerde, istemci, `Accept-Encoding` üst bilgisini istekle göndererek yeteneklerini bilgilendirmelidir.</span><span class="sxs-lookup"><span data-stu-id="9a906-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="9a906-129">Bir sunucu sıkıştırılmış içerik gönderdiğinde, sıkıştırılmış yanıtın kodlanmasının `Content-Encoding` üst bilgisine bilgi içermelidir.</span><span class="sxs-lookup"><span data-stu-id="9a906-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="9a906-130">Ara yazılım tarafından desteklenen içerik kodlama göstergeleri aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="9a906-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="9a906-131">üst bilgi değerlerini `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="9a906-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="9a906-132">Desteklenen ara yazılım</span><span class="sxs-lookup"><span data-stu-id="9a906-132">Middleware Supported</span></span> | <span data-ttu-id="9a906-133">Açıklama</span><span class="sxs-lookup"><span data-stu-id="9a906-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="9a906-134">Evet (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="9a906-134">Yes (default)</span></span>        | [<span data-ttu-id="9a906-135">Brotli sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="9a906-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="9a906-136">Hayır</span><span class="sxs-lookup"><span data-stu-id="9a906-136">No</span></span>                   | [<span data-ttu-id="9a906-137">Sıkıştırılmış veri biçimini söndür</span><span class="sxs-lookup"><span data-stu-id="9a906-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="9a906-138">Hayır</span><span class="sxs-lookup"><span data-stu-id="9a906-138">No</span></span>                   | [<span data-ttu-id="9a906-139">W3C verimli XML değişim</span><span class="sxs-lookup"><span data-stu-id="9a906-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="9a906-140">Evet</span><span class="sxs-lookup"><span data-stu-id="9a906-140">Yes</span></span>                  | [<span data-ttu-id="9a906-141">Gzip dosya biçimi</span><span class="sxs-lookup"><span data-stu-id="9a906-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="9a906-142">Evet</span><span class="sxs-lookup"><span data-stu-id="9a906-142">Yes</span></span>                  | <span data-ttu-id="9a906-143">"Kodlama yok" tanımlayıcısı: Yanıt kodlanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="9a906-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="9a906-144">Hayır</span><span class="sxs-lookup"><span data-stu-id="9a906-144">No</span></span>                   | [<span data-ttu-id="9a906-145">Java arşivleri için ağ aktarım biçimi</span><span class="sxs-lookup"><span data-stu-id="9a906-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="9a906-146">Evet</span><span class="sxs-lookup"><span data-stu-id="9a906-146">Yes</span></span>                  | <span data-ttu-id="9a906-147">Tüm kullanılabilir içerik kodlamaları açıkça istenmedi</span><span class="sxs-lookup"><span data-stu-id="9a906-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="9a906-148">üst bilgi değerlerini `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="9a906-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="9a906-149">Desteklenen ara yazılım</span><span class="sxs-lookup"><span data-stu-id="9a906-149">Middleware Supported</span></span> | <span data-ttu-id="9a906-150">Açıklama</span><span class="sxs-lookup"><span data-stu-id="9a906-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="9a906-151">Hayır</span><span class="sxs-lookup"><span data-stu-id="9a906-151">No</span></span>                   | [<span data-ttu-id="9a906-152">Brotli sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="9a906-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="9a906-153">Hayır</span><span class="sxs-lookup"><span data-stu-id="9a906-153">No</span></span>                   | [<span data-ttu-id="9a906-154">Sıkıştırılmış veri biçimini söndür</span><span class="sxs-lookup"><span data-stu-id="9a906-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="9a906-155">Hayır</span><span class="sxs-lookup"><span data-stu-id="9a906-155">No</span></span>                   | [<span data-ttu-id="9a906-156">W3C verimli XML değişim</span><span class="sxs-lookup"><span data-stu-id="9a906-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="9a906-157">Evet (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="9a906-157">Yes (default)</span></span>        | [<span data-ttu-id="9a906-158">Gzip dosya biçimi</span><span class="sxs-lookup"><span data-stu-id="9a906-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="9a906-159">Evet</span><span class="sxs-lookup"><span data-stu-id="9a906-159">Yes</span></span>                  | <span data-ttu-id="9a906-160">"Kodlama yok" tanımlayıcısı: Yanıt kodlanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="9a906-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="9a906-161">Hayır</span><span class="sxs-lookup"><span data-stu-id="9a906-161">No</span></span>                   | [<span data-ttu-id="9a906-162">Java arşivleri için ağ aktarım biçimi</span><span class="sxs-lookup"><span data-stu-id="9a906-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="9a906-163">Evet</span><span class="sxs-lookup"><span data-stu-id="9a906-163">Yes</span></span>                  | <span data-ttu-id="9a906-164">Tüm kullanılabilir içerik kodlamaları açıkça istenmedi</span><span class="sxs-lookup"><span data-stu-id="9a906-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="9a906-165">Daha fazla bilgi için, [IANA resmi Içerik kodlama listesine](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)bakın.</span><span class="sxs-lookup"><span data-stu-id="9a906-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="9a906-166">Ara yazılım, özel `Accept-Encoding` üst bilgi değerleri için ek sıkıştırma sağlayıcıları eklemenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="9a906-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="9a906-167">Daha fazla bilgi için aşağıdaki [özel sağlayıcılar](#custom-providers) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="9a906-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="9a906-168">Ara yazılım, sıkıştırma düzenlerini önceliklendirmek için istemci tarafından gönderildiğinde kalite değeri (qvalue, `q`) ağırlığa yeniden davranıyor.</span><span class="sxs-lookup"><span data-stu-id="9a906-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="9a906-169">Daha fazla bilgi için bkz. [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="9a906-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="9a906-170">Sıkıştırma algoritmaları, sıkıştırma hızı ve sıkıştırmanın verimliliği arasında bir zorunluluğunu getirir tabidir.</span><span class="sxs-lookup"><span data-stu-id="9a906-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="9a906-171">Bu bağlamdaki *verimlilik* , sıkıştırmadan sonra çıkışın boyutunu ifade eder.</span><span class="sxs-lookup"><span data-stu-id="9a906-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="9a906-172">En en uygun sıkıştırma, en *iyi* boyut ile elde edilir.</span><span class="sxs-lookup"><span data-stu-id="9a906-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="9a906-173">Sıkıştırılmış içerik isteme, gönderme, önbelleğe alma ve alma ile ilgili üstbilgiler aşağıdaki tabloda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9a906-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="9a906-174">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="9a906-174">Header</span></span>             | <span data-ttu-id="9a906-175">Rol</span><span class="sxs-lookup"><span data-stu-id="9a906-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="9a906-176">İstemci için kabul edilebilir içerik kodlama düzenlerini göstermek üzere istemciden sunucusuna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="9a906-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="9a906-177">Yük içindeki içeriğin kodlamasını göstermek için sunucudan istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="9a906-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="9a906-178">Sıkıştırma gerçekleştiğinde, yanıt sıkıştırıldığında gövde içeriği değiştiği için `Content-Length` üst bilgisi kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="9a906-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="9a906-179">Sıkıştırma gerçekleştiğinde, gövde içeriği değiştiğinden ve karma artık geçerli olmadığından `Content-MD5` üst bilgisi kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="9a906-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="9a906-180">İçeriğin MIME türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="9a906-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="9a906-181">Her yanıt `Content-Type`belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="9a906-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="9a906-182">Ara yazılım, yanıtın sıkıştırılıp sıkıştırılmadığını belirlemede bu değeri denetler.</span><span class="sxs-lookup"><span data-stu-id="9a906-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="9a906-183">Ara yazılım, kodlayamadığı bir dizi [varsayılan MIME türünü](#mime-types) belirtir, ancak MIME türlerini değiştirebilir veya ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9a906-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="9a906-184">Sunucu tarafından istemciler ve proxy 'ler için bir `Accept-Encoding` değeri ile gönderildiğinde, `Vary` üst bilgisi, isteğin `Accept-Encoding` üst bilgisinin değerine göre önbelleğe alma (değişiklik) gerektiğini istemci veya ara sunucuya bildirir.</span><span class="sxs-lookup"><span data-stu-id="9a906-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="9a906-185">`Vary: Accept-Encoding` üst bilgisiyle içerik döndürmesinin sonucu, hem sıkıştırılmış hem de sıkıştırılmamış yanıtların ayrı olarak önbelleğe alınıp alınmayadır.</span><span class="sxs-lookup"><span data-stu-id="9a906-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="9a906-186">[Örnek uygulamayla](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples)yanıt sıkıştırma ara yazılımı 'nın özelliklerini gezin.</span><span class="sxs-lookup"><span data-stu-id="9a906-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="9a906-187">Örnek şunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="9a906-187">The sample illustrates:</span></span>

* <span data-ttu-id="9a906-188">Gzip ve özel sıkıştırma sağlayıcıları kullanılarak uygulama yanıtlarının sıkıştırılması.</span><span class="sxs-lookup"><span data-stu-id="9a906-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="9a906-189">Sıkıştırma için varsayılan MIME türleri listesine MIME türü ekleme.</span><span class="sxs-lookup"><span data-stu-id="9a906-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="9a906-190">Paket</span><span class="sxs-lookup"><span data-stu-id="9a906-190">Package</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9a906-191">Yanıt sıkıştırma ara yazılımı, ASP.NET Core uygulamalarında örtük olarak bulunan [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="9a906-191">Response Compression Middleware is provided by the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package, which is implicitly included in ASP.NET Core apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9a906-192">Ara yazılımı bir projeye eklemek için Microsoft. [aspnetcore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paketini içeren [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)öğesine bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9a906-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="9a906-193">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9a906-193">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="9a906-194">Aşağıdaki kod, varsayılan MIME türleri ve sıkıştırma sağlayıcıları için yanıt sıkıştırma ara yazılımı 'nın nasıl etkinleştirileceğini gösterir ([Brotli](#brotli-compression-provider) ve [gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="9a906-194">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="9a906-195">Aşağıdaki kod, varsayılan MIME türleri ve [gzip sıkıştırma sağlayıcısı](#gzip-compression-provider)Için yanıt sıkıştırma ara yazılımını nasıl etkinleştireceğinizi göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="9a906-195">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

<span data-ttu-id="9a906-196">Notlar:</span><span class="sxs-lookup"><span data-stu-id="9a906-196">Notes:</span></span>

* <span data-ttu-id="9a906-197">`app.UseResponseCompression`, yanıtları sıkıştıran herhangi bir ara yazılım önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9a906-197">`app.UseResponseCompression` must be called before any middleware that compresses responses.</span></span> <span data-ttu-id="9a906-198">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index#middleware-order>.</span><span class="sxs-lookup"><span data-stu-id="9a906-198">For more information, see <xref:fundamentals/middleware/index#middleware-order>.</span></span>
* <span data-ttu-id="9a906-199">`Accept-Encoding` istek üst bilgisini ayarlamak ve yanıt üst bilgilerini, boyutunu ve gövdesini incelemek için [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)veya [Postman](https://www.getpostman.com/) gibi bir araç kullanın.</span><span class="sxs-lookup"><span data-stu-id="9a906-199">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="9a906-200">`Accept-Encoding` üst bilgisi olmadan örnek uygulamaya bir istek gönderir ve yanıtın sıkıştırılmamış olduğunu gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="9a906-200">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="9a906-201">`Content-Encoding` ve `Vary` üstbilgileri yanıt üzerinde yok.</span><span class="sxs-lookup"><span data-stu-id="9a906-201">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Accept-Encoding üst bilgisi olmadan bir isteğin sonucunu gösteren Fiddler penceresi.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="9a906-204">`Accept-Encoding: br` üstbilgisiyle örnek uygulamaya bir istek gönderir (Brotli Compression) ve yanıtın sıkıştırıldığını gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="9a906-204">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="9a906-205">`Content-Encoding` ve `Vary` üst bilgileri yanıtta bulunur.</span><span class="sxs-lookup"><span data-stu-id="9a906-205">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Accept-Encoding üst bilgisi ve br değeri olan bir isteğin sonucunu gösteren Fiddler penceresi.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="9a906-209">`Accept-Encoding: gzip` üst bilgisiyle örnek uygulamaya bir istek gönderir ve yanıtın sıkıştırıldığını gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="9a906-209">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="9a906-210">`Content-Encoding` ve `Vary` üst bilgileri yanıtta bulunur.</span><span class="sxs-lookup"><span data-stu-id="9a906-210">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Accept-Encoding üst bilgisi ve gzip değeri ile bir isteğin sonucunu gösteren Fiddler penceresi.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="9a906-214">Sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="9a906-214">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="9a906-215">Brotli sıkıştırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="9a906-215">Brotli Compression Provider</span></span>

<span data-ttu-id="9a906-216">[Brotli sıkıştırılmış veri biçimiyle](https://tools.ietf.org/html/rfc7932)yanıtları sıkıştırmak için <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> kullanın.</span><span class="sxs-lookup"><span data-stu-id="9a906-216">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="9a906-217"><xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>hiçbir sıkıştırma sağlayıcısı açıkça eklenmemişse:</span><span class="sxs-lookup"><span data-stu-id="9a906-217">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="9a906-218">Brotli sıkıştırma sağlayıcısı, [gzip sıkıştırma sağlayıcısıyla](#gzip-compression-provider)birlikte, varsayılan olarak sıkıştırma sağlayıcılarının dizisine eklenir.</span><span class="sxs-lookup"><span data-stu-id="9a906-218">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="9a906-219">Brotli sıkıştırılmış veri biçimi istemci tarafından desteklenerek sıkıştırma varsayılan olarak Brotli Compression.</span><span class="sxs-lookup"><span data-stu-id="9a906-219">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="9a906-220">Brotli istemci tarafından desteklenmiyorsa, istemci gzip sıkıştırmasını destekliyorsa sıkıştırma varsayılan olarak gzip olur.</span><span class="sxs-lookup"><span data-stu-id="9a906-220">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="9a906-221">Herhangi bir sıkıştırma sağlayıcısı açıkça eklendiğinde, Brotoli sıkıştırma sağlayıcısının eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="9a906-221">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="9a906-222">Sıkıştırma düzeyini <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="9a906-222">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="9a906-223">Brotli sıkıştırma sağlayıcısı, en hızlı sıkıştırma düzeyi ([CompressionLevel. en hızlı](xref:System.IO.Compression.CompressionLevel)) olarak varsayılan olarak en verimli sıkıştırmayı oluşturmayabilir.</span><span class="sxs-lookup"><span data-stu-id="9a906-223">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="9a906-224">En verimli sıkıştırma isteniyorsa, ara yazılımı en uygun sıkıştırma için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="9a906-224">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="9a906-225">Sıkıştırma düzeyi</span><span class="sxs-lookup"><span data-stu-id="9a906-225">Compression Level</span></span> | <span data-ttu-id="9a906-226">Açıklama</span><span class="sxs-lookup"><span data-stu-id="9a906-226">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="9a906-227">CompressionLevel. en hızlı</span><span class="sxs-lookup"><span data-stu-id="9a906-227">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="9a906-228">Elde edilen çıktı en iyi şekilde sıkıştırısa bile, sıkıştırma mümkün olduğunca hızlı tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="9a906-228">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="9a906-229">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="9a906-229">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="9a906-230">Sıkıştırma gerçekleştirilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="9a906-230">No compression should be performed.</span></span> |
| [<span data-ttu-id="9a906-231">CompressionLevel. optimum</span><span class="sxs-lookup"><span data-stu-id="9a906-231">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="9a906-232">Sıkıştırmanın tamamlanmasının daha uzun sürse bile yanıtlar en iyi şekilde sıkıştırılır.</span><span class="sxs-lookup"><span data-stu-id="9a906-232">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="9a906-233">Gzip sıkıştırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="9a906-233">Gzip Compression Provider</span></span>

<span data-ttu-id="9a906-234">[Gzip dosya biçimiyle](https://tools.ietf.org/html/rfc1952)yanıtları sıkıştırmak için <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> kullanın.</span><span class="sxs-lookup"><span data-stu-id="9a906-234">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="9a906-235"><xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>hiçbir sıkıştırma sağlayıcısı açıkça eklenmemişse:</span><span class="sxs-lookup"><span data-stu-id="9a906-235">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="9a906-236">Gzip sıkıştırma sağlayıcısı varsayılan olarak, [Brotli sıkıştırma sağlayıcısıyla](#brotli-compression-provider)birlikte sıkıştırma sağlayıcılarının dizisine eklenir.</span><span class="sxs-lookup"><span data-stu-id="9a906-236">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="9a906-237">Brotli sıkıştırılmış veri biçimi istemci tarafından desteklenerek sıkıştırma varsayılan olarak Brotli Compression.</span><span class="sxs-lookup"><span data-stu-id="9a906-237">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="9a906-238">Brotli istemci tarafından desteklenmiyorsa, istemci gzip sıkıştırmasını destekliyorsa sıkıştırma varsayılan olarak gzip olur.</span><span class="sxs-lookup"><span data-stu-id="9a906-238">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="9a906-239">Gzip sıkıştırma sağlayıcısı, varsayılan olarak sıkıştırma sağlayıcılarının dizisine eklenir.</span><span class="sxs-lookup"><span data-stu-id="9a906-239">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="9a906-240">İstemci gzip sıkıştırmasını destekliyorsa, sıkıştırma varsayılan olarak gzip olur.</span><span class="sxs-lookup"><span data-stu-id="9a906-240">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="9a906-241">Herhangi bir sıkıştırma sağlayıcısı açık olarak eklendiğinde gzip sıkıştırma sağlayıcısının eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="9a906-241">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

<span data-ttu-id="9a906-242">Sıkıştırma düzeyini <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="9a906-242">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="9a906-243">Gzip sıkıştırma sağlayıcısı, en hızlı sıkıştırma düzeyine ([CompressionLevel. en hızlı](xref:System.IO.Compression.CompressionLevel)) göre varsayılan olarak en verimli sıkıştırmayı oluşturmayabilir.</span><span class="sxs-lookup"><span data-stu-id="9a906-243">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="9a906-244">En verimli sıkıştırma isteniyorsa, ara yazılımı en uygun sıkıştırma için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="9a906-244">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="9a906-245">Sıkıştırma düzeyi</span><span class="sxs-lookup"><span data-stu-id="9a906-245">Compression Level</span></span> | <span data-ttu-id="9a906-246">Açıklama</span><span class="sxs-lookup"><span data-stu-id="9a906-246">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="9a906-247">CompressionLevel. en hızlı</span><span class="sxs-lookup"><span data-stu-id="9a906-247">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="9a906-248">Elde edilen çıktı en iyi şekilde sıkıştırısa bile, sıkıştırma mümkün olduğunca hızlı tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="9a906-248">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="9a906-249">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="9a906-249">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="9a906-250">Sıkıştırma gerçekleştirilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="9a906-250">No compression should be performed.</span></span> |
| [<span data-ttu-id="9a906-251">CompressionLevel. optimum</span><span class="sxs-lookup"><span data-stu-id="9a906-251">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="9a906-252">Sıkıştırmanın tamamlanmasının daha uzun sürse bile yanıtlar en iyi şekilde sıkıştırılır.</span><span class="sxs-lookup"><span data-stu-id="9a906-252">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="9a906-253">Özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="9a906-253">Custom providers</span></span>

<span data-ttu-id="9a906-254"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>ile özel sıkıştırma uygulamaları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9a906-254">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="9a906-255"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*>, bu `ICompressionProvider` ürettiği içerik kodlamasını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="9a906-255">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="9a906-256">Ara yazılım bu bilgileri, isteğin `Accept-Encoding` üstbilgisinde belirtilen listeye göre sağlayıcıyı seçmek üzere kullanır.</span><span class="sxs-lookup"><span data-stu-id="9a906-256">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="9a906-257">İstemci, örnek uygulamayı kullanarak `Accept-Encoding: mycustomcompression` üstbilgiyle bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="9a906-257">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="9a906-258">Ara yazılım özel sıkıştırma uygulamasını kullanır ve bir `Content-Encoding: mycustomcompression` üstbilgisiyle yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="9a906-258">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="9a906-259">Özel bir sıkıştırma uygulamasının çalışması için istemcinin özel kodlamayı açıp açabilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="9a906-259">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="9a906-260">`Accept-Encoding: mycustomcompression` üst bilgisiyle örnek uygulamaya bir istek gönderir ve yanıt üst bilgilerini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="9a906-260">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="9a906-261">`Vary` ve `Content-Encoding` üst bilgileri yanıtta bulunur.</span><span class="sxs-lookup"><span data-stu-id="9a906-261">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="9a906-262">Yanıt gövdesi (gösterilmez) örnek tarafından sıkıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="9a906-262">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="9a906-263">Örneğin `CustomCompressionProvider` sınıfında bir sıkıştırma uygulamanız yok.</span><span class="sxs-lookup"><span data-stu-id="9a906-263">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="9a906-264">Ancak örnek, bu tür bir sıkıştırma algoritmasını nerede uygulayacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="9a906-264">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Accept-Encoding üst bilgisi ve mycustomcompression değeri ile bir isteğin sonucunu gösteren Fiddler penceresi.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="9a906-267">MIME türleri</span><span class="sxs-lookup"><span data-stu-id="9a906-267">MIME types</span></span>

<span data-ttu-id="9a906-268">Ara yazılım, sıkıştırma için varsayılan bir MIME türleri kümesi belirtir:</span><span class="sxs-lookup"><span data-stu-id="9a906-268">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="9a906-269">MIME türlerini, yanıt sıkıştırma ara yazılım seçenekleriyle değiştirin veya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9a906-269">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="9a906-270">`text/*` gibi joker karakter MIME türlerinin desteklenmediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="9a906-270">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="9a906-271">Örnek uygulama, `image/svg+xml` için bir MIME türü ekler ve ASP.NET Core başlık görüntüsünü (*Banner. SVG*) sıkıştırır ve sunar.</span><span class="sxs-lookup"><span data-stu-id="9a906-271">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="9a906-272">Güvenli protokolle sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="9a906-272">Compression with secure protocol</span></span>

<span data-ttu-id="9a906-273">Güvenli bağlantılar üzerinden sıkıştırılan yanıtlar, varsayılan olarak devre dışı bırakılan `EnableForHttps` seçeneğiyle denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="9a906-273">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="9a906-274">Dinamik olarak oluşturulan sayfalarla sıkıştırma kullanmak, [SUA](https://wikipedia.org/wiki/CRIME_(security_exploit)) ve [ihlal](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları gibi güvenlik sorunlarına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="9a906-274">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="9a906-275">Vary üst bilgisi ekleniyor</span><span class="sxs-lookup"><span data-stu-id="9a906-275">Adding the Vary header</span></span>

<span data-ttu-id="9a906-276">`Accept-Encoding` üst bilgisine göre yanıtları sıkıştırılırken, büyük olasılıkla birden çok sıkıştırılmış yanıt ve sıkıştırılmamış bir sürüm vardır.</span><span class="sxs-lookup"><span data-stu-id="9a906-276">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="9a906-277">İstemci ve proxy 'nin birden çok sürümün var olduğunu ve depolanması gerektiğini bildirmek için, `Vary` üst bilgisi bir `Accept-Encoding` değeriyle eklenir.</span><span class="sxs-lookup"><span data-stu-id="9a906-277">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="9a906-278">ASP.NET Core 2,0 veya sonraki bir sürümde, yanıt sıkıştırıldığında ara yazılım `Vary` üst bilgisini otomatik olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="9a906-278">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="9a906-279">NGINX ters proxy 'nin arkasında ara yazılım sorunu</span><span class="sxs-lookup"><span data-stu-id="9a906-279">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="9a906-280">Bir istek NGINX tarafından proxy kullanılırken `Accept-Encoding` üst bilgisi kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="9a906-280">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="9a906-281">`Accept-Encoding` üst bilgisinin kaldırılması, ara yazılımın yanıtı sıkıştırmasını önler.</span><span class="sxs-lookup"><span data-stu-id="9a906-281">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="9a906-282">Daha fazla bilgi için bkz. [NGINX: sıkıştırma ve açma](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="9a906-282">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="9a906-283">Bu sorun, [NGINX için doğrudan sıkıştırma (ASPNET/Basicara yazılım #123)](https://github.com/aspnet/BasicMiddleware/issues/123)ile izlenebilir.</span><span class="sxs-lookup"><span data-stu-id="9a906-283">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="9a906-284">IIS dinamik sıkıştırma ile çalışma</span><span class="sxs-lookup"><span data-stu-id="9a906-284">Working with IIS dynamic compression</span></span>

<span data-ttu-id="9a906-285">Bir uygulama için devre dışı bırakmak istediğiniz sunucu düzeyinde yapılandırılmış etkin bir IIS dinamik sıkıştırma modülünüzün varsa, modülü *Web. config* dosyasına ek olarak devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="9a906-285">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="9a906-286">Daha fazla bilgi için bkz. [IIS modüllerini devre dışı bırakma](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="9a906-286">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="9a906-287">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="9a906-287">Troubleshooting</span></span>

<span data-ttu-id="9a906-288">[Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)veya [Postman](https://www.getpostman.com/)gibi bir araç kullanarak `Accept-Encoding` istek üst bilgisini ayarlamanıza ve yanıt üst bilgilerini, boyutunu ve gövdesini incelemeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="9a906-288">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="9a906-289">Varsayılan olarak, yanıt sıkıştırma ara yazılımı aşağıdaki koşullara uyan yanıtları sıkıştırır:</span><span class="sxs-lookup"><span data-stu-id="9a906-289">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="9a906-290">`Accept-Encoding` üst bilgisi, `br`, `gzip`, `*`veya oluşturduğunuz özel bir sıkıştırma sağlayıcısıyla eşleşen özel bir kodlama değeri ile birlikte bulunur.</span><span class="sxs-lookup"><span data-stu-id="9a906-290">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="9a906-291">Değer `identity` olmamalı ya da 0 (sıfır) olarak bir kalite değeri (qvalue, `q`) ayarı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9a906-291">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="9a906-292">MIME türü (`Content-Type`) ayarlanmalıdır ve <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>yapılandırılmış bir MIME türüyle eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="9a906-292">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="9a906-293">İstek `Content-Range` üstbilgisini içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="9a906-293">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="9a906-294">Yanıt sıkıştırma ara yazılımı seçeneklerinde güvenli Protokolü (https) yapılandırılmadığı takdirde istek güvenli olmayan protokol (http) kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9a906-294">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="9a906-295">*Güvenli içerik sıkıştırması etkinleştirildiğinde [yukarıda açıklanan](#compression-with-secure-protocol) tehlikede göz önünde yer.*</span><span class="sxs-lookup"><span data-stu-id="9a906-295">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="9a906-296">`Accept-Encoding` üst bilgisi, oluşturduğunuz özel bir sıkıştırma sağlayıcısıyla eşleşen `gzip`, `*`veya özel bir kodlama değeriyle birlikte bulunur.</span><span class="sxs-lookup"><span data-stu-id="9a906-296">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="9a906-297">Değer `identity` olmamalı ya da 0 (sıfır) olarak bir kalite değeri (qvalue, `q`) ayarı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9a906-297">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="9a906-298">MIME türü (`Content-Type`) ayarlanmalıdır ve <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>yapılandırılmış bir MIME türüyle eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="9a906-298">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="9a906-299">İstek `Content-Range` üstbilgisini içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="9a906-299">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="9a906-300">Yanıt sıkıştırma ara yazılımı seçeneklerinde güvenli Protokolü (https) yapılandırılmadığı takdirde istek güvenli olmayan protokol (http) kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9a906-300">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="9a906-301">*Güvenli içerik sıkıştırması etkinleştirildiğinde [yukarıda açıklanan](#compression-with-secure-protocol) tehlikede göz önünde yer.*</span><span class="sxs-lookup"><span data-stu-id="9a906-301">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9a906-302">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9a906-302">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="9a906-303">Mozilla Geliştirici Ağı: Accept-Encoding</span><span class="sxs-lookup"><span data-stu-id="9a906-303">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="9a906-304">RFC 7231 Bölüm 3.1.2.1: Içerik Işbirliği</span><span class="sxs-lookup"><span data-stu-id="9a906-304">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="9a906-305">RFC 7230 Section 4.2.3: gzip kodlaması</span><span class="sxs-lookup"><span data-stu-id="9a906-305">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="9a906-306">GZIP dosya biçimi belirtimi sürüm 4,3</span><span class="sxs-lookup"><span data-stu-id="9a906-306">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
