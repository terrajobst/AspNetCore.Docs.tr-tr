---
title: ASP.NET Core 'de istek ve yanıt işlemleri
author: jkotalik
description: İstek gövdesini okumayı ve yanıt gövdesini ASP.NET Core yazmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 08/29/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: b473fa02e1d23f02bc5d2e15fa54ab7b1dbbb17c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667218"
---
# <a name="request-and-response-operations-in-aspnet-core"></a>ASP.NET Core 'de istek ve yanıt işlemleri

, [Kotin Kotalik](https://github.com/jkotalik) tarafından

Bu makalede, istek gövdesinden okuma ve yanıt gövdesine yazma işlemleri açıklanmaktadır. Bu işlemler için kod, ara yazılım yazılırken gerekli olabilir. İşlemler MVC ve Razor Pages tarafından işlendiği için, yazma ara yazılımı dışında, özel kod genellikle gerekli değildir.

İstek ve yanıt gövdelerinde iki soyut vardır: <xref:System.IO.Stream> ve <xref:System.IO.Pipelines.Pipe>. İstek okuma için, [HttpRequest. Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) bir <xref:System.IO.Stream>ve `HttpRequest.BodyReader` bir <xref:System.IO.Pipelines.PipeReader>. Yanıt yazma için [HttpResponse. Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) bir <xref:System.IO.Stream>ve `HttpResponse.BodyWriter` bir <xref:System.IO.Pipelines.PipeWriter>.

İşlem [hatları](/dotnet/standard/io/pipelines) akışlar üzerinde önerilir. Akışlar bazı basit işlemler için daha kolay olabilir, ancak işlem hatları performans avantajına sahiptir ve çoğu senaryoda daha kolay kullanılır. ASP.NET Core dahili akışlar yerine işlem hatlarını kullanmaya başlıyor. Örneklere şunlar dahildir:

* `FormReader`
* `TextReader`
* `TextWriter`
* `HttpResponse.WriteAsync`

Akışlar çerçeveden kaldırılmıyor. Akışlar .NET genelinde çalışmaya devam eder ve birçok akış türü `FileStreams` ve `ResponseCompression`gibi kanal eşdeğerlerine sahip değildir.

## <a name="stream-examples"></a>Stream örnekleri

Hedefin, yeni satırları bölerek tüm istek gövdesini bir dizeler listesi olarak okuyan bir ara yazılım oluşturduğunu varsayalım. Basit bir akış uygulamasının aşağıdaki örnekteki gibi görünebilir:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

Bu kod işe yarar ancak bazı sorunlar vardır:

* `StringBuilder`eklemeden önce, örnek hemen oluşturulan başka bir dize (`encodedString`) oluşturur. Bu işlem akıştaki tüm baytlar için gerçekleşir, bu nedenle sonuç, tüm istek gövdesinin boyutunun fazladan bellek ayırmasından oluşur.
* Örnek, yeni satırlara bölmeden önce tüm dizeyi okur. Bayt dizisindeki yeni satırları denetlemek daha etkilidir.

Yukarıdaki sorunlardan bazılarını düzelten bir örnek aşağıda verilmiştir:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

Bu önceki örnek:

* Herhangi bir yeni satır karakteri olmadıkça tüm istek gövdesini bir `StringBuilder` arabelleğe almaz.
* Dizedeki `Split` çağırmaz.

Ancak, yine de bazı sorunlar vardır:

* Yeni satır karakterleri seyrek ise, dize içinde istek gövdesinin büyük bir bölümü arabelleğe alınır.
* Kod, dizeler oluşturmaya (`remainingString`) devam eder ve bunları dize arabelleğine ekler ve bu da ek bir ayırmaya neden olur.

Bu sorunlar düzeltilebilir, ancak kod çok daha karmaşık bir geliştirme sayesinde daha karmaşık hale geliyor. İşlem hatları, en az kod karmaşıklığı ile bu sorunları çözmenin bir yolunu sağlar.

## <a name="pipelines"></a>İşlem hatları

Aşağıdaki örnek, `PipeReader`kullanarak aynı senaryonun nasıl işlenebileceğini gösterir:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

Bu örnek, akışlar uygulamalarında bulunan birçok sorunu düzeltir:

* `PipeReader` kullanılmayan baytları işlediği için bir dize arabelleği gerekmez.
* Kodlanmış dizeler doğrudan döndürülen dizeler listesine eklenir.
* Dize oluşturma, dize tarafından kullanılan belleğin yanı sıra (`ToArray()` çağrısı dışında) ayırma ücretsizdir.

## <a name="adapters"></a>Örünü

Hem `Body` hem de `BodyReader/BodyWriter` özellikleri `HttpRequest` ve `HttpResponse`için kullanılabilir. Farklı bir akışa `Body` ayarladığınızda, yeni bir bağdaştırıcılar kümesi otomatik olarak her türü birbirlerine uyarlar. `HttpRequest.Body` yeni bir akışa ayarlarsanız, `HttpRequest.BodyReader` otomatik olarak `HttpRequest.Body`sarmalanmış yeni bir `PipeReader` ayarlanır.

## <a name="startasync"></a>StartAsync

`HttpResponse.StartAsync`, üst bilgilerin değiştirilemeyen ve `OnStarting` geri çağırmaların çalıştırılacağı olduğunu göstermek için kullanılır. Sunucu olarak Kestrel kullanırken, `PipeReader` kullanmadan önce `StartAsync` çağırmak, `GetMemory` tarafından döndürülen belleğin dış bir arabellek yerine Kestrel 'in iç <xref:System.IO.Pipelines.Pipe> ait olduğunu garanti eder.

## <a name="additional-resources"></a>Ek kaynaklar

* [System. ıO. işlem hatlarını tanıtma](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
* <xref:fundamentals/middleware/write>
