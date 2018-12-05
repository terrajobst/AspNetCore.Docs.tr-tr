---
title: ASP.NET core'da yanıt sıkıştırma
author: guardrex
description: Yanıt sıkıştırma ve ASP.NET Core uygulamalarında yanıt sıkıştırma ara yazılımı kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: performance/response-compression
ms.openlocfilehash: 2516fbb30e55990dc4ad0d92069853bc26874bc9
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861894"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="7de6d-103">ASP.NET core'da yanıt sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="7de6d-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="7de6d-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7de6d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7de6d-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7de6d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7de6d-106">Ağ bant genişliği sınırlı bir kaynaktır.</span><span class="sxs-lookup"><span data-stu-id="7de6d-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="7de6d-107">Yanıt boyutu genellikle azaltma, uygulama yanıt verme hızını genellikle önemli ölçüde artırır.</span><span class="sxs-lookup"><span data-stu-id="7de6d-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="7de6d-108">Yük boyutları azaltmak için bir uygulamanın yanıtları sıkıştırma yoludur.</span><span class="sxs-lookup"><span data-stu-id="7de6d-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="7de6d-109">Yanıt sıkıştırma ara yazılımı kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="7de6d-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="7de6d-110">IIS, Apache veya Ngınx sunucu tabanlı yanıt sıkıştırma teknolojileri kullanın.</span><span class="sxs-lookup"><span data-stu-id="7de6d-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="7de6d-111">Ara yazılım performansını büyük olasılıkla, sunucu modüllerinin eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="7de6d-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="7de6d-112">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) sunucu ve [Kestrel](xref:fundamentals/servers/kestrel) sunucusu şu anda yerleşik sıkıştırma desteği sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="7de6d-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="7de6d-113">Yanıt sıkıştırma ara yazılımı, işiniz kullanın:</span><span class="sxs-lookup"><span data-stu-id="7de6d-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="7de6d-114">Aşağıdaki sunucu tabanlı sıkıştırma teknolojileri kullanılamıyor:</span><span class="sxs-lookup"><span data-stu-id="7de6d-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="7de6d-115">IIS dinamik sıkıştırması Modülü</span><span class="sxs-lookup"><span data-stu-id="7de6d-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="7de6d-116">Apache mod_deflate Modülü</span><span class="sxs-lookup"><span data-stu-id="7de6d-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="7de6d-117">Ngınx sıkıştırma ve açma</span><span class="sxs-lookup"><span data-stu-id="7de6d-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="7de6d-118">Doğrudan barındırma:</span><span class="sxs-lookup"><span data-stu-id="7de6d-118">Hosting directly on:</span></span>
  * <span data-ttu-id="7de6d-119">[HTTP.sys](xref:fundamentals/servers/httpsys) sunucu (eski adıyla [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="7de6d-119">[HTTP.sys](xref:fundamentals/servers/httpsys) server (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * <span data-ttu-id="7de6d-120">[Kestrel'i](xref:fundamentals/servers/kestrel) sunucusu</span><span class="sxs-lookup"><span data-stu-id="7de6d-120">[Kestrel](xref:fundamentals/servers/kestrel) server</span></span>

## <a name="response-compression"></a><span data-ttu-id="7de6d-121">Yanıt sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="7de6d-121">Response compression</span></span>

<span data-ttu-id="7de6d-122">Genellikle, yerel olarak sıkıştırılmış herhangi bir yanıt, yanıt sıkıştırması arasında yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="7de6d-123">Yerel olarak genellikle sıkıştırılmış yanıtlarını içerir: CSS, JavaScript, HTML, XML ve JSON.</span><span class="sxs-lookup"><span data-stu-id="7de6d-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="7de6d-124">PNG dosyaları gibi yerel olarak sıkıştırılmış varlıklar sıkıştırın olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="7de6d-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="7de6d-125">Daha fazla yerel olarak sıkıştırılmış bir yanıt sıkıştırma çalışırsanız, küçük bir ek boyut ve iletim zaman kısalma sıkıştırma işlemek için geçen zaman büyük olasılıkla overshadowed.</span><span class="sxs-lookup"><span data-stu-id="7de6d-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="7de6d-126">Yaklaşık 150-1000 bayt (bağlı olarak dosyanın içeriği ve sıkıştırma verimliliğini) küçük dosyaları sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="7de6d-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="7de6d-127">Küçük dosyalar sıkıştırılıyor yükünü bir sıkıştırılmış dosyayı sıkıştırılmamış dosyasından büyük üretebilir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="7de6d-128">Sıkıştırılmış içerik işleme bir istemci, istemci yeteneklerini sunucusu göndererek bilgilendirmeniz `Accept-Encoding` istek üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="7de6d-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="7de6d-129">Bir sunucu sıkıştırılmış içerik gönderdiğinde bilgileri içermelidir `Content-Encoding` sıkıştırılmış yanıt nasıl kodlandığını üzerinde başlığı.</span><span class="sxs-lookup"><span data-stu-id="7de6d-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="7de6d-130">Ara yazılım tarafından desteklenen içerik kodlama gösterimleri aşağıdaki tabloda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="7de6d-131">`Accept-Encoding` Üstbilgi değerleri</span><span class="sxs-lookup"><span data-stu-id="7de6d-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="7de6d-132">Desteklenen bir ara yazılım</span><span class="sxs-lookup"><span data-stu-id="7de6d-132">Middleware Supported</span></span> | <span data-ttu-id="7de6d-133">Açıklama</span><span class="sxs-lookup"><span data-stu-id="7de6d-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="7de6d-134">Evet (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="7de6d-134">Yes (default)</span></span>        | [<span data-ttu-id="7de6d-135">Brotli sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="7de6d-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="7de6d-136">Hayır</span><span class="sxs-lookup"><span data-stu-id="7de6d-136">No</span></span>                   | [<span data-ttu-id="7de6d-137">DEFLATE sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="7de6d-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="7de6d-138">Hayır</span><span class="sxs-lookup"><span data-stu-id="7de6d-138">No</span></span>                   | [<span data-ttu-id="7de6d-139">W3C XML verimli değişimi</span><span class="sxs-lookup"><span data-stu-id="7de6d-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="7de6d-140">Evet</span><span class="sxs-lookup"><span data-stu-id="7de6d-140">Yes</span></span>                  | [<span data-ttu-id="7de6d-141">Gzip dosya biçimi</span><span class="sxs-lookup"><span data-stu-id="7de6d-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="7de6d-142">Evet</span><span class="sxs-lookup"><span data-stu-id="7de6d-142">Yes</span></span>                  | <span data-ttu-id="7de6d-143">"Kodlaması" tanımlayıcısı: yanıt değil kodlanmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="7de6d-144">Hayır</span><span class="sxs-lookup"><span data-stu-id="7de6d-144">No</span></span>                   | [<span data-ttu-id="7de6d-145">Ağ aktarım biçimi için Java arşivleri</span><span class="sxs-lookup"><span data-stu-id="7de6d-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="7de6d-146">Evet</span><span class="sxs-lookup"><span data-stu-id="7de6d-146">Yes</span></span>                  | <span data-ttu-id="7de6d-147">Kodlama açıkça istenen herhangi bir kullanılabilir içerik</span><span class="sxs-lookup"><span data-stu-id="7de6d-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="7de6d-148">`Accept-Encoding` Üstbilgi değerleri</span><span class="sxs-lookup"><span data-stu-id="7de6d-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="7de6d-149">Desteklenen bir ara yazılım</span><span class="sxs-lookup"><span data-stu-id="7de6d-149">Middleware Supported</span></span> | <span data-ttu-id="7de6d-150">Açıklama</span><span class="sxs-lookup"><span data-stu-id="7de6d-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="7de6d-151">Hayır</span><span class="sxs-lookup"><span data-stu-id="7de6d-151">No</span></span>                   | [<span data-ttu-id="7de6d-152">Brotli sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="7de6d-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="7de6d-153">Hayır</span><span class="sxs-lookup"><span data-stu-id="7de6d-153">No</span></span>                   | [<span data-ttu-id="7de6d-154">DEFLATE sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="7de6d-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="7de6d-155">Hayır</span><span class="sxs-lookup"><span data-stu-id="7de6d-155">No</span></span>                   | [<span data-ttu-id="7de6d-156">W3C XML verimli değişimi</span><span class="sxs-lookup"><span data-stu-id="7de6d-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="7de6d-157">Evet (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="7de6d-157">Yes (default)</span></span>        | [<span data-ttu-id="7de6d-158">Gzip dosya biçimi</span><span class="sxs-lookup"><span data-stu-id="7de6d-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="7de6d-159">Evet</span><span class="sxs-lookup"><span data-stu-id="7de6d-159">Yes</span></span>                  | <span data-ttu-id="7de6d-160">"Kodlaması" tanımlayıcısı: yanıt değil kodlanmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="7de6d-161">Hayır</span><span class="sxs-lookup"><span data-stu-id="7de6d-161">No</span></span>                   | [<span data-ttu-id="7de6d-162">Ağ aktarım biçimi için Java arşivleri</span><span class="sxs-lookup"><span data-stu-id="7de6d-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="7de6d-163">Evet</span><span class="sxs-lookup"><span data-stu-id="7de6d-163">Yes</span></span>                  | <span data-ttu-id="7de6d-164">Kodlama açıkça istenen herhangi bir kullanılabilir içerik</span><span class="sxs-lookup"><span data-stu-id="7de6d-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="7de6d-165">Daha fazla bilgi için [IANA resmi içerik kodlama listesi](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="7de6d-165">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="7de6d-166">Ara yazılım özel ek sıkıştırmasını sağlayıcıları eklemenizi sağlayan `Accept-Encoding` üstbilgi değerleri.</span><span class="sxs-lookup"><span data-stu-id="7de6d-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="7de6d-167">Daha fazla bilgi için [özel sağlayıcılar](#custom-providers) aşağıda.</span><span class="sxs-lookup"><span data-stu-id="7de6d-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="7de6d-168">Ara yazılım kalite değeri tepki uyumlu (qvalue, `q`) sıkıştırma düzeni önceliğini belirlemek için istemci tarafından gönderilen ağırlığı.</span><span class="sxs-lookup"><span data-stu-id="7de6d-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="7de6d-169">Daha fazla bilgi için [RFC 7231: kabul kodlama](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="7de6d-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="7de6d-170">Sıkıştırma hız ve sıkıştırma verimliliğini arasında bir denge sıkıştırma algoritmaları tabidir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="7de6d-171">*Verimliliği* bu bağlamda sıkıştırma sonrasında çıkış boyutunu ifade eder.</span><span class="sxs-lookup"><span data-stu-id="7de6d-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="7de6d-172">En küçük boyuta göre en sağlanır *en iyi* sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="7de6d-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="7de6d-173">Gönderme, önbelleğe alma ve sıkıştırılmış içerik almak isteyen ilgili üstbilgileri, aşağıdaki tabloda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="7de6d-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="7de6d-174">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="7de6d-174">Header</span></span>             | <span data-ttu-id="7de6d-175">Rol</span><span class="sxs-lookup"><span data-stu-id="7de6d-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="7de6d-176">İstemciden sunucuya istemcinin kabul edilebilir düzenleri kodlama içeriği belirtmek için gönderilen.</span><span class="sxs-lookup"><span data-stu-id="7de6d-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="7de6d-177">Sunucudan yükünde içerik kodlamasını belirtmek için istemciye gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="7de6d-178">Sıkıştırma oluştuğunda `Content-Length` üstbilgi kaldırılır, yanıt sıkıştırılmışsa, gövde içeriği sonraki değişiklikler.</span><span class="sxs-lookup"><span data-stu-id="7de6d-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="7de6d-179">Sıkıştırma oluştuğunda `Content-MD5` üst bilgisi kaldırıldı, gövde içeriği değiştirildi ve karma artık geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="7de6d-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="7de6d-180">İçeriğin MIME türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="7de6d-181">Her yanıt belirtmeniz gerekir, `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="7de6d-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="7de6d-182">Ara yazılımın yanıt sıkıştırılmış belirlemek için bu değer denetler.</span><span class="sxs-lookup"><span data-stu-id="7de6d-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="7de6d-183">Bir ara yazılım belirtir [MIME türleri varsayılan](#mime-types) kodlama, ancak değiştirin veya MIME türleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7de6d-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="7de6d-184">Değerine sahip bir sunucu tarafından gönderilen `Accept-Encoding` proxy'leri ve istemcilere `Vary` üst bilgisi gösterir önbelleğe alması proxy ve istemci için (farklı) değerini temel alarak yanıtlarını `Accept-Encoding` isteği üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="7de6d-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="7de6d-185">İçerik döndüren sonucunu `Vary: Accept-Encoding` başlığıdır hem sıkıştırılmış ve sıkıştırılmamış yanıtları ayrı olarak önbelleğe.</span><span class="sxs-lookup"><span data-stu-id="7de6d-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="7de6d-186">Yanıt sıkıştırma ara yazılımı ile özelliklerini [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="7de6d-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="7de6d-187">Örnek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="7de6d-187">The sample illustrates:</span></span>

* <span data-ttu-id="7de6d-188">Gzip ve özel sıkıştırmasını sağlayıcıları kullanarak uygulama yanıtlarının sıkıştırılması.</span><span class="sxs-lookup"><span data-stu-id="7de6d-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="7de6d-189">Bir MIME türü sıkıştırma için MIME türleri varsayılan listeye ekleme.</span><span class="sxs-lookup"><span data-stu-id="7de6d-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="7de6d-190">Paket</span><span class="sxs-lookup"><span data-stu-id="7de6d-190">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7de6d-191">Ara yazılım bir projeye dahil etmek için bir başvuru ekleyin. [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), içeren [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paket.</span><span class="sxs-lookup"><span data-stu-id="7de6d-191">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="7de6d-192">Ara yazılım bir projeye dahil etmek için bir başvuru ekleyin. [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), içeren [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paket.</span><span class="sxs-lookup"><span data-stu-id="7de6d-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="7de6d-193">Ara yazılım bir projeye dahil etmek için bir başvuru ekleyin. [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paket.</span><span class="sxs-lookup"><span data-stu-id="7de6d-193">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="7de6d-194">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7de6d-194">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7de6d-195">Aşağıdaki kod nasıl yanıt sıkıştırma ara yazılımı varsayılan MIME türleri ve sıkıştırmasını sağlayıcıları için etkinleştirileceğini gösterir ([Brotli](#brotli-compression-provider) ve [Gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="7de6d-195">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7de6d-196">Aşağıdaki kod nasıl yanıt sıkıştırma ara yazılımı varsayılan MIME türleri için etkinleştirileceğini gösterir ve [Gzip sıkıştırma sağlayıcısı](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="7de6d-196">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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
> <span data-ttu-id="7de6d-197">Gibi bir araç kullanın [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), veya [Postman](https://www.getpostman.com/) ayarlanacak `Accept-Encoding` istek üst bilgisi ve yanıt üstbilgileri, boyutu ve gövde çalışın.</span><span class="sxs-lookup"><span data-stu-id="7de6d-197">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="7de6d-198">Örnek uygulamaya talebinizi `Accept-Encoding` üstbilgi ve yanıt sıkıştırılmamış olup olmadığına bakın.</span><span class="sxs-lookup"><span data-stu-id="7de6d-198">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="7de6d-199">`Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut değildir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-199">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Accept-Encoding üst bilgisi olmayan bir isteğinin sonucunu gösteren fiddler penceresi.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7de6d-202">Örnek uygulamada talebinizi `Accept-Encoding: br` üst bilgisi (Brotli sıkıştırma) ve yanıt sıkıştırılmışsa gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="7de6d-202">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="7de6d-203">`Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut.</span><span class="sxs-lookup"><span data-stu-id="7de6d-203">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Accept-Encoding üst bilgisiyle bir isteğinin sonucunu ve br değerini gösteren fiddler penceresi.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7de6d-207">Örnek uygulamada talebinizi `Accept-Encoding: gzip` üstbilgi ve yanıt sıkıştırılmışsa gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="7de6d-207">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="7de6d-208">`Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut.</span><span class="sxs-lookup"><span data-stu-id="7de6d-208">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Accept-Encoding üst bilgisiyle bir isteğinin sonucunu ve gzip değerini gösteren fiddler penceresi.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="7de6d-212">sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="7de6d-212">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="7de6d-213">Brotli sıkıştırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="7de6d-213">Brotli Compression Provider</span></span>

<span data-ttu-id="7de6d-214">Kullanım `BrotliCompressionProvider` ile yanıtları sıkıştırma [Brotli sıkıştırılmış veri biçimi](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="7de6d-214">Use the `BrotliCompressionProvider` to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="7de6d-215">Sıkıştırma yok sağlayıcıları için açıkça eklenirse <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="7de6d-215">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="7de6d-216">Brotli sıkıştırma sağlayıcısı sıkıştırmasını sağlayıcıları ile birlikte bir dizi varsayılan olarak eklenir [Gzip sıkıştırma sağlayıcısı](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="7de6d-216">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="7de6d-217">Sıkıştırma Brotli sıkıştırma için varsayılan olarak Brotli sıkıştırılmış veri biçimi, istemci tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-217">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="7de6d-218">Gzip sıkıştırma istemcinin desteklediğinde sıkıştırma Brotli istemci tarafından desteklenmiyorsa, Gzip için varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="7de6d-218">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="7de6d-219">Tüm sıkıştırmasını sağlayıcıları açıkça eklendiğinde Brotoli sıkıştırma sağlayıcının eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="7de6d-219">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

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

<span data-ttu-id="7de6d-220">Sıkıştırma düzeyi ile ayarlanmış `BrotliCompressionProviderOptions`.</span><span class="sxs-lookup"><span data-stu-id="7de6d-220">Set the compression level with `BrotliCompressionProviderOptions`.</span></span> <span data-ttu-id="7de6d-221">Brotli sıkıştırma sağlayıcısı varsayılan olarak en hızlı sıkıştırma düzeyini ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), hangi değil üretebilir en verimli sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="7de6d-221">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="7de6d-222">En verimli sıkıştırma isterseniz, ara yazılım en iyi sıkıştırma için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="7de6d-222">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="7de6d-223">Sıkıştırma düzeyi</span><span class="sxs-lookup"><span data-stu-id="7de6d-223">Compression Level</span></span> | <span data-ttu-id="7de6d-224">Açıklama</span><span class="sxs-lookup"><span data-stu-id="7de6d-224">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="7de6d-225">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="7de6d-225">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7de6d-226">Sıkıştırma, elde edilen çıkış en uygun şekilde sıkıştırılmaz olsa bile, mümkün olan en kısa sürede tamamlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-226">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="7de6d-227">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="7de6d-227">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7de6d-228">Sıkıştırma yok gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-228">No compression should be performed.</span></span> |
| [<span data-ttu-id="7de6d-229">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="7de6d-229">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7de6d-230">Sıkıştırma tamamlanması daha uzun sürer bile yanıtları en uygun şekilde, sıkıştırılmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-230">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="7de6d-231">Gzip sıkıştırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="7de6d-231">Gzip Compression Provider</span></span>

<span data-ttu-id="7de6d-232">Kullanım <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> ile yanıtları sıkıştırma [Gzip dosya biçimi](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="7de6d-232">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="7de6d-233">Sıkıştırma yok sağlayıcıları için açıkça eklenirse <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="7de6d-233">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="7de6d-234">Gzip sıkıştırma sağlayıcısı sıkıştırmasını sağlayıcıları ile birlikte bir dizi varsayılan olarak eklenir [Brotli sıkıştırma sağlayıcısı](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="7de6d-234">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="7de6d-235">Sıkıştırma Brotli sıkıştırma için varsayılan olarak Brotli sıkıştırılmış veri biçimi, istemci tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-235">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="7de6d-236">Gzip sıkıştırma istemcinin desteklediğinde sıkıştırma Brotli istemci tarafından desteklenmiyorsa, Gzip için varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="7de6d-236">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="7de6d-237">Gzip sıkıştırma sağlayıcısı sıkıştırmasını sağlayıcıları dizisi için varsayılan olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-237">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="7de6d-238">Gzip sıkıştırma istemcinin desteklediğinde, Gzip sıkıştırma varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="7de6d-238">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="7de6d-239">Gzip sıkıştırma sağlayıcısı herhangi sıkıştırmasını sağlayıcıları açıkça eklendiğinde eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="7de6d-239">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

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

<span data-ttu-id="7de6d-240">Sıkıştırma düzeyi ile ayarlanmış <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="7de6d-240">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="7de6d-241">Gzip sıkıştırma sağlayıcısı varsayılan olarak en hızlı sıkıştırma düzeyini ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), hangi değil üretebilir en verimli sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="7de6d-241">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="7de6d-242">En verimli sıkıştırma isterseniz, ara yazılım en iyi sıkıştırma için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="7de6d-242">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="7de6d-243">Sıkıştırma düzeyi</span><span class="sxs-lookup"><span data-stu-id="7de6d-243">Compression Level</span></span> | <span data-ttu-id="7de6d-244">Açıklama</span><span class="sxs-lookup"><span data-stu-id="7de6d-244">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="7de6d-245">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="7de6d-245">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7de6d-246">Sıkıştırma, elde edilen çıkış en uygun şekilde sıkıştırılmaz olsa bile, mümkün olan en kısa sürede tamamlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-246">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="7de6d-247">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="7de6d-247">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7de6d-248">Sıkıştırma yok gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-248">No compression should be performed.</span></span> |
| [<span data-ttu-id="7de6d-249">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="7de6d-249">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7de6d-250">Sıkıştırma tamamlanması daha uzun sürer bile yanıtları en uygun şekilde, sıkıştırılmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-250">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="7de6d-251">Özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="7de6d-251">Custom providers</span></span>

<span data-ttu-id="7de6d-252">Özel sıkıştırma uygulamalarıyla oluşturma <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="7de6d-252">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="7de6d-253"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> Temsil eden bu kodlama içeriğe `ICompressionProvider` üretir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-253">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="7de6d-254">Ara yazılım, belirtilen liste göre sağlayıcıyı seçmek için bu bilgileri kullanır. `Accept-Encoding` isteği üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="7de6d-254">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="7de6d-255">Örnek uygulama kullanma, istemci bir istek gönderir `Accept-Encoding: mycustomcompression` başlığı.</span><span class="sxs-lookup"><span data-stu-id="7de6d-255">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="7de6d-256">Ara yazılım özel sıkıştırma uygulaması kullanır ve yanıtı döndürür bir `Content-Encoding: mycustomcompression` başlığı.</span><span class="sxs-lookup"><span data-stu-id="7de6d-256">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="7de6d-257">İstemci, sırayla çalışmak özel sıkıştırma uygulaması için özel kodlama sıkıştırmasını mümkün olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-257">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

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

<span data-ttu-id="7de6d-258">Örnek uygulamada talebinizi `Accept-Encoding: mycustomcompression` üstbilgi ve yanıt üstbilgileri gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="7de6d-258">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="7de6d-259">`Vary` Ve `Content-Encoding` üstbilgileri yanıtta mevcut.</span><span class="sxs-lookup"><span data-stu-id="7de6d-259">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="7de6d-260">(Gösterilmemiştir) yanıt gövdesi olarak örnek sıkıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="7de6d-260">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="7de6d-261">Sıkıştırma uygulamasında hiç `CustomCompressionProvider` sınıfının örneği.</span><span class="sxs-lookup"><span data-stu-id="7de6d-261">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="7de6d-262">Ancak, bu tür bir sıkıştırma algoritması burada uygulamak örnek gösterir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-262">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Accept-Encoding üst bilgisiyle bir isteğinin sonucunu ve mycustomcompression değerini gösteren fiddler penceresi.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="7de6d-265">MIME türleri</span><span class="sxs-lookup"><span data-stu-id="7de6d-265">MIME types</span></span>

<span data-ttu-id="7de6d-266">Ara yazılım bir varsayılan sıkıştırma için MIME türlerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="7de6d-266">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="7de6d-267">Yanıt sıkıştırma ara yazılımı seçenekleri MIME türleri ekleyin veya değiştirin.</span><span class="sxs-lookup"><span data-stu-id="7de6d-267">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="7de6d-268">Unutmayın, joker karakter MIME türleri gibi `text/*` desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="7de6d-268">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="7de6d-269">Örnek uygulama için bir MIME türü ekler `image/svg+xml` sıkıştırır ve ASP.NET Core hizmet başlık resmi (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="7de6d-269">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

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

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="7de6d-270">Güvenli protokol sıkıştırması</span><span class="sxs-lookup"><span data-stu-id="7de6d-270">Compression with secure protocol</span></span>

<span data-ttu-id="7de6d-271">Sıkıştırılmış yanıtlar güvenli bağlantılar üzerinden ile denetlenebilir `EnableForHttps` seçeneği, varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="7de6d-271">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="7de6d-272">Sıkıştırma ile dinamik olarak oluşturulan sayfaları kullanarak yol açabilir. güvenlik sorunları gibi [SUÇ](https://wikipedia.org/wiki/CRIME_(security_exploit)) ve [ihlal](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları.</span><span class="sxs-lookup"><span data-stu-id="7de6d-272">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="7de6d-273">Vary üstbilgi ekleme</span><span class="sxs-lookup"><span data-stu-id="7de6d-273">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="7de6d-274">Yanıtları sıkıştırma zaman temelinde `Accept-Encoding` üst bilgi, potansiyel olarak birden çok sıkıştırılmış sürümlerinin yanıtın ve sıkıştırılmamış bir vardır.</span><span class="sxs-lookup"><span data-stu-id="7de6d-274">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="7de6d-275">Birden çok sürümünün var ve depolanması gereken, istemci ve proxy önbellek isteyin için `Vary` üst bilgisi ile eklenen bir `Accept-Encoding` değeri.</span><span class="sxs-lookup"><span data-stu-id="7de6d-275">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="7de6d-276">ASP.NET Core 2.0 veya sonraki sürümlerde, ara yazılım ekler `Vary` otomatik olarak yanıt sıkıştırılmışsa, üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="7de6d-276">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="7de6d-277">Yanıtları sıkıştırma zaman temelinde `Accept-Encoding` üst bilgi, potansiyel olarak birden çok sıkıştırılmış sürümlerinin yanıtın ve sıkıştırılmamış bir vardır.</span><span class="sxs-lookup"><span data-stu-id="7de6d-277">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="7de6d-278">Birden çok sürümünün var ve depolanması gereken, istemci ve proxy önbellek isteyin için `Vary` üst bilgisi ile eklenen bir `Accept-Encoding` değeri.</span><span class="sxs-lookup"><span data-stu-id="7de6d-278">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="7de6d-279">İçinde ASP.NET Core 1.x, ekleme `Vary` üstbilgisi yanıt için el ile gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="7de6d-279">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="7de6d-280">Ters proxy arkasında olduğunda bir Ngınx ara yazılım sorunu</span><span class="sxs-lookup"><span data-stu-id="7de6d-280">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="7de6d-281">Bir isteği Ngınx tarafından proxy olduğunda `Accept-Encoding` üstbilgi kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="7de6d-281">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="7de6d-282">Kaldırılmasını `Accept-Encoding` üst bilgi yanıtı sıkıştırmasını ara yazılım engeller.</span><span class="sxs-lookup"><span data-stu-id="7de6d-282">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="7de6d-283">Daha fazla bilgi için [NGINX: sıkıştırma ve açma](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="7de6d-283">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="7de6d-284">Bu sorunu tarafından izlenen [Nginx için doğrudan sıkıştırma ekleyeceğimi (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="7de6d-284">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="7de6d-285">IIS dinamik sıkıştırması ile çalışma</span><span class="sxs-lookup"><span data-stu-id="7de6d-285">Working with IIS dynamic compression</span></span>

<span data-ttu-id="7de6d-286">Bir active IIS dinamik sıkıştırması bir uygulama için devre dışı bırakmak istediğiniz sunucu düzeyinde yapılandırılan modülü varsa, ek olarak modülle devre dışı *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="7de6d-286">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="7de6d-287">Daha fazla bilgi için [devre dışı bırakma IIS modüllerini](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="7de6d-287">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7de6d-288">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="7de6d-288">Troubleshooting</span></span>

<span data-ttu-id="7de6d-289">Gibi bir araç kullanın [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), veya [Postman](https://www.getpostman.com/), ayarlamak izin `Accept-Encoding` istek üst bilgisi ve yanıt üstbilgileri, boyutu ve gövde çalışın.</span><span class="sxs-lookup"><span data-stu-id="7de6d-289">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="7de6d-290">Varsayılan olarak, aşağıdaki koşullara uyması yanıtları yanıt sıkıştırma ara yazılımı sıkıştırır:</span><span class="sxs-lookup"><span data-stu-id="7de6d-290">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="7de6d-291">`Accept-Encoding` Üst bilgi değeri olan mevcut `br`, `gzip`, `*`, veya sizin belirlediğiniz bir özel sıkıştırma sağlayıcısı ile eşleşen özel kodlama.</span><span class="sxs-lookup"><span data-stu-id="7de6d-291">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="7de6d-292">Değeri olmamalıdır `identity` veya bir kalite değeri (qvalue, `q`) 0 (sıfır) ayarlama.</span><span class="sxs-lookup"><span data-stu-id="7de6d-292">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="7de6d-293">MIME türü (`Content-Type`) ayarlanmalıdır ve yapılandırılmış bir MIME türü eşleşmelidir <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="7de6d-293">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="7de6d-294">İstek içermemelidir `Content-Range` başlığı.</span><span class="sxs-lookup"><span data-stu-id="7de6d-294">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="7de6d-295">İstek, yanıt sıkıştırma ara yazılımı seçenekleri güvenli Protokolü (https) yapılandırılmadığı sürece güvenli olmayan bir protokol (http) kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-295">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="7de6d-296">*Tehlike Not [yukarıda açıklanan](#compression-with-secure-protocol) güvenli içerik sıkıştırmayı etkinleştirilirken.*</span><span class="sxs-lookup"><span data-stu-id="7de6d-296">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="7de6d-297">`Accept-Encoding` Üst bilgi değeri olan mevcut `gzip`, `*`, veya sizin belirlediğiniz bir özel sıkıştırma sağlayıcısı ile eşleşen özel kodlama.</span><span class="sxs-lookup"><span data-stu-id="7de6d-297">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="7de6d-298">Değeri olmamalıdır `identity` veya bir kalite değeri (qvalue, `q`) 0 (sıfır) ayarlama.</span><span class="sxs-lookup"><span data-stu-id="7de6d-298">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="7de6d-299">MIME türü (`Content-Type`) ayarlanmalıdır ve yapılandırılmış bir MIME türü eşleşmelidir <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="7de6d-299">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="7de6d-300">İstek içermemelidir `Content-Range` başlığı.</span><span class="sxs-lookup"><span data-stu-id="7de6d-300">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="7de6d-301">İstek, yanıt sıkıştırma ara yazılımı seçenekleri güvenli Protokolü (https) yapılandırılmadığı sürece güvenli olmayan bir protokol (http) kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7de6d-301">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="7de6d-302">*Tehlike Not [yukarıda açıklanan](#compression-with-secure-protocol) güvenli içerik sıkıştırmayı etkinleştirilirken.*</span><span class="sxs-lookup"><span data-stu-id="7de6d-302">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="7de6d-303">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7de6d-303">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="7de6d-304">Mozilla Geliştirici ağ: Kabul-Encoding</span><span class="sxs-lookup"><span data-stu-id="7de6d-304">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="7de6d-305">RFC 7231 bölüm 3.1.2.1: İçerik Codings</span><span class="sxs-lookup"><span data-stu-id="7de6d-305">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="7de6d-306">RFC 7230 bölüm 4.2.3: Gzip kodlama</span><span class="sxs-lookup"><span data-stu-id="7de6d-306">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="7de6d-307">GZIP dosyası biçim belirtimi sürümü 4.3</span><span class="sxs-lookup"><span data-stu-id="7de6d-307">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
