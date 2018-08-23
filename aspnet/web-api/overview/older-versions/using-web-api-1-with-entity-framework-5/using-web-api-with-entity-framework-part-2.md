---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: '2. Bölüm: etki alanı modellerini oluşturma | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: cb98f42df411a7ba12ff4566c30ddfbf253253d4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756318"
---
<a name="part-2-creating-the-domain-models"></a>2. Bölüm: etki alanı modellerini oluşturma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Model ekleme

Yaklaşım Entity Framework için üç yol vardır:

- Veritabanı ilk: bir veritabanı ile başlayın ve Entity Framework kod oluşturur.
- Model-first: görsel bir model ile başlayın ve Entity Framework, hem veritabanı hem de kodu oluşturur.
- Kod öncelikli: kodla başlayın ve Entity Framework veritabanı oluşturur.

Bizim etki alanı nesnelerini POCOs (düz eski CLR nesneler) tanımlayarak başlatmak için ilk kod yaklaşımı kullanıyoruz. Code first yaklaşımıyla etki alanı nesnelerini, veritabanı katmanı, işlem veya Kalıcılık gibi desteklemek için ek bir kod gerekmez. (Özellikle, bunlar devralınacak gerekmez [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) sınıfı.) Entity Framework veritabanı şeması nasıl oluşturduğunu denetlemek için veri ek açıklamaları kullanmaya devam edebilirsiniz.

POCOs açıklayan herhangi bir ek özellikleri taşımayan çünkü [veritabanı durumu](https://msdn.microsoft.com/library/system.data.entitystate.aspx), bunlar kolayca JSON veya XML seri hale getirilebilir. Öğreticinin ilerleyen bölümlerinde anlatıldığı gibi Bununla birlikte, her zaman doğrudan istemcilere Entity Framework Modellerinizi sunmalıdır anlamına gelmez.

Aşağıdaki POCOs oluşturacağız:

- Ürün
- Sırası
- OrderDetail

Her bir sınıf oluşturmak için Çözüm Gezgini'nde modeller klasörü sağ tıklatın. Bağlam menüsünden seçin **Ekle** seçip **sınıfı.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Ekleme bir `Product` aşağıdaki uygulama ile sınıfı:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Kural gereği, Entity Framework kullanan `Id` özelliği birincil anahtarı olarak ve bir veritabanı tablosundaki kimlik sütunu eşlenir. Yeni bir oluşturduğunuzda `Product` örneği için bir değer ayarlamanız gerekmez `Id`, veritabanı değeri oluşturur.

**ScaffoldColumn** özniteliği söyler atlamak için ASP.NET MVC `Id` Düzenleyicisi form oluşturulurken kullanılan özellik. **Gerekli** özniteliği model doğrulamak için kullanılır. Belirtir `Name` özelliği, boş olmayan bir dize olmalıdır.

Ekleme `Order` sınıfı:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Ekleme `OrderDetail` sınıfı:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Yabancı anahtar ilişkileri

Çok sayıda sipariş ayrıntılarını sipariş içerir ve her Sipariş Ayrıntısı için tek bir ürün ifade eder. Bu ilişkileri göstermek için `OrderDetail` sınıf adında özelliklere tanımlar `OrderId` ve `ProductId`. Entity Framework, bu özellikleri temsil eden yabancı anahtarlar ve yabancı anahtar kısıtlamaları veritabanına ekler çıkarımlar.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

`Order` Ve `OrderDetail` sınıfları, ilgili nesnelere başvurular "Gezinti" özelliklerini de içerir. Bir sipariş göz önünde bulundurulduğunda, siparişteki ürünleri gezinme özelliklerini izleyerek gidebilirsiniz.

Şimdi, projeyi derleyin. Entity Framework veritabanı şeması oluşturmak derlenmiş bir bütünleştirilmiş kod gerektirir böylece modellerinin özelliklerini bulmak için yansıtma kullanır.

## <a name="configure-the-media-type-formatters"></a>Medya türü Biçimlendiricileri yapılandırın

A [medya türü biçimlendiricisi](../../formats-and-model-binding/media-formatters.md) Web API HTTP yanıt gövdesinde yazdığında, verilerinizi serileştiren bir nesnedir. Yerleşik biçimlendiricileri JSON ve XML çıkışı destekler. Varsayılan olarak, her ikisi de bu biçimlendiricileri değere göre tüm nesneleri serileştirmek.

Bir nesne grafiğinin döngüsel başvurular içeriyorsa serileştirme değeriyle ilgili bir sorun oluşturur. Durum tam olarak olan `Order` ve `OrderDetail` her diğerine bir başvuru bulunduğundan, sınıfları. Biçimlendirici başvuruları değere göre her nesne yazarken izleyin ve daire olarak gidin. Bu nedenle, varsayılan davranışı değiştirmek ihtiyacımız var.

Çözüm Gezgini'nde uygulama genişletin\_başlangıç klasörü ve WebApiConfig.cs adlı dosyayı açın. Aşağıdaki kodu ekleyin `WebApiConfig` sınıfı:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Bu kod, nesne başvuruları korumak için JSON biçimlendiricisini ayarlar ve XML biçimlendiricisi ardışık düzen tarafından tamamen kaldırır. (Nesne başvuruları korumak için XML biçimlendiricisi yapılandırabilirsiniz ancak biraz daha fazla iş olduğunu ve bu uygulama için yalnızca JSON gerekiyor. Daha fazla bilgi için [işleme döngüsel nesne başvuruları](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Önceki](using-web-api-with-entity-framework-part-1.md)
> [İleri](using-web-api-with-entity-framework-part-3.md)
