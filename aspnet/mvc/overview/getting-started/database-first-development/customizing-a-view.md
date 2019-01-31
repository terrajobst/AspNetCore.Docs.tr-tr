---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Öğretici: ASP.NET MVC uygulaması ile EF veritabanı ilk için görünümünü özelleştirme'
description: Bu öğreticide, sunu artırmak için otomatik olarak oluşturulan görünümler değişen odaklanır.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236503"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a>Öğretici: ASP.NET MVC uygulaması ile EF veritabanı ilk için görünümünü özelleştirme

MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir. Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.

Bu öğreticide, sunu artırmak için otomatik olarak oluşturulan görünümler değişen odaklanır.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Kursları Öğrenci ayrıntıları sayfasına ekleme
> * Kursları sayfasına eklenen onaylayın

## <a name="prerequisites"></a>Önkoşullar

* [Veritabanını değiştirme](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a>Kursları Öğrenci ayrıntıları ekleyin

Oluşturulan kod, uygulamanız için iyi bir başlangıç noktası sağlar, ancak bunu mutlaka tüm uygulamanızda ihtiyacınız olan işlevleri sağlamaz. Kod, uygulamanızın belirli gereksinimlerini karşılayacak şekilde özelleştirebilirsiniz. Şu anda, uygulamanız için seçilen Öğrenci kayıtlı kursları görüntülemez. Bu bölümde, her öğrencinin için için kayıtlı kursları ekleyeceksiniz **ayrıntıları** Öğrenci için görünümü.

Açık **görünümleri** > **Öğrenciler** > *Details.cshtml*. Son aşağıda &lt;/dl&gt; etiketi, ancak kapatmadan önce &lt;/div&gt; etiketi, aşağıdaki kodu ekleyin.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Bu kod, her kayıt için bir satır için seçilen Öğrenci kayıt tabloda görüntüleyen bir tablo oluşturur. **Görünen** yöntemi HTML ifadeyi temsil eden nesne için (ModelItem) oluşturur. Görüntüleme yöntemi kullanın (yerine yalnızca özellik değeri, kod ekleme) değeri, doğru türü ve şablon türüne göre biçimlendirildiğinde emin olmak için. Bu örnekte, her ifade tek bir özellik döngüsünde geçerli kaydı döndürür ve metin olarak işlenir, ilkel türler değerlerdir.

## <a name="confirm-courses-are-added"></a>Kursları eklenen onaylayın

Çözümü çalıştırın. Tıklayın **Öğrenciler listesi** seçip **ayrıntıları** Öğrenciler biri için. Kayıtlı kursları Görünümü'nde dahil edilmiş görürsünüz.

![Öğrenci kaydı](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Öğrenci ayrıntıları sayfasına eklenen kursları
> * Kursları sayfasına eklenen Onaylandı

Doğrulama gereksinimlerini belirtin ve biçimlendirme görüntülemek için veri ek açıklamaları ekleme konusunda bilgi edinmek için sonraki öğreticiye ilerleyin.
> [!div class="nextstepaction"]
> [Veri doğrulama geliştirin](enhancing-data-validation.md)
