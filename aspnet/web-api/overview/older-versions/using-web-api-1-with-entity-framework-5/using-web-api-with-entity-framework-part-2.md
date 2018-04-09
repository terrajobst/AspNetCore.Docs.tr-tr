---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: '2. Kısım: etki alanı modelleri oluşturma | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84631494c1be266c21e5e5702182df717b1d29b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-2-creating-the-domain-models"></a>2. Kısım: etki alanı modelleri oluşturma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Model ekleme

Yaklaşım Entity Framework için üç yol vardır:

- Veritabanı ilk: bir veritabanıyla'ı başlatın ve Entity Framework kod oluşturur.
- Model ilk: görsel bir modelle'ı başlatın ve Entity Framework veritabanı ve kod oluşturur.
- Kod ilk: koduyla'ı başlatın ve Entity Framework veritabanı oluşturur.

Bizim etki alanı nesnelerini POCOs (düz eski CLR nesneler) tanımlayarak başlatmak için ilk kod yaklaşımı kullanıyoruz. Kod ilk yaklaşımda, etki alanı nesnelerini işlemleri ya da kalıcılığı gibi veritabanı katmanı desteklemek için ek kod gerekmez. (Özellikle, bunlar devralınmalıdır gerekmez [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) sınıfı.) Entity Framework veritabanı şeması nasıl oluşturduğunu denetlemek için veri ek açıklamaları kullanmaya devam edebilirsiniz.

POCOs açıklayan herhangi bir ek özellik taşımayan çünkü [veritabanı durumu](https://msdn.microsoft.com/library/system.data.entitystate.aspx), bunlar kolayca JSON veya XML seri hale getirilebilir. Daha sonra öğreticide anlatıldığı gibi Bununla birlikte, her zaman Entity Framework Modellerinizi doğrudan istemcilere maruz bırakmamalısınız anlamına gelmez.

Aşağıdaki POCOs oluşturacağız:

- Ürün
- Sırası
- OrderDetail

Her sınıf oluşturmak için Çözüm Gezgini'nde modeller klasörü sağ tıklatın. Bağlam menüsünden seçin **Ekle** ve ardından **sınıfı.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Ekleme bir `Product` aşağıdaki uygulama ile sınıfı:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Kurala göre Entity Framework kullanan `Id` özelliği birincil anahtarı olarak ve veritabanı tablosundaki kimlik sütunu eşler. Yeni bir oluşturduğunuzda `Product` örneği için bir değer ayarlamak olmaz `Id`, veritabanı değeri oluşturur.

**ScaffoldColumn** özniteliği söyler atlamak için ASP.NET MVC `Id` Düzenleyicisi form oluştururken özelliği. **Gerekli** özniteliği model doğrulamak için kullanılır. Bunu belirleyen `Name` özelliği, boş olmayan bir dize olmalıdır.

Ekleme `Order` sınıfı:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Ekleme `OrderDetail` sınıfı:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Yabancı anahtar ilişkileri

Tek bir ürün her Sipariş Ayrıntısı başvuruyor ve bir sipariş birçok sipariş ayrıntılarını içerir. Bu ilişkileri göstermek için `OrderDetail` sınıfı tanımlayan özellikleri adlı `OrderId` ve `ProductId`. Entity Framework, bu özellikler yabancı anahtarları temsil eder ve yabancı anahtar kısıtlamaları veritabanına ekler Infer.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

`Order` Ve `OrderDetail` sınıfları Ayrıca ilişkili nesneleri başvurular içeren "Gezinti" özellikleri içerir. Bir sipariş verildiğinde, ürünleri sırada Gezinti özellikleri izleyerek gidebilirsiniz.

Projeyi şimdi derleyin. Entity Framework yansıma veritabanı şeması oluşturmak derlenmiş bir bütünleştirilmiş kod gerektirdiği şekilde modellerinin özelliklerini bulmak için kullanır.

## <a name="configure-the-media-type-formatters"></a>Medya türü Biçimlendiricilerini yapılandırın

A [medya türü biçimlendiricisi](../../formats-and-model-binding/media-formatters.md) Web API HTTP yanıt gövdesi yazdığında, verilerinizi serileştiren bir nesnedir. Yerleşik biçimlendiricileri JSON ve XML çıkış destekler. Varsayılan olarak, bu biçimlendiricileri her ikisi de değerine göre tüm nesneleri serileştirir.

Döngüsel başvuru bir nesne grafiğinin içeriyorsa, serileştirme değeriyle ilgili bir sorun oluşturur. Tam olarak durumuyla olan `Order` ve `OrderDetail` her bir başvuru diğer tutan çünkü sınıfları. Biçimlendirici değeriyle her nesne yazma başvuruları izleyin ve daire olarak gidin. Bu nedenle, size varsayılan davranışı değiştirmeniz gerekir.

Çözüm Gezgini'nde, uygulama genişletin\_başlangıç klasörü ve WebApiConfig.cs adlı dosyayı açın. Aşağıdaki kodu ekleyin `WebApiConfig` sınıfı:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Bu kod, nesne başvuruları korumak için JSON biçimlendirici ayarlar ve XML biçimlendiricisi ardışık düzen tarafından tamamen kaldırır. (Nesne başvuruları korumak için XML biçimlendiricisi yapılandırabilirsiniz, ancak biraz daha fazla iş olduğunu ve yalnızca JSON bu uygulama için ihtiyacımız. Daha fazla bilgi için bkz: [işleme döngüsel nesne başvuruları](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Önceki](using-web-api-with-entity-framework-part-1.md)
> [sonraki](using-web-api-with-entity-framework-part-3.md)
