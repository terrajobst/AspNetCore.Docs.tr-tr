---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: '1. Kısım: Genel bakış ve proje oluşturma | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9cdff0cb0cad9adad546c8f8d46ba9b010e1079
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-creating-the-project"></a>1. Kısım: Genel bakış ve projesi oluşturma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework bir nesne/ilişkisel bir eşleme çerçevedir. İlişkisel bir veritabanındaki varlıklar için etki alanı nesnelerini kodunuzda eşler. Çoğunlukla, Entity Framework bunu sizin için mvc'deki olduğundan veritabanı katmanı hakkında endişelenmeniz gerekmez. Kodunuzu nesneleri yönetir ve değişiklikler veritabanına kalıcı.

## <a name="about-the-tutorial"></a>Öğretici hakkında

Bu öğreticide, bir basit depolama uygulaması oluşturacaksınız. Uygulama için iki ana bölümü vardır. Normal kullanıcılara ürünleri görüntülemek ve siparişleri oluşturun:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Yöneticiler oluşturamaz, silemez veya ürünleri düzenleyin:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Bilgi edineceksiniz yetenekleri

Öğrenecekleriniz aşağıda verilmiştir:

- Entity Framework ile ASP.NET Web API kullanma
- Knockout.js dinamik istemci kullanıcı Arabirimi oluşturmak için nasıl kullanılacağını.
- Form kimlik doğrulaması Web API ile kullanıcıların kimliklerini doğrulamak için nasıl kullanılacağını.

Bu öğretici müstakil olsa da, aşağıdaki öğreticiler ilk okumak isteyebilirsiniz:

- [İlk ASP.NET Web API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [CRUD işlemleri destekleyen bir Web API oluşturma](../creating-a-web-api-that-supports-crud-operations.md)

Bazı bilgisine [ASP.NET MVC](../../../../mvc/index.md) de yararlıdır.

## <a name="overview"></a>Genel Bakış

Yüksek bir düzeyde uygulama mimarisi şöyledir:

- ASP.NET MVC için istemci HTML sayfaları oluşturur.
- ASP.NET Web API CRUD işlemleri verileri (ürünleri ve siparişleri) kullanıma sunar.
- Entity Framework veritabanı varlıklarının içinde Web API tarafından kullanılan C# modelleri çevirir.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Etki alanı nesnelerini uygulamasının çeşitli katmanına nasıl temsil edildiğini Aşağıdaki diyagramda gösterilmiştir: veritabanı katmanı, nesne modeli ve son olarak istemci HTTP üzerinden veri iletmek için kullanılan gönderme biçimini.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Visual Studio projesi oluşturma

Visual Web Developer Express veya Visual Studio'nın tam sürümünü kullanarak Eğitmen proje oluşturabilirsiniz.

Gelen **Başlat** sayfasında, **yeni proje**.

İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü. Altında **Visual C#**seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. "ProductStore" adını verin ve projeyi tıklatın **Tamam**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulama** tıklatıp **Tamam**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

"Internet uygulama" şablonu form kimlik doğrulamasını destekleyen bir ASP.NET MVC uygulaması oluşturur. Uygulamayı şimdi çalıştırırsanız, zaten bazı özelliklere sahiptir:

- Yeni kullanıcılar sağ üst köşesinde yer alan "Register" bağlantısını tıklatarak kaydedebilirsiniz.
- Kayıtlı kullanıcılar "Oturum" bağlantısını tıklatarak oturum açabilir.

Üyelik bilgilerini otomatik olarak oluşturulan bir veritabanında kalıcı hale getirilir. ASP.NET MVC form kimlik doğrulaması hakkında daha fazla bilgi için bkz: [izlenecek yol: ASP.NET MVC, form kimlik doğrulaması kullanarak](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>CSS dosyası güncelleştir

Bu adım yüzeysel bağlıdır, ancak önceki ekran görüntüleri gibi işleme sayfaları hale getirir.

Çözüm Gezgini'nde, içerik klasörünü genişletin ve Site.css adlı dosyayı açın. Aşağıdaki CSS stil ekleyin:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Next](using-web-api-with-entity-framework-part-2.md)
