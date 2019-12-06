---
title: ASP.NET Core 'de yanıt sıkıştırması
author: guardrex
description: Yanıt sıkıştırması ve ASP.NET Core uygulamalarında yanıt sıkıştırma ara yazılımı kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: performance/response-compression
ms.openlocfilehash: 04b2ffd7047e8b127968adb5d40e0141365fb5fe
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880903"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="00a1c-103">ASP.NET Core 'de yanıt sıkıştırması</span><span class="sxs-lookup"><span data-stu-id="00a1c-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="00a1c-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="00a1c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="00a1c-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="00a1c-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="00a1c-106">Ağ bant genişliği sınırlı bir kaynaktır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="00a1c-107">Yanıt boyutunu azaltmak genellikle önemli ölçüde önemli ölçüde bir uygulamanın yanıt hızını artırır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="00a1c-108">Yük boyutlarını azaltmanın bir yolu, uygulamanın yanıtlarını sıkıştırmaktır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="00a1c-109">Yanıt sıkıştırma ara yazılımı ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="00a1c-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="00a1c-110">IIS, Apache veya NGINX 'te sunucu tabanlı yanıt sıkıştırma teknolojilerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="00a1c-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="00a1c-111">Ara yazılım performansı büyük olasılıkla sunucu modülleriyle eşleşmemelidir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="00a1c-112">[Http. sys Server](xref:fundamentals/servers/httpsys) Server ve [Kestrel](xref:fundamentals/servers/kestrel) Server şu anda yerleşik sıkıştırma desteği sunmaz.</span><span class="sxs-lookup"><span data-stu-id="00a1c-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="00a1c-113">Şu durumlarda yanıt sıkıştırma ara yazılımını kullanın:</span><span class="sxs-lookup"><span data-stu-id="00a1c-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="00a1c-114">Aşağıdaki sunucu tabanlı sıkıştırma teknolojileri kullanılamıyor:</span><span class="sxs-lookup"><span data-stu-id="00a1c-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="00a1c-115">IIS dinamik sıkıştırma modülü</span><span class="sxs-lookup"><span data-stu-id="00a1c-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="00a1c-116">Apache mod_deflate modülü</span><span class="sxs-lookup"><span data-stu-id="00a1c-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="00a1c-117">NGINX sıkıştırma ve açma</span><span class="sxs-lookup"><span data-stu-id="00a1c-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="00a1c-118">Doğrudan barındırma:</span><span class="sxs-lookup"><span data-stu-id="00a1c-118">Hosting directly on:</span></span>
  * <span data-ttu-id="00a1c-119">[Http. sys sunucusu](xref:fundamentals/servers/httpsys) (eski adıyla webListener)</span><span class="sxs-lookup"><span data-stu-id="00a1c-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="00a1c-120">Kestrel sunucusu</span><span class="sxs-lookup"><span data-stu-id="00a1c-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="00a1c-121">Yanıt sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="00a1c-121">Response compression</span></span>

