---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'İlk ASP.NET MVC ile EF veritabanında: veri modelleri ve Web uygulaması oluşturma | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: a1c4e3365d320ee54d378de33a77666558a854d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398251"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>İlk ASP.NET MVC ile EF veritabanında: veri modelleri ve Web uygulaması oluşturma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir. Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.
> 
> Web uygulaması oluşturma ve veritabanı tablolarınızı dayalı veri modelleri oluşturma bu serinin odaklanır.


## <a name="create-a-new-aspnet-web-application"></a>Yeni bir ASP.NET Web uygulaması oluşturma

Yeni bir çözüm veya veritabanı projesini aynı çözümde Visual Studio'da yeni bir proje oluşturun ve seçin **ASP.NET Web uygulaması** şablonu. Projeyi adlandırın **ContosoSite**.

![Proje oluşturma](creating-the-web-application/_static/image1.png)

**Tamam**'ı tıklatın.

Yeni ASP.NET projesi penceresinde **MVC** şablonu. Temizleyebilir **bulutta Barındır** , daha sonra uygulamanızı buluta dağıtmadan çünkü şu an için seçenek. Tıklayın **Tamam** uygulama oluşturmak için.

![MVC şablonu seçin](creating-the-web-application/_static/image2.png)

Projenin varsayılan dosya ve klasörlerle oluşturulur.

Bu öğreticide, Entity Framework 6 kullanır. NuGet paketlerini Yönet penceresi projenizdeki Entity Framework sürümünü denetleyin. Gerekirse, Entity Framework sürümünüzü güncelleştirin.

![Sürüm Göster](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Modelleri oluşturma

Bu gibi durumlarda, Entity Framework modelleri artık veritabanı tablolarından oluşturacaksınız. Bu modeli verilerle çalışmak için kullanacağınız sınıflardır. Her model, veritabanındaki bir tabloda yansıtır ve tablosundaki sütunlara karşılık gelen özelliklerle içerir.

Sağ **modelleri** klasörü ve select **Ekle** ve **yeni öğe**.

![Yeni Öğe Ekle](creating-the-web-application/_static/image4.png)

Yeni Öğe Ekle penceresinde **veri** sol bölmede ve **ADO.NET varlık veri modeli** Orta bölmedeki seçeneklerden. Yeni model dosyası adı **ContosoModel**.

![Model oluşturma](creating-the-web-application/_static/image5.png)

**Ekle**'yi tıklatın.

Varlık veri modeli Sihirbazı'nda seçin **EF veritabanı Tasarımcısından**.

![veritabanından oluştur](creating-the-web-application/_static/image6.png)

**İleri**'ye tıklayın.

Geliştirme ortamınızda tanımlanmış veritabanı bağlantınız varsa, önceden seçilmiş Bu bağlantılardan birini görebilirsiniz. Ancak, bu öğreticinin ilk bölümünde oluşturduğunuz veritabanına yeni bir bağlantı oluşturmak istiyorsunuz. Tıklayın **yeni bağlantı** düğmesi.

![Veritabanı'na bağlanma](creating-the-web-application/_static/image7.png)

Bağlantı Özellikleri penceresinde veritabanınızı oluşturulduğu yerel sunucunun adını belirtin (Bu durumda **(localdb) \ProjectsV12**). Sunucu adı girdikten sonra ContosoUniversityData kullanılabilir veritabanlarını seçin.

![bağlantı özelliklerini ayarlama](creating-the-web-application/_static/image8.png)

**Tamam**'ı tıklatın.

Doğru bağlantı özellikleri artık görüntülenir. Web.Config dosyasında bağlantı için varsayılan adı kullanabilirsiniz

![bağlantı ayarları](creating-the-web-application/_static/image9.png)

**İleri**'ye tıklayın.

Seçin **tabloları** üç tüm tablolar için modeller oluşturmak için.

![tabloları seçme](creating-the-web-application/_static/image10.png)

**Son**'a tıklayın.

Bir güvenlik uyarısı alırsanız seçin **Tamam** şablon çalıştırmaya devam etmek için.

Veritabanı tablolarından modelleri oluşturulur ve özelliklerini ve tablolar arasındaki ilişkileri gösteren bir diyagram görüntülenir.

![modelin diyagramı](creating-the-web-application/_static/image11.png)

Modeller klasörü artık veritabanından oluşturulan modelleri ilgili çok sayıda yeni dosyaları içerir.

![Yeni model dosyaları göster](creating-the-web-application/_static/image12.png)

**ContosoModel.Context.cs** dosyası içerir, türetilen bir sınıf **DbContext** sınıfı ve bir veritabanı tablosuna karşılık gelen her bir model sınıfı için bir özellik sağlar. **Course.cs**, **Enrollment.cs**, ve **Student.cs** dosyaları veritabanı tablolarını temsil eden model sınıfları içerir. Yapı iskelesi ile çalışırken bağlam sınıfını hem model sınıfları kullanır.

Bu öğretici ile devam etmeden önce projeyi derleyin. Bölüm projesi oluşturulmadı çalışmaz ancak bu, sonraki bölümde, veri modellerine göre kod oluşturur.

> [!div class="step-by-step"]
> [Önceki](setting-up-database.md)
> [İleri](generating-views.md)
