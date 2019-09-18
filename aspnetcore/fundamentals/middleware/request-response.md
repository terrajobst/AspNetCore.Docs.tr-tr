---
title: ASP.NET Core 'de istek ve yanıt işlemleri
author: jkotalik
description: İstek gövdesini okumayı ve yanıt gövdesini ASP.NET Core yazmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 08/29/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: 5e531c0ce0ed48097054fd81ddc3655a66cc7c5f
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081681"
---
# <a name="request-and-response-operations-in-aspnet-core"></a>ASP.NET Core 'de istek ve yanıt işlemleri

, [Kotin Kotalik](https://github.com/jkotalik) tarafından

Bu makalede, istek gövdesinden okuma ve yanıt gövdesine yazma işlemleri açıklanmaktadır. Bu işlemler için kod, ara yazılım yazılırken gerekli olabilir. İşlemler MVC ve Razor Pages tarafından işlendiği için, yazma ara yazılımı dışında, özel kod genellikle gerekli değildir.

İstek ve yanıt gövdelerinin iki soyutlamaları vardır: <xref:System.IO.Stream> ve. <xref:System.IO.Pipelines.Pipe> İstek okuma için, [HttpRequest. Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) bir <xref:System.IO.Stream>ve `HttpRequest.BodyReader` olur <xref:System.IO.Pipelines.PipeReader>. Yanıt yazma için [HttpResponse. Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) bir <xref:System.IO.Stream>ve `HttpResponse.BodyWriter` olur <xref:System.IO.Pipelines.PipeWriter>.

İşlem hatları akışlar üzerinde önerilir. Akışlar bazı basit işlemler için daha kolay olabilir, ancak işlem hatları performans avantajına sahiptir ve çoğu senaryoda daha kolay kullanılır. ASP.NET Core dahili akışlar yerine işlem hatlarını kullanmaya başlıyor. Örnekler:

* `FormReader`
* `TextReader`
* `TextWriter`
* `HttpResponse.WriteAsync`

Akışlar çerçeveden kaldırılmıyor. Akışlar .net genelinde çalışmaya devam eder ve birçok akış türü, `FileStreams` ve `ResponseCompression`gibi kanal eşdeğerlerine sahip değildir.

## <a name="stream-examples"></a>Stream örnekleri

Hedefin, yeni satırları bölerek tüm istek gövdesini bir dizeler listesi olarak okuyan bir ara yazılım oluşturduğunu varsayalım. Basit bir akış uygulamasının aşağıdaki örnekteki gibi görünebilir:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

Bu kod işe yarar ancak bazı sorunlar vardır:

* ' A eklemeden önce, örnek hemen oluşturulan başka bir dize`encodedString`() oluşturur. `StringBuilder` Bu işlem akıştaki tüm baytlar için gerçekleşir, bu nedenle sonuç, tüm istek gövdesinin boyutunun fazladan bellek ayırmasından oluşur.
* Örnek, yeni satırlara bölmeden önce tüm dizeyi okur. Bayt dizisindeki yeni satırları denetlemek daha etkilidir.

Yukarıdaki sorunlardan bazılarını düzelten bir örnek aşağıda verilmiştir:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

Bu önceki örnek:

* Herhangi bir `StringBuilder` yeni satır karakteri olmadıkça tüm istek gövdesini arabelleğe almaz.
* Dizeye çağrı `Split` yapmaz.

Ancak, yine de bazı sorunlar vardır:

* Yeni satır karakterleri seyrek ise, dize içinde istek gövdesinin büyük bir bölümü arabelleğe alınır.
* Kod dizeler (`remainingString`) oluşturmaya devam eder ve bunları dize arabelleğine ekler ve bu da ek bir ayırmaya neden olur.

Bu sorunlar düzeltilebilir, ancak kod çok daha karmaşık bir geliştirme sayesinde daha karmaşık hale geliyor. İşlem hatları, en az kod karmaşıklığı ile bu sorunları çözmenin bir yolunu sağlar.

## <a name="pipelines"></a>düzenler

Aşağıdaki örnek, ile `PipeReader`aynı senaryonun nasıl işlenebileceğini göstermektedir:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

Bu örnek, akışlar uygulamalarında bulunan birçok sorunu düzeltir:

* Kullanılmayan baytları `PipeReader` işlediği için bir dize arabelleği gerekmez.
* Kodlanmış dizeler doğrudan döndürülen dizeler listesine eklenir.
* Dize oluşturma, dize tarafından kullanılan belleğin yanı sıra ( `ToArray()` çağrı dışında) ayırma ücretsizdir.

## <a name="adapters"></a>Örünü

Ve için `Body` heriki`HttpResponse`özellikdekullanılabilir. `BodyReader/BodyWriter` `HttpRequest` Farklı bir akışa `Body` ayarlandığında, yeni bir bağdaştırıcı kümesi, her türü otomatik olarak birbirlerine uyarlar. Yeni bir akışa `HttpRequest.Body` ayarlarsanız, `HttpRequest.BodyReader` otomatik olarak sarmalanmış `HttpRequest.Body`yeni `PipeReader` olarak ayarlanır.

## <a name="startasync"></a>StartAsync

`HttpResponse.StartAsync`üstbilgilerin değiştirilemeyen ve geri çağırmaların çalıştırıldığı `OnStarting` belirtmek için kullanılır. Sunucu olarak Kestrel kullanırken `StartAsync` , tarafından `GetMemory` döndürülen belleğin, `PipeReader` dış bir arabellek yerine Kestrel 'ın iç <xref:System.IO.Pipelines.Pipe> öğesine ait olduğunu garanti altına almadan önce çağrılıyor.

## <a name="additional-resources"></a>Ek kaynaklar

* [System. ıO. işlem hatlarını tanıtma](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
* <xref:fundamentals/middleware/write>
