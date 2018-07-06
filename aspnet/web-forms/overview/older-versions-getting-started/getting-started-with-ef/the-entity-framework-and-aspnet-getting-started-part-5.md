---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: ASP.NET 4 Entity Framework 4.0 Database First çalışmaya başlama ve Web Forms - 5. Bölüm | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması, Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamayı ediyor...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: b175bb8d6edb8b59c14da59f4576e9029e663dde
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835265"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Entity Framework 4.0 Database First çalışmaya başlama ve ASP.NET 4 Web Forms - 5. Bölüm
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>İlgili verileri ile çalışma, devam ettirildi

Önceki öğreticide kullanmaya başladınız `EntityDataSource` ilgili verilerle çalışmak için denetimi. Veri gezintisi özelliklerinde düzenlenebilir ve birden fazla hiyerarşi düzeyleri görüntülenen. Bu öğreticide ile ilgili verileri ekleyerek ve ilişkileri silme ve var olan bir varlığa yönelik bir ilişkisi olan yeni bir varlık ekleyerek çalışmaya devam ederler.

Departmanlar için atanan kursları ekleyen bir sayfa oluşturacaksınız. Departmanlar zaten var ve aynı anda yeni bir kurs oluşturduğunuzda, var olan bir bölüm arasında ilişki kurmak.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

(Seçtiğiniz iki varlık arasında ilişki ekleme) bir kurs alıştırma atayarak bir çoktan çoğa ilişki ile çalışan bir sayfa da oluşturursunuz veya bir kurs bir eğitmen kaldırma (iki varlık arasında ilişki kaldırma, seçin). Veritabanına eklenen yeni bir satır sonuçlanıyor bir eğitmen ve bir kurs arasına ilişki ekleme `CourseInstructor` ilişkilendirme tablosu; bir ilişki içerir satırdan silinmesi kaldırma `CourseInstructor` ilişkilendirme tablo. Ancak, varlık Çerçevesi'nde başvuruda bulunmadan Gezinti özelliklerini ayarlayarak bunu `CourseInstructor` açıkça tablo.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Varolan bir varlık için varlık ilişkisi ekleme

Adlı yeni bir web sayfası oluşturma *CoursesAdd.aspx* kullanan *Site.Master* ana sayfa ve eklemek için aşağıdaki biçimlendirme `Content` adlı Denetim `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Bu biçimlendirme oluşturur bir `EntityDataSource` sağlayan ekleme ve bir işleyici belirten kursları seçtiği denetim `Inserting` olay. İşleyici, güncelleştirme için kullanacağınız `Department` yeni bir gezinti özelliği `Course` varlık oluşturulur.

Biçimlendirme ayrıca oluşturur bir `DetailsView` yeni eklemek için kullanılacak denetim `Course` varlıklar. İşaretleme için ilişkili alanları kullanan `Course` varlık özellikleri. Girmek zorunda `CourseID` bir sistem tarafından oluşturulan Kimliği alanı olmadığından değer. Bunun yerine, bu kurs oluşturulduğunda el ile belirtilmelidir bir kurs sayıdır.

Bir şablon alan için kullandığınız `Department` gezinme özelliğinin Gezinti özellikleri kullanılamaz çünkü `BoundField` denetimleri. Şablon alanını bölümü seçmek için aşağı açılan listesini sağlar. Açılır listede bağlı `Departments` varlık kümesi kullanarak `Eval` yerine `Bind`, yeniden bunları güncelleştirmek için Gezinti özellikleri doğrudan bağlanamadığı için. İçin bir işleyici belirttiğiniz `DropDownList` denetimin `Init` olay güncelleştirmeleri kod tarafından denetim kullanmak için bir başvuru depolayabileceğiniz `DepartmentID` yabancı anahtar.

İçinde *CoursesAdd.aspx.cs* kısmi sınıf bildiriminden hemen sonra bir başvuru tutmak için bir sınıf alanı eklemek `DepartmentsDropDownList` denetimi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

İçin bir işleyici eklemek `DepartmentsDropDownList` denetimin `Init` olay böylece denetimine bir başvuru depolayabilirsiniz. Bu, kullanıcı girilen değer almak ve güncelleştirmek için kullanın sağlar `DepartmentID` değerini `Course` varlık.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

İçin bir işleyici eklemek `DetailsView` denetimin `Inserting` olay:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Kullanıcı tıkladığında `Insert`, `Inserting` yeni kayıt eklenmeden önce olayı oluşturulur. İşleyici kodu alır `DepartmentID` gelen `DropDownList` için kullanılacak değeri ayarlamak için kullanır ve Denetim `DepartmentID` özelliği `Course` varlık.

Varlık Çerçevesi'Bu kursa ekleme ilgileniriz `Courses` ilişkilendirilen gezinme özelliğini `Department` varlık. Ayrıca bölümüne ekler `Department` gezinti özelliği `Course` varlık.

Sayfayı çalıştırın.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Kimlik, başlık, KREDİLERİ, bir dizi girin ve bir bölüm seçin ve ardından tıklayın **Ekle**.

Çalıştırma *Courses.aspx* sayfasında ve yeni kurs görmek için aynı bölüm seçin.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Çoktan çoğa ilişki ile çalışma

Arasındaki ilişkiyi `Courses` varlık kümesi ve `People` varlık bir çoktan çoğa ilişki kümesidir. A `Course` varlığın yok adlı bir gezinti özelliği `People` sıfır, bir veya daha fazla ilgili içerebilir `Person` varlıklar (Bu kurs öğretmeyi atanan eğitmenler temsil eder). Ve `Person` varlığın yok adlı bir gezinti özelliği `Courses` sıfır, bir veya daha fazla ilgili içerebilir `Course` varlıklar (kursları temsil eden Bu eğitmen öğretmeyi atanır). Birden çok kursları bir eğitmen öğretin ve bir kurs birden çok Eğitmenler tarafından verilen. Kılavuzun bu bölümünde ekleyecek ve arasındaki ilişkilerin silinebileceğini `Person` ve `Course` ilgili varlıkları Gezinti özelliklerini güncelleştirerek varlık.

Adlı yeni bir web sayfası oluşturma *InstructorsCourses.aspx* kullanan *Site.Master* ana sayfa ve eklemek için aşağıdaki biçimlendirme `Content` adlı Denetim `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Bu biçimlendirme oluşturur bir `EntityDataSource` denetim adını alır ve `PersonID` , `Person` eğitmen için varlıklar. A `DropDrownList` denetimin bağlı `EntityDataSource` denetimi. `DropDownList` Denetimi için bir işleyici belirtir `DataBound` olay. Bu işleyici kursları görüntüle databind iki açılan listelere kullanacaksınız.

