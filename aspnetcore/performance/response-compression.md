---
title: ASP.NET core'da yanıt sıkıştırma
author: guardrex
description: Yanıt sıkıştırma ve ASP.NET Core uygulamalarında yanıt sıkıştırma ara yazılımı kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: performance/response-compression
ms.openlocfilehash: 51ab51652a7b3f9b4ef97b3abbffe2e398c0bfb5
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637761"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="1886f-103">ASP.NET core'da yanıt sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="1886f-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="1886f-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1886f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1886f-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1886f-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1886f-106">Ağ bant genişliği sınırlı bir kaynaktır.</span><span class="sxs-lookup"><span data-stu-id="1886f-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="1886f-107">Yanıt boyutu genellikle azaltma, uygulama yanıt verme hızını genellikle önemli ölçüde artırır.</span><span class="sxs-lookup"><span data-stu-id="1886f-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="1886f-108">Yük boyutları azaltmak için bir uygulamanın yanıtları sıkıştırma yoludur.</span><span class="sxs-lookup"><span data-stu-id="1886f-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="1886f-109">Yanıt sıkıştırma ara yazılımı kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="1886f-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="1886f-110">IIS, Apache veya Ngınx sunucu tabanlı yanıt sıkıştırma teknolojileri kullanın.</span><span class="sxs-lookup"><span data-stu-id="1886f-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="1886f-111">Ara yazılım performansını büyük olasılıkla, sunucu modüllerinin eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="1886f-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="1886f-112">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) sunucu ve [Kestrel](xref:fundamentals/servers/kestrel) sunucusu şu anda yerleşik sıkıştırma desteği sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="1886f-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="1886f-113">Yanıt sıkıştırma ara yazılımı, işiniz kullanın:</span><span class="sxs-lookup"><span data-stu-id="1886f-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="1886f-114">Aşağıdaki sunucu tabanlı sıkıştırma teknolojileri kullanılamıyor:</span><span class="sxs-lookup"><span data-stu-id="1886f-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="1886f-115">IIS dinamik sıkıştırması Modülü</span><span class="sxs-lookup"><span data-stu-id="1886f-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="1886f-116">Apache mod_deflate Modülü</span><span class="sxs-lookup"><span data-stu-id="1886f-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="1886f-117">Ngınx sıkıştırma ve açma</span><span class="sxs-lookup"><span data-stu-id="1886f-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="1886f-118">Doğrudan barındırma:</span><span class="sxs-lookup"><span data-stu-id="1886f-118">Hosting directly on:</span></span>
  * <span data-ttu-id="1886f-119">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıyla WebListener olarak adlandırılır)</span><span class="sxs-lookup"><span data-stu-id="1886f-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="1886f-120">Kestrel'i sunucusu</span><span class="sxs-lookup"><span data-stu-id="1886f-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="1886f-121">Yanıt sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="1886f-121">Response compression</span></span>

