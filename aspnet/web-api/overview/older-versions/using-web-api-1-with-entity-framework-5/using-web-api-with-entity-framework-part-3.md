---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: '3. Bölüm: yönetim denetleyicisi oluşturma | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: caf7649f2e420b4063966fd1a371c3d9a5eb4d04
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815463"
---
<a name="part-3-creating-an-admin-controller"></a>3. Bölüm: yönetim denetleyicisi oluşturma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Yönetim Denetleyicisi Ekle

Bu bölümde, CRUD destekleyen bir Web API denetleyicisi ekleyeceğiz (oluşturma, okuma, güncelleştirme ve silme) ürünleri işlemleri. Denetleyici, Entity Framework veritabanı katmanı ile iletişim kurmak için kullanır. Yalnızca Yöneticiler bu denetleyici kullanmanız mümkün olacaktır. Müşteriler, ürünler başka bir denetleyici erişir.

Çözüm Gezgini'nde denetleyicileri klasörüne sağ tıklayın. Seçin **ekleme** ardından **denetleyicisi**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

İçinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı `AdminController`. Altında **şablon**seçin &quot;Entity Framework kullanarak okuma/yazma eylemleri olan API denetleyicisi&quot;. Altında **Model sınıfı**, "Ürün (ProductStore.Models)" seçin. Altında **veri bağlamı**seçin "&lt;yeni veri bağlamı&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Varsa **Model sınıfı** açılan göstermiyor herhangi bir model sınıfları, projeye derlenmiş olduğundan emin olun. Entity Framework, yansıma, kullanır, bu nedenle derlenmiş bütünleştirilmiş kodu gerekiyor.


Seçme "&lt;yeni veri bağlamı&gt;" açılır **yeni veri bağlamı** iletişim. Veri bağlamının adı `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Tıklayın **Tamam** kapatmak için **yeni veri bağlamı** iletişim. İçinde **denetleyici Ekle** iletişim kutusunda, tıklayın **Ekle**.

Projeye eklenen aşağıda verilmiştir:

- Adlı bir sınıf `OrdersContext` türetilen **DbContext**. Bu sınıf, bağlantılı POCO modelleri ve veritabanı arasındaki sağlar.
- Adlı bir Web API denetleyicisi `AdminController`. Bu denetleyici üzerinde CRUD işlemleri destekleyen `Product` örnekleri. Kullandığı `OrdersContext` Entity Framework ile iletişim kurmak için sınıf.
- Web.config dosyasında yeni bir veritabanı bağlantı dizesi.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

OrdersContext.cs dosyasını açın. Bildirim oluşturucu veritabanı bağlantı dizesinin adını belirtir. Bu ad, bağlantı dizesini Web.config dosyasına eklenen ifade eder.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Aşağıdaki özellikleri `OrdersContext` sınıfı:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

A **olan DB** sorgulanabilir varlık kümesini temsil eder. Tam liste için işte `OrdersContext` sınıfı:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

`AdminController` Sınıfı, temel CRUD işlevselliği uygulamak beş yöntemleri tanımlar. Her yöntem istemci çağırabileceği bir URI karşılık gelmektedir:

| Denetleyici yöntemi | Açıklama | URI | HTTP yöntemi |
| --- | --- | --- | --- |
| GetProducts | Tüm ürünler alır. | API/ürünleri | AL |
| GetProduct | Bir ürün kimliğine göre bulur | API/ürünler/*kimliği* | AL |
| PutProduct | Bir ürün güncelleştirir. | API/ürünler/*kimliği* | PUT |
| PostProduct | Yeni bir ürün oluşturur. | API/ürünleri | YAYINLA |
| DeleteProduct | Bir ürün siler. | API/ürünler/*kimliği* | DELETE |

Her yöntem içine yapılan çağrılar `OrdersContext` veritabanını sorgulamak için. Koleksiyon (PUT, POST ve DELETE) değiştirme yöntemlerini çağıran `db.SaveChanges` veritabanına değişiklikleri kalıcı hale getirmek için. Denetleyicileri HTTP istek başına oluşturulan ve atıldı, bir yöntem döndürmeden önce değişiklikleri kalıcı hale getirmek için gerekli olacak şekilde.

## <a name="add-a-database-initializer"></a>Bir veritabanı Başlatıcı Ekle

Entity Framework başlangıçta veritabanını doldurmak ve modelleri değiştirdiğinizde veritabanını otomatik olarak yeniden olanak tanıyan kullanışlı bir özelliğine sahiptir. Modelleri değiştirseniz bile bazı test verilerini her zaman sahip olduğundan bu geliştirme sırasında yararlı bir özelliktir.

Çözüm Gezgini'nde, modeller klasörü sağ tıklatın ve adlı yeni bir sınıf oluşturun `OrdersContextInitializer`. Aşağıdaki uygulama içinde yapıştırın:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Öğesinden devralan tarafından **DropCreateDatabaseIfModelChanges** sınıfı, biz söyleyen model sınıfları ki değiştirme her veritabanını bırakmak için Entity Framework. Entity Framework oluşturur (veya yeniden oluşturur) veritabanı, onu çağırır **çekirdek** tablolarını doldurmak için yöntemi. Kullandığımız **çekirdek** bazı örnek ürünlerinin yanı sıra örnek sipariş eklemek için yöntemi.

Bu özelliği test etmek için harika bir deneyimdir ancak kullanmayın **DropCreateDatabaseIfModelChanges** sınıfı üretim ortamında, çünkü birisi bir model sınıfı değişirse, veri kaybına neden olabilir.

Ardından, Global.asax açın ve aşağıdaki kodu ekleyin **uygulama\_Başlat** yöntemi:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Denetleyici için bir istek gönderin

Bu noktada, biz herhangi bir istemci kodu yazmadınız, ancak bir web tarayıcısı ya da bir HTTP hata ayıklama kullanarak API aracı gibi web çağırabilirsiniz [Fiddler](http://www.fiddler2.com/fiddler2/). Visual Studio'da hata ayıklamayı başlatmak için F5 tuşuna basın. Web tarayıcınızda açılacak `http://localhost:*portnum*/`burada *portnum* bazı bağlantı noktası numarasıdır.

Bir HTTP isteği Gönder "`http://localhost:*portnum*/api/admin`. İlk istek, oluşturma ve veritabanının çekirdeğini oluşturma Entify Framework gerektiğinden tamamlanmak için yavaş olabilir. Yanıt, aşağıdakine benzer bir şey gerekir:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Önceki](using-web-api-with-entity-framework-part-2.md)
> [İleri](using-web-api-with-entity-framework-part-4.md)