Biçimlendirme da aşağıdaki grubunun bir kurs için seçili eğitmen atamak için kullanılacak denetimleri oluşturur:

- A `DropDownList` atamak için bir kurs seçmek için denetim. Bu denetim, şu anda seçili eğitmen için atanmamış kurslar ile doldurulur.
- A `Button` atama başlatmak için denetim.
- A `Label` denetimi atama başarısız olursa bir hata iletisi görüntüler.

Son olarak, biçimlendirme, ayrıca bir kurs seçili Eğitmen kaldırmak için kullanılacak denetimler grubu oluşturur.

İçinde *InstructorsCourses.aspx.cs*, kullanarak bir ekleme deyimi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Kursları görüntüleyen açılan listelerini doldurmak için bir yöntem ekleyin:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Bu kod tüm kurslardan alır `Courses` varlık kümesi ve kurslardan alır `Courses` gezinti özelliği `Person` seçili eğitmen için varlık. Bu eğitmen için hangi kursları atanma şeklini belirleyen ve açılır listede uygun şekilde doldurur.

İçin bir işleyici eklemek `Assign` düğmenin `Click` olay:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Bu kod alır `Person` seçili Eğitmen varlığı alır `Course` seçili kursa, varlık ve seçili kursa ekler `Courses` Eğitmen gezinti özelliği `Person` varlık. Değişiklikleri veritabanına kaydeder ve sonuçları hemen görülebilir şekilde açılan listeler yeniden doldurur.

İçin bir işleyici eklemek `Remove` düğmenin `Click` olay:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Bu kod alır `Person` seçili Eğitmen varlığı alır `Course` seçili kursa, varlık ve seçili kursuna kaldırır `Person` varlığın `Courses` gezinme özelliği. Değişiklikleri veritabanına kaydeder ve sonuçları hemen görülebilir şekilde açılan listeler yeniden doldurur.

Kodu `Page_Load` hata iletileri xenapp'i yöntemi görünür değildir rapor ve işleyicileri eklemek için herhangi bir hata olduğunda `DataBound` ve `SelectedIndexChanged` kursları açılan listeler doldurmak için Eğitmenler aşağı açılan listesinin olayları:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Sayfayı çalıştırın.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Bir eğitmen seçin. <strong>Kurs atama</strong> aşağı açılan liste görüntüler Eğitmen öğretmek değildir, kursları ve <strong>kurs kaldırmak</strong> Eğitmen zaten atanan kursları aşağı açılan liste görüntüler. İçinde <strong>kurs atama</strong> bölümünde bir kurs seçin ve ardından <strong>atama</strong>. Kursu taşır <strong>kurs kaldırmak</strong> aşağı açılan listesi. Kurs seçin <strong>kurs kaldırmak</strong> 'ye tıklayın <strong>Kaldır</strong><em>.</em> Kursu taşır <strong>kurs atama</strong> aşağı açılan listesi.

Şimdi, ilgili verilerle çalışmak için bazı daha yollarını öğrendiniz. Aşağıdaki öğreticide devralma veri modelinde sürdürülebilirliği uygulamanızı geliştirmek için nasıl kullanılacağını öğreneceksiniz.

> [!div class="step-by-step"]
> [Önceki](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [İleri](the-entity-framework-and-aspnet-getting-started-part-6.md)
