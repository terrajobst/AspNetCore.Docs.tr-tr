---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: "EF veritabanıyla ilk ASP.NET MVC: görünümleri oluşturma | Microsoft Docs"
author: tfitzmac
description: "ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 5fccb3c56af0945ec448becff777a3e92dc160d7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>EF veritabanıyla ilk ASP.NET MVC: görünümler oluşturma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri otomatik olarak görüntüleme, düzenleme, oluşturmak kullanıcıların sağlayan kodu oluşturmak ve veritabanı tablosunda bulunan verileri silme gösterilmektedir. Oluşturulan kodun veritabanı tablosundaki sütunlarla karşılık gelir.
> 
> Bu serinin parçası görünümleri ve denetleyicilerini oluşturmak için ASP.NET yapı İskelesi kullanma üzerine odaklanmıştır.


## <a name="add-scaffold"></a>İskele Ekle

Standart veri işlemleri modeli sınıfları için sağlayacak kodu oluşturmak hazır olursunuz. İskele öğesini ekleyerek kodu ekleyin. Ekleyebilirsiniz yapı iskelesi türü için birçok seçeneğiniz vardır; Bu öğreticide, iskele bir denetleyici ve önceki bölümde oluşturduğunuz Öğrenci ve kayıt modelleri karşılık görünümleri içerir.

Projenizi tutarlılığını korumak için yeni denetleyicisi varolan ekleyecek **denetleyicileri** klasör. Sağ **denetleyicileri** klasörü ve select **Ekle** – **yeni iskele kurulmuş öğe**.

![iskele Ekle](generating-views/_static/image1.png)

Seçin **Entity Framework kullanarak MVC 5 denetleyici, görünümleri olan** seçeneği. Bu seçenek, denetleyici ve güncelleştirme, silme, oluşturma ve veri modelinizi görüntülemek için görünümler oluşturur.

![MVC denetleyicisi ekleme](generating-views/_static/image2.png)

Seçin **Öğrenci** model sınıfı ve select **ContosoUniversityEntities** bağlam sınıfını için. Denetleyici adı olarak tutun **StudentsController**,

![Denetleyici belirtin](generating-views/_static/image3.png)

**Ekle**'yi tıklatın.

Bir hata alırsanız, projeyi önceki bölümde oluşturmak değil çünkü olabilir. Öyleyse projeyi oluşturmayı deneyin ve ardından kurulmuş öğe yeniden ekleyin.

Kod oluşturma işlemi tamamlandıktan sonra yeni bir denetleyici ve görünüm projenizde görürsünüz.

![Görünümleri Göster](generating-views/_static/image4.png)

Aynı adımları yeniden gerçekleştirir, ancak kayıt sınıfı için iskele Ekle. Tamamlandığında, olmalıdır bir **EnrollmentsController.cs** dosya ve klasörü altında **görünümleri** adlı **kayıtları** oluştur, Sil, ayrıntıları, düzenleme ve dizin ile Görünümler.

![Görünümleri Göster](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>Bağlantılar için yeni görünümler ekleme

Yeni görünümlerinize gitmek kolaylaştırmak için birkaç köprüler dizini görünümlerine Öğrenciler ve kayıtları için ekleyebilirsiniz. Konumundaki dosyayı açın **Views/Home/Index.cshtml**, siteniz için giriş sayfası olduğu. Jumbotron altına aşağıdaki kodu ekleyin.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

ActionLink için ilk parametresi bağlantıdaki görüntülenecek metni yöntemidir. İkinci parametre eylemi ve üçüncü parametre denetleyicinin adı. Örneğin, ilk bağlantı StudentsController dizin eylemde işaret eder. Gerçek köprü bu değerleri oluşturulur. İlk bağlantı Sonuçta götüren **Index.cshtml** içinde dosya **görünümler/Öğrenciler** klasör.

## <a name="display-student-views"></a>Öğrenci görünümleri görüntüler

Projenize doğru eklenen kodu Öğrenciler listesini görüntüler ve kullanıcıların Düzenle oluşturmak ya da veritabanında Öğrenci kayıtlarını sil olanak sağlayan doğrular.

Sağ **Views/Home/Index.cshtml** dosyasını bulun ve seçin **tarayıcıda görüntüle**. Bu sayfada Öğrenciler listesi için bağlantıya tıklayın.

![](generating-views/_static/image6.png)

Bu sayfada, bağlantılar ve öğrenciler bu verileri değiştirmek için listesi dikkat edin.

![Öğrenciler listesi](generating-views/_static/image7.png)

Tıklatın **Yeni Oluştur** bağlamak ve yeni Öğrenci için bazı değerler sağlayın.

![Yeni Öğrenci oluşturma](generating-views/_static/image8.png)

Tıklatın **oluşturma**ve yeni Öğrenci listenize eklenir dikkat edin.

![Yeni Öğrenci listesiyle](generating-views/_static/image9.png)

Seçin **Düzenle** bağlamak ve bazı değerleri Öğrenci için değiştirin.

![Öğrenci Düzenle](generating-views/_static/image10.png)

Tıklatın **kaydetmek**ve Öğrenci kayıt değiştirildi dikkat edin.

Son olarak, seçin **silmek** tıklayarak kaydını silmek istediğinizi onaylamak ve bağlama **silmek** düğmesi.

![Öğrenci Sil](generating-views/_static/image11.png)

Herhangi bir kod yazmak zorunda kalmadan Öğrenci tablodaki verileri ortak işlemleri görünümleri eklediniz.

Bir alan için metin etiketi veritabanı özelliğine dayalı olduğunu fark etmiş olabilirsiniz (gibi **LastName**) olmayan mutlaka web sayfasında görüntülemek istediğiniz. Örneğin, etiket olması tercih edebilirsiniz **Soyadı**. Öğreticide daha sonra bu görüntüleme sorunu düzeltir.

## <a name="display-enrollment-views"></a>Kayıt görünümleri görüntüler

Veritabanınızı Öğrenci ve kayıt tabloları indirmelere ve kayıt tabloları arasında bir-çok ilişkisi arasında bir-çok ilişkisi içerir. Kayıt görünümleri, bu ilişkileri düzgün bir şekilde işler. Seçin ve site için giriş sayfasına gidin **kayıtları listesi** bağlantısını ve ardından **Yeni Oluştur** bağlantı. Yeni bir kayıt oluşturmak için bir form görünümü görüntüler. Özellikle, formun ilişkili tabloları değerlerle doldurulan iki açılan listeleri içerdiğine dikkat edin.

![kaydı oluşturma](generating-views/_static/image12.png)

Ayrıca, sağlanan değerlerin doğrulama otomatik olarak tabanlı alanın veri türünü uygulanır. Sınıf bir sayı gerektirir ve bu nedenle uyumlu olmayan bir değer sağlamak çalışırsanız bir hata iletisi görüntülenir.

![doğrulama iletisi](generating-views/_static/image13.png)

Otomatik olarak oluşturulan görünümleri veritabanındaki verilerle çalışmak kullanıcıları etkinleştirme doğrulanmıştır. Bu serideki sonraki öğretici veritabanını güncelleştirmek ve web uygulamasında karşılık gelen değişiklikleri yapın.

>[!div class="step-by-step"]
[Önceki](creating-the-web-application.md)
[sonraki](changing-the-database.md)
