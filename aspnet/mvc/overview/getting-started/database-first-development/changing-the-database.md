---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: "EF veritabanıyla ilk ASP.NET MVC: veritabanı değiştirme | Microsoft Docs"
author: tfitzmac
description: "ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 1ffe753812e5eef817f03ab488a28ae5fcefd41e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a>EF veritabanıyla ilk ASP.NET MVC: veritabanı değiştirme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri otomatik olarak görüntüleme, düzenleme, oluşturmak kullanıcıların sağlayan kodu oluşturmak ve veritabanı tablosunda bulunan verileri silme gösterilmektedir. Oluşturulan kodun veritabanı tablosundaki sütunlarla karşılık gelir.
> 
> Veritabanı yapısında bir güncelleştirme yapmadan ve web uygulama boyunca bu değişikliği yayılıyor dizisinin bu bölümü odaklanır.


## <a name="add-a-column"></a>Bir sütun ekleyin

Bir tablo yapısı veritabanınızda güncelleştirirseniz, değişikliğinizin veri model, Görünüm ve denetleyici yayılır emin olmak gerekir.

Bu öğretici için yeni bir sütun Öğrenci Orta adını kaydetmek için Öğrenci tabloya ekleyeceksiniz. Bu sütun eklemek için veritabanı projesi açın ve Student.sql dosyasını açın. Tasarımcı veya T-SQL kodunu adlı bir sütun ekler **MiddleName** , bir NVARCHAR(50) ve NULL değerlere izin verir.

![İkinci adı ekleyin](changing-the-database/_static/image1.png)

Bu değişiklik, veritabanı projesi (veya F5) başlatarak yerel veritabanınızı dağıtın. Yeni alanın tablosuna eklenir. SQL Server nesne Gezgini'nde onu görmüyorsanız bölmesindeki Yenile düğmesini tıklatın.

![Yeni bir sütun Göster](changing-the-database/_static/image2.png)

Veritabanı tablosunda yeni bir sütun var, ancak veri modeli sınıfında şu anda yok. Model, yeni bir sütun eklemek için güncelleştirmeniz gerekir. İçinde **modelleri** klasörü, açık **ContosoModel.edmx** modeli diyagramını görüntülemek için dosya. Öğrenci model MiddleName özelliği içermiyor dikkat edin. Herhangi bir yere tasarım yüzeyine sağ tıklatın ve seçin **güncelleştirme modeli veritabanından**.

![Bir güncelleştirme modeli](changing-the-database/_static/image3.png)

Güncelleştirme Sihirbazı'nda seçin **yenileme** sekmesi ve **Öğrenci** tablo.

![Güncelleştirme Sihirbazı](changing-the-database/_static/image4.png)

**Son**'a tıklayın.

Güncelleştirme işlemi tamamlandıktan sonra veritabanı şemasını yeni içermektedir **MiddleName** özelliği. Kaydet **ContosoModel.edmx** dosya. Bu dosya için yayılması yeni bir özellik için kaydetmelisiniz **Student.cs** sınıfı. Şimdi, veritabanı ve modeli güncelleştirdiniz.

Çözümü oluşturun.

Ne yazık ki, görünümleri yeni özellik hala içermez. Görünümlerini güncelleştirmek için iki seçeneğiniz vardır - ya da görünümleri Öğrenci sınıfı için askılamayı yeniden ekleyerek yeniden oluşturabileceğiniz veya varolan görünümlerinize yeni özellik el ile ekleyebilirsiniz. Bu öğreticide, yapı iskelesi ekleyeceksiniz yeniden özelleştirilmiş değişiklikleri otomatik olarak oluşturulan görünümlerine yaptığınız değil çünkü. Görünümlere değişiklikler yaptığınızda ve bu değişiklikleri kaybetmek istiyor musunuz özelliğini el ile eklemeyi düşünebilirsiniz.

Görünümleri yeniden oluşturulan emin olmak için silme **Öğrenciler** klasörü altında **görünümleri**ve silme **StudentsController**. Sonra sağ tıklatın **denetleyicileri** klasörü ve eklemek için askılamayı **Öğrenci** modeli. Yeniden, denetleyici adı **StudentsController**. Seçin **Tamam**.

Görünümleri artık MiddleName özelliği içerir.

![Orta adını göster](changing-the-database/_static/image5.png)

Sonraki bölümde Öğrenci kayıtla ilgili ayrıntıları gösteren görünümü özelleştirmek için kod ekleyeceksiniz.

>[!div class="step-by-step"]
[Önceki](generating-views.md)
[sonraki](customizing-a-view.md)
