---
title: ASP.NET core'da yanıt sıkıştırma
author: guardrex
description: Yanıt sıkıştırma ve ASP.NET Core uygulamalarında yanıt sıkıştırma ara yazılımı kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: performance/response-compression
ms.openlocfilehash: 3a01c2d572c0026944347f736f9658a7872e6c35
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028290"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="72547-103">ASP.NET core'da yanıt sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="72547-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="72547-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="72547-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="72547-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="72547-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="72547-106">Ağ bant genişliği sınırlı bir kaynaktır.</span><span class="sxs-lookup"><span data-stu-id="72547-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="72547-107">Yanıt boyutu genellikle azaltma, uygulama yanıt verme hızını genellikle önemli ölçüde artırır.</span><span class="sxs-lookup"><span data-stu-id="72547-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="72547-108">Yük boyutları azaltmak için bir uygulamanın yanıtları sıkıştırma yoludur.</span><span class="sxs-lookup"><span data-stu-id="72547-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="72547-109">Yanıt sıkıştırma ara yazılımı kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="72547-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="72547-110">IIS, Apache veya Ngınx sunucu tabanlı yanıt sıkıştırma teknolojileri kullanın.</span><span class="sxs-lookup"><span data-stu-id="72547-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="72547-111">Ara yazılım performansını büyük olasılıkla, sunucu modüllerinin eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="72547-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="72547-112">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) ve [Kestrel](xref:fundamentals/servers/kestrel) şu anda yerleşik sıkıştırma desteği sunmamaktadır.</span><span class="sxs-lookup"><span data-stu-id="72547-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) and [Kestrel](xref:fundamentals/servers/kestrel) don't currently offer built-in compression support.</span></span>

<span data-ttu-id="72547-113">Yanıt sıkıştırma ara yazılımı, işiniz kullanın:</span><span class="sxs-lookup"><span data-stu-id="72547-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="72547-114">Aşağıdaki sunucu tabanlı sıkıştırma teknolojileri kullanılamıyor:</span><span class="sxs-lookup"><span data-stu-id="72547-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="72547-115">IIS dinamik sıkıştırması Modülü</span><span class="sxs-lookup"><span data-stu-id="72547-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="72547-116">Apache mod_deflate Modülü</span><span class="sxs-lookup"><span data-stu-id="72547-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="72547-117">Ngınx sıkıştırma ve açma</span><span class="sxs-lookup"><span data-stu-id="72547-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="72547-118">Doğrudan barındırma:</span><span class="sxs-lookup"><span data-stu-id="72547-118">Hosting directly on:</span></span>
  * <span data-ttu-id="72547-119">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıyla [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="72547-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * [<span data-ttu-id="72547-120">Kestrel</span><span class="sxs-lookup"><span data-stu-id="72547-120">Kestrel</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="72547-121">Yanıt sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="72547-121">Response compression</span></span>

<span data-ttu-id="72547-122">Genellikle, yerel olarak sıkıştırılmış herhangi bir yanıt, yanıt sıkıştırması arasında yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="72547-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="72547-123">Yerel olarak genellikle sıkıştırılmış yanıtlarını içerir: CSS, JavaScript, HTML, XML ve JSON.</span><span class="sxs-lookup"><span data-stu-id="72547-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="72547-124">PNG dosyaları gibi yerel olarak sıkıştırılmış varlıklar sıkıştırın olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="72547-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="72547-125">Daha fazla yerel olarak sıkıştırılmış bir yanıt sıkıştırma çalışırsanız, küçük bir ek boyut ve iletim zaman kısalma sıkıştırma işlemek için geçen zaman büyük olasılıkla overshadowed.</span><span class="sxs-lookup"><span data-stu-id="72547-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="72547-126">Yaklaşık 150-1000 bayt (bağlı olarak dosyanın içeriği ve sıkıştırma verimliliğini) küçük dosyaları sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="72547-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="72547-127">Küçük dosyalar sıkıştırılıyor yükünü bir sıkıştırılmış dosyayı sıkıştırılmamış dosyasından büyük üretebilir.</span><span class="sxs-lookup"><span data-stu-id="72547-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="72547-128">Sıkıştırılmış içerik işleme bir istemci, istemci yeteneklerini sunucusu göndererek bilgilendirmeniz `Accept-Encoding` istek üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="72547-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="72547-129">Bir sunucu sıkıştırılmış içerik gönderdiğinde bilgileri içermelidir `Content-Encoding` sıkıştırılmış yanıt nasıl kodlandığını üzerinde başlığı.</span><span class="sxs-lookup"><span data-stu-id="72547-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="72547-130">Ara yazılım tarafından desteklenen içerik kodlama gösterimleri aşağıdaki tabloda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="72547-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="72547-131">`Accept-Encoding` Üstbilgi değerleri</span><span class="sxs-lookup"><span data-stu-id="72547-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="72547-132">Desteklenen bir ara yazılım</span><span class="sxs-lookup"><span data-stu-id="72547-132">Middleware Supported</span></span> | <span data-ttu-id="72547-133">Açıklama</span><span class="sxs-lookup"><span data-stu-id="72547-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="72547-134">Evet (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="72547-134">Yes (default)</span></span>        | [<span data-ttu-id="72547-135">Brotli sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="72547-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="72547-136">Hayır</span><span class="sxs-lookup"><span data-stu-id="72547-136">No</span></span>                   | [<span data-ttu-id="72547-137">DEFLATE sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="72547-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="72547-138">Hayır</span><span class="sxs-lookup"><span data-stu-id="72547-138">No</span></span>                   | [<span data-ttu-id="72547-139">W3C XML verimli değişimi</span><span class="sxs-lookup"><span data-stu-id="72547-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="72547-140">Evet</span><span class="sxs-lookup"><span data-stu-id="72547-140">Yes</span></span>                  | [<span data-ttu-id="72547-141">Gzip dosya biçimi</span><span class="sxs-lookup"><span data-stu-id="72547-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="72547-142">Evet</span><span class="sxs-lookup"><span data-stu-id="72547-142">Yes</span></span>                  | <span data-ttu-id="72547-143">"Kodlaması" tanımlayıcısı: yanıt değil kodlanmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="72547-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="72547-144">Hayır</span><span class="sxs-lookup"><span data-stu-id="72547-144">No</span></span>                   | [<span data-ttu-id="72547-145">Ağ aktarım biçimi için Java arşivleri</span><span class="sxs-lookup"><span data-stu-id="72547-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="72547-146">Evet</span><span class="sxs-lookup"><span data-stu-id="72547-146">Yes</span></span>                  | <span data-ttu-id="72547-147">Kodlama açıkça istenen herhangi bir kullanılabilir içerik</span><span class="sxs-lookup"><span data-stu-id="72547-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="72547-148">`Accept-Encoding` Üstbilgi değerleri</span><span class="sxs-lookup"><span data-stu-id="72547-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="72547-149">Desteklenen bir ara yazılım</span><span class="sxs-lookup"><span data-stu-id="72547-149">Middleware Supported</span></span> | <span data-ttu-id="72547-150">Açıklama</span><span class="sxs-lookup"><span data-stu-id="72547-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="72547-151">Hayır</span><span class="sxs-lookup"><span data-stu-id="72547-151">No</span></span>                   | [<span data-ttu-id="72547-152">Brotli sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="72547-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="72547-153">Hayır</span><span class="sxs-lookup"><span data-stu-id="72547-153">No</span></span>                   | [<span data-ttu-id="72547-154">DEFLATE sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="72547-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="72547-155">Hayır</span><span class="sxs-lookup"><span data-stu-id="72547-155">No</span></span>                   | [<span data-ttu-id="72547-156">W3C XML verimli değişimi</span><span class="sxs-lookup"><span data-stu-id="72547-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="72547-157">Evet (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="72547-157">Yes (default)</span></span>        | [<span data-ttu-id="72547-158">Gzip dosya biçimi</span><span class="sxs-lookup"><span data-stu-id="72547-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="72547-159">Evet</span><span class="sxs-lookup"><span data-stu-id="72547-159">Yes</span></span>                  | <span data-ttu-id="72547-160">"Kodlaması" tanımlayıcısı: yanıt değil kodlanmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="72547-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="72547-161">Hayır</span><span class="sxs-lookup"><span data-stu-id="72547-161">No</span></span>                   | [<span data-ttu-id="72547-162">Ağ aktarım biçimi için Java arşivleri</span><span class="sxs-lookup"><span data-stu-id="72547-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="72547-163">Evet</span><span class="sxs-lookup"><span data-stu-id="72547-163">Yes</span></span>                  | <span data-ttu-id="72547-164">Kodlama açıkça istenen herhangi bir kullanılabilir içerik</span><span class="sxs-lookup"><span data-stu-id="72547-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="72547-165">Daha fazla bilgi için [IANA resmi içerik kodlama listesi](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="72547-165">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="72547-166">Ara yazılım özel ek sıkıştırmasını sağlayıcıları eklemenizi sağlayan `Accept-Encoding` üstbilgi değerleri.</span><span class="sxs-lookup"><span data-stu-id="72547-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="72547-167">Daha fazla bilgi için [özel sağlayıcılar](#custom-providers) aşağıda.</span><span class="sxs-lookup"><span data-stu-id="72547-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="72547-168">Ara yazılım kalite değeri tepki uyumlu (qvalue, `q`) sıkıştırma düzeni önceliğini belirlemek için istemci tarafından gönderilen ağırlığı.</span><span class="sxs-lookup"><span data-stu-id="72547-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="72547-169">Daha fazla bilgi için [RFC 7231: kabul kodlama](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="72547-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="72547-170">Sıkıştırma hız ve sıkıştırma verimliliğini arasında bir denge sıkıştırma algoritmaları tabidir.</span><span class="sxs-lookup"><span data-stu-id="72547-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="72547-171">*Verimliliği* bu bağlamda sıkıştırma sonrasında çıkış boyutunu ifade eder.</span><span class="sxs-lookup"><span data-stu-id="72547-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="72547-172">En küçük boyuta göre en sağlanır *en iyi* sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="72547-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="72547-173">Gönderme, önbelleğe alma ve sıkıştırılmış içerik almak isteyen ilgili üstbilgileri, aşağıdaki tabloda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="72547-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="72547-174">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="72547-174">Header</span></span>             | <span data-ttu-id="72547-175">Rol</span><span class="sxs-lookup"><span data-stu-id="72547-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="72547-176">İstemciden sunucuya istemcinin kabul edilebilir düzenleri kodlama içeriği belirtmek için gönderilen.</span><span class="sxs-lookup"><span data-stu-id="72547-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="72547-177">Sunucudan yükünde içerik kodlamasını belirtmek için istemciye gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="72547-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="72547-178">Sıkıştırma oluştuğunda `Content-Length` üstbilgi kaldırılır, yanıt sıkıştırılmışsa, gövde içeriği sonraki değişiklikler.</span><span class="sxs-lookup"><span data-stu-id="72547-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="72547-179">Sıkıştırma oluştuğunda `Content-MD5` üst bilgisi kaldırıldı, gövde içeriği değiştirildi ve karma artık geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="72547-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="72547-180">İçeriğin MIME türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="72547-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="72547-181">Her yanıt belirtmeniz gerekir, `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="72547-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="72547-182">Ara yazılımın yanıt sıkıştırılmış belirlemek için bu değer denetler.</span><span class="sxs-lookup"><span data-stu-id="72547-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="72547-183">Bir ara yazılım belirtir [MIME türleri varsayılan](#mime-types) kodlama, ancak değiştirin veya MIME türleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="72547-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="72547-184">Değerine sahip bir sunucu tarafından gönderilen `Accept-Encoding` proxy'leri ve istemcilere `Vary` üst bilgisi gösterir önbelleğe alması proxy ve istemci için (farklı) değerini temel alarak yanıtlarını `Accept-Encoding` isteği üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="72547-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="72547-185">İçerik döndüren sonucunu `Vary: Accept-Encoding` başlığıdır hem sıkıştırılmış ve sıkıştırılmamış yanıtları ayrı olarak önbelleğe.</span><span class="sxs-lookup"><span data-stu-id="72547-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="72547-186">Yanıt sıkıştırma ara yazılımı ile özelliklerini [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="72547-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="72547-187">Örnek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="72547-187">The sample illustrates:</span></span>

* <span data-ttu-id="72547-188">Gzip ve özel sıkıştırmasını sağlayıcıları kullanarak uygulama yanıtlarının sıkıştırılması.</span><span class="sxs-lookup"><span data-stu-id="72547-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="72547-189">Bir MIME türü sıkıştırma için MIME türleri varsayılan listeye ekleme.</span><span class="sxs-lookup"><span data-stu-id="72547-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="72547-190">Paket</span><span class="sxs-lookup"><span data-stu-id="72547-190">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="72547-191">Ara yazılım bir projeye dahil etmek için bir başvuru ekleyin. [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), içeren [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paket.</span><span class="sxs-lookup"><span data-stu-id="72547-191">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="72547-192">Ara yazılım bir projeye dahil etmek için bir başvuru ekleyin. [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), içeren [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paket.</span><span class="sxs-lookup"><span data-stu-id="72547-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="72547-193">Ara yazılım bir projeye dahil etmek için bir başvuru ekleyin. [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paket.</span><span class="sxs-lookup"><span data-stu-id="72547-193">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="72547-194">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="72547-194">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="72547-195">Aşağıdaki kod nasıl yanıt sıkıştırma ara yazılımı varsayılan MIME türleri ve sıkıştırmasını sağlayıcıları için etkinleştirileceğini gösterir ([Brotli](#brotli-compression-provider) ve [Gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="72547-195">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="72547-196">Aşağıdaki kod nasıl yanıt sıkıştırma ara yazılımı varsayılan MIME türleri için etkinleştirileceğini gösterir ve [Gzip sıkıştırma sağlayıcısı](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="72547-196">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

> [!NOTE]
> <span data-ttu-id="72547-197">Gibi bir araç kullanın [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), veya [Postman](https://www.getpostman.com/) ayarlanacak `Accept-Encoding` istek üst bilgisi ve yanıt üstbilgileri, boyutu ve gövde çalışın.</span><span class="sxs-lookup"><span data-stu-id="72547-197">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="72547-198">Örnek uygulamaya talebinizi `Accept-Encoding` üstbilgi ve yanıt sıkıştırılmamış olup olmadığına bakın.</span><span class="sxs-lookup"><span data-stu-id="72547-198">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="72547-199">`Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut değildir.</span><span class="sxs-lookup"><span data-stu-id="72547-199">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Accept-Encoding üst bilgisi olmayan bir isteğinin sonucunu gösteren fiddler penceresi.](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="72547-202">Örnek uygulamada talebinizi `Accept-Encoding: gzip` üstbilgi ve yanıt sıkıştırılmışsa gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="72547-202">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="72547-203">`Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut.</span><span class="sxs-lookup"><span data-stu-id="72547-203">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Accept-Encoding üst bilgisiyle bir isteğinin sonucunu ve gzip değerini gösteren fiddler penceresi.](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="72547-207">sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="72547-207">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="72547-208">Brotli sıkıştırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="72547-208">Brotli Compression Provider</span></span>

<span data-ttu-id="72547-209">Kullanım `BrotliCompressionProvider` ile yanıtları sıkıştırma [Brotli sıkıştırılmış veri biçimi](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="72547-209">Use the `BrotliCompressionProvider` to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="72547-210">Sıkıştırma yok sağlayıcıları için açıkça eklenirse <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="72547-210">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="72547-211">Brotli sıkıştırma sağlayıcısı sıkıştırmasını sağlayıcıları ile birlikte bir dizi varsayılan olarak eklenir [Gzip sıkıştırma sağlayıcısı](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="72547-211">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="72547-212">Sıkıştırma Brotli sıkıştırma için varsayılan olarak Brotli sıkıştırılmış veri biçimi, istemci tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="72547-212">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="72547-213">Gzip sıkıştırma istemcinin desteklediğinde sıkıştırma Brotli istemci tarafından desteklenmiyorsa, Gzip için varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="72547-213">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="72547-214">Tüm sıkıştırmasını sağlayıcıları açıkça eklendiğinde Brotoli sıkıştırma sağlayıcının eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="72547-214">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

<span data-ttu-id="72547-215">Sıkıştırma düzeyi ile ayarlanmış `BrotliCompressionProviderOptions`.</span><span class="sxs-lookup"><span data-stu-id="72547-215">Set the compression level with `BrotliCompressionProviderOptions`.</span></span> <span data-ttu-id="72547-216">Brotli sıkıştırma sağlayıcısı varsayılan olarak en hızlı sıkıştırma düzeyini ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), hangi değil üretebilir en verimli sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="72547-216">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="72547-217">En verimli sıkıştırma isterseniz, ara yazılım en iyi sıkıştırma için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="72547-217">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="72547-218">Sıkıştırma düzeyi</span><span class="sxs-lookup"><span data-stu-id="72547-218">Compression Level</span></span> | <span data-ttu-id="72547-219">Açıklama</span><span class="sxs-lookup"><span data-stu-id="72547-219">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="72547-220">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="72547-220">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="72547-221">Sıkıştırma, elde edilen çıkış en uygun şekilde sıkıştırılmaz olsa bile, mümkün olan en kısa sürede tamamlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="72547-221">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="72547-222">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="72547-222">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="72547-223">Sıkıştırma yok gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="72547-223">No compression should be performed.</span></span> |
| [<span data-ttu-id="72547-224">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="72547-224">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="72547-225">Sıkıştırma tamamlanması daha uzun sürer bile yanıtları en uygun şekilde, sıkıştırılmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="72547-225">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="72547-226">Gzip sıkıştırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="72547-226">Gzip Compression Provider</span></span>

<span data-ttu-id="72547-227">Kullanım <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> ile yanıtları sıkıştırma [Gzip dosya biçimi](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="72547-227">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="72547-228">Sıkıştırma yok sağlayıcıları için açıkça eklenirse <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="72547-228">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="72547-229">Gzip sıkıştırma sağlayıcısı sıkıştırmasını sağlayıcıları ile birlikte bir dizi varsayılan olarak eklenir [Brotli sıkıştırma sağlayıcısı](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="72547-229">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="72547-230">Sıkıştırma Brotli sıkıştırma için varsayılan olarak Brotli sıkıştırılmış veri biçimi, istemci tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="72547-230">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="72547-231">Gzip sıkıştırma istemcinin desteklediğinde sıkıştırma Brotli istemci tarafından desteklenmiyorsa, Gzip için varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="72547-231">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="72547-232">Gzip sıkıştırma sağlayıcısı sıkıştırmasını sağlayıcıları dizisi için varsayılan olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="72547-232">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="72547-233">Gzip sıkıştırma istemcinin desteklediğinde, Gzip sıkıştırma varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="72547-233">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="72547-234">Gzip sıkıştırma sağlayıcısı herhangi sıkıştırmasını sağlayıcıları açıkça eklendiğinde eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="72547-234">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

<span data-ttu-id="72547-235">Sıkıştırma düzeyi ile ayarlanmış <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="72547-235">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="72547-236">Gzip sıkıştırma sağlayıcısı varsayılan olarak en hızlı sıkıştırma düzeyini ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), hangi değil üretebilir en verimli sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="72547-236">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="72547-237">En verimli sıkıştırma isterseniz, ara yazılım en iyi sıkıştırma için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="72547-237">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="72547-238">Sıkıştırma düzeyi</span><span class="sxs-lookup"><span data-stu-id="72547-238">Compression Level</span></span> | <span data-ttu-id="72547-239">Açıklama</span><span class="sxs-lookup"><span data-stu-id="72547-239">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="72547-240">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="72547-240">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="72547-241">Sıkıştırma, elde edilen çıkış en uygun şekilde sıkıştırılmaz olsa bile, mümkün olan en kısa sürede tamamlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="72547-241">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="72547-242">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="72547-242">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="72547-243">Sıkıştırma yok gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="72547-243">No compression should be performed.</span></span> |
| [<span data-ttu-id="72547-244">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="72547-244">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="72547-245">Sıkıştırma tamamlanması daha uzun sürer bile yanıtları en uygun şekilde, sıkıştırılmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="72547-245">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="72547-246">Özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="72547-246">Custom providers</span></span>

<span data-ttu-id="72547-247">Özel sıkıştırma uygulamalarıyla oluşturma <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="72547-247">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="72547-248"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> Temsil eden bu kodlama içeriğe `ICompressionProvider` üretir.</span><span class="sxs-lookup"><span data-stu-id="72547-248">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="72547-249">Ara yazılım, belirtilen liste göre sağlayıcıyı seçmek için bu bilgileri kullanır. `Accept-Encoding` isteği üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="72547-249">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="72547-250">Örnek uygulama kullanma, istemci bir istek gönderir `Accept-Encoding: mycustomcompression` başlığı.</span><span class="sxs-lookup"><span data-stu-id="72547-250">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="72547-251">Ara yazılım özel sıkıştırma uygulaması kullanır ve yanıtı döndürür bir `Content-Encoding: mycustomcompression` başlığı.</span><span class="sxs-lookup"><span data-stu-id="72547-251">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="72547-252">İstemci, sırayla çalışmak özel sıkıştırma uygulaması için özel kodlama sıkıştırmasını mümkün olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="72547-252">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

```csharp
public class CustomCompressionProvider : ICompressionProvider
{
    public string EncodingName => "mycustomcompression";
    public bool SupportsFlush => true;

    public Stream CreateStream(Stream outputStream)
    {
        // Create a custom compression stream wrapper here
        return outputStream;
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="72547-253">Örnek uygulamada talebinizi `Accept-Encoding: mycustomcompression` üstbilgi ve yanıt üstbilgileri gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="72547-253">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="72547-254">`Vary` Ve `Content-Encoding` üstbilgileri yanıtta mevcut.</span><span class="sxs-lookup"><span data-stu-id="72547-254">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="72547-255">(Gösterilmemiştir) yanıt gövdesi olarak örnek sıkıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="72547-255">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="72547-256">Sıkıştırma uygulamasında hiç `CustomCompressionProvider` sınıfının örneği.</span><span class="sxs-lookup"><span data-stu-id="72547-256">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="72547-257">Ancak, bu tür bir sıkıştırma algoritması burada uygulamak örnek gösterir.</span><span class="sxs-lookup"><span data-stu-id="72547-257">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Accept-Encoding üst bilgisiyle bir isteğinin sonucunu ve mycustomcompression değerini gösteren fiddler penceresi.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="72547-260">MIME türleri</span><span class="sxs-lookup"><span data-stu-id="72547-260">MIME types</span></span>

<span data-ttu-id="72547-261">Ara yazılım bir varsayılan sıkıştırma için MIME türlerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="72547-261">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="72547-262">Yanıt sıkıştırma ara yazılımı seçenekleri MIME türleri ekleyin veya değiştirin.</span><span class="sxs-lookup"><span data-stu-id="72547-262">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="72547-263">Unutmayın, joker karakter MIME türleri gibi `text/*` desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="72547-263">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="72547-264">Örnek uygulama için bir MIME türü ekler `image/svg+xml` sıkıştırır ve ASP.NET Core hizmet başlık resmi (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="72547-264">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="72547-265">Güvenli protokol sıkıştırması</span><span class="sxs-lookup"><span data-stu-id="72547-265">Compression with secure protocol</span></span>

<span data-ttu-id="72547-266">Sıkıştırılmış yanıtlar güvenli bağlantılar üzerinden ile denetlenebilir `EnableForHttps` seçeneği, varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="72547-266">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="72547-267">Sıkıştırma ile dinamik olarak oluşturulan sayfaları kullanarak yol açabilir. güvenlik sorunları gibi [SUÇ](https://wikipedia.org/wiki/CRIME_(security_exploit)) ve [ihlal](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları.</span><span class="sxs-lookup"><span data-stu-id="72547-267">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="72547-268">Vary üstbilgi ekleme</span><span class="sxs-lookup"><span data-stu-id="72547-268">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="72547-269">Yanıtları sıkıştırma zaman temelinde `Accept-Encoding` üst bilgi, potansiyel olarak birden çok sıkıştırılmış sürümlerinin yanıtın ve sıkıştırılmamış bir vardır.</span><span class="sxs-lookup"><span data-stu-id="72547-269">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="72547-270">Birden çok sürümünün var ve depolanması gereken, istemci ve proxy önbellek isteyin için `Vary` üst bilgisi ile eklenen bir `Accept-Encoding` değeri.</span><span class="sxs-lookup"><span data-stu-id="72547-270">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="72547-271">ASP.NET Core 2.0 veya sonraki sürümlerde, ara yazılım ekler `Vary` otomatik olarak yanıt sıkıştırılmışsa, üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="72547-271">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="72547-272">Yanıtları sıkıştırma zaman temelinde `Accept-Encoding` üst bilgi, potansiyel olarak birden çok sıkıştırılmış sürümlerinin yanıtın ve sıkıştırılmamış bir vardır.</span><span class="sxs-lookup"><span data-stu-id="72547-272">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="72547-273">Birden çok sürümünün var ve depolanması gereken, istemci ve proxy önbellek isteyin için `Vary` üst bilgisi ile eklenen bir `Accept-Encoding` değeri.</span><span class="sxs-lookup"><span data-stu-id="72547-273">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="72547-274">İçinde ASP.NET Core 1.x, ekleme `Vary` üstbilgisi yanıt için el ile gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="72547-274">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="72547-275">Ters proxy arkasında olduğunda bir Ngınx ara yazılım sorunu</span><span class="sxs-lookup"><span data-stu-id="72547-275">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="72547-276">Bir isteği Ngınx tarafından proxy olduğunda `Accept-Encoding` üstbilgi kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="72547-276">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="72547-277">Bu, ara yazılımın yanıt sıkıştırmasını önler.</span><span class="sxs-lookup"><span data-stu-id="72547-277">This prevents the middleware from compressing the response.</span></span> <span data-ttu-id="72547-278">Daha fazla bilgi için [NGINX: sıkıştırma ve açma](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="72547-278">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="72547-279">Bu sorunu tarafından izlenen [Ngınx (BasicMiddleware #123) için doğrudan sıkıştırma ekleyeceğimi](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="72547-279">This issue is tracked by [Figure out pass-through compression for Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="72547-280">IIS dinamik sıkıştırması ile çalışma</span><span class="sxs-lookup"><span data-stu-id="72547-280">Working with IIS dynamic compression</span></span>

<span data-ttu-id="72547-281">Bir active IIS dinamik sıkıştırması bir uygulama için devre dışı bırakmak istediğiniz sunucu düzeyinde yapılandırılan modülü varsa, ek olarak modülle devre dışı *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="72547-281">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="72547-282">Daha fazla bilgi için [devre dışı bırakma IIS modüllerini](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="72547-282">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="72547-283">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="72547-283">Troubleshooting</span></span>

<span data-ttu-id="72547-284">Gibi bir araç kullanın [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), veya [Postman](https://www.getpostman.com/), ayarlamak izin `Accept-Encoding` istek üst bilgisi ve yanıt üstbilgileri, boyutu ve gövde çalışın.</span><span class="sxs-lookup"><span data-stu-id="72547-284">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="72547-285">Varsayılan olarak, aşağıdaki koşullara uyması yanıtları yanıt sıkıştırma ara yazılımı sıkıştırır:</span><span class="sxs-lookup"><span data-stu-id="72547-285">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="72547-286">`Accept-Encoding` Üst bilgi değeri olan mevcut `br`, `gzip`, `*`, veya sizin belirlediğiniz bir özel sıkıştırma sağlayıcısı ile eşleşen özel kodlama.</span><span class="sxs-lookup"><span data-stu-id="72547-286">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="72547-287">Değeri olmamalıdır `identity` veya bir kalite değeri (qvalue, `q`) 0 (sıfır) ayarlama.</span><span class="sxs-lookup"><span data-stu-id="72547-287">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="72547-288">MIME türü (`Content-Type`) ayarlanmalıdır ve yapılandırılmış bir MIME türü eşleşmelidir <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="72547-288">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="72547-289">İstek içermemelidir `Content-Range` başlığı.</span><span class="sxs-lookup"><span data-stu-id="72547-289">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="72547-290">İstek, yanıt sıkıştırma ara yazılımı seçenekleri güvenli Protokolü (https) yapılandırılmadığı sürece güvenli olmayan bir protokol (http) kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="72547-290">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="72547-291">*Tehlike Not [yukarıda açıklanan](#compression-with-secure-protocol) güvenli içerik sıkıştırmayı etkinleştirilirken.*</span><span class="sxs-lookup"><span data-stu-id="72547-291">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="72547-292">`Accept-Encoding` Üst bilgi değeri olan mevcut `gzip`, `*`, veya sizin belirlediğiniz bir özel sıkıştırma sağlayıcısı ile eşleşen özel kodlama.</span><span class="sxs-lookup"><span data-stu-id="72547-292">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="72547-293">Değeri olmamalıdır `identity` veya bir kalite değeri (qvalue, `q`) 0 (sıfır) ayarlama.</span><span class="sxs-lookup"><span data-stu-id="72547-293">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="72547-294">MIME türü (`Content-Type`) ayarlanmalıdır ve yapılandırılmış bir MIME türü eşleşmelidir <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="72547-294">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="72547-295">İstek içermemelidir `Content-Range` başlığı.</span><span class="sxs-lookup"><span data-stu-id="72547-295">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="72547-296">İstek, yanıt sıkıştırma ara yazılımı seçenekleri güvenli Protokolü (https) yapılandırılmadığı sürece güvenli olmayan bir protokol (http) kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="72547-296">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="72547-297">*Tehlike Not [yukarıda açıklanan](#compression-with-secure-protocol) güvenli içerik sıkıştırmayı etkinleştirilirken.*</span><span class="sxs-lookup"><span data-stu-id="72547-297">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="72547-298">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="72547-298">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="72547-299">Mozilla Geliştirici ağ: Kabul-Encoding</span><span class="sxs-lookup"><span data-stu-id="72547-299">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="72547-300">RFC 7231 bölüm 3.1.2.1: İçerik Codings</span><span class="sxs-lookup"><span data-stu-id="72547-300">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="72547-301">RFC 7230 bölüm 4.2.3: Gzip kodlama</span><span class="sxs-lookup"><span data-stu-id="72547-301">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="72547-302">GZIP dosyası biçim belirtimi sürümü 4.3</span><span class="sxs-lookup"><span data-stu-id="72547-302">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
