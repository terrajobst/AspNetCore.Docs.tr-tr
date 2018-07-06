---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Varlık ilişkilerini işleme | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: fcfddb3d56d0be641d2df9d92c334776975621be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826163"
---
<a name="handling-entity-relations"></a>Varlık ilişkilerini işleme
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](https://github.com/MikeWasson/BookService)

Bu bölümde bazı ayrıntılar EF ilgili varlıkları nasıl yükler ve model sınıflarınızı döngüsel Gezinti özelliklerini nasıl ele alınacağını açıklar. (Bu bölümde arka plan bilgileri sağlar ve bu öğreticiyi tamamlamak için gerekli değildir. Tercih ederseniz, atlamak [5. bölüm.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Eager yavaş yükleme karşılaştırması yükleniyor

EF sahip ilişkisel bir veritabanı kullanılırken EF ilgili verileri nasıl yükler anlamak önemlidir.

EF oluşturur SQL sorgularını görmek kullanışlıdır. SQL izleme için aşağıdaki kod satırını ekleyin. `BookServiceContext` Oluşturucusu:

[!code-csharp[Main](part-4/samples/sample1.cs)]

/Api/books için bir GET isteği gönderirseniz, JSON aşağıdaki gibi döndürür:

[!code-console[Main](part-4/samples/sample2.cmd)]

Kitap geçerli AuthorId içerse Yazar özelliği null olduğunu görebilirsiniz. EF ilgili Yazar varlıkları yüklenmemesi olmasıdır. İzleme günlüğü SQL sorgusu bu onaylar:

[!code-console[Main](part-4/samples/sample3.sql)]

SELECT deyimi Books tablosundan alır ve yazar tabloya başvurmuyor.

Başvuru için işte yöntemi `BooksController` kitap listesi döndüren sınıfı.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Biz Yazar JSON verilerini bir parçası olarak nasıl döndürebilir görelim. Entity Framework ilgili verileri yüklemek için üç yol vardır: açık yükleme istekli yükleme ve yavaş yükleniyor. Nasıl çalıştığını anlamak önemlidir her yöntem ile dengelemeleri vardır.

### <a name="eager-loading"></a>İstekli yükleme

İle *istekli yükleme*, EF ilgili varlıkları ilk veritabanı sorgusunun bir parçası olarak yükler. İstekli yükleme gerçekleştirmek için **System.Data.Entity.Include** genişletme yöntemi.

[!code-csharp[Main](part-4/samples/sample5.cs)]

Bu sorguda Yazar verileri içerecek şekilde EF bildirir. Şimdi, bu değişikliği yapın ve uygulamayı çalıştırın, JSON verilerini şöyle görünür:

[!code-console[Main](part-4/samples/sample6.cmd)]

İzleme günlüğü EF kitap ve yazar tablolarında birleştirme gerçekleştirilen gösterir.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Yavaş yükleniyor

Söz konusu varlığa ilişkin gezinme özelliğini başvurusu kaldırıldığında yavaş yükleniyor ile EF ilgili varlık otomatik olarak yükler. Yavaş yükleniyor etkinleştirmek için gezinme özelliği sanal olun. Örneğin, kitap sınıfında:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Aşağıdaki kodu göz önünde bulundurun:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Gecikmeli yükleme etkinleştirildiğinde erişme `Author` özelliği `books[0]` Yazar veritabanını sorgulamak EF neden olur.

EF isteğe bağlı olarak her bir ilgili varlığı alır bir sorgu gönderdiğinden yavaş yükleniyor birden çok veritabanı dönüşle gerektirir. Genellikle, seri hale getirme nesneleri için devre dışı yavaş yükleniyor istersiniz. Tüm özellikler ilgili varlıkları yükleme tetikler modeli okumak seri hale getirici sahiptir. Örneğin, EF kitap listesi ile yavaş yükleniyor etkin serileştiren olduğunda SQL sorguları aşağıdadır. EF üç yazarlar için üç ayrı sorgular yapar görebilirsiniz.

[!code-console[Main](part-4/samples/sample10.sql)]

Karşılaşılmaya devam süreleri, yavaş yükleniyor kullanmak isteyebilirsiniz. İstekli yükleme çok karmaşık bir birleştirme oluşturulacak EF neden olabilir. İlgili varlıkları verilerin küçük bir kısmı için ihtiyacınız olabilecek veya yavaş yükleniyor daha verimli olacaktır.

Serileştirme sorunları önlemek için bir veri aktarımı nesneleri (Dto) yerine varlık nesneleri serileştirmek için yoludur. Bu yaklaşım makalenin sonraki bölümlerinde aktaracağınızı göstereceğiz.

### <a name="explicit-loading"></a>Açık yükleme

Kod içinde açıkça ilgili verileri alma açık yükleme yavaş yükleniyor için benzer; bir gezinti özelliğine erişirken otomatik olarak gerçekleşmez. Açık yükleme ilgili verileri yüklemek ne zaman üzerinde daha fazla denetim verir ancak ek bir kod gerektirir. Açık yükleme hakkında daha fazla bilgi için bkz: [ilgili varlıkları yükleme](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Gezinti özellikleri ve döngüsel başvurular

Ben kitap ve yazma modelleri tanımlandığında, ben bir gezinti özelliği tanımlanmış `Book` kitap yazarı ilişki sınıfı ancak ben bir gezinti özelliği diğer yönde tanımlamıyor.

Karşılık gelen gezinme özelliğini eklerseniz ne olur `Author` sınıfı?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Ne yazık ki, modeli seri olduğunda bu sorun oluşturur. İlgili verileri yükleme, döngüsel Nesne grafiği oluşturur.

![](part-4/_static/image1.png)

Grafik seri hale getirmek JSON veya XML biçimlendiricisi çalıştığında bir özel durum oluşturur. İki biçimlendiricileri farklı bir özel durum iletileri görüntülemeyi tercih edebilirsiniz. JSON biçimlendirici için bir örnek aşağıda verilmiştir:

[!code-console[Main](part-4/samples/sample12.cmd)]

XML biçimlendirici şu şekildedir:

[!code-xml[Main](part-4/samples/sample13.xml)]

Tek bir çözüm, sonraki bölümde açıklayan ı Dto'lar kullanmaktır. Alternatif olarak, JSON ve XML biçimlendiricileri grafı döngüler işlemek için yapılandırabilirsiniz. Daha fazla bilgi için [işleme döngüsel nesne başvuruları](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Bu öğreticide, ihtiyacınız olmayan `Author.Book` bırakılabilir için gezinme özelliği.

> [!div class="step-by-step"]
> [Önceki](part-3.md)
> [İleri](part-5.md)
