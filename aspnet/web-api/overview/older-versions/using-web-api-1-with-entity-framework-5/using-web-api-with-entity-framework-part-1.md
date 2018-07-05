---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: '1. Bölüm: Genel bakış ve projeyi oluşturma | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f0616383fce2e92f7d1a0b63bf840208f7327bf7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394067"
---
<a name="part-1-overview-and-creating-the-project"></a>1. Bölüm: Genel bakış ve projeyi oluşturma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework, bir nesne/ilişkisel eşleme çerçevesidir. Kodunuzda etki alanı nesnelerini ilişkisel bir veritabanındaki varlıkları eşlenir. Çoğunlukla, Entity Framework bunu sizin için üstlenir çünkü veritabanı katmanı hakkında endişelenmeniz gerekmez. Kodunuzu nesneleri yönetir ve bir veritabanına değişiklikleri kalıcı.

## <a name="about-the-tutorial"></a>Öğretici

Bu öğreticide, bir basit depolama uygulaması oluşturacaksınız. Uygulama için iki ana bölümü vardır. Normal kullanıcıların ürünleri görüntüleyip sipariş oluşturur:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Yöneticiler oluşturamaz, silemez veya ürünleri Düzenle:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Beceriler hakkında bilgi edineceksiniz

Öğrenecekleriniz aşağıda verilmiştir:

- Entity Framework ile ASP.NET Web API kullanma
- Knockout.js dinamik istemci kullanıcı Arabirimi oluşturmak için kullanma
- Form kimlik doğrulaması, kullanıcıların kimliklerini doğrulamak için Web API'si ile kullanma

Bu öğreticiyi kendi içinde olsa da, aşağıdaki öğreticilerde ilk okumak isteyebilirsiniz:

- [İlk ASP.NET Web API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [CRUD işlemleri destekleyen bir Web API'si oluşturma](../creating-a-web-api-that-supports-crud-operations.md)

Bazı bilgisine [ASP.NET MVC](../../../../mvc/index.md) da yararlıdır.

## <a name="overview"></a>Genel Bakış

Yüksek düzeyde, uygulama mimarisinden şöyledir:

- ASP.NET MVC HTML sayfaları için istemci bilgisi oluşturur.
- ASP.NET Web API (ürünler ve siparişler) veri CRUD işlemleri sunar.
- Varlık çerçevesi veritabanı varlıklarını Web API'si tarafından kullanılan C# modelleri çevirir.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Etki alanı nesnelerini uygulamasının çeşitli katmanına nasıl temsil edildiğini Aşağıdaki diyagramda gösterilmiştir: veritabanı katmanı, nesne modeli ve son olarak istemci HTTP üzerinden veri iletmek için kullanılan gönderme biçimini.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Visual Studio projesi oluşturma

Visual Web Developer Express veya tam Visual Studio sürümünü kullanarak öğretici projesinin oluşturabilirsiniz.

Gelen **Başlat** sayfasında **yeni proje**.

İçinde **şablonları** bölmesinde **yüklü şablonlar** genişletin **Visual C#** düğümü. Altında **Visual C#** seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. "ProductStore" Projeyi adlandırın ve tıklayın **Tamam**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulaması** tıklatıp **Tamam**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

"Internet uygulama" şablonu, forms kimlik doğrulamasını destekleyen bir ASP.NET MVC uygulaması oluşturur. Uygulamayı şimdi çalıştırırsanız, zaten bazı özelliklere sahiptir:

- Yeni kullanıcılar, sağ üst köşedeki "Register" bağlantısına tıklayarak kaydedebilir.
- Kayıtlı kullanıcılar "Oturum Aç" bağlantısına tıklayarak oturum açabilir.

Üyelik bilgileri otomatik olarak oluşturulan bir veritabanında kalıcı hale getirilir. ASP.NET mvc'de form kimlik doğrulaması hakkında daha fazla bilgi için bkz: [izlenecek yol: ASP.NET mvc'de form kimlik doğrulaması kullanarak](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>CSS dosyasını güncelleştirme

Bu adım yüzeysel, ancak önceki ekran görüntüleri gibi işleme sayfaları hale getirir.

Çözüm Gezgini'nde içerik klasörünü genişletin ve Site.css adlı dosyayı açın. Aşağıdaki CSS stilleri ekleyin:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Next](using-web-api-with-entity-framework-part-2.md)