<span data-ttu-id="1886f-122">Genellikle, yerel olarak sıkıştırılmış herhangi bir yanıt, yanıt sıkıştırması arasında yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="1886f-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="1886f-123">Yerel olarak genellikle sıkıştırılmış yanıtlarını içerir: CSS, JavaScript, HTML, XML ve JSON.</span><span class="sxs-lookup"><span data-stu-id="1886f-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="1886f-124">PNG dosyaları gibi yerel olarak sıkıştırılmış varlıklar sıkıştırın olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="1886f-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="1886f-125">Daha fazla yerel olarak sıkıştırılmış bir yanıt sıkıştırma çalışırsanız, küçük bir ek boyut ve iletim zaman kısalma sıkıştırma işlemek için geçen zaman büyük olasılıkla overshadowed.</span><span class="sxs-lookup"><span data-stu-id="1886f-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="1886f-126">Yaklaşık 150-1000 bayt (bağlı olarak dosyanın içeriği ve sıkıştırma verimliliğini) küçük dosyaları sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="1886f-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="1886f-127">Küçük dosyalar sıkıştırılıyor yükünü bir sıkıştırılmış dosyayı sıkıştırılmamış dosyasından büyük üretebilir.</span><span class="sxs-lookup"><span data-stu-id="1886f-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="1886f-128">Sıkıştırılmış içerik işleme bir istemci, istemci yeteneklerini sunucusu göndererek bilgilendirmeniz `Accept-Encoding` istek üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="1886f-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="1886f-129">Bir sunucu sıkıştırılmış içerik gönderdiğinde bilgileri içermelidir `Content-Encoding` sıkıştırılmış yanıt nasıl kodlandığını üzerinde başlığı.</span><span class="sxs-lookup"><span data-stu-id="1886f-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="1886f-130">Ara yazılım tarafından desteklenen içerik kodlama gösterimleri aşağıdaki tabloda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="1886f-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="1886f-131">`Accept-Encoding` Üstbilgi değerleri</span><span class="sxs-lookup"><span data-stu-id="1886f-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="1886f-132">Desteklenen bir ara yazılım</span><span class="sxs-lookup"><span data-stu-id="1886f-132">Middleware Supported</span></span> | <span data-ttu-id="1886f-133">Açıklama</span><span class="sxs-lookup"><span data-stu-id="1886f-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="1886f-134">Evet (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="1886f-134">Yes (default)</span></span>        | [<span data-ttu-id="1886f-135">Brotli sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="1886f-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="1886f-136">Hayır</span><span class="sxs-lookup"><span data-stu-id="1886f-136">No</span></span>                   | [<span data-ttu-id="1886f-137">DEFLATE sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="1886f-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="1886f-138">Hayır</span><span class="sxs-lookup"><span data-stu-id="1886f-138">No</span></span>                   | [<span data-ttu-id="1886f-139">W3C XML verimli değişimi</span><span class="sxs-lookup"><span data-stu-id="1886f-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="1886f-140">Evet</span><span class="sxs-lookup"><span data-stu-id="1886f-140">Yes</span></span>                  | [<span data-ttu-id="1886f-141">Gzip dosya biçimi</span><span class="sxs-lookup"><span data-stu-id="1886f-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="1886f-142">Evet</span><span class="sxs-lookup"><span data-stu-id="1886f-142">Yes</span></span>                  | <span data-ttu-id="1886f-143">"Kodlaması" tanımlayıcısı: Yanıt kodlanmalı değil.</span><span class="sxs-lookup"><span data-stu-id="1886f-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="1886f-144">Hayır</span><span class="sxs-lookup"><span data-stu-id="1886f-144">No</span></span>                   | [<span data-ttu-id="1886f-145">Ağ aktarım biçimi için Java arşivleri</span><span class="sxs-lookup"><span data-stu-id="1886f-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="1886f-146">Evet</span><span class="sxs-lookup"><span data-stu-id="1886f-146">Yes</span></span>                  | <span data-ttu-id="1886f-147">Kodlama açıkça istenen herhangi bir kullanılabilir içerik</span><span class="sxs-lookup"><span data-stu-id="1886f-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="1886f-148">`Accept-Encoding` Üstbilgi değerleri</span><span class="sxs-lookup"><span data-stu-id="1886f-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="1886f-149">Desteklenen bir ara yazılım</span><span class="sxs-lookup"><span data-stu-id="1886f-149">Middleware Supported</span></span> | <span data-ttu-id="1886f-150">Açıklama</span><span class="sxs-lookup"><span data-stu-id="1886f-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="1886f-151">Hayır</span><span class="sxs-lookup"><span data-stu-id="1886f-151">No</span></span>                   | [<span data-ttu-id="1886f-152">Brotli sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="1886f-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="1886f-153">Hayır</span><span class="sxs-lookup"><span data-stu-id="1886f-153">No</span></span>                   | [<span data-ttu-id="1886f-154">DEFLATE sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="1886f-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="1886f-155">Hayır</span><span class="sxs-lookup"><span data-stu-id="1886f-155">No</span></span>                   | [<span data-ttu-id="1886f-156">W3C XML verimli değişimi</span><span class="sxs-lookup"><span data-stu-id="1886f-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="1886f-157">Evet (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="1886f-157">Yes (default)</span></span>        | [<span data-ttu-id="1886f-158">Gzip dosya biçimi</span><span class="sxs-lookup"><span data-stu-id="1886f-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="1886f-159">Evet</span><span class="sxs-lookup"><span data-stu-id="1886f-159">Yes</span></span>                  | <span data-ttu-id="1886f-160">"Kodlaması" tanımlayıcısı: Yanıt kodlanmalı değil.</span><span class="sxs-lookup"><span data-stu-id="1886f-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="1886f-161">Hayır</span><span class="sxs-lookup"><span data-stu-id="1886f-161">No</span></span>                   | [<span data-ttu-id="1886f-162">Ağ aktarım biçimi için Java arşivleri</span><span class="sxs-lookup"><span data-stu-id="1886f-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="1886f-163">Evet</span><span class="sxs-lookup"><span data-stu-id="1886f-163">Yes</span></span>                  | <span data-ttu-id="1886f-164">Kodlama açıkça istenen herhangi bir kullanılabilir içerik</span><span class="sxs-lookup"><span data-stu-id="1886f-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="1886f-165">Daha fazla bilgi için [IANA resmi içerik kodlama listesi](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="1886f-165">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="1886f-166">Ara yazılım özel ek sıkıştırmasını sağlayıcıları eklemenizi sağlayan `Accept-Encoding` üstbilgi değerleri.</span><span class="sxs-lookup"><span data-stu-id="1886f-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="1886f-167">Daha fazla bilgi için [özel sağlayıcılar](#custom-providers) aşağıda.</span><span class="sxs-lookup"><span data-stu-id="1886f-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="1886f-168">Ara yazılım kalite değeri tepki uyumlu (qvalue, `q`) sıkıştırma düzeni önceliğini belirlemek için istemci tarafından gönderilen ağırlığı.</span><span class="sxs-lookup"><span data-stu-id="1886f-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="1886f-169">Daha fazla bilgi için [RFC 7231: Kabul kodlama](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="1886f-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="1886f-170">Sıkıştırma hız ve sıkıştırma verimliliğini arasında bir denge sıkıştırma algoritmaları tabidir.</span><span class="sxs-lookup"><span data-stu-id="1886f-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="1886f-171">*Verimliliği* bu bağlamda sıkıştırma sonrasında çıkış boyutunu ifade eder.</span><span class="sxs-lookup"><span data-stu-id="1886f-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="1886f-172">En küçük boyuta göre en sağlanır *en iyi* sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="1886f-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="1886f-173">Gönderme, önbelleğe alma ve sıkıştırılmış içerik almak isteyen ilgili üstbilgileri, aşağıdaki tabloda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="1886f-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="1886f-174">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="1886f-174">Header</span></span>             | <span data-ttu-id="1886f-175">Rol</span><span class="sxs-lookup"><span data-stu-id="1886f-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="1886f-176">İstemciden sunucuya istemcinin kabul edilebilir düzenleri kodlama içeriği belirtmek için gönderilen.</span><span class="sxs-lookup"><span data-stu-id="1886f-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="1886f-177">Sunucudan yükünde içerik kodlamasını belirtmek için istemciye gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="1886f-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="1886f-178">Sıkıştırma oluştuğunda `Content-Length` üstbilgi kaldırılır, yanıt sıkıştırılmışsa, gövde içeriği sonraki değişiklikler.</span><span class="sxs-lookup"><span data-stu-id="1886f-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="1886f-179">Sıkıştırma oluştuğunda `Content-MD5` üst bilgisi kaldırıldı, gövde içeriği değiştirildi ve karma artık geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="1886f-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="1886f-180">İçeriğin MIME türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="1886f-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="1886f-181">Her yanıt belirtmeniz gerekir, `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="1886f-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="1886f-182">Ara yazılımın yanıt sıkıştırılmış belirlemek için bu değer denetler.</span><span class="sxs-lookup"><span data-stu-id="1886f-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="1886f-183">Bir ara yazılım belirtir [MIME türleri varsayılan](#mime-types) kodlama, ancak değiştirin veya MIME türleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1886f-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="1886f-184">Değerine sahip bir sunucu tarafından gönderilen `Accept-Encoding` proxy'leri ve istemcilere `Vary` üst bilgisi gösterir önbelleğe alması proxy ve istemci için (farklı) değerini temel alarak yanıtlarını `Accept-Encoding` isteği üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="1886f-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="1886f-185">İçerik döndüren sonucunu `Vary: Accept-Encoding` başlığıdır hem sıkıştırılmış ve sıkıştırılmamış yanıtları ayrı olarak önbelleğe.</span><span class="sxs-lookup"><span data-stu-id="1886f-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="1886f-186">Yanıt sıkıştırma ara yazılımı ile özelliklerini [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="1886f-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="1886f-187">Örnek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="1886f-187">The sample illustrates:</span></span>

* <span data-ttu-id="1886f-188">Gzip ve özel sıkıştırmasını sağlayıcıları kullanarak uygulama yanıtlarının sıkıştırılması.</span><span class="sxs-lookup"><span data-stu-id="1886f-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="1886f-189">Bir MIME türü sıkıştırma için MIME türleri varsayılan listeye ekleme.</span><span class="sxs-lookup"><span data-stu-id="1886f-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="1886f-190">Paket</span><span class="sxs-lookup"><span data-stu-id="1886f-190">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1886f-191">Ara yazılım bir projeye dahil etmek için bir başvuru ekleyin. [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), içeren [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paket.</span><span class="sxs-lookup"><span data-stu-id="1886f-191">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1886f-192">Ara yazılım bir projeye dahil etmek için bir başvuru ekleyin. [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), içeren [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paket.</span><span class="sxs-lookup"><span data-stu-id="1886f-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1886f-193">Ara yazılım bir projeye dahil etmek için bir başvuru ekleyin. [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paket.</span><span class="sxs-lookup"><span data-stu-id="1886f-193">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="1886f-194">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="1886f-194">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1886f-195">Aşağıdaki kod nasıl yanıt sıkıştırma ara yazılımı varsayılan MIME türleri ve sıkıştırmasını sağlayıcıları için etkinleştirileceğini gösterir ([Brotli](#brotli-compression-provider) ve [Gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="1886f-195">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1886f-196">Aşağıdaki kod nasıl yanıt sıkıştırma ara yazılımı varsayılan MIME türleri için etkinleştirileceğini gösterir ve [Gzip sıkıştırma sağlayıcısı](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="1886f-196">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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
> <span data-ttu-id="1886f-197">Gibi bir araç kullanın [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), veya [Postman](https://www.getpostman.com/) ayarlanacak `Accept-Encoding` istek üst bilgisi ve yanıt üstbilgileri, boyutu ve gövde çalışın.</span><span class="sxs-lookup"><span data-stu-id="1886f-197">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="1886f-198">Örnek uygulamaya talebinizi `Accept-Encoding` üstbilgi ve yanıt sıkıştırılmamış olup olmadığına bakın.</span><span class="sxs-lookup"><span data-stu-id="1886f-198">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="1886f-199">`Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut değildir.</span><span class="sxs-lookup"><span data-stu-id="1886f-199">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Accept-Encoding üst bilgisi olmayan bir isteğinin sonucunu gösteren fiddler penceresi.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1886f-202">Örnek uygulamada talebinizi `Accept-Encoding: br` üst bilgisi (Brotli sıkıştırma) ve yanıt sıkıştırılmışsa gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="1886f-202">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="1886f-203">`Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut.</span><span class="sxs-lookup"><span data-stu-id="1886f-203">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Accept-Encoding üst bilgisiyle bir isteğinin sonucunu ve br değerini gösteren fiddler penceresi.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1886f-207">Örnek uygulamada talebinizi `Accept-Encoding: gzip` üstbilgi ve yanıt sıkıştırılmışsa gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="1886f-207">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="1886f-208">`Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut.</span><span class="sxs-lookup"><span data-stu-id="1886f-208">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Accept-Encoding üst bilgisiyle bir isteğinin sonucunu ve gzip değerini gösteren fiddler penceresi.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="1886f-212">sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="1886f-212">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="1886f-213">Brotli sıkıştırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="1886f-213">Brotli Compression Provider</span></span>

<span data-ttu-id="1886f-214">Kullanım `BrotliCompressionProvider` ile yanıtları sıkıştırma [Brotli sıkıştırılmış veri biçimi](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="1886f-214">Use the `BrotliCompressionProvider` to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="1886f-215">Sıkıştırma yok sağlayıcıları için açıkça eklenirse <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="1886f-215">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="1886f-216">Brotli sıkıştırma sağlayıcısı sıkıştırmasını sağlayıcıları ile birlikte bir dizi varsayılan olarak eklenir [Gzip sıkıştırma sağlayıcısı](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="1886f-216">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="1886f-217">Sıkıştırma Brotli sıkıştırma için varsayılan olarak Brotli sıkıştırılmış veri biçimi, istemci tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="1886f-217">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="1886f-218">Gzip sıkıştırma istemcinin desteklediğinde sıkıştırma Brotli istemci tarafından desteklenmiyorsa, Gzip için varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="1886f-218">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="1886f-219">Tüm sıkıştırmasını sağlayıcıları açıkça eklendiğinde Brotoli sıkıştırma sağlayıcının eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="1886f-219">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

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

<span data-ttu-id="1886f-220">Sıkıştırma düzeyi ile ayarlanmış `BrotliCompressionProviderOptions`.</span><span class="sxs-lookup"><span data-stu-id="1886f-220">Set the compression level with `BrotliCompressionProviderOptions`.</span></span> <span data-ttu-id="1886f-221">Brotli sıkıştırma sağlayıcısı varsayılan olarak en hızlı sıkıştırma düzeyini ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), hangi değil üretebilir en verimli sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="1886f-221">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="1886f-222">En verimli sıkıştırma isterseniz, ara yazılım en iyi sıkıştırma için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1886f-222">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="1886f-223">Sıkıştırma düzeyi</span><span class="sxs-lookup"><span data-stu-id="1886f-223">Compression Level</span></span> | <span data-ttu-id="1886f-224">Açıklama</span><span class="sxs-lookup"><span data-stu-id="1886f-224">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="1886f-225">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="1886f-225">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="1886f-226">Sıkıştırma, elde edilen çıkış en uygun şekilde sıkıştırılmaz olsa bile, mümkün olan en kısa sürede tamamlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1886f-226">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="1886f-227">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="1886f-227">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="1886f-228">Sıkıştırma yok gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="1886f-228">No compression should be performed.</span></span> |
| [<span data-ttu-id="1886f-229">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="1886f-229">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="1886f-230">Sıkıştırma tamamlanması daha uzun sürer bile yanıtları en uygun şekilde, sıkıştırılmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1886f-230">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="1886f-231">Gzip sıkıştırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="1886f-231">Gzip Compression Provider</span></span>

<span data-ttu-id="1886f-232">Kullanım <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> ile yanıtları sıkıştırma [Gzip dosya biçimi](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="1886f-232">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="1886f-233">Sıkıştırma yok sağlayıcıları için açıkça eklenirse <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="1886f-233">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="1886f-234">Gzip sıkıştırma sağlayıcısı sıkıştırmasını sağlayıcıları ile birlikte bir dizi varsayılan olarak eklenir [Brotli sıkıştırma sağlayıcısı](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="1886f-234">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="1886f-235">Sıkıştırma Brotli sıkıştırma için varsayılan olarak Brotli sıkıştırılmış veri biçimi, istemci tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="1886f-235">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="1886f-236">Gzip sıkıştırma istemcinin desteklediğinde sıkıştırma Brotli istemci tarafından desteklenmiyorsa, Gzip için varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="1886f-236">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="1886f-237">Gzip sıkıştırma sağlayıcısı sıkıştırmasını sağlayıcıları dizisi için varsayılan olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="1886f-237">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="1886f-238">Gzip sıkıştırma istemcinin desteklediğinde, Gzip sıkıştırma varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="1886f-238">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="1886f-239">Gzip sıkıştırma sağlayıcısı herhangi sıkıştırmasını sağlayıcıları açıkça eklendiğinde eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="1886f-239">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

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

<span data-ttu-id="1886f-240">Sıkıştırma düzeyi ile ayarlanmış <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="1886f-240">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="1886f-241">Gzip sıkıştırma sağlayıcısı varsayılan olarak en hızlı sıkıştırma düzeyini ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), hangi değil üretebilir en verimli sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="1886f-241">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="1886f-242">En verimli sıkıştırma isterseniz, ara yazılım en iyi sıkıştırma için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1886f-242">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="1886f-243">Sıkıştırma düzeyi</span><span class="sxs-lookup"><span data-stu-id="1886f-243">Compression Level</span></span> | <span data-ttu-id="1886f-244">Açıklama</span><span class="sxs-lookup"><span data-stu-id="1886f-244">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="1886f-245">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="1886f-245">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="1886f-246">Sıkıştırma, elde edilen çıkış en uygun şekilde sıkıştırılmaz olsa bile, mümkün olan en kısa sürede tamamlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1886f-246">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="1886f-247">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="1886f-247">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="1886f-248">Sıkıştırma yok gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="1886f-248">No compression should be performed.</span></span> |
| [<span data-ttu-id="1886f-249">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="1886f-249">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="1886f-250">Sıkıştırma tamamlanması daha uzun sürer bile yanıtları en uygun şekilde, sıkıştırılmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1886f-250">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="1886f-251">Özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="1886f-251">Custom providers</span></span>

<span data-ttu-id="1886f-252">Özel sıkıştırma uygulamalarıyla oluşturma <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="1886f-252">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="1886f-253"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> Temsil eden bu kodlama içeriğe `ICompressionProvider` üretir.</span><span class="sxs-lookup"><span data-stu-id="1886f-253">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="1886f-254">Ara yazılım, belirtilen liste göre sağlayıcıyı seçmek için bu bilgileri kullanır. `Accept-Encoding` isteği üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="1886f-254">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="1886f-255">Örnek uygulama kullanma, istemci bir istek gönderir `Accept-Encoding: mycustomcompression` başlığı.</span><span class="sxs-lookup"><span data-stu-id="1886f-255">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="1886f-256">Ara yazılım özel sıkıştırma uygulaması kullanır ve yanıtı döndürür bir `Content-Encoding: mycustomcompression` başlığı.</span><span class="sxs-lookup"><span data-stu-id="1886f-256">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="1886f-257">İstemci, sırayla çalışmak özel sıkıştırma uygulaması için özel kodlama sıkıştırmasını mümkün olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1886f-257">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

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

<span data-ttu-id="1886f-258">Örnek uygulamada talebinizi `Accept-Encoding: mycustomcompression` üstbilgi ve yanıt üstbilgileri gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="1886f-258">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="1886f-259">`Vary` Ve `Content-Encoding` üstbilgileri yanıtta mevcut.</span><span class="sxs-lookup"><span data-stu-id="1886f-259">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="1886f-260">(Gösterilmemiştir) yanıt gövdesi olarak örnek sıkıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="1886f-260">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="1886f-261">Sıkıştırma uygulamasında hiç `CustomCompressionProvider` sınıfının örneği.</span><span class="sxs-lookup"><span data-stu-id="1886f-261">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="1886f-262">Ancak, bu tür bir sıkıştırma algoritması burada uygulamak örnek gösterir.</span><span class="sxs-lookup"><span data-stu-id="1886f-262">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Accept-Encoding üst bilgisiyle bir isteğinin sonucunu ve mycustomcompression değerini gösteren fiddler penceresi.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="1886f-265">MIME türleri</span><span class="sxs-lookup"><span data-stu-id="1886f-265">MIME types</span></span>

<span data-ttu-id="1886f-266">Ara yazılım bir varsayılan sıkıştırma için MIME türlerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="1886f-266">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="1886f-267">Yanıt sıkıştırma ara yazılımı seçenekleri MIME türleri ekleyin veya değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1886f-267">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="1886f-268">Unutmayın, joker karakter MIME türleri gibi `text/*` desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="1886f-268">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="1886f-269">Örnek uygulama için bir MIME türü ekler `image/svg+xml` sıkıştırır ve ASP.NET Core hizmet başlık resmi (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="1886f-269">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

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

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="1886f-270">Güvenli protokol sıkıştırması</span><span class="sxs-lookup"><span data-stu-id="1886f-270">Compression with secure protocol</span></span>

<span data-ttu-id="1886f-271">Sıkıştırılmış yanıtlar güvenli bağlantılar üzerinden ile denetlenebilir `EnableForHttps` seçeneği, varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="1886f-271">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="1886f-272">Sıkıştırma ile dinamik olarak oluşturulan sayfaları kullanarak yol açabilir. güvenlik sorunları gibi [SUÇ](https://wikipedia.org/wiki/CRIME_(security_exploit)) ve [ihlal](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları.</span><span class="sxs-lookup"><span data-stu-id="1886f-272">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="1886f-273">Vary üstbilgi ekleme</span><span class="sxs-lookup"><span data-stu-id="1886f-273">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1886f-274">Yanıtları sıkıştırma zaman temelinde `Accept-Encoding` üst bilgi, potansiyel olarak birden çok sıkıştırılmış sürümlerinin yanıtın ve sıkıştırılmamış bir vardır.</span><span class="sxs-lookup"><span data-stu-id="1886f-274">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="1886f-275">Birden çok sürümünün var ve depolanması gereken, istemci ve proxy önbellek isteyin için `Vary` üst bilgisi ile eklenen bir `Accept-Encoding` değeri.</span><span class="sxs-lookup"><span data-stu-id="1886f-275">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="1886f-276">ASP.NET Core 2.0 veya sonraki sürümlerde, ara yazılım ekler `Vary` otomatik olarak yanıt sıkıştırılmışsa, üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="1886f-276">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1886f-277">Yanıtları sıkıştırma zaman temelinde `Accept-Encoding` üst bilgi, potansiyel olarak birden çok sıkıştırılmış sürümlerinin yanıtın ve sıkıştırılmamış bir vardır.</span><span class="sxs-lookup"><span data-stu-id="1886f-277">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="1886f-278">Birden çok sürümünün var ve depolanması gereken, istemci ve proxy önbellek isteyin için `Vary` üst bilgisi ile eklenen bir `Accept-Encoding` değeri.</span><span class="sxs-lookup"><span data-stu-id="1886f-278">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="1886f-279">İçinde ASP.NET Core 1.x, ekleme `Vary` üstbilgisi yanıt için el ile gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="1886f-279">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="1886f-280">Ters proxy arkasında olduğunda bir Ngınx ara yazılım sorunu</span><span class="sxs-lookup"><span data-stu-id="1886f-280">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="1886f-281">Bir isteği Ngınx tarafından proxy olduğunda `Accept-Encoding` üstbilgi kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="1886f-281">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="1886f-282">Kaldırılmasını `Accept-Encoding` üst bilgi yanıtı sıkıştırmasını ara yazılım engeller.</span><span class="sxs-lookup"><span data-stu-id="1886f-282">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="1886f-283">Daha fazla bilgi için [NGINX: Sıkıştırma ve açma](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="1886f-283">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="1886f-284">Bu sorunu tarafından izlenen [Nginx için doğrudan sıkıştırma ekleyeceğimi (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="1886f-284">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="1886f-285">IIS dinamik sıkıştırması ile çalışma</span><span class="sxs-lookup"><span data-stu-id="1886f-285">Working with IIS dynamic compression</span></span>

<span data-ttu-id="1886f-286">Bir active IIS dinamik sıkıştırması bir uygulama için devre dışı bırakmak istediğiniz sunucu düzeyinde yapılandırılan modülü varsa, ek olarak modülle devre dışı *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="1886f-286">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="1886f-287">Daha fazla bilgi için [devre dışı bırakma IIS modüllerini](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="1886f-287">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1886f-288">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="1886f-288">Troubleshooting</span></span>

<span data-ttu-id="1886f-289">Gibi bir araç kullanın [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), veya [Postman](https://www.getpostman.com/), ayarlamak izin `Accept-Encoding` istek üst bilgisi ve yanıt üstbilgileri, boyutu ve gövde çalışın.</span><span class="sxs-lookup"><span data-stu-id="1886f-289">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="1886f-290">Varsayılan olarak, aşağıdaki koşullara uyması yanıtları yanıt sıkıştırma ara yazılımı sıkıştırır:</span><span class="sxs-lookup"><span data-stu-id="1886f-290">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="1886f-291">`Accept-Encoding` Üst bilgi değeri olan mevcut `br`, `gzip`, `*`, veya sizin belirlediğiniz bir özel sıkıştırma sağlayıcısı ile eşleşen özel kodlama.</span><span class="sxs-lookup"><span data-stu-id="1886f-291">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="1886f-292">Değeri olmamalıdır `identity` veya bir kalite değeri (qvalue, `q`) 0 (sıfır) ayarlama.</span><span class="sxs-lookup"><span data-stu-id="1886f-292">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="1886f-293">MIME türü (`Content-Type`) ayarlanmalıdır ve yapılandırılmış bir MIME türü eşleşmelidir <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="1886f-293">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="1886f-294">İstek içermemelidir `Content-Range` başlığı.</span><span class="sxs-lookup"><span data-stu-id="1886f-294">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="1886f-295">İstek, yanıt sıkıştırma ara yazılımı seçenekleri güvenli Protokolü (https) yapılandırılmadığı sürece güvenli olmayan bir protokol (http) kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1886f-295">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="1886f-296">*Tehlike Not [yukarıda açıklanan](#compression-with-secure-protocol) güvenli içerik sıkıştırmayı etkinleştirilirken.*</span><span class="sxs-lookup"><span data-stu-id="1886f-296">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="1886f-297">`Accept-Encoding` Üst bilgi değeri olan mevcut `gzip`, `*`, veya sizin belirlediğiniz bir özel sıkıştırma sağlayıcısı ile eşleşen özel kodlama.</span><span class="sxs-lookup"><span data-stu-id="1886f-297">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="1886f-298">Değeri olmamalıdır `identity` veya bir kalite değeri (qvalue, `q`) 0 (sıfır) ayarlama.</span><span class="sxs-lookup"><span data-stu-id="1886f-298">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="1886f-299">MIME türü (`Content-Type`) ayarlanmalıdır ve yapılandırılmış bir MIME türü eşleşmelidir <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="1886f-299">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="1886f-300">İstek içermemelidir `Content-Range` başlığı.</span><span class="sxs-lookup"><span data-stu-id="1886f-300">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="1886f-301">İstek, yanıt sıkıştırma ara yazılımı seçenekleri güvenli Protokolü (https) yapılandırılmadığı sürece güvenli olmayan bir protokol (http) kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1886f-301">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="1886f-302">*Tehlike Not [yukarıda açıklanan](#compression-with-secure-protocol) güvenli içerik sıkıştırmayı etkinleştirilirken.*</span><span class="sxs-lookup"><span data-stu-id="1886f-302">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="1886f-303">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1886f-303">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="1886f-304">Mozilla Geliştirici ağ: Kabul kodlama</span><span class="sxs-lookup"><span data-stu-id="1886f-304">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="1886f-305">RFC 7231 bölüm 3.1.2.1: İçerik Codings</span><span class="sxs-lookup"><span data-stu-id="1886f-305">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="1886f-306">RFC 7230 bölüm 4.2.3: Gzip kodlama</span><span class="sxs-lookup"><span data-stu-id="1886f-306">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="1886f-307">GZIP dosyası biçim belirtimi sürümü 4.3</span><span class="sxs-lookup"><span data-stu-id="1886f-307">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
