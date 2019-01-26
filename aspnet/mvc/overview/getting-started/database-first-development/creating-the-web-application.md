---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Öğretici: Oluşturma veritabanı ilk ASP.NET MVC ile EF için veri modelleri ve Web uygulaması'
description: Bu makalede bir web uygulaması oluşturma ve veritabanı tablolarınızı dayalı veri modelleri oluşturma odaklanır.
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 095d355866c9ab8fba3759f3e05e2a521992f3d6
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889775"
---
# <a name="tutorial-create-the-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a>Öğretici: Oluşturma veritabanı ilk ASP.NET MVC ile EF için veri modelleri ve Web uygulaması

 MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir. Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.

Bu makalede bir web uygulaması oluşturma ve veritabanı tablolarınızı dayalı veri modelleri oluşturma odaklanır.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * ASP.NET web uygulaması oluşturma
> * Modelleri oluşturma

## <a name="prerequisites"></a>Önkoşullar

* [Entity Framework 6 veritabanı MVC 5 kullanarak First ile çalışmaya başlama](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a>ASP.NET web uygulaması oluşturma

Yeni bir çözüm veya veritabanı projesini aynı çözümde Visual Studio'da yeni bir proje oluşturun ve seçin **ASP.NET Web uygulaması** şablonu. Projeyi adlandırın **ContosoSite**.

![Proje oluşturma](creating-the-web-application/_static/image1.png)

**Tamam**'ı tıklatın.

Yeni ASP.NET projesi penceresinde **MVC** şablonu. Temizleyebilir **bulutta Barındır** , daha sonra uygulamanızı buluta dağıtmadan çünkü şu an için seçenek. Tıklayın **Tamam** uygulama oluşturmak için.

Projenin varsayılan dosya ve klasörlerle oluşturulur.

Bu öğreticide, Entity Framework 6 kullanır. NuGet paketlerini Yönet penceresi projenizdeki Entity Framework sürümünü denetleyin. Gerekirse, Entity Framework sürümünüzü güncelleştirin.

![Sürüm Göster](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Modelleri oluşturma

Bu gibi durumlarda, Entity Framework modelleri artık veritabanı tablolarından oluşturacaksınız. Bu modeli verilerle çalışmak için kullanacağınız sınıflardır. Her model, veritabanındaki bir tabloda yansıtır ve tablosundaki sütunlara karşılık gelen özelliklerle içerir.

Sağ **modelleri** klasörü ve select **Ekle** ve **yeni öğe**.

Yeni Öğe Ekle penceresinde **veri** sol bölmede ve **ADO.NET varlık veri modeli** Orta bölmedeki seçeneklerden. Yeni model dosyası adı **ContosoModel**.

**Ekle**'yi tıklatın.

Varlık veri modeli Sihirbazı'nda seçin **EF veritabanı Tasarımcısından**.

**İleri**'ye tıklayın.

Geliştirme ortamınızda tanımlanmış veritabanı bağlantınız varsa, önceden seçilmiş Bu bağlantılardan birini görebilirsiniz. Ancak, bu öğreticinin ilk bölümünde oluşturduğunuz veritabanına yeni bir bağlantı oluşturmak istiyorsunuz. Tıklayın **yeni bağlantı** düğmesi.

Bağlantı Özellikleri penceresinde veritabanınızı oluşturulduğu yerel sunucunun adını belirtin (Bu durumda **(localdb) \Projects13**). Sunucu adı girdikten sonra ContosoUniversityData kullanılabilir veritabanlarını seçin.

![bağlantı özelliklerini ayarlama](creating-the-web-application/_static/image8.png)

**Tamam**'ı tıklatın.

Doğru bağlantı özellikleri artık görüntülenir. Web.Config dosyasında bağlantı için varsayılan adı kullanabilirsiniz.

**İleri**'ye tıklayın.

Entity Framework'ün en son sürümü seçin.

**İleri**'ye tıklayın.

Seçin **tabloları** üç tüm tablolar için modeller oluşturmak için.

**Son**'a tıklayın.

Bir güvenlik uyarısı alırsanız seçin **Tamam** şablon çalıştırmaya devam etmek için.

Veritabanı tablolarından modelleri oluşturulur ve özelliklerini ve tablolar arasındaki ilişkileri gösteren bir diyagram görüntülenir.

![modelin diyagramı](creating-the-web-application/_static/image11.png)

Modeller klasörü artık veritabanından oluşturulan modelleri ilgili çok sayıda yeni dosyaları içerir.

**ContosoModel.Context.cs** dosyası içerir, türetilen bir sınıf **DbContext** sınıfı ve bir veritabanı tablosuna karşılık gelen her bir model sınıfı için bir özellik sağlar. **Course.cs**, **Enrollment.cs**, ve **Student.cs** dosyaları veritabanı tablolarını temsil eden model sınıfları içerir. Yapı iskelesi ile çalışırken bağlam sınıfını hem model sınıfları kullanır.

Bu öğretici ile devam etmeden önce projeyi derleyin. Bölüm projesi oluşturulmadı çalışmaz ancak bu, sonraki bölümde, veri modellerine göre kod oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * ASP.NET web uygulaması oluşturuldu
> * Model

Nasıl oluşturulacağını öğrenmek için sonraki makaleye ilerleyin veri modellerine göre kod oluşturur.
> [!div class="nextstepaction"]
> [Görünüm oluşturma](generating-views.md)