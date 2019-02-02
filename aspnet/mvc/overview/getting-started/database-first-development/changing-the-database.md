---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Öğretici: Veritabanı ile ASP.NET MVC uygulaması için EF veritabanı ilk değiştirme'
description: Bu öğreticide bir güncelleştirme yapmak için veritabanı yapısı ve web uygulama boyunca bu değişiklik yayma odaklanır.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667641"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a>Öğretici: Veritabanı ile ASP.NET MVC uygulaması için EF veritabanı ilk değiştirme

MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir. Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.

Bu öğreticide bir güncelleştirme yapmak için veritabanı yapısı ve web uygulama boyunca bu değişiklik yayma odaklanır.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Sütun ekleme
> * Görünümlere özelliği ekleme

## <a name="prerequisites"></a>Önkoşullar

* [Görünüm oluşturma](generating-views.md)

## <a name="add-a-column"></a>Sütun ekleme

Bir tablonun yapısını veritabanınızda güncelleştirirseniz, değişikliğiniz veri modeli, görünümler ve denetleyici yayıldığından emin olmak gerekir.

Bu öğreticide, Öğrenci ikinci adını kaydetmek için Öğrenci tabloya yeni bir sütun ekleyeceksiniz. Bu sütunu eklemek için veritabanı projesini açın ve Student.sql dosyasını açın. Tasarımcı veya T-SQL kodu adlı bir sütun eklemek **MiddleName** , bir NVARCHAR(50) ve NULL değerlere izin verir.

Bu değişiklik, veritabanı projesi (veya F5) başlatarak yerel veritabanınıza dağıtın. Yeni alan tablosuna eklenir. SQL Server nesne Gezgini'nde, görmüyorsanız bölmeyi yenile düğmesine tıklayın.

![Yeni bir sütun Göster](changing-the-database/_static/image2.png)

Veritabanı tablosunda yeni bir sütun var, ancak veri modeli sınıfında şu anda yok. Model, yeni bir sütun içerecek şekilde güncelleştirmeniz gerekir. İçinde **modelleri** açık klasör **ContosoModel.edmx** modeli diyagramını görüntülemek için dosya. Öğrenci model MiddleName özelliği içermiyor dikkat edin. Tasarım yüzeyinde herhangi bir yere sağ tıklatıp seçin **veritabanından bir güncelleştirme modeli**.

Güncelleştirme Sihirbazı'nda seçin **Yenile** sekmesini seçip **tabloları** > **dbo** > **Öğrenci**. **Son**'a tıklayın.

Güncelleştirme işlemi tamamlandıktan sonra veritabanı diyagramı yeni içerir **MiddleName** özelliği. Kaydet **ContosoModel.edmx** dosya. Yeni bir özellik için yayılması için bu dosyayı kaydetmeniz gerekir **Student.cs** sınıfı. Şimdi, veritabanı ve modeli de güncelleştirdik.

Çözümü oluşturun.

## <a name="add-the-property-to-the-views"></a>Görünümlere özelliği ekleme

Ne yazık ki, görünümler, yeni özellik hala içermez. Görünümlerin güncelleştirilmesi için iki seçeneğiniz vardır - ya da görünümleri yapı iskelesi Öğrenci sınıfı için bir kez yeniden ekleyerek yeniden oluşturabilirsiniz veya yeni özellik mevcut görünümlerinizde el ile ekleyebilirsiniz. Bu öğreticide, yapı iskelesi ekleyeceksiniz yeniden otomatik olarak oluşturulan görünümler için özelleştirilmiş tüm değişiklikleri yapmasanız olduğundan. Değişiklikleri görünümlerine yaptığınızda ve değişiklikleri kaybetmek istiyor musunuz özelliği el ile eklemeyi düşünebilirsiniz.

Görünümleri yeniden oluşturulmuş olduğundan emin olmak için silme **Öğrenciler** klasörü altında **görünümleri**ve silme **StudentsController**. Ardından, sağ tıklayarak **denetleyicileri** klasörü ve için yapı iskelesi Ekle **Öğrenci** modeli. Denetleyici yeniden adlandırın **StudentsController**. **Add (Ekle)** seçeneğini belirleyin.

Çözümü yeniden oluşturun. Görünümler, artık MiddleName özelliği içerir.

![İkinci Ad Göster](changing-the-database/_static/image5.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Bir sütun eklendi
> * Görünümlere özelliği eklendi

Bir öğrenci kayıt hakkındaki ayrıntıları göstermek için görünümün nasıl özelleştirileceğini öğrenmek için sonraki öğreticiye ilerleyin.
> [!div class="nextstepaction"]
> [Bir görünümü özelleştirme](customizing-a-view.md)