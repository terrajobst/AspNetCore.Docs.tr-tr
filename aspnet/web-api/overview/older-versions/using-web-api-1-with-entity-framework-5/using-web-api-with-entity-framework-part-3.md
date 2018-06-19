---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: '3. Kısım: bir yönetim denetleyicisi oluşturma | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870550"
---
<a name="part-3-creating-an-admin-controller"></a>3. Kısım: bir yönetim denetleyicisi oluşturma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Bir yönetim denetleyicisi ekleme

Bu bölümde, CRUD destekleyen bir Web API denetleyicisi ekleyeceğiz (oluşturma, okuma, güncelleştirme ve silme) işlemleri ürünlerine. Denetleyici, Entity Framework veritabanı katmanı ile iletişim kurmak için kullanır. Yalnızca Yöneticiler bu denetleyici kullanmanız mümkün olacaktır. Müşteriler ürünleri başka bir denetleyici aracılığıyla erişir.

Çözüm Gezgini'nde denetleyicileri klasörü sağ tıklatın. Seçin **ekleme** ve ardından **denetleyicisi**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

İçinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı `AdminController`. Altında **şablonu**seçin &quot;Entity Framework kullanarak okuma/yazma eylemlerine sahip API denetleyicisi&quot;. Altında **Model sınıfı**, "Ürün (ProductStore.Models)" seçin. Altında **veri bağlamı**seçin "&lt;yeni veri bağlamı&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Varsa **Model sınıfı** açılan göstermiyor herhangi modeli sınıfları proje derlenmiş emin olun. Entity Framework yansıma, kullanır, bu nedenle derlenmiş derleme gerekiyor.


Seçme "&lt;yeni veri bağlamı&gt;" açılır **yeni veri bağlamı** iletişim. Veri bağlamı adı `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Tıklatın **Tamam** kapatmak için **yeni veri bağlamı** iletişim. İçinde **denetleyici Ekle** iletişim kutusunda, tıklatın **Ekle**.

İşte projeye eklenen:

- Adlı bir sınıf `OrdersContext` , türetilen **DbContext**. Bu sınıf, POCO modelleri ve veritabanı arasındaki Birleştirici sağlar.
- Adlı bir Web API denetleyicisi `AdminController`. Bu denetleyici üzerinde CRUD işlemleri destekleyen `Product` örnekleri. Kullandığı `OrdersContext` Entity Framework ile iletişim kurmak için sınıf.
- Web.config dosyasında yeni bir veritabanı bağlantı dizesi.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

OrdersContext.cs dosyasını açın. Bildirim Oluşturucusu veritabanı bağlantı dizesinin adını belirtir. Bu ad, bağlantı dizesi Web.config dosyasına eklendi. ifade eder.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Aşağıdaki özellikleri ekleyin `OrdersContext` sınıfı:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

A **DbSet** sorgulanabilir varlık kümesini temsil eder. Tam listesi için işte `OrdersContext` sınıfı:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

`AdminController` Sınıfı, temel CRUD işlevselliğini uygulayan beş yöntemleri tanımlar. Her bir yöntemi, istemci çağırabileceği bir URI karşılık gelir:

| Denetleyici yöntemi | Açıklama | URI | HTTP yöntemi |
| --- | --- | --- | --- |
| GetProducts | Tüm ürünleri alır. | API/ürünleri | AL |
| GetProduct | Ürün Kimliği tarafından bulur | api/products/*id* | AL |
| PutProduct | Bir ürün güncelleştirir. | api/products/*id* | PUT |
| PostProduct | Yeni bir ürün oluşturur. | API/ürünleri | YAYINLA |
| DeleteProduct | Bir ürün siler. | api/products/*id* | DELETE |

İçine her bir yöntemi çağırır `OrdersContext` veritabanını sorgulamak için. Topluluğun (PUT, POST ve Sil) değiştirme yöntemlerini çağıran `db.SaveChanges` veritabanı değişiklikleri kalıcı hale getirmek için. Denetleyicileri HTTP istek başına oluşturulan ve atıldı, bir yöntem döndürmeden önce değişiklikleri kalıcı hale getirmek gerekli olmayacak biçimde.

## <a name="add-a-database-initializer"></a>Bir veritabanı Başlatıcısı ekleme

Entity Framework veritabanı başlangıçtaki doldurmak ve modelleri değiştirdiğinizde veritabanı otomatik olarak yeniden sağlayan iyi bir özelliği vardır. Modelleri değişse bile bazı test verilerini her zaman olduğu için bu özelliği geliştirme sırasında yararlıdır.

Çözüm Gezgini'nde, modeller klasörü sağ tıklatın ve adlı yeni bir sınıf oluşturun `OrdersContextInitializer`. Aşağıdaki uygulama yapıştırın:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

İçinden devralma tarafından **DropCreateDatabaseIfModelChanges** sınıfı, biz söyleyen Entity Framework biz modeli sınıflarını değiştirmek her veritabanını bırakın. Entity Framework oluşturur (veya yeniden oluşturur,) veritabanı çağırır **çekirdek** tabloları doldurmak için yöntem. Kullanırız **çekirdek** bazı örnek ürünleri artı bir örnek sipariş ekleme yöntemi.

Bu özellik, test etmek için harikadır ancak kullanmayın **DropCreateDatabaseIfModelChanges** birisi bir model sınıfı değişirse, verilerinizi kaybedebilirsiniz çünkü üretimde,, sınıfı.

Ardından, Global.asax açın ve aşağıdaki kodu ekleyin **uygulama\_Başlat** yöntemi:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Denetleyici için bir istek gönderin

Bu noktada, size herhangi bir istemci kod yazmadınız, ancak bir web tarayıcısı ya da bir HTTP hata ayıklama kullanarak API aracı gibi web çağırabileceği [Fiddler](http://www.fiddler2.com/fiddler2/). Visual Studio'da hata ayıklamayı başlatmak için F5 tuşuna basın. Web tarayıcınız açılacak `http://localhost:*portnum*/`, burada *portnum* bazı bağlantı noktası numarasıdır.

Bir HTTP isteği Gönder "`http://localhost:*portnum*/api/admin`. Entify Framework oluşturmak ve veritabanı oluşturmak gerektiğinden ilk istek tamamlanmak için yavaş olabilir. Yanıt aşağıdakine benzer bir şey gerekir:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Önceki](using-web-api-with-entity-framework-part-2.md)
> [sonraki](using-web-api-with-entity-framework-part-4.md)
