---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'İlk ASP.NET MVC ile EF veritabanında: görünümler oluşturma | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: aspnetcontent
ms.date: 12/29/2014
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 9c85b330ac415d8c73d58b31a5108197b754669f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835334"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>İlk ASP.NET MVC ile EF veritabanında: görünümler oluşturma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir. Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.
> 
> Bu serinin görünümleri ve denetleyicileri oluşturmak için ASP.NET iskeleti oluşturma kullanmaya odaklanmıştır.


## <a name="add-scaffold"></a>Yapı iskelesi Ekle

Standart veri modeli sınıfları işlemlerinde sağlayacak kodu oluşturmak hazır olursunuz. Bir yapı iskelesi öğesi ekleyerek, kodu ekleyin. Yapı iskelesi ekleyebileceğiniz türü için pek çok seçenek vardır; Bu öğreticide, iskele bir denetleyici ve önceki bölümde oluşturduğunuz Öğrenci ve kayıt modelleri karşılık gelen görünümleri içerir.

Projenizdeki tutarlılık sağlamak için yeni denetleyici varolan ekleyeceksiniz **denetleyicileri** klasör. Sağ **denetleyicileri** klasörü ve select **Ekle** – **yeni iskele kurulmuş öğe**.

![Yapı iskelesi Ekle](generating-views/_static/image1.png)

Seçin **MVC 5 denetleyici Entity Framework kullanarak görünümler ile** seçeneği. Bu seçenek, güncelleştirme, silme, oluşturma ve veri modelinizde görüntüleme için Görünüm ve denetleyici oluşturur.

![MVC denetleyicisi Ekle](generating-views/_static/image2.png)

Seçin **Öğrenci** model sınıfı ve seçin için **ContosoUniversityEntities** bağlamı sınıfı. Denetleyici adı olarak tutmak **StudentsController**,

![Denetleyici belirtin](generating-views/_static/image3.png)

**Ekle**'yi tıklatın.

Bir hata alırsanız, projeyi önceki bölümde derlenmedi olabilir. Bu durumda, projeyi derlemeyi deneyin ve ardından iskele kurulmuş öğe yeniden ekleyin.

Kod oluşturma işlemi tamamlandıktan sonra yeni bir denetleyici ve görünüm projenizde görürsünüz.

![Görünümleri Göster](generating-views/_static/image4.png)

Aynı adımları yeniden gerçekleştirin, ancak kayıt sınıfı için bir yapı iskelesi Ekle. İşiniz bittiğinde, olmalıdır bir **EnrollmentsController.cs** dosya ve klasör altında **görünümleri** adlı **kayıtları** oluşturma, silme, Ayrıntılar, düzenleme ve dizin ile Görünümler.

![Görünümleri Göster](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>Yeni görünümlere bağlantılar ekleme

Yeni görünümlerinizde gitmek kolaylaştırmak için birkaç köprüler dizin görünümlerine Öğrenciler ve kayıtlar için ekleyebilirsiniz. Dosyayı açmak **Views/Home/Index.cshtml**, siteniz için giriş sayfası olduğu. Jumbotron altına aşağıdaki kodu ekleyin.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

ActionLink için ilk parametre bağlantıdaki görüntülenecek metin yöntemidir. İkinci parametre eylemi ve üçüncü parametre denetleyicinin adı. Örneğin, ilk bağlantı StudentsController dizin eylemi işaret eder. Gerçek köprü, bu değerleri oluşturulur. İlk bağlantı Sonuçta götüren **Index.cshtml** içinde dosya **görünümler/Öğrenciler** klasör.

## <a name="display-student-views"></a>Öğrenci görünümleri görüntüler

Doğru projenize eklediğiniz kodun Öğrenciler listesini görüntüler ve kullanıcıların düzenleme, oluşturma veya veritabanında Öğrenci kayıt silme sağlayan doğrular.

Sağ **Views/Home/Index.cshtml** dosya ve seçin **tarayıcıda görüntüle**. Bu sayfada, Öğrenciler listesi için bağlantıya tıklayın.

![](generating-views/_static/image6.png)

Bu sayfada Öğrenciler ve bağlantıları bu verileri değiştirmek için listesini dikkat edin.

![Öğrenciler listesi](generating-views/_static/image7.png)

Tıklayın **Yeni Oluştur** bağlamak ve yeni bir öğrenci için bazı değerler sağlayın.

![Yeni Öğrenci oluşturma](generating-views/_static/image8.png)

Tıklayın **Oluştur**ve yeni Öğrenci listenize eklenir dikkat edin.

![Yeni Öğrenci listesi](generating-views/_static/image9.png)

Seçin **Düzenle** bağlamak ve bazı değerler için bir öğrenci değiştirin.

![öğrenciyi Düzenle](generating-views/_static/image10.png)

Tıklayın **Kaydet**, Öğrenci kayıt değiştirildi dikkat edin.

Son olarak, seçin **Sil** tıklayarak kaydı silmek istediğinizi onaylayın ve bağlama **Sil** düğmesi.

![Öğrenci Sil](generating-views/_static/image11.png)

Herhangi bir kod yazmadan Öğrenci tablodaki verileri ortak işlemleri görünümleri ekledik.

Bir alan için bir metin etiketi veritabanı özelliği dayalı olduğunu fark etmiş olabilirsiniz (gibi **LastName**) değil gerekmeyen web sayfasında görüntülemek istiyorsunuz. Örneğin, etiketin olması tercih edebilirsiniz **Soyadı**. Öğreticinin ilerleyen bölümlerinde bu görüntüleme sorunu düzeltir.

## <a name="display-enrollment-views"></a>Kayıt görünümleri görüntüler

Veritabanınızı Öğrenci ve kayıt tabloları ve kursa ve kayıt tablolar arasında bir-çok ilişki arasında bir-çok ilişkisi içerir. Kayıt görünümleri bu ilişkilerin düzgün bir şekilde işler. Sitenizi ve seçin için giriş sayfasına gidin **kayıtları listesi** bağlantısını ve ardından **Yeni Oluştur** bağlantı. Görünüm, yeni bir kayıt oluşturmak için bir form görüntüler. Özellikle, formun ilgili tablolardan değerlerle doldurulur iki açılır liste içerdiğine dikkat edin.

![kayıt oluşturma](generating-views/_static/image12.png)

Ayrıca, sağlanan değerlerin doğrulama otomatik olarak temel alan veri türünü uygulanır. Sınıf bir sayı, gerekli, uyumlu olmayan bir değer sağlamak denerseniz bir hata iletisi görüntülenir.

![doğrulama iletisi](generating-views/_static/image13.png)

Otomatik olarak oluşturulan görünümler veritabanındaki verilerle çalışmak kullanıcıları etkinleştirmek doğrulanmıştır. Bu serideki sonraki öğretici, veritabanını güncellemek ve web uygulamasında karşılık gelen değişiklikleri yapın.

> [!div class="step-by-step"]
> [Önceki](creating-the-web-application.md)
> [İleri](changing-the-database.md)
