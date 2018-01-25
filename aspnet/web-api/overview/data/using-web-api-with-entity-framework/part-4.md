---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: "Varlık İlişkileriyle işleme | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 58a9dfb621630f23b37247b96ed3a19a661857f1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="handling-entity-relations"></a>İşleme varlık ilişkileri
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](https://github.com/MikeWasson/BookService)

Bu bölümde bazı ayrıntılarını nasıl EF ilgili varlıklar yükler ve model sınıflarınızı döngüsel Gezinti özellikleri nasıl ele alınacağını açıklar. (Bu bölümde arka plan bilgi sağlar ve öğreticiyi tamamlamak için gerekli değildir. İsterseniz, geçin [bölümü 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Eager karşı geç yükleme yükleniyor

EF ilişkisel veritabanı ile kullanırken, nasıl EF ilgili verileri yükler anlamak önemlidir.

EF oluşturur SQL sorgularını görmek yararlıdır. SQL izleme için aşağıdaki kod satırını ekleyin `BookServiceContext` Oluşturucusu:

[!code-csharp[Main](part-4/samples/sample1.cs)]

/Api/books için bir GET isteği gönderirseniz, JSON aşağıdaki gibi döndürür:

[!code-console[Main](part-4/samples/sample2.cmd)]

Geçerli AuthorId defteri içeriyor olsa bile Yazar özelliği null olduğunu görebilirsiniz. EF ilgili Yazar varlıklar yüklenmemesi olmasıdır. SQL sorgusunun izleme günlüğü bu onaylar:

[!code-console[Main](part-4/samples/sample3.sql)]

SELECT deyimi Books tablosundan alır ve yazar tablo başvurmuyor.

Başvuru için işte yönteminde `BooksController` books listesini döndürür sınıfı.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Biz JSON verilerini bir parçası olarak yazar nasıl döndürebilir görelim. Entity Framework ilgili veri yüklemek için üç yol vardır: istekli yükleme, yavaş yükleniyor ve açık yükleniyor. Nasıl çalıştığını anlamak önemlidir dengelemeler ile her tekniği vardır.

### <a name="eager-loading"></a>İstekli yükleniyor

İle *istekli yükleme*, EF ilgili varlıklar ilk veritabanı sorgusunun bir parçası olarak yükler. İstekli yükleme gerçekleştirmek için kullanın **System.Data.Entity.Include** genişletme yöntemi.

[!code-csharp[Main](part-4/samples/sample5.cs)]

Bu sorguda Yazar verileri içerecek şekilde EF bildirir. Şimdi, bu değişikliği yapmak ve uygulama çalıştırırsanız, JSON verilerini şöyle görünür:

[!code-console[Main](part-4/samples/sample6.cmd)]

İzleme günlüğü EF kitap ve yazar tablolarında birleştirme gerçekleştirilen gösterir.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Yavaş yükleniyor

Bu varlık için gezinme özelliği başvuru yapıldı zaman yavaş yükleniyor ile EF ilgili varlık otomatik olarak yükler. Yavaş yükleniyor etkinleştirmek için gezinti özelliği sanal olun. Örneğin, kitap sınıfında:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Şimdi aşağıdaki kodu göz önünde bulundurun:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Yavaş yükleniyor etkinleştirildiğinde, erişme `Author` özellikte `books[0]` yazarına veritabanını sorgulamak EF neden olur.

EF ilgili varlık alır her zaman bir sorgu gönderdiğinden yavaş yükleniyor birden çok veritabanı dönüşle gerektirir. Genellikle, seri hale nesneler için devre dışı yavaş yükleniyor istersiniz. Tüm ilgili varlıklar yüklenirken tetikler model üzerinde özelliklerini okumak seri hale getirici sahiptir. Örneğin, burada alırken SQL sorguları EF defterleri listesi etkin yavaş yükleniyor ile serileştirir. EF üç yazarlar için üç ayrı sorgulara yapar görebilirsiniz.

[!code-console[Main](part-4/samples/sample10.sql)]

Ne zaman yavaş yükleniyor kullanmak isteyebilirsiniz zamanlar hala vardır. İstekli yükleme çok karmaşık bir birleşim oluşturmak EF neden olabilir. Veya küçük bir alt veri kümesine ilgili varlıklar gerekebilir ve yavaş yükleniyor daha verimli olacaktır.

Seri hale getirme sorunları önlemek için bir veri aktarımı nesneleri (DTOs) yerine varlık nesnesi seri hale getirmek için yoludur. Bu yaklaşım makalenin sonraki bölümlerinde göster.

### <a name="explicit-loading"></a>Açık yükleniyor

Kod içinde açıkça ilgili verileri almak açık yükleme yavaş yüklemeye benzerdir; bir gezinti özelliğine erişirken otomatik olarak gerçekleşmez. Açık yükleme zaman ilgili verileri yüklemek daha fazla denetim sağlar, ancak ek kod gerektirir. Açık yükleme hakkında daha fazla bilgi için bkz: [yüklenirken ilgili varlıklar](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Gezinti özellikleri ve dairesel başvurular

Kitap ve yazar modelleri tanımlandığında, t bir gezinti özelliği tanımlanan `Book` kitap yazarı ilişki sınıfı ancak t bir gezinti özelliği ters yönde tanımlamıyor.

Karşılık gelen gezinme özelliğini eklerseniz ne olur `Author` sınıfı?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Modelleri seri ne yazık ki, bu bir sorun oluşturur. İlgili verileri yüklerseniz, döngüsel bir nesne grafiğinin oluşturur.

![](part-4/_static/image1.png)

Grafik serileştirmek JSON veya XML biçimlendiricisi çalıştığında, bir özel durum oluşturur. İki biçimlendiricileri farklı özel durum iletileri durum. JSON biçimlendirici örnek aşağıda verilmiştir:

[!code-console[Main](part-4/samples/sample12.cmd)]

XML biçimlendirici şöyledir:

[!code-xml[Main](part-4/samples/sample13.xml)]

Bir çözüm, sonraki bölümde açıklanmaktadır DTOs kullanmaktır. Alternatif olarak, grafik döngüleri işlemek için JSON ve XML biçimlendiricileri yapılandırabilirsiniz. Daha fazla bilgi için bkz: [işleme döngüsel nesne başvuruları](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Bu öğretici için gerekli olmayan `Author.Book` bırakılabilir için gezinme özelliği.

>[!div class="step-by-step"]
[Önceki](part-3.md)
[sonraki](part-5.md)
