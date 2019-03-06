---
title: ASP.NET Core istek ve yanıt işlemlerinde
author: jkotalik
description: İstek gövdesini okuyup yanıt gövdesi içinde ASP.NET Core hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jkotalik
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: f29c0aff95887d639cf3d1a8c8fe9f9228159f7a
ms.sourcegitcommit: 6ddd8a7675c1c1d997c8ab2d4498538e44954cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57400997"
---
# <a name="request-and-response-operations-in-aspnet-core"></a>ASP.NET Core istek ve yanıt işlemlerinde

Tarafından [Justin Kotalik](https://github.com/jkotalik)

Bu makalede, gövdeden okuma ve yazma için yanıt gövdesini açıklanmaktadır. Ara yazılım yazarken bu işlemler için kod yazmanız gerekebilir. Aksi takdirde, genellikle işlemleri, MVC ve Razor sayfaları tarafından işlenir. çünkü bu kodu yazmak gerekmez.

ASP.NET Core 3. 0 ', istek ve yanıt gövdeleri için iki soyutlama vardır: <xref:System.IO.Stream> ve <xref:System.IO.Pipelines.Pipe>. İsteği okumak için [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) olduğu bir <xref:System.IO.Stream>, ve `HttpRequest.BodyPipe` olduğu bir <xref:System.IO.Pipelines.PipeReader>. Yanıt yazmak için [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) olduğu bir `HttpResponse.BodyPipe` olduğu bir <xref:System.IO.Pipelines.PipeWriter>.

İşlem hatları üzerinde akışları öneririz. Akışları kullanmak için bazı basit işlemler daha kolay olabilir, ancak işlem hatları performans avantajı vardır ve çoğu senaryoda kullanmak daha kolaydır. 3. 0 ', işlem hatları akışları yerine dahili olarak kullanmak üzere ASP.NET Core başlatıyor. Örnekler:

- `FormReader`
- `TextReader`
- `TexWriter`
- `HttpResponse.WriteAsync`

Akışları kullanımdan kaldırılıyor değildir. .NET içinde kullanılacak devam etmek ve birçok akış türleri gibi kanal eşdeğerleri olmayan `FileStreams` ve `ResponseCompression`.

## <a name="stream-examples"></a>Stream örnekleri

Yeni satırlara bölme dizelerden oluşan bir liste olarak tüm istek gövdesini okuyan bir ara yazılım oluşturmak istediğinizi varsayalım. Basit bir akış uygulaması aşağıdaki örnekteki gibi görünebilir:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

Bu kod çalışır, ancak bazı sorunlar vardır:

- Sonuna ekleme önce `StringBuilder`, başka bir dize örneği oluşturur (`encodedString`), harekete hemen hemen. Sonucu ek bellek ayırma boyutu tüm istek gövdesi, bu nedenle bu işlem, tüm bayt akışı olarak gerçekleşir.
- Örneğin, yeni satırlara bölme önce dizenin tamamını okur. Bayt dizisinde yeni satırları denetlemek için daha verimli olacaktır.

Bu sorunlardan bazıları düzelten bir örnek aşağıda verilmiştir:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

Bu örnekte:

- Tüm istek gövdesinde arabelleğe almayan bir `StringBuilder` olmadığı sürece herhangi bir yeni satır karakteri.
- Değil çağırma `Split` dizesini.

Ancak, yine de bazı sorunlar şunlardır:

- Yeni satır karakterleri seyrek, istek gövdesi çoğunu dizesini arabelleğe alınıp.
- Dizeleri hala oluşturur (`remainingString`) ve bunları bir ek ayırmayı sonuçları dize arabelleğine ekler.

Bu sorun düzeltilebilir, ancak kod ile küçük geliştirme ve daha karmaşık hale. İşlem hatları, çok az kod karmaşıklığı ile ilgili sorunları çözmek için bir yol sağlar.

## <a name="pipelines"></a>İşlem hatları

Aşağıdaki örnek, aynı senaryoyu nasıl olabileceğini gösterir kullanarak işlenen bir `PipeReader`:

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

Bu örnekte akışları uygulamaları olan çok sayıda sorunu giderir:

- Gerek yoktur dize arabellek için çünkü `PipeReader` kullanılmamış bayt işler.
- Kodlanmış dizeleri doğrudan döndürülen dizelerin listesine eklenir.
- Dize oluşturulması dizesi tarafından kullanılan bellek yanı sıra ayırma ücretsiz (dışında `ToArray()` arayın).

## <a name="adapters"></a>Bağdaştırıcıları

Artık hem `Body` ve `BodyPipe` özellikler için kullanılabilir `HttpRequest` ve `HttpResponse`, ayarladığınızda ne `Body` farklı bir akışa? 3. 0'bağdaştırıcıları yeni bir dizi otomatik olarak diğer her tür uyarlayın. Örneğin, ayarlarsanız `HttpRequest.Body` yeni bir akışa `HttpRequest.BodyPipe` otomatik olarak yeni bir için ayarlanır `PipeReader` sonuna geldik `HttpRequest.Body`. Aynı davranışı ayarı için geçerli `BodyPipe` özelliği. Varsa `HttpResponse.BodyPipe` yeni bir için ayarlanır `PipeWriter`, `HttpResponse.Body` sarmalayan yeni bir akış için otomatik olarak ayarlanır `HttpResponse.BodyPipe`.

## <a name="startasync"></a>StartAsync

`HttpResponse.StartAsync` 3. 0'da yenidir. Üstbilgileri değiştirilemeyen olduğunu gösterir ve çalıştırmak için kullanılan `OnStarting` geri çağırmalar. Çağırmak zorunda 3.0-preview3, `StartAsync` kullanmadan önce `HttpRequest.BodyPipe`, gelecek sürümlerde bir öneri olacaktır. Kullanmadan önce StartAsync Kestrel sunucusu olarak kullanırken, çağırma `PipeReader` tarafından döndürülen belleğin garanti `GetMemory` Kestrel için ait iç ait <xref:System.IO.Pipelines.Pipe> yerine bir dış arabellek.

## <a name="additional-resources"></a>Ek kaynaklar

* [System.IO.Pipelines ile tanışın](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
* <xref:fundamentals/middleware/write>