---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: ASP.NET Web API 2'deki medya Biçimlendiricileri | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 7b7ba2fb3f1bba0447e700c84a017266cba305e6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752568"
---
<a name="media-formatters-in-aspnet-web-api-2"></a>ASP.NET Web API 2'deki medya Biçimlendiricileri
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Bu öğreticide, ASP.NET Web API'de ek medya biçimleri için destek gösterilmektedir.

## <a name="internet-media-types"></a>Internet medya türleri

Bir MIME türü olarak da bilinen bir medya türü bir veri parçasını biçimini tanımlar. HTTP, ileti gövdesinin biçimi medya türleri açıklanmaktadır. Medya türü, iki dizeyi, bir tür ve alt oluşur. Örneğin:

- metin/html
- Görüntü/png
- Uygulama/json

HTTP iletisinin Varlık gövdesi içeriyorsa, Content-Type üst bilgisi, ileti gövdesinin biçimi belirtir. Bu, ileti gövdesi içeriğini ayrıştırmayı alıcı bildirir.

Örneğin, bir HTTP yanıtı bir PNG görüntüsü içeriyorsa, yanıt aşağıdaki üst bilgileri olabilir.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

İstemci bir istek iletisi gönderirken bir Accept üst bilgisi dahil edebilirsiniz. Accept üst bilgisi, sunucunun istemci medya türde sunucudan istediği söyler. Örneğin:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Bu üst bilgi, istemci HTML, XHTML veya XML istediğini sunucu söyler.

Web API'sini nasıl serileştirir ve HTTP ileti gövdesi seri durumdan çıkarır medya türünü belirler. Web API'si, XML, JSON, BSON ve form-urlencoded verileri için yerleşik desteği vardır ve ek medya türleri yazarak destekleyebilir bir *medya biçimlendiricisi*.

Bir medya biçimlendiricisi oluşturmak için bu sınıflardan birine türetilir:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Bu sınıfı kullanan zaman uyumsuz okuma ve yazma yöntemleri.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Bu sınıfın türetildiği **MediaTypeFormatter** ancak sychronous okuma/yazma yöntemleri kullanır.

Öğesinden türetme **BufferedMediaTypeFormatter** çünkü zaman uyumsuz kod yok, ancak çağıran iş parçacığını g/ç sırasında engellemek geldiğini de kolaydır.

## <a name="example-creating-a-csv-media-formatter"></a>Örnek: bir CSV medya biçimlendiricisi oluşturma

Aşağıdaki örnek, bir virgülle ayrılmış değerler (CSV) biçiminde bir ürün nesnesine serileştirebilen bir medya türü biçimlendiricisini gösterir. Bu örnekte bu öğreticide tanımlanan ürün türü [bu destekler CRUD işlemleri bir Web API'si oluşturma](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Ürün nesnesinin tanımı aşağıda verilmiştir:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Bir CSV biçimlendirici uygulamak için türetilen bir sınıf tanımlama **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

Oluşturucuda Biçimlendiricinin desteklediği medya türleriyle ekleyin. Bu örnekte, bir tek bir medya türü biçimlendirici destekler &quot;metin/csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Geçersiz kılma **CanWriteType** serialize yöntemi, biçimlendirici türlerini belirtmek için:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

Bu örnekte, biçimlendirici tek serileştirebiliyorsa `Product` nesneleri koleksiyonlarının yanı sıra `Product` nesneleri.

Benzer şekilde, geçersiz kılma **CanReadType** yöntemi, biçimlendirici türlerini belirtmek için seri durumdan çıkarabilen. Yöntemi yalnızca döndürecek şekilde bu örnekte, biçimlendirici seri durumundan çıkarma desteklemiyor **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Son olarak, geçersiz kılma **WriteToStream** yöntemi. Bu yöntem bir tür yazarak bir akışa serileştirir. Ayrıca, biçimlendirici seri durumundan çıkarma destekliyorsa, geçersiz kılma **ReadFromStream** yöntemi.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Bir medya biçimlendiricisi Web API ardışık düzenine eklemek için

Bir medya türü biçimlendiricisi Web API ardışık düzenine eklemek için **Biçimlendiricileri** özelliği **HttpConfiguration** nesne.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Karakter kodlamaları

İsteğe bağlı olarak, bir medya biçimlendiricisi UTF-8 veya ISO 8859-1 gibi birden çok karakter kodlamaları destekler.

Oluşturucusunun içinde bir veya daha fazla Ekle [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) türlerine **SupportedEncodings** koleksiyonu. Varsayılan ilk kodlama yerleştirin.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

İçinde **WriteToStream** ve **ReadFromStream** yöntemleri çağırmak [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) tercih edilen karakter kodlamasını seçin. Bu yöntem Desteklenen kodlamalar listesiyle istek üstbilgilerini eşleşir. Döndürülen kullanın **kodlama** okuma veya yazma akıştan zaman:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
