---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Medya Biçimlendiricileri ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 1cb1c7e0f832a0a0160276fbd41facc017e2ae3e
ms.sourcegitcommit: 50d40c83fa641d283c097f986dde5341ebe1b44c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2018
---
<a name="media-formatters-in-aspnet-web-api-2"></a>ASP.NET Web API 2 medya Biçimlendiricileri
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Bu öğreticide, ASP.NET Web API'de ek medya biçimlerinin desteklemek gösterilmiştir.

## <a name="internet-media-types"></a>Internet medya türleri

Bir MIME türü olarak da adlandırılan bir medya türünün bir parça veri biçimini tanımlar. HTTP, ileti gövdesinin biçimi medya türleri açıklanmaktadır. Bir medya türünün türünü ve alt olmak üzere iki dizeyi oluşur. Örneğin:

- metin/html
- Görüntü/png
- Uygulama/json

HTTP iletisinin bir varlık gövdesi içeriyorsa, Content-Type üstbilgisi ileti gövdesinin biçimi belirtir. Bu, ileti gövdesi içeriğini nasıl alıcı bildirir.

Örneğin, bir HTTP yanıtının bir PNG resim içeriyorsa, yanıt aşağıdaki üst bilgiler olabilir.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

İstemci bir istek iletisi gönderirken bir Accept üstbilgisi içerebilir. Accept üstbilgisi, sunucunun istemci hangi medya türde sunucudan istediği söyler. Örneğin:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Bu üst istemci HTML, XHTML veya XML istediği sunucusuna söyler.

Medya türü nasıl Web API serileştirir ve HTTP ileti gövdesi seri durumdan çıkarır belirler. Web API'si XML, JSON, BSON ve form urlencoded verileri için yerleşik destek vardır ve yazarak ek medya türlerini destekleyebilir bir *medya biçimlendiricisi*.

Medya biçimlendiricisi oluşturmak için bu sınıfların birinden türetilen:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Bu sınıfı kullanır zaman uyumsuz okuma ve yazma yöntemleri.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Bu sınıfın türetildiği **MediaTypeFormatter** ancak sychronous okuma/yazma yöntemleri kullanır.

Türetme **BufferedMediaTypeFormatter** zaman uyumsuz kodu yok, ancak aynı zamanda çağıran iş parçacığı g/ç sırasında engelleme gelir basittir.

## <a name="example-creating-a-csv-media-formatter"></a>Örnek: bir CSV medya biçimlendiricisi oluşturma

Aşağıdaki örnek, virgülle ayrılmış değerler (CSV) biçiminde bir Product nesnesi seri hale getirebilir medya türü biçimlendiricisini gösterir. Bu örnek öğreticide tanımlanan ürün türünü kullanan [CRUD işlemleri destekler bir Web API oluşturma](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Ürün nesnesinin tanımı aşağıda verilmiştir:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Bir CSV biçimlendirici uygulamak için türeyen bir sınıf tanımlama **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

Oluşturucuda Biçimlendiricinin desteklediği medya türleriyle ekleyin. Bu örnekte, bir tek medya türü biçimlendirici destekleyen &quot;metin/csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Geçersiz kılma **CanWriteType** serialize yöntemi, biçimlendirici türlerini belirtmek için:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

Bu örnekte, biçimlendirici tek serileştirebilen `Product` nesneleri yanı sıra koleksiyonları `Product` nesneleri.

Benzer şekilde, geçersiz kılma **CanReadType** hangi biçimlendirici türlerini belirtmek için yöntemi çıkarabilen. Yöntem yalnızca döndürecek şekilde bu örnekte, biçimlendirici seri durumdan çıkarma, desteklemiyor **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Son olarak, geçersiz kılma **WriteToStream** yöntemi. Bu yöntem, bir akışa yazarak bir türü serileştirir. Ayrıca, biçimlendirici seri durumdan çıkarma destekliyorsa, geçersiz kılma **ReadFromStream** yöntemi.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Web API ardışık medya biçimlendiricisi ekleme

Bir medya türü biçimlendiricisi Web API ardışık düzenine eklemek için kullanın **Biçimlendiricileri** özelliği **HttpConfiguration** nesnesi.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Karakter kodlamaları

İsteğe bağlı olarak, bir medya biçimlendiricisi UTF-8 veya ISO 8859-1 gibi birden çok karakter kodlamaları destekleyebilir.

Bir veya daha fazla oluşturucuda eklemek [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) türlerine **SupportedEncodings** koleksiyonu. Varsayılan ilk kodlama yerleştirin.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

İçinde **WriteToStream** ve **ReadFromStream** yöntemlerini çağırın [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) tercih edilen karakter kodlamasını seçin. Bu yöntem Desteklenen kodlamalar listesine karşı istek üstbilgileri eşleşir. Döndürülen kullanmak **kodlama** zaman okuma veya yazma akıştan:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