<span data-ttu-id="00a1c-122">Genellikle, yerel olarak sıkıştırılmamış herhangi bir yanıt, yanıt sıkıştırmasından faydalanabilir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="00a1c-123">Yerel olarak sıkıştırılan yanıtlar genellikle şunlardır: CSS, JavaScript, HTML, XML ve JSON.</span><span class="sxs-lookup"><span data-stu-id="00a1c-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="00a1c-124">PNG dosyaları gibi yerel olarak sıkıştırılan varlıkları sıkıştırmamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="00a1c-125">Yerel olarak sıkıştırılan bir yanıtı daha fazla sıkıştırmaya çalışırsanız, boyut ve iletim süresi bakımından küçük bir ek azaltma büyük olasılıkla sıkıştırmayı işlemek için geçen süre kadar fazla gölgelenir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="00a1c-126">150-1000 bayttan daha küçük dosyaları sıkıştırmayın (dosyanın içeriğine ve sıkıştırma verimliliğine bağlı olarak).</span><span class="sxs-lookup"><span data-stu-id="00a1c-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="00a1c-127">Küçük dosyaları sıkıştırma ek yükü sıkıştırılmamış dosyadan daha büyük bir sıkıştırılmış dosya üretebilir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="00a1c-128">Bir istemci sıkıştırılmış içeriği işleyebilişlerde, istemci, `Accept-Encoding` üst bilgisini istekle göndererek yeteneklerini bilgilendirmelidir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="00a1c-129">Bir sunucu sıkıştırılmış içerik gönderdiğinde, sıkıştırılmış yanıtın kodlanmasının `Content-Encoding` üst bilgisine bilgi içermelidir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="00a1c-130">Ara yazılım tarafından desteklenen içerik kodlama göstergeleri aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="00a1c-131">üst bilgi değerlerini `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="00a1c-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="00a1c-132">Desteklenen ara yazılım</span><span class="sxs-lookup"><span data-stu-id="00a1c-132">Middleware Supported</span></span> | <span data-ttu-id="00a1c-133">Açıklama</span><span class="sxs-lookup"><span data-stu-id="00a1c-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="00a1c-134">Evet (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="00a1c-134">Yes (default)</span></span>        | [<span data-ttu-id="00a1c-135">Brotli sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="00a1c-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="00a1c-136">Hayır</span><span class="sxs-lookup"><span data-stu-id="00a1c-136">No</span></span>                   | [<span data-ttu-id="00a1c-137">Sıkıştırılmış veri biçimini söndür</span><span class="sxs-lookup"><span data-stu-id="00a1c-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="00a1c-138">Hayır</span><span class="sxs-lookup"><span data-stu-id="00a1c-138">No</span></span>                   | [<span data-ttu-id="00a1c-139">W3C verimli XML değişim</span><span class="sxs-lookup"><span data-stu-id="00a1c-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="00a1c-140">Evet</span><span class="sxs-lookup"><span data-stu-id="00a1c-140">Yes</span></span>                  | [<span data-ttu-id="00a1c-141">Gzip dosya biçimi</span><span class="sxs-lookup"><span data-stu-id="00a1c-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="00a1c-142">Evet</span><span class="sxs-lookup"><span data-stu-id="00a1c-142">Yes</span></span>                  | <span data-ttu-id="00a1c-143">"Kodlama yok" tanımlayıcısı: Yanıt kodlanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="00a1c-144">Hayır</span><span class="sxs-lookup"><span data-stu-id="00a1c-144">No</span></span>                   | [<span data-ttu-id="00a1c-145">Java arşivleri için ağ aktarım biçimi</span><span class="sxs-lookup"><span data-stu-id="00a1c-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="00a1c-146">Evet</span><span class="sxs-lookup"><span data-stu-id="00a1c-146">Yes</span></span>                  | <span data-ttu-id="00a1c-147">Tüm kullanılabilir içerik kodlamaları açıkça istenmedi</span><span class="sxs-lookup"><span data-stu-id="00a1c-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="00a1c-148">üst bilgi değerlerini `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="00a1c-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="00a1c-149">Desteklenen ara yazılım</span><span class="sxs-lookup"><span data-stu-id="00a1c-149">Middleware Supported</span></span> | <span data-ttu-id="00a1c-150">Açıklama</span><span class="sxs-lookup"><span data-stu-id="00a1c-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="00a1c-151">Hayır</span><span class="sxs-lookup"><span data-stu-id="00a1c-151">No</span></span>                   | [<span data-ttu-id="00a1c-152">Brotli sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="00a1c-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="00a1c-153">Hayır</span><span class="sxs-lookup"><span data-stu-id="00a1c-153">No</span></span>                   | [<span data-ttu-id="00a1c-154">Sıkıştırılmış veri biçimini söndür</span><span class="sxs-lookup"><span data-stu-id="00a1c-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="00a1c-155">Hayır</span><span class="sxs-lookup"><span data-stu-id="00a1c-155">No</span></span>                   | [<span data-ttu-id="00a1c-156">W3C verimli XML değişim</span><span class="sxs-lookup"><span data-stu-id="00a1c-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="00a1c-157">Evet (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="00a1c-157">Yes (default)</span></span>        | [<span data-ttu-id="00a1c-158">Gzip dosya biçimi</span><span class="sxs-lookup"><span data-stu-id="00a1c-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="00a1c-159">Evet</span><span class="sxs-lookup"><span data-stu-id="00a1c-159">Yes</span></span>                  | <span data-ttu-id="00a1c-160">"Kodlama yok" tanımlayıcısı: Yanıt kodlanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="00a1c-161">Hayır</span><span class="sxs-lookup"><span data-stu-id="00a1c-161">No</span></span>                   | [<span data-ttu-id="00a1c-162">Java arşivleri için ağ aktarım biçimi</span><span class="sxs-lookup"><span data-stu-id="00a1c-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="00a1c-163">Evet</span><span class="sxs-lookup"><span data-stu-id="00a1c-163">Yes</span></span>                  | <span data-ttu-id="00a1c-164">Tüm kullanılabilir içerik kodlamaları açıkça istenmedi</span><span class="sxs-lookup"><span data-stu-id="00a1c-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="00a1c-165">Daha fazla bilgi için, [IANA resmi Içerik kodlama listesine](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)bakın.</span><span class="sxs-lookup"><span data-stu-id="00a1c-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="00a1c-166">Ara yazılım, özel `Accept-Encoding` üst bilgi değerleri için ek sıkıştırma sağlayıcıları eklemenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="00a1c-167">Daha fazla bilgi için aşağıdaki [özel sağlayıcılar](#custom-providers) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="00a1c-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="00a1c-168">Ara yazılım, sıkıştırma düzenlerini önceliklendirmek için istemci tarafından gönderildiğinde kalite değeri (qvalue, `q`) ağırlığa yeniden davranıyor.</span><span class="sxs-lookup"><span data-stu-id="00a1c-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="00a1c-169">Daha fazla bilgi için bkz. [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="00a1c-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="00a1c-170">Sıkıştırma algoritmaları, sıkıştırma hızı ve sıkıştırmanın verimliliği arasında bir zorunluluğunu getirir tabidir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="00a1c-171">Bu bağlamdaki *verimlilik* , sıkıştırmadan sonra çıkışın boyutunu ifade eder.</span><span class="sxs-lookup"><span data-stu-id="00a1c-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="00a1c-172">En en uygun sıkıştırma, en *iyi* boyut ile elde edilir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="00a1c-173">Sıkıştırılmış içerik isteme, gönderme, önbelleğe alma ve alma ile ilgili üstbilgiler aşağıdaki tabloda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="00a1c-174">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="00a1c-174">Header</span></span>             | <span data-ttu-id="00a1c-175">Rol</span><span class="sxs-lookup"><span data-stu-id="00a1c-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="00a1c-176">İstemci için kabul edilebilir içerik kodlama düzenlerini göstermek üzere istemciden sunucusuna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="00a1c-177">Yük içindeki içeriğin kodlamasını göstermek için sunucudan istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="00a1c-178">Sıkıştırma gerçekleştiğinde, yanıt sıkıştırıldığında gövde içeriği değiştiği için `Content-Length` üst bilgisi kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="00a1c-179">Sıkıştırma gerçekleştiğinde, gövde içeriği değiştiğinden ve karma artık geçerli olmadığından `Content-MD5` üst bilgisi kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="00a1c-180">İçeriğin MIME türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="00a1c-181">Her yanıt `Content-Type`belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="00a1c-182">Ara yazılım, yanıtın sıkıştırılıp sıkıştırılmadığını belirlemede bu değeri denetler.</span><span class="sxs-lookup"><span data-stu-id="00a1c-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="00a1c-183">Ara yazılım, kodlayamadığı bir dizi [varsayılan MIME türünü](#mime-types) belirtir, ancak MIME türlerini değiştirebilir veya ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="00a1c-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="00a1c-184">Sunucu tarafından istemciler ve proxy 'ler için bir `Accept-Encoding` değeri ile gönderildiğinde, `Vary` üst bilgisi, isteğin `Accept-Encoding` üst bilgisinin değerine göre önbelleğe alma (değişiklik) gerektiğini istemci veya ara sunucuya bildirir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="00a1c-185">`Vary: Accept-Encoding` üst bilgisiyle içerik döndürmesinin sonucu, hem sıkıştırılmış hem de sıkıştırılmamış yanıtların ayrı olarak önbelleğe alınıp alınmayadır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="00a1c-186">[Örnek uygulamayla](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples)yanıt sıkıştırma ara yazılımı 'nın özelliklerini gezin.</span><span class="sxs-lookup"><span data-stu-id="00a1c-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="00a1c-187">Örnek şunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="00a1c-187">The sample illustrates:</span></span>

* <span data-ttu-id="00a1c-188">Gzip ve özel sıkıştırma sağlayıcıları kullanılarak uygulama yanıtlarının sıkıştırılması.</span><span class="sxs-lookup"><span data-stu-id="00a1c-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="00a1c-189">Sıkıştırma için varsayılan MIME türleri listesine MIME türü ekleme.</span><span class="sxs-lookup"><span data-stu-id="00a1c-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="00a1c-190">Paket</span><span class="sxs-lookup"><span data-stu-id="00a1c-190">Package</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="00a1c-191">Yanıt sıkıştırma ara yazılımı, ASP.NET Core uygulamalarında örtük olarak bulunan [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-191">Response Compression Middleware is provided by the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package, which is implicitly included in ASP.NET Core apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="00a1c-192">Ara yazılımı bir projeye eklemek için Microsoft. [aspnetcore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paketini içeren [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)öğesine bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="00a1c-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="00a1c-193">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="00a1c-193">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="00a1c-194">Aşağıdaki kod, varsayılan MIME türleri ve sıkıştırma sağlayıcıları için yanıt sıkıştırma ara yazılımı 'nın nasıl etkinleştirileceğini gösterir ([Brotli](#brotli-compression-provider) ve [gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="00a1c-194">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="00a1c-195">Aşağıdaki kod, varsayılan MIME türleri ve [gzip sıkıştırma sağlayıcısı](#gzip-compression-provider)Için yanıt sıkıştırma ara yazılımını nasıl etkinleştireceğinizi göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="00a1c-195">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

<span data-ttu-id="00a1c-196">Notlar:</span><span class="sxs-lookup"><span data-stu-id="00a1c-196">Notes:</span></span>

* <span data-ttu-id="00a1c-197">`app.UseResponseCompression`, `app.UseMvc`önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-197">`app.UseResponseCompression` must be called before `app.UseMvc`.</span></span>
* <span data-ttu-id="00a1c-198">`Accept-Encoding` istek üst bilgisini ayarlamak ve yanıt üst bilgilerini, boyutunu ve gövdesini incelemek için [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)veya [Postman](https://www.getpostman.com/) gibi bir araç kullanın.</span><span class="sxs-lookup"><span data-stu-id="00a1c-198">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="00a1c-199">`Accept-Encoding` üst bilgisi olmadan örnek uygulamaya bir istek gönderir ve yanıtın sıkıştırılmamış olduğunu gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="00a1c-199">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="00a1c-200">`Content-Encoding` ve `Vary` üstbilgileri yanıt üzerinde yok.</span><span class="sxs-lookup"><span data-stu-id="00a1c-200">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Accept-Encoding üst bilgisi olmadan bir isteğin sonucunu gösteren Fiddler penceresi.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="00a1c-203">`Accept-Encoding: br` üstbilgisiyle örnek uygulamaya bir istek gönderir (Brotli Compression) ve yanıtın sıkıştırıldığını gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="00a1c-203">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="00a1c-204">`Content-Encoding` ve `Vary` üst bilgileri yanıtta bulunur.</span><span class="sxs-lookup"><span data-stu-id="00a1c-204">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Accept-Encoding üst bilgisi ve br değeri olan bir isteğin sonucunu gösteren Fiddler penceresi.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="00a1c-208">`Accept-Encoding: gzip` üst bilgisiyle örnek uygulamaya bir istek gönderir ve yanıtın sıkıştırıldığını gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="00a1c-208">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="00a1c-209">`Content-Encoding` ve `Vary` üst bilgileri yanıtta bulunur.</span><span class="sxs-lookup"><span data-stu-id="00a1c-209">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Accept-Encoding üst bilgisi ve gzip değeri ile bir isteğin sonucunu gösteren Fiddler penceresi.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="00a1c-213">Sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="00a1c-213">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="00a1c-214">Brotli sıkıştırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="00a1c-214">Brotli Compression Provider</span></span>

<span data-ttu-id="00a1c-215">[Brotli sıkıştırılmış veri biçimiyle](https://tools.ietf.org/html/rfc7932)yanıtları sıkıştırmak için <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> kullanın.</span><span class="sxs-lookup"><span data-stu-id="00a1c-215">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="00a1c-216"><xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>hiçbir sıkıştırma sağlayıcısı açıkça eklenmemişse:</span><span class="sxs-lookup"><span data-stu-id="00a1c-216">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="00a1c-217">Brotli sıkıştırma sağlayıcısı, [gzip sıkıştırma sağlayıcısıyla](#gzip-compression-provider)birlikte, varsayılan olarak sıkıştırma sağlayıcılarının dizisine eklenir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-217">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="00a1c-218">Brotli sıkıştırılmış veri biçimi istemci tarafından desteklenerek sıkıştırma varsayılan olarak Brotli Compression.</span><span class="sxs-lookup"><span data-stu-id="00a1c-218">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="00a1c-219">Brotli istemci tarafından desteklenmiyorsa, istemci gzip sıkıştırmasını destekliyorsa sıkıştırma varsayılan olarak gzip olur.</span><span class="sxs-lookup"><span data-stu-id="00a1c-219">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="00a1c-220">Herhangi bir sıkıştırma sağlayıcısı açıkça eklendiğinde, Brotoli sıkıştırma sağlayıcısının eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="00a1c-220">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="00a1c-221">Sıkıştırma düzeyini <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="00a1c-221">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="00a1c-222">Brotli sıkıştırma sağlayıcısı, en hızlı sıkıştırma düzeyi ([CompressionLevel. en hızlı](xref:System.IO.Compression.CompressionLevel)) olarak varsayılan olarak en verimli sıkıştırmayı oluşturmayabilir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-222">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="00a1c-223">En verimli sıkıştırma isteniyorsa, ara yazılımı en uygun sıkıştırma için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="00a1c-223">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="00a1c-224">Sıkıştırma düzeyi</span><span class="sxs-lookup"><span data-stu-id="00a1c-224">Compression Level</span></span> | <span data-ttu-id="00a1c-225">Açıklama</span><span class="sxs-lookup"><span data-stu-id="00a1c-225">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="00a1c-226">CompressionLevel. en hızlı</span><span class="sxs-lookup"><span data-stu-id="00a1c-226">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="00a1c-227">Elde edilen çıktı en iyi şekilde sıkıştırısa bile, sıkıştırma mümkün olduğunca hızlı tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-227">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="00a1c-228">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="00a1c-228">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="00a1c-229">Sıkıştırma gerçekleştirilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-229">No compression should be performed.</span></span> |
| [<span data-ttu-id="00a1c-230">CompressionLevel. optimum</span><span class="sxs-lookup"><span data-stu-id="00a1c-230">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="00a1c-231">Sıkıştırmanın tamamlanmasının daha uzun sürse bile yanıtlar en iyi şekilde sıkıştırılır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-231">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="00a1c-232">Gzip sıkıştırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="00a1c-232">Gzip Compression Provider</span></span>

<span data-ttu-id="00a1c-233">[Gzip dosya biçimiyle](https://tools.ietf.org/html/rfc1952)yanıtları sıkıştırmak için <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> kullanın.</span><span class="sxs-lookup"><span data-stu-id="00a1c-233">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="00a1c-234"><xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>hiçbir sıkıştırma sağlayıcısı açıkça eklenmemişse:</span><span class="sxs-lookup"><span data-stu-id="00a1c-234">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="00a1c-235">Gzip sıkıştırma sağlayıcısı varsayılan olarak, [Brotli sıkıştırma sağlayıcısıyla](#brotli-compression-provider)birlikte sıkıştırma sağlayıcılarının dizisine eklenir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-235">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="00a1c-236">Brotli sıkıştırılmış veri biçimi istemci tarafından desteklenerek sıkıştırma varsayılan olarak Brotli Compression.</span><span class="sxs-lookup"><span data-stu-id="00a1c-236">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="00a1c-237">Brotli istemci tarafından desteklenmiyorsa, istemci gzip sıkıştırmasını destekliyorsa sıkıştırma varsayılan olarak gzip olur.</span><span class="sxs-lookup"><span data-stu-id="00a1c-237">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="00a1c-238">Gzip sıkıştırma sağlayıcısı, varsayılan olarak sıkıştırma sağlayıcılarının dizisine eklenir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-238">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="00a1c-239">İstemci gzip sıkıştırmasını destekliyorsa, sıkıştırma varsayılan olarak gzip olur.</span><span class="sxs-lookup"><span data-stu-id="00a1c-239">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="00a1c-240">Herhangi bir sıkıştırma sağlayıcısı açık olarak eklendiğinde gzip sıkıştırma sağlayıcısının eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="00a1c-240">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

<span data-ttu-id="00a1c-241">Sıkıştırma düzeyini <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="00a1c-241">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="00a1c-242">Gzip sıkıştırma sağlayıcısı, en hızlı sıkıştırma düzeyine ([CompressionLevel. en hızlı](xref:System.IO.Compression.CompressionLevel)) göre varsayılan olarak en verimli sıkıştırmayı oluşturmayabilir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-242">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="00a1c-243">En verimli sıkıştırma isteniyorsa, ara yazılımı en uygun sıkıştırma için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="00a1c-243">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="00a1c-244">Sıkıştırma düzeyi</span><span class="sxs-lookup"><span data-stu-id="00a1c-244">Compression Level</span></span> | <span data-ttu-id="00a1c-245">Açıklama</span><span class="sxs-lookup"><span data-stu-id="00a1c-245">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="00a1c-246">CompressionLevel. en hızlı</span><span class="sxs-lookup"><span data-stu-id="00a1c-246">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="00a1c-247">Elde edilen çıktı en iyi şekilde sıkıştırısa bile, sıkıştırma mümkün olduğunca hızlı tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-247">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="00a1c-248">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="00a1c-248">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="00a1c-249">Sıkıştırma gerçekleştirilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-249">No compression should be performed.</span></span> |
| [<span data-ttu-id="00a1c-250">CompressionLevel. optimum</span><span class="sxs-lookup"><span data-stu-id="00a1c-250">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="00a1c-251">Sıkıştırmanın tamamlanmasının daha uzun sürse bile yanıtlar en iyi şekilde sıkıştırılır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-251">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="00a1c-252">Özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="00a1c-252">Custom providers</span></span>

<span data-ttu-id="00a1c-253"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>ile özel sıkıştırma uygulamaları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="00a1c-253">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="00a1c-254"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*>, bu `ICompressionProvider` ürettiği içerik kodlamasını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="00a1c-254">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="00a1c-255">Ara yazılım bu bilgileri, isteğin `Accept-Encoding` üstbilgisinde belirtilen listeye göre sağlayıcıyı seçmek üzere kullanır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-255">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="00a1c-256">İstemci, örnek uygulamayı kullanarak `Accept-Encoding: mycustomcompression` üstbilgiyle bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-256">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="00a1c-257">Ara yazılım özel sıkıştırma uygulamasını kullanır ve bir `Content-Encoding: mycustomcompression` üstbilgisiyle yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="00a1c-257">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="00a1c-258">Özel bir sıkıştırma uygulamasının çalışması için istemcinin özel kodlamayı açıp açabilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-258">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="00a1c-259">`Accept-Encoding: mycustomcompression` üst bilgisiyle örnek uygulamaya bir istek gönderir ve yanıt üst bilgilerini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="00a1c-259">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="00a1c-260">`Vary` ve `Content-Encoding` üst bilgileri yanıtta bulunur.</span><span class="sxs-lookup"><span data-stu-id="00a1c-260">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="00a1c-261">Yanıt gövdesi (gösterilmez) örnek tarafından sıkıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="00a1c-261">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="00a1c-262">Örneğin `CustomCompressionProvider` sınıfında bir sıkıştırma uygulamanız yok.</span><span class="sxs-lookup"><span data-stu-id="00a1c-262">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="00a1c-263">Ancak örnek, bu tür bir sıkıştırma algoritmasını nerede uygulayacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-263">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Accept-Encoding üst bilgisi ve mycustomcompression değeri ile bir isteğin sonucunu gösteren Fiddler penceresi.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="00a1c-266">MIME türleri</span><span class="sxs-lookup"><span data-stu-id="00a1c-266">MIME types</span></span>

<span data-ttu-id="00a1c-267">Ara yazılım, sıkıştırma için varsayılan bir MIME türleri kümesi belirtir:</span><span class="sxs-lookup"><span data-stu-id="00a1c-267">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="00a1c-268">MIME türlerini, yanıt sıkıştırma ara yazılım seçenekleriyle değiştirin veya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="00a1c-268">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="00a1c-269">`text/*` gibi joker karakter MIME türlerinin desteklenmediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="00a1c-269">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="00a1c-270">Örnek uygulama, `image/svg+xml` için bir MIME türü ekler ve ASP.NET Core başlık görüntüsünü (*Banner. SVG*) sıkıştırır ve sunar.</span><span class="sxs-lookup"><span data-stu-id="00a1c-270">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="00a1c-271">Güvenli protokolle sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="00a1c-271">Compression with secure protocol</span></span>

<span data-ttu-id="00a1c-272">Güvenli bağlantılar üzerinden sıkıştırılan yanıtlar, varsayılan olarak devre dışı bırakılan `EnableForHttps` seçeneğiyle denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-272">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="00a1c-273">Dinamik olarak oluşturulan sayfalarla sıkıştırma kullanmak, [SUA](https://wikipedia.org/wiki/CRIME_(security_exploit)) ve [ihlal](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları gibi güvenlik sorunlarına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-273">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="00a1c-274">Vary üst bilgisi ekleniyor</span><span class="sxs-lookup"><span data-stu-id="00a1c-274">Adding the Vary header</span></span>

<span data-ttu-id="00a1c-275">`Accept-Encoding` üst bilgisine göre yanıtları sıkıştırılırken, büyük olasılıkla birden çok sıkıştırılmış yanıt ve sıkıştırılmamış bir sürüm vardır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-275">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="00a1c-276">İstemci ve proxy 'nin birden çok sürümün var olduğunu ve depolanması gerektiğini bildirmek için, `Vary` üst bilgisi bir `Accept-Encoding` değeriyle eklenir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-276">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="00a1c-277">ASP.NET Core 2,0 veya sonraki bir sürümde, yanıt sıkıştırıldığında ara yazılım `Vary` üst bilgisini otomatik olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="00a1c-277">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="00a1c-278">NGINX ters proxy 'nin arkasında ara yazılım sorunu</span><span class="sxs-lookup"><span data-stu-id="00a1c-278">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="00a1c-279">Bir istek NGINX tarafından proxy kullanılırken `Accept-Encoding` üst bilgisi kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-279">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="00a1c-280">`Accept-Encoding` üst bilgisinin kaldırılması, ara yazılımın yanıtı sıkıştırmasını önler.</span><span class="sxs-lookup"><span data-stu-id="00a1c-280">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="00a1c-281">Daha fazla bilgi için bkz. [NGINX: sıkıştırma ve açma](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="00a1c-281">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="00a1c-282">Bu sorun, [NGINX için doğrudan sıkıştırma (ASPNET/Basicara yazılım #123)](https://github.com/aspnet/BasicMiddleware/issues/123)ile izlenebilir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-282">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="00a1c-283">IIS dinamik sıkıştırma ile çalışma</span><span class="sxs-lookup"><span data-stu-id="00a1c-283">Working with IIS dynamic compression</span></span>

<span data-ttu-id="00a1c-284">Bir uygulama için devre dışı bırakmak istediğiniz sunucu düzeyinde yapılandırılmış etkin bir IIS dinamik sıkıştırma modülünüzün varsa, modülü *Web. config* dosyasına ek olarak devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="00a1c-284">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="00a1c-285">Daha fazla bilgi için bkz. [IIS modüllerini devre dışı bırakma](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="00a1c-285">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="00a1c-286">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="00a1c-286">Troubleshooting</span></span>

<span data-ttu-id="00a1c-287">[Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)veya [Postman](https://www.getpostman.com/)gibi bir araç kullanarak `Accept-Encoding` istek üst bilgisini ayarlamanıza ve yanıt üst bilgilerini, boyutunu ve gövdesini incelemeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-287">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="00a1c-288">Varsayılan olarak, yanıt sıkıştırma ara yazılımı aşağıdaki koşullara uyan yanıtları sıkıştırır:</span><span class="sxs-lookup"><span data-stu-id="00a1c-288">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="00a1c-289">`Accept-Encoding` üst bilgisi, `br`, `gzip`, `*`veya oluşturduğunuz özel bir sıkıştırma sağlayıcısıyla eşleşen özel bir kodlama değeri ile birlikte bulunur.</span><span class="sxs-lookup"><span data-stu-id="00a1c-289">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="00a1c-290">Değer `identity` olmamalı ya da 0 (sıfır) olarak bir kalite değeri (qvalue, `q`) ayarı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-290">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="00a1c-291">MIME türü (`Content-Type`) ayarlanmalıdır ve <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>yapılandırılmış bir MIME türüyle eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-291">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="00a1c-292">İstek `Content-Range` üstbilgisini içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-292">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="00a1c-293">Yanıt sıkıştırma ara yazılımı seçeneklerinde güvenli Protokolü (https) yapılandırılmadığı takdirde istek güvenli olmayan protokol (http) kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-293">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="00a1c-294">*Güvenli içerik sıkıştırması etkinleştirildiğinde [yukarıda açıklanan](#compression-with-secure-protocol) tehlikede göz önünde yer.*</span><span class="sxs-lookup"><span data-stu-id="00a1c-294">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="00a1c-295">`Accept-Encoding` üst bilgisi, oluşturduğunuz özel bir sıkıştırma sağlayıcısıyla eşleşen `gzip`, `*`veya özel bir kodlama değeriyle birlikte bulunur.</span><span class="sxs-lookup"><span data-stu-id="00a1c-295">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="00a1c-296">Değer `identity` olmamalı ya da 0 (sıfır) olarak bir kalite değeri (qvalue, `q`) ayarı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-296">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="00a1c-297">MIME türü (`Content-Type`) ayarlanmalıdır ve <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>yapılandırılmış bir MIME türüyle eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-297">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="00a1c-298">İstek `Content-Range` üstbilgisini içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="00a1c-298">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="00a1c-299">Yanıt sıkıştırma ara yazılımı seçeneklerinde güvenli Protokolü (https) yapılandırılmadığı takdirde istek güvenli olmayan protokol (http) kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="00a1c-299">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="00a1c-300">*Güvenli içerik sıkıştırması etkinleştirildiğinde [yukarıda açıklanan](#compression-with-secure-protocol) tehlikede göz önünde yer.*</span><span class="sxs-lookup"><span data-stu-id="00a1c-300">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="00a1c-301">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="00a1c-301">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="00a1c-302">Mozilla Geliştirici Ağı: Accept-Encoding</span><span class="sxs-lookup"><span data-stu-id="00a1c-302">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="00a1c-303">RFC 7231 Bölüm 3.1.2.1: Içerik Işbirliği</span><span class="sxs-lookup"><span data-stu-id="00a1c-303">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="00a1c-304">RFC 7230 Section 4.2.3: gzip kodlaması</span><span class="sxs-lookup"><span data-stu-id="00a1c-304">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="00a1c-305">GZIP dosya biçimi belirtimi sürüm 4,3</span><span class="sxs-lookup"><span data-stu-id="00a1c-305">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
