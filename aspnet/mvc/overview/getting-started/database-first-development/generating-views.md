---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Öğretici: Görünümleri EF veritabanı ilk için ile ASP.NET MVC uygulaması oluşturma'
description: Bu öğreticide, görünümleri ve denetleyicileri oluşturmak için ASP.NET iskeleti oluşturma kullanmaya odaklanmıştır.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 7a56c0f9197a99427bcde6103ebc69d245e8ce63
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236425"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a>Öğretici: Görünümleri EF veritabanı ilk için ile ASP.NET MVC uygulaması oluşturma

MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir. Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.

Bu öğreticide, görünümleri ve denetleyicileri oluşturmak için ASP.NET iskeleti oluşturma kullanmaya odaklanmıştır.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Yapı iskelesi Ekle
> * Yeni görünümlere bağlantılar ekleme
> * Öğrenci görünümleri görüntüler
> * Kayıt görünümleri görüntüler

## <a name="prerequisite"></a>Önkoşul

* [Web uygulama ve veri modelleri oluşturma](creating-the-web-application.md)

## <a name="add-scaffold"></a>Yapı iskelesi Ekle

Standart veri modeli sınıfları işlemlerinde sağlayacak kodu oluşturmak hazır olursunuz. Bir yapı iskelesi öğesi ekleyerek, kodu ekleyin. Yapı iskelesi ekleyebileceğiniz türü için pek çok seçenek vardır; Bu öğreticide, iskele bir denetleyici ve önceki bölümde oluşturduğunuz Öğrenci ve kayıt modelleri karşılık gelen görünümleri içerir.

Projenizdeki tutarlılık sağlamak için yeni denetleyici varolan ekleyeceksiniz **denetleyicileri** klasör. Sağ **denetleyicileri** klasörü ve select **Ekle** > **yeni iskele kurulmuş öğe**.

Seçin **MVC 5 denetleyici Entity Framework kullanarak görünümler ile** seçeneği. Bu seçenek, güncelleştirme, silme, oluşturma ve veri modelinizde görüntüleme için Görünüm ve denetleyici oluşturur.

![MVC denetleyicisi Ekle](generating-views/_static/image2.png)

Seçin **Öğrenci (ContosoSite.Models)** seçin ve model sınıfı için **ContosoUniversityDataEntities (ContosoSite.Models)** bağlamı sınıfı. Denetleyici adı olarak tutmak **StudentsController**.

**Ekle**'yi tıklatın.

Bir hata alırsanız, projeyi önceki bölümde derlenmedi olabilir. Bu durumda, projeyi derlemeyi deneyin ve ardından iskele kurulmuş öğe yeniden ekleyin.

Kod oluşturma işlemi tamamlandıktan sonra yeni bir denetleyici ve görünüm projenizin görürsünüz **denetleyicileri** ve **görünümleri** > **Öğrenciler** klasörleri .


Aynı adımları yeniden gerçekleştirir, ancak için bir yapı iskelesi Ekle **kayıt** sınıfı. İşiniz bittiğinde, sahip olduğunuz bir **EnrollmentsController.cs** dosya ve klasör altında **görünümleri** adlı **kayıtları** oluşturma, silme, Ayrıntılar, düzenleme ve dizin görünümlere sahip.

## <a name="add-links-to-new-views"></a>Yeni görünümlere bağlantılar ekleme

Yeni görünümlerinizde gitmek kolaylaştırmak için birkaç köprüler dizin görünümlerine Öğrenciler ve kayıtlar için ekleyebilirsiniz. Dosyayı açmak **görünümleri** > **giriş** > *Index.cshtml*, siteniz için giriş sayfası olduğu. Jumbotron altına aşağıdaki kodu ekleyin.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

ActionLink için ilk parametre bağlantıdaki görüntülenecek metin yöntemidir. İkinci parametre eylemi ve üçüncü parametre denetleyicinin adı. Örneğin, ilk bağlantı StudentsController dizin eylemi işaret eder. Gerçek köprü, bu değerleri oluşturulur. İlk bağlantı Sonuçta götüren **Index.cshtml** içinde dosya **görünümler/Öğrenciler** klasör.

## <a name="display-student-views"></a>Öğrenci görünümleri görüntüler

Doğru projenize eklediğiniz kodun Öğrenciler listesini görüntüler ve kullanıcıların düzenleme, oluşturma veya veritabanında Öğrenci kayıt silme sağlayan doğrular.

Sağ **görünümleri** > **giriş** > *Index.cshtml* dosya ve seçin **tarayıcıda görüntüle**. Uygulama giriş sayfasında **Öğrenciler listesi**.

![](generating-views/_static/image6.png)

Üzerinde **dizin** sayfasında, Öğrenciler ve bağlantıları bu verileri değiştirmek için listesi dikkat edin. Seçin **Yeni Oluştur** bağlamak ve yeni bir öğrenci için bazı değerler sağlayın. Tıklayın **Oluştur**ve yeni Öğrenci listenize eklenir dikkat edin.

Yeniden **dizin** sayfasında **Düzenle** bağlamak ve bazı değerler için bir öğrenci değiştirin. Tıklayın **Kaydet**, Öğrenci kayıt değiştirildi dikkat edin.

Son olarak, seçin **Sil** tıklayarak kaydı silmek istediğinizi onaylayın ve bağlama **Sil** düğmesi.

Herhangi bir kod yazmadan Öğrenci tablodaki verileri ortak işlemleri görünümleri ekledik.

Bir alan için bir metin etiketi veritabanı özelliği dayalı olduğunu fark etmiş olabilirsiniz (gibi **LastName**) değil gerekmeyen web sayfasında görüntülemek istiyorsunuz. Örneğin, etiketin olması tercih edebilirsiniz **Soyadı**. Öğreticinin ilerleyen bölümlerinde bu görüntüleme sorunu düzeltir.

## <a name="display-enrollment-views"></a>Kayıt görünümleri görüntüler

Veritabanınızı Öğrenci ve kayıt tabloları ve kursa ve kayıt tablolar arasında bir-çok ilişki arasında bir-çok ilişkisi içerir. Kayıt görünümleri bu ilişkilerin düzgün bir şekilde işler. Sitenizi ve seçin için giriş sayfasına gidin **kayıtları listesi** bağlantısını ve ardından **Yeni Oluştur** bağlantı.

Görünüm, yeni bir kayıt oluşturmak için bir form görüntüler. Özellikle, formun içerdiğini fark bir **CourseID** aşağı açılan liste ve **StudentID** aşağı açılan listesi. Her ikisi de, ilgili tablolardan değerlerle doldurulur.

Ayrıca, sağlanan değerlerin doğrulama otomatik olarak temel alan veri türünü uygulanır. **Sınıf** sayı, gerekli uyumlu olmayan bir değer sağlamak denerseniz bir hata iletisi görüntülenir: *Sınıf alan bir sayı olmalıdır.*

Otomatik olarak oluşturulan görünümler veritabanındaki verilerle çalışmak kullanıcıları etkinleştirmek doğrulanmıştır. Bu serideki sonraki öğretici, veritabanını güncellemek ve web uygulamasında karşılık gelen değişiklikleri yapın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Eklenen yapı iskelesi
> * Yeni görünümlere eklenmiş bağlantılar
> * Görüntülenen Öğrenci görünümleri
> * Görüntülenen kayıt görünümleri

Veritabanını değiştirme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.
> [!div class="nextstepaction"]
> [Veritabanını değiştirme](changing-the-database.md)