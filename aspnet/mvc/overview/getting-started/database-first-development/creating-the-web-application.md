---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'EF veritabanıyla ilk ASP.NET MVC: veri modelleri ve Web uygulaması oluşturma | Microsoft Docs'
author: tfitzmac
description: ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 04ccc00fa48702608fdc7b5b00d73778985852f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>EF veritabanıyla ilk ASP.NET MVC: veri modelleri ve Web uygulaması oluşturma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri otomatik olarak görüntüleme, düzenleme, oluşturmak kullanıcıların sağlayan kodu oluşturmak ve veritabanı tablosunda bulunan verileri silme gösterilmektedir. Oluşturulan kodun veritabanı tablosundaki sütunlarla karşılık gelir.
> 
> Web uygulaması oluşturma ve veritabanı tablolarınızı tabanlı veri modelleri oluşturma dizisinin bu bölümü odaklanır.


## <a name="create-a-new-aspnet-web-application"></a>Yeni bir ASP.NET Web uygulaması oluşturma

Yeni bir çözüm veya veritabanı projesi olarak aynı çözüme Visual Studio'da yeni proje oluşturma ve seçin **ASP.NET Web uygulaması** şablonu. Proje adı **ContosoSite**.

![Proje oluşturma](creating-the-web-application/_static/image1.png)

**Tamam**'ı tıklatın.

Yeni ASP.NET projesi penceresinde seçin **MVC** şablonu. Temizleyebilir **bulutta Barındır** , daha sonra uygulamayı buluta dağıtacağınız çünkü şimdilik seçeneği. Tıklatın **Tamam** uygulaması oluşturmak için.

![MVC şablonunu seçin](creating-the-web-application/_static/image2.png)

Projenin varsayılan dosya ve klasörleri ile oluşturulur.

Bu öğreticide, Entity Framework 6 kullanır. Projenizdeki NuGet paketlerini Yönet penceresinin aracılığıyla Entity Framework sürümünü denetleyin. Gerekirse, Entity Framework sürümüne güncelleştirin.

![Sürüm Göster](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Modelleri oluşturma

Şimdi veritabanı tablolarından Entity Framework modelleri oluşturacak. Bu modeller verilerle çalışmak için kullanacağınız sınıflarıdır. Her model veritabanındaki bir tablo yansıtır ve tablosundaki sütunlara karşılık gelen özellikleri içerir.

Sağ **modelleri** klasörü ve select **Ekle** ve **yeni öğe**.

![Yeni Öğe Ekle](creating-the-web-application/_static/image4.png)

Yeni Öğe Ekle penceresinde seçin **veri** sol bölmede ve **ADO.NET varlık veri modeli** Orta bölmede Seçenekler. Yeni model dosyası adı **ContosoModel**.

![Model oluşturma](creating-the-web-application/_static/image5.png)

**Ekle**'yi tıklatın.

Varlık veri modeli Sihirbazı'nda seçin **veritabanından EF Designer**.

![veritabanından oluştur](creating-the-web-application/_static/image6.png)

**İleri**'ye tıklayın.

Veritabanı bağlantıları geliştirme ortamınızı içinde tanımlı varsa, önceden seçilmiş bu bağlantılarından biri görebilirsiniz. Ancak, bu öğreticinin ilk bölümünde oluşturulan veritabanı için yeni bir bağlantı oluşturmak istediğiniz. Tıklatın **yeni bağlantı** düğmesi.

![veritabanına bağlan](creating-the-web-application/_static/image7.png)

Bağlantı Özellikleri penceresinde veritabanınızı oluşturulduğu yerel sunucunun adını girin (Bu durumda **(localdb) \ProjectsV12**). Sunucu adı sağladıktan sonra ContosoUniversityData kullanılabilir veritabanlarından seçin.

![bağlantı özelliklerini ayarlama](creating-the-web-application/_static/image8.png)

**Tamam**'ı tıklatın.

Doğru bağlantı özellikleri artık görüntülenir. Web.Config dosyasında bağlantı için varsayılan adı kullanabilirsiniz

![bağlantı ayarları](creating-the-web-application/_static/image9.png)

**İleri**'ye tıklayın.

Seçin **tabloları** tüm üç tablolar için model oluşturmak için.

![tabloları seçin](creating-the-web-application/_static/image10.png)

**Son**'a tıklayın.

Bir güvenlik uyarısı alırsanız, seçin **Tamam** şablon çalıştırmaya devam etmek için.

Modeller veritabanı tablolarının oluşturulur ve özellikleri ve tablolar arasındaki ilişkileri gösteren diyagram görüntülenir.

![modelin diyagramı](creating-the-web-application/_static/image11.png)

Modeller klasörü artık veritabanından oluşturulan modelleri ilgili çok sayıda yeni dosyaları içerir.

![Yeni model dosyaları göster](creating-the-web-application/_static/image12.png)

**ContosoModel.Context.cs** dosyayı içeren türeyen bir sınıf **DbContext** sınıfı ve bir veritabanı tablosuna karşılık gelen her model sınıfı bir özellik sunar. **Course.cs**, **Enrollment.cs**, ve **Student.cs** dosyaları veritabanları tabloları temsil eden model sınıfları içerir. Yapı iskelesi ile çalışırken, bağlam sınıfını ve modeli sınıfları kullanır.

Bu öğretici ile devam etmeden önce projeyi oluşturun. Proje oluşturulmadı bölüm çalışmaz ancak bu, sonraki bölümde, veri modellerinde göre kod oluşturur.

> [!div class="step-by-step"]
> [Önceki](setting-up-database.md)
> [sonraki](generating-views.md)
