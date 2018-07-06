---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'İlk ASP.NET MVC ile EF veritabanında: veritabanını değiştirme | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: aspnetcontent
ms.date: 10/01/2014
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 0176274694426a527ed0862b5138919d4cf5c319
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840948"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a>İlk ASP.NET MVC ile EF veritabanında: veritabanını değiştirme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir. Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.
> 
> Bu serinin veritabanı yapısında bir güncelleştirme yapmak ve web uygulama boyunca bu değişiklik yayma odaklanır.


## <a name="add-a-column"></a>Sütun ekleme

Bir tablonun yapısını veritabanınızda güncelleştirirseniz, değişikliğiniz veri modeli, görünümler ve denetleyici yayıldığından emin olmak gerekir.

Bu öğreticide, Öğrenci ikinci adını kaydetmek için Öğrenci tabloya yeni bir sütun ekleyeceksiniz. Bu sütunu eklemek için veritabanı projesini açın ve Student.sql dosyasını açın. Tasarımcı veya T-SQL kodu adlı bir sütun eklemek **MiddleName** , bir NVARCHAR(50) ve NULL değerlere izin verir.

![İkinci ad ekleyin](changing-the-database/_static/image1.png)

Bu değişiklik, veritabanı projesi (veya F5) başlatarak yerel veritabanınıza dağıtın. Yeni alan tablosuna eklenir. SQL Server nesne Gezgini'nde, görmüyorsanız bölmeyi yenile düğmesine tıklayın.

![Yeni bir sütun Göster](changing-the-database/_static/image2.png)

Veritabanı tablosunda yeni bir sütun var, ancak veri modeli sınıfında şu anda yok. Model, yeni bir sütun içerecek şekilde güncelleştirmeniz gerekir. İçinde **modelleri** açık klasör **ContosoModel.edmx** modeli diyagramını görüntülemek için dosya. Öğrenci model MiddleName özelliği içermiyor dikkat edin. Tasarım yüzeyinde herhangi bir yere sağ tıklatıp seçin **veritabanından bir güncelleştirme modeli**.

![bir güncelleştirme modeli](changing-the-database/_static/image3.png)

Güncelleştirme Sihirbazı'nda seçin **Yenile** sekmesi ve **Öğrenci** tablo.

![Güncelleştirme Sihirbazı](changing-the-database/_static/image4.png)

**Son**'a tıklayın.

Güncelleştirme işlemi tamamlandıktan sonra veritabanı diyagramı yeni içerir **MiddleName** özelliği. Kaydet **ContosoModel.edmx** dosya. Yeni bir özellik için yayılması için bu dosyayı kaydetmeniz gerekir **Student.cs** sınıfı. Şimdi, veritabanı ve modeli de güncelleştirdik.

Çözümü oluşturun.

Ne yazık ki, görünümler, yeni özellik hala içermez. Görünümlerin güncelleştirilmesi için iki seçeneğiniz vardır - ya da görünümleri yapı iskelesi Öğrenci sınıfı için bir kez yeniden ekleyerek yeniden oluşturabilirsiniz veya yeni özellik mevcut görünümlerinizde el ile ekleyebilirsiniz. Bu öğreticide, yapı iskelesi ekleyeceksiniz yeniden otomatik olarak oluşturulan görünümler için özelleştirilmiş tüm değişiklikleri yapmasanız olduğundan. Değişiklikleri görünümlerine yaptığınızda ve değişiklikleri kaybetmek istiyor musunuz özelliği el ile eklemeyi düşünebilirsiniz.

Görünümleri yeniden oluşturulmuş olduğundan emin olmak için silme **Öğrenciler** klasörü altında **görünümleri**ve silme **StudentsController**. Ardından, sağ tıklayarak **denetleyicileri** klasörü ve için yapı iskelesi Ekle **Öğrenci** modeli. Denetleyici yeniden adlandırın **StudentsController**. Seçin **Tamam**.

Görünümler, artık MiddleName özelliği içerir.

![İkinci Ad Göster](changing-the-database/_static/image5.png)

Sonraki bölümde, bir öğrenci kayıt hakkındaki ayrıntıları göstermek için bu görünümü özelleştirmek için kod ekleyeceksiniz.

> [!div class="step-by-step"]
> [Önceki](generating-views.md)
> [İleri](customizing-a-view.md)
