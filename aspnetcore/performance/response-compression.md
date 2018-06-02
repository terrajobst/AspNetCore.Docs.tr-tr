---
title: ASP.NET Core için yanıt sıkıştırma Ara
author: guardrex
description: Yanıt sıkıştırma ve ASP.NET Core uygulamaları yanıt sıkıştırma ara yazılım kullanma hakkında bilgi edinin.
manager: wpickett
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/response-compression
ms.openlocfilehash: 152799500577dd09247bcee8c87cde39ca20aa79
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729580"
---
# <a name="response-compression-middleware-for-aspnet-core"></a><span data-ttu-id="0c575-103">ASP.NET Core için yanıt sıkıştırma Ara</span><span class="sxs-lookup"><span data-stu-id="0c575-103">Response Compression Middleware for ASP.NET Core</span></span>

<span data-ttu-id="0c575-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0c575-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0c575-105">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0c575-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0c575-106">Ağ bant genişliği sınırlı bir kaynaktır.</span><span class="sxs-lookup"><span data-stu-id="0c575-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="0c575-107">Genellikle yanıt boyutunun azaltılması, bir uygulamanın yanıtlama genellikle önemli ölçüde artırır.</span><span class="sxs-lookup"><span data-stu-id="0c575-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="0c575-108">Yükü boyutları azaltmak için tek bir uygulamanın yanıtları sıkıştırma yoludur.</span><span class="sxs-lookup"><span data-stu-id="0c575-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="0c575-109">Yanıt sıkıştırma ara yazılımı kullanmak ne zaman</span><span class="sxs-lookup"><span data-stu-id="0c575-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="0c575-110">IIS, Apache veya Nginx sunucu tabanlı yanıt sıkıştırmayı teknolojileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="0c575-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="0c575-111">Ara yazılım performansını büyük olasılıkla, sunucu modüllerinin eşleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="0c575-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="0c575-112">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) ve [Kestrel](xref:fundamentals/servers/kestrel) şu anda yerleşik sıkıştırma desteği sunmuyoruz.</span><span class="sxs-lookup"><span data-stu-id="0c575-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) and [Kestrel](xref:fundamentals/servers/kestrel) don't currently offer built-in compression support.</span></span>

<span data-ttu-id="0c575-113">Bunu olduğunuzda yanıt sıkıştırma Ara kullanın:</span><span class="sxs-lookup"><span data-stu-id="0c575-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="0c575-114">Aşağıdaki sunucu tabanlı sıkıştırmayı teknolojileri kullanılamıyor:</span><span class="sxs-lookup"><span data-stu-id="0c575-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="0c575-115">IIS dinamik sıkıştırma Modülü</span><span class="sxs-lookup"><span data-stu-id="0c575-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="0c575-116">Apache mod_deflate Modülü</span><span class="sxs-lookup"><span data-stu-id="0c575-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="0c575-117">Nginx sıkıştırma ve açma</span><span class="sxs-lookup"><span data-stu-id="0c575-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="0c575-118">Doğrudan barındırma:</span><span class="sxs-lookup"><span data-stu-id="0c575-118">Hosting directly on:</span></span>
  * <span data-ttu-id="0c575-119">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="0c575-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * [<span data-ttu-id="0c575-120">Kestrel</span><span class="sxs-lookup"><span data-stu-id="0c575-120">Kestrel</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="0c575-121">Yanıt sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="0c575-121">Response compression</span></span>

<span data-ttu-id="0c575-122">Genellikle, yerel olarak sıkıştırılmış herhangi bir yanıt gelen yanıt sıkıştırma yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="0c575-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="0c575-123">Yerel olarak genellikle sıkıştırılmış yanıtlarını içerir: CSS, JavaScript, HTML, XML ve JSON.</span><span class="sxs-lookup"><span data-stu-id="0c575-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="0c575-124">PNG dosyaları gibi yerel sıkıştırılmış varlıklar sıkıştırın döndürmemelidir.</span><span class="sxs-lookup"><span data-stu-id="0c575-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="0c575-125">Tüm küçük ek boyutunu ve iletim süresindeki azalma, büyük olasılıkla daha fazla yerel sıkıştırılmış yanıt Sıkıştır çalışırsanız, sıkıştırma işlemek için geçen zaman overshadowed.</span><span class="sxs-lookup"><span data-stu-id="0c575-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="0c575-126">Yaklaşık 150-1000 bayt (göre dosyanın içeriğini ve sıkıştırma verimliliğini) daha küçük dosyalarını sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="0c575-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="0c575-127">Küçük dosyalar sıkıştırma yükünü bir sıkıştırılmış dosyayı sıkıştırılmamış dosyasından büyük üretebilir.</span><span class="sxs-lookup"><span data-stu-id="0c575-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="0c575-128">Bir istemci sıkıştırılmış içerik işlerken istemci yeteneklerini sunucu göndererek bilgilendirmeniz `Accept-Encoding` istek üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="0c575-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="0c575-129">Bir sunucu sıkıştırılmış içerik gönderdiğinde, bilgileri içermelidir `Content-Encoding` sıkıştırılmış yanıtı nasıl kodlandığını üzerinde üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="0c575-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="0c575-130">Ara yazılım tarafından desteklenen içerik kodlama belirlemeleri aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="0c575-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="0c575-131">`Accept-Encoding` Üstbilgi değerleri</span><span class="sxs-lookup"><span data-stu-id="0c575-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="0c575-132">Desteklenen Ara</span><span class="sxs-lookup"><span data-stu-id="0c575-132">Middleware Supported</span></span> | <span data-ttu-id="0c575-133">Açıklama</span><span class="sxs-lookup"><span data-stu-id="0c575-133">Description</span></span>                                                 |
| ------------------------------- | :------------------: | ----------------------------------------------------------- |
| `br`                            | <span data-ttu-id="0c575-134">Hayır</span><span class="sxs-lookup"><span data-stu-id="0c575-134">No</span></span>                   | <span data-ttu-id="0c575-135">Brotli sıkıştırılmış veri biçimi</span><span class="sxs-lookup"><span data-stu-id="0c575-135">Brotli Compressed Data Format</span></span>                               |
| `compress`                      | <span data-ttu-id="0c575-136">Hayır</span><span class="sxs-lookup"><span data-stu-id="0c575-136">No</span></span>                   | <span data-ttu-id="0c575-137">UNIX "Sıkıştır" veri biçimi</span><span class="sxs-lookup"><span data-stu-id="0c575-137">UNIX "compress" data format</span></span>                                 |
| `deflate`                       | <span data-ttu-id="0c575-138">Hayır</span><span class="sxs-lookup"><span data-stu-id="0c575-138">No</span></span>                   | <span data-ttu-id="0c575-139">"" zlib"veri biçimi içindeki sıkıştırılmış verileri deflate"</span><span class="sxs-lookup"><span data-stu-id="0c575-139">"deflate" compressed data inside the "zlib" data format</span></span>     |
| `exi`                           | <span data-ttu-id="0c575-140">Hayır</span><span class="sxs-lookup"><span data-stu-id="0c575-140">No</span></span>                   | <span data-ttu-id="0c575-141">W3C verimli XML değişimi</span><span class="sxs-lookup"><span data-stu-id="0c575-141">W3C Efficient XML Interchange</span></span>                               |
| `gzip`                          | <span data-ttu-id="0c575-142">Evet (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="0c575-142">Yes (default)</span></span>        | <span data-ttu-id="0c575-143">gzip dosya biçimi</span><span class="sxs-lookup"><span data-stu-id="0c575-143">gzip file format</span></span>                                            |
| `identity`                      | <span data-ttu-id="0c575-144">Evet</span><span class="sxs-lookup"><span data-stu-id="0c575-144">Yes</span></span>                  | <span data-ttu-id="0c575-145">"Kodlaması yok" tanımlayıcısı: yanıt kodlanmamış gerekir.</span><span class="sxs-lookup"><span data-stu-id="0c575-145">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="0c575-146">Hayır</span><span class="sxs-lookup"><span data-stu-id="0c575-146">No</span></span>                   | <span data-ttu-id="0c575-147">Java arşivlerini ağ aktarımı biçimi</span><span class="sxs-lookup"><span data-stu-id="0c575-147">Network Transfer Format for Java Archives</span></span>                   |
| `*`                             | <span data-ttu-id="0c575-148">Evet</span><span class="sxs-lookup"><span data-stu-id="0c575-148">Yes</span></span>                  | <span data-ttu-id="0c575-149">Açıkça İstenen kodlama herhangi bir kullanılabilir içerik</span><span class="sxs-lookup"><span data-stu-id="0c575-149">Any available content encoding not explicitly requested</span></span>     |

<span data-ttu-id="0c575-150">Daha fazla bilgi için bkz: [IANA resmi içerik kodlama listesi](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="0c575-150">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="0c575-151">Ara yazılım, özel ek sıkıştırma sağlayıcıları eklemenize olanak tanır `Accept-Encoding` üstbilgi değerleri.</span><span class="sxs-lookup"><span data-stu-id="0c575-151">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="0c575-152">Daha fazla bilgi için bkz: [özel sağlayıcılar](#custom-providers) aşağıda.</span><span class="sxs-lookup"><span data-stu-id="0c575-152">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="0c575-153">Ara yazılım kalite değeri tepki yeteneğine sahiptir (qvalue, `q`) sıkıştırma düzenleri önceliğini belirlemek için istemci tarafından gönderilen ağırlığı.</span><span class="sxs-lookup"><span data-stu-id="0c575-153">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="0c575-154">Daha fazla bilgi için bkz: [RFC 7231: kabul-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="0c575-154">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="0c575-155">Sıkıştırma algoritmaları kolaylığını sıkıştırma hızı ve sıkıştırma verimliliğini arasında tabidir.</span><span class="sxs-lookup"><span data-stu-id="0c575-155">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="0c575-156">*Etkinliğini* bu bağlamda sıkıştırma sonrasında çıktı boyutuna başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="0c575-156">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="0c575-157">En küçük boyuta göre en elde edilen *en iyi* sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="0c575-157">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="0c575-158">Gönderme, önbelleğe alma ve sıkıştırılmış içerik alma isteyen söz konusu üstbilgileri, aşağıdaki tabloda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="0c575-158">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="0c575-159">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="0c575-159">Header</span></span>             | <span data-ttu-id="0c575-160">Rol</span><span class="sxs-lookup"><span data-stu-id="0c575-160">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="0c575-161">İstemciden sunucuya içerik istemci için kabul edilebilir düzenleri kodlama göstermek için gönderilen.</span><span class="sxs-lookup"><span data-stu-id="0c575-161">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="0c575-162">Sunucudan yükünde içerik kodlamasını belirtmek için istemciye gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="0c575-162">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="0c575-163">Sıkıştırma oluştuğunda `Content-Length` üstbilgi kaldırılır, yanıt sıkıştırılmışsa gövde içerik değiştiğinde itibaren.</span><span class="sxs-lookup"><span data-stu-id="0c575-163">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="0c575-164">Sıkıştırma oluştuğunda `Content-MD5` üstbilgi kaldırılır, gövde içeriği değişti ve karma artık geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="0c575-164">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="0c575-165">İçeriğin MIME türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="0c575-165">Specifies the MIME type of the content.</span></span> <span data-ttu-id="0c575-166">Her yanıt belirtmelisiniz, `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="0c575-166">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="0c575-167">Ara yazılımın yanıt sıkıştırılmış belirlemek için bu değeri denetler.</span><span class="sxs-lookup"><span data-stu-id="0c575-167">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="0c575-168">Ara yazılım bir dizi belirtir [MIME türleri varsayılan](#mime-types) kodlamak, ancak değiştirin veya MIME türleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0c575-168">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="0c575-169">Değerini sunucu tarafından gönderilen `Accept-Encoding` istemcileri ve proxy'ler, `Vary` üstbilgi gösterir istemci veya önbellek proxy (farklı) değeri temel alarak yanıtlarını `Accept-Encoding` isteği üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="0c575-169">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="0c575-170">İçerikle döndürme sonucunu `Vary: Accept-Encoding` başlığıdır her ikisi de sıkıştırılmış ve sıkıştırılmamış yanıtlarını önbelleğe alınır ayrı olarak.</span><span class="sxs-lookup"><span data-stu-id="0c575-170">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="0c575-171">Yanıt sıkıştırma Ara yazılımla özelliklerini keşfedebilirsiniz [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="0c575-171">You can explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="0c575-172">Örnek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0c575-172">The sample illustrates:</span></span>

* <span data-ttu-id="0c575-173">Gzip ve özel sıkıştırma sağlayıcılarını kullanarak uygulama yanıtlarının sıkıştırılması.</span><span class="sxs-lookup"><span data-stu-id="0c575-173">The compression of app responses using gzip and custom compression providers.</span></span>
* <span data-ttu-id="0c575-174">Bir MIME türü sıkıştırma için MIME türlerini varsayılan listesine ekleme.</span><span class="sxs-lookup"><span data-stu-id="0c575-174">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="0c575-175">Paket</span><span class="sxs-lookup"><span data-stu-id="0c575-175">Package</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0c575-176">Ara yazılım projenize eklemek için bir başvuru ekleyin [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paket.</span><span class="sxs-lookup"><span data-stu-id="0c575-176">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span> <span data-ttu-id="0c575-177">Bu özellik veya üstünü ASP.NET Core 1.1 hedef uygulamaları için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0c575-177">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0c575-178">Ara yazılım projenize eklemek için bir başvuru ekleyin [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paketini veya kullanmak [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="0c575-178">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package or use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="0c575-179">Ara yazılım projenize eklemek için bir başvuru ekleyin [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paketini veya kullanmak [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="0c575-179">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package or use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="0c575-180">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0c575-180">Configuration</span></span>

<span data-ttu-id="0c575-181">Aşağıdaki kod, yanıt sıkıştırma ara yazılımı varsayılan MIME türleri için varsayılan gzip sıkıştırması ile etkinleştirmek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="0c575-181">The following code shows how to enable the Response Compression Middleware with the default gzip compression and for default MIME types.</span></span>

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
> <span data-ttu-id="0c575-182">Gibi bir araç kullanın [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), veya [Postman](https://www.getpostman.com/) ayarlamak için `Accept-Encoding` isteği başlığı ve yanıt üstbilgileri, boyutu ve gövde araştırmak.</span><span class="sxs-lookup"><span data-stu-id="0c575-182">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="0c575-183">Örnek uygulama için bir istek göndermek `Accept-Encoding` üstbilgi ve yanıt sıkıştırılmamış olup olmadığına bakın.</span><span class="sxs-lookup"><span data-stu-id="0c575-183">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="0c575-184">`Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="0c575-184">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Accept-Encoding üst bilgi içermeyen bir isteğinin sonucunu gösteren fiddler penceresi.](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="0c575-187">Örnek uygulama için bir istek göndermek `Accept-Encoding: gzip` üstbilgi ve yanıt sıkıştırılmışsa uyun.</span><span class="sxs-lookup"><span data-stu-id="0c575-187">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="0c575-188">`Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut.</span><span class="sxs-lookup"><span data-stu-id="0c575-188">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Accept-Encoding üstbilgiyle isteğinin sonucunu ve gzip değerini gösteren fiddler penceresi.](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="0c575-192">sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="0c575-192">Providers</span></span>

### <a name="gzipcompressionprovider"></a><span data-ttu-id="0c575-193">GzipCompressionProvider</span><span class="sxs-lookup"><span data-stu-id="0c575-193">GzipCompressionProvider</span></span>

<span data-ttu-id="0c575-194">Kullanım [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) gzip ile yanıtları sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="0c575-194">Use the [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) to compress responses with gzip.</span></span> <span data-ttu-id="0c575-195">Hiçbiri belirtilmezse, varsayılan sıkıştırma sağlayıcısı budur.</span><span class="sxs-lookup"><span data-stu-id="0c575-195">This is the default compression provider if none are specified.</span></span> <span data-ttu-id="0c575-196">Sıkıştırma düzeyi ile ayarlayabilirsiniz [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).</span><span class="sxs-lookup"><span data-stu-id="0c575-196">You can set the compression level with the [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).</span></span>

<span data-ttu-id="0c575-197">Gzip sıkıştırma sağlayıcısı hızlı sıkıştırma düzeyi varsayılan olarak ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), hangi değil verebilir en verimli sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="0c575-197">The gzip compression provider defaults to the fastest compression level ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="0c575-198">En verimli sıkıştırma istenirse, ara yazılım en iyi sıkıştırma için yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0c575-198">If the most efficient compression is desired, you can configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="0c575-199">Sıkıştırma düzeyi</span><span class="sxs-lookup"><span data-stu-id="0c575-199">Compression Level</span></span> | <span data-ttu-id="0c575-200">Açıklama</span><span class="sxs-lookup"><span data-stu-id="0c575-200">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="0c575-201">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="0c575-201">CompressionLevel.Fastest</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="0c575-202">Sonuçta çıktı en iyi şekilde sıkıştırılmaz olsa bile sıkıştırma mümkün olan en kısa sürede tamamlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="0c575-202">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="0c575-203">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="0c575-203">CompressionLevel.NoCompression</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="0c575-204">Sıkıştırma yok gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="0c575-204">No compression should be performed.</span></span> |
| [<span data-ttu-id="0c575-205">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="0c575-205">CompressionLevel.Optimal</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="0c575-206">Sıkıştırma tamamlamak için daha uzun sürer olsa bile yanıtları en iyi şekilde, sıkıştırılmış.</span><span class="sxs-lookup"><span data-stu-id="0c575-206">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0c575-207">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0c575-207">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0c575-208">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0c575-208">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

---

## <a name="mime-types"></a><span data-ttu-id="0c575-209">MIME türleri</span><span class="sxs-lookup"><span data-stu-id="0c575-209">MIME types</span></span>

<span data-ttu-id="0c575-210">Ara yazılım varsayılan bir sıkıştırma için MIME türlerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="0c575-210">The middleware specifies a default set of MIME types for compression:</span></span>

* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

<span data-ttu-id="0c575-211">MIME türleri yanıt sıkıştırma ara yazılım seçenekleri ile ekleme ya da değiştirin.</span><span class="sxs-lookup"><span data-stu-id="0c575-211">You can replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="0c575-212">Bu joker karakter MIME Not türleri, aşağıdaki gibi `text/*` desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="0c575-212">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="0c575-213">Örnek uygulama için bir MIME türü ekler `image/svg+xml` sıkıştırır ve ASP.NET Core başlık resmi hizmet (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="0c575-213">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0c575-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0c575-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0c575-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0c575-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

---

### <a name="custom-providers"></a><span data-ttu-id="0c575-216">Özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="0c575-216">Custom providers</span></span>

<span data-ttu-id="0c575-217">Özel sıkıştırma uygulamaları ile oluşturabileceğiniz [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider).</span><span class="sxs-lookup"><span data-stu-id="0c575-217">You can create custom compression implementations with [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider).</span></span> <span data-ttu-id="0c575-218">[EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) bu kodlama içeriğini temsil eder `ICompressionProvider` üretir.</span><span class="sxs-lookup"><span data-stu-id="0c575-218">The [EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="0c575-219">Ara yazılım, belirtilen liste temel sağlayıcı seçmek için bu bilgileri kullanır. `Accept-Encoding` isteği üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="0c575-219">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="0c575-220">Örnek uygulaması kullanarak, istemci isteği gönderdikten `Accept-Encoding: mycustomcompression` üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="0c575-220">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="0c575-221">Ara yazılım ile yanıt verir ve özel sıkıştırma uygulama kullanır bir `Content-Encoding: mycustomcompression` üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="0c575-221">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="0c575-222">İstemci sırayla çalışması özel sıkıştırma uygulaması için özel kodlama sıkıştırmasını kurabilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="0c575-222">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0c575-223">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0c575-223">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0c575-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0c575-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

<span data-ttu-id="0c575-225">Örnek uygulama için bir istek göndermek `Accept-Encoding: mycustomcompression` üstbilgi ve yanıt üstbilgileri uyun.</span><span class="sxs-lookup"><span data-stu-id="0c575-225">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="0c575-226">`Vary` Ve `Content-Encoding` üstbilgileri yanıtta mevcut.</span><span class="sxs-lookup"><span data-stu-id="0c575-226">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="0c575-227">(Gösterilmez) yanıt gövdesi örnek sıkıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="0c575-227">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="0c575-228">Sıkıştırma uygulamasında hiç `CustomCompressionProvider` sınıfının örneği.</span><span class="sxs-lookup"><span data-stu-id="0c575-228">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="0c575-229">Ancak, burada bu tür bir sıkıştırma algoritması uygulamak örnek göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="0c575-229">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Accept-Encoding üstbilgiyle isteğinin sonucunu ve mycustomcompression değerini gösteren fiddler penceresi.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="0c575-232">Güvenli bir protokol ile sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="0c575-232">Compression with secure protocol</span></span>

<span data-ttu-id="0c575-233">Güvenli bağlantı üzerinden sıkıştırılmış yanıtlar ile denetlenebilir `EnableForHttps` seçeneği varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="0c575-233">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="0c575-234">Dinamik olarak üretilen sayfalarıyla sıkıştırma kullanılarak açabilir güvenlik sorunları gibi [SUÇ](https://wikipedia.org/wiki/CRIME_(security_exploit)) ve [ihlali](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları.</span><span class="sxs-lookup"><span data-stu-id="0c575-234">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="0c575-235">Ayırmayı üstbilgisi ekleme</span><span class="sxs-lookup"><span data-stu-id="0c575-235">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0c575-236">Yanıtları sıkıştırma zaman temel `Accept-Encoding` üstbilgisi, büyük olasılıkla, yanıtı birden çok sıkıştırılmış sürümlerini ve sıkıştırılmamış bir vardır.</span><span class="sxs-lookup"><span data-stu-id="0c575-236">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="0c575-237">Birden çok sürümü var ve depolanması gereken, istemci ve proxy önbellekleri istemek için `Vary` üstbilgi eklenmiş olup bir `Accept-Encoding` değeri.</span><span class="sxs-lookup"><span data-stu-id="0c575-237">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="0c575-238">ASP.NET Core 2.0 veya sonraki sürümlerde, ara yazılımı ekler `Vary` yanıt otomatik olarak sıkıştırılır zaman üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="0c575-238">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0c575-239">Yanıtları sıkıştırma zaman temel `Accept-Encoding` üstbilgisi, büyük olasılıkla, yanıtı birden çok sıkıştırılmış sürümlerini ve sıkıştırılmamış bir vardır.</span><span class="sxs-lookup"><span data-stu-id="0c575-239">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="0c575-240">Birden çok sürümü var ve depolanması gereken, istemci ve proxy önbellekleri istemek için `Vary` üstbilgi eklenmiş olup bir `Accept-Encoding` değeri.</span><span class="sxs-lookup"><span data-stu-id="0c575-240">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="0c575-241">ASP.NET Core içinde 1.x, ekleme `Vary` üstbilgisi yanıt için el ile gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="0c575-241">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

<span data-ttu-id="0c575-242">[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="0c575-242">[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="0c575-243">Bir Nginx ters proxy zaman ardında ara yazılım sorunu</span><span class="sxs-lookup"><span data-stu-id="0c575-243">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="0c575-244">Bir istek Nginx tarafından yönlendirilirken olduğunda `Accept-Encoding` üstbilgi kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="0c575-244">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="0c575-245">Bu ara yazılımın yanıt sıkıştırmasını önler.</span><span class="sxs-lookup"><span data-stu-id="0c575-245">This prevents the middleware from compressing the response.</span></span> <span data-ttu-id="0c575-246">Daha fazla bilgi için bkz: [NGINX: sıkıştırma ve açma](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="0c575-246">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="0c575-247">Bu sorunu tarafından izlenen [Nginx (BasicMiddleware #123) için geçiş sıkıştırması tahmin](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="0c575-247">This issue is tracked by [Figure out pass-through compression for Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="0c575-248">IIS dinamik sıkıştırma ile çalışma</span><span class="sxs-lookup"><span data-stu-id="0c575-248">Working with IIS dynamic compression</span></span>

<span data-ttu-id="0c575-249">Bir etkin IIS dinamik sıkıştırma bir uygulama için devre dışı bırakmak istediğiniz sunucu düzeyinde yapılandırılan modülü varsa, ek olarak ile bunu yapabilirsiniz, *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="0c575-249">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, you can do so with an addition to your *web.config* file.</span></span> <span data-ttu-id="0c575-250">Daha fazla bilgi için bkz: [devre dışı bırakma IIS modüllerini](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="0c575-250">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="0c575-251">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="0c575-251">Troubleshooting</span></span>

<span data-ttu-id="0c575-252">Gibi bir araç kullanın [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), veya [Postman](https://www.getpostman.com/), ayarlama izin `Accept-Encoding` isteği başlığı ve yanıt üstbilgileri, boyutu ve gövde araştırmak.</span><span class="sxs-lookup"><span data-stu-id="0c575-252">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="0c575-253">Yanıt sıkıştırma Ara aşağıdaki koşullara uyan yanıtları sıkıştırır:</span><span class="sxs-lookup"><span data-stu-id="0c575-253">The Response Compression Middleware compresses responses that meet the following conditions:</span></span>

* <span data-ttu-id="0c575-254">`Accept-Encoding` Üstbilgi değeri olan mevcut `gzip`, `*`, veya belirlediğinize göre bir özel sıkıştırma sağlayıcısı eşleşen özel kodlama.</span><span class="sxs-lookup"><span data-stu-id="0c575-254">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="0c575-255">Değer olmamalıdır `identity` veya kalite değeri (qvalue, `q`) 0 (sıfır) ayarlama.</span><span class="sxs-lookup"><span data-stu-id="0c575-255">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="0c575-256">MIME türü (`Content-Type`) ayarlanmalı ve yapılandırılmış bir MIME türü eşleşmelidir [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).</span><span class="sxs-lookup"><span data-stu-id="0c575-256">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).</span></span>
* <span data-ttu-id="0c575-257">İstek içermemelidir `Content-Range` üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="0c575-257">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="0c575-258">Güvenli bir protokol (https) yanıt sıkıştırma ara yazılımı seçenekleri yapılandırılmadığı sürece istek güvenli olmayan bir protokol (http) kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="0c575-258">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="0c575-259">*Tehlike Not [yukarıda açıklanan](#compression-with-secure-protocol) güvenli içerik sıkıştırma etkinleştirirken.*</span><span class="sxs-lookup"><span data-stu-id="0c575-259">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0c575-260">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0c575-260">Additional resources</span></span>

* [<span data-ttu-id="0c575-261">Uygulama Başlatma</span><span class="sxs-lookup"><span data-stu-id="0c575-261">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="0c575-262">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="0c575-262">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="0c575-263">Mozilla Geliştirici ağ: Kabul kodlama</span><span class="sxs-lookup"><span data-stu-id="0c575-263">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="0c575-264">RFC 7231 bölüm 3.1.2.1: İçerik Codings</span><span class="sxs-lookup"><span data-stu-id="0c575-264">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="0c575-265">RFC 7230 bölüm 4.2.3: Gzip kodlama</span><span class="sxs-lookup"><span data-stu-id="0c575-265">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="0c575-266">GZIP dosya biçimi belirtimi sürümü 4.3</span><span class="sxs-lookup"><span data-stu-id="0c575-266">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
