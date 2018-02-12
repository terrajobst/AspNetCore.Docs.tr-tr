---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: "Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - bölüm 5 | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamasıdır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 5efc5ff367d5da5df060eba0028399af898a69fa
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - bölüm 5
====================
by [Tom Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>İlgili verileri ile çalışma, devam

Önceki öğreticide kullanmaya başladınız `EntityDataSource` ilgili verilerle çalışmak için denetim. Birden çok hiyerarşi düzeyi görüntülenir ve gezinti Özellikleri'nde veri düzenlenebilir. Bu öğreticide ekleme ve silme ilişkileri ve var olan bir varlığa bir ilişkisi olan yeni bir varlık ekleyerek ilgili verilerle çalışmaya devam edeceğiz.

Departmanlar için atanan kurslar ekler bir sayfa oluşturacaksınız. Departmanlar zaten var ve aynı zamanda yeni indirmelere oluşturduğunuzda, onu ve var olan bir bölüm arasında bir ilişki kurmak.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Ayrıca bir eğitmen (seçtiğiniz iki varlık arasında bir ilişki ekleme) indirmelere atayarak bir çoktan çoğa ilişki ile çalışan bir sayfa oluşturacaksınız veya bir eğitmen indirmelere kaldırma (iki varlık arasında bir ilişki kaldırma, seçin). Veritabanında bir eğitmen ve bir indirmelere arasında bir ilişki ekleme sonuçları için eklenmekte olan yeni bir satıra `CourseInstructor` ilişkilendirme tablo; bir ilişki içerir satırdan silme kaldırma `CourseInstructor` ilişkilendirme tablo. Ancak, Entity Framework başvurma olmadan Gezinti özellikleri ayarlayarak bunu `CourseInstructor` açıkça tablo.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Var olan bir varlığa bir ilişkisi olan bir varlık ekleme

Adlı yeni bir web sayfası oluşturun *CoursesAdd.aspx* kullanan *Site.Master* ana sayfa ve aşağıdaki biçimlendirmeleri eklemek `Content` adlı Denetim `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Bu biçimlendirme oluşturur bir `EntityDataSource` sağlayan ekleme ve bir işleyici belirten kursları seçen denetim `Inserting` olay. İşleyici güncelleştirmek için kullanacağınız `Department` yeni bir gezinti özelliği `Course` varlık oluşturulur.

İşaretleme da oluşturur bir `DetailsView` yeni eklemek için kullanılacak denetim `Course` varlıklar. İşaretleme ilişkili alanları için kullandığı `Course` varlık özellikleri. Girmek zorunda `CourseID` bu bir sistem tarafından oluşturulan Kimliği alanı olmadığı için değer. Bunun yerine, onu indirmelere oluşturulduğunda, el ile belirtilmelidir indirmelere olan bir sayıdır.

Bir şablon alan için kullandığınız `Department` gezinti özelliği Gezinti özellikleri ile kullanılamadığından `BoundField` denetimleri. Şablon alanını departman seçmek için aşağı açılan listesini sağlar. Aşağı açılan liste bağlı `Departments` varlık kullanarak kümesi `Eval` yerine `Bind`, yeniden bunları güncelleştirmek için Gezinti özellikleri doğrudan bağlanamadığı için. İçin bir işleyici belirttiğiniz `DropDownList` denetimin `Init` Olay denetimi kullanmak için bir başvuru güncelleştirmeleri kodla depolayabileceğiniz `DepartmentID` yabancı anahtar.

İçinde *CoursesAdd.aspx.cs* kısmi sınıf bildiriminden hemen sonra bir başvuru tutmak için bir sınıf alanı ekleme `DepartmentsDropDownList` denetimi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

İçin bir işleyici ekleyin `DepartmentsDropDownList` denetimin `Init` olay böylece denetimi başvuru depolayabilirsiniz. Bu kullanıcı girildikten değerini almak ve güncelleştirmek için kullanmanıza olanak tanır `DepartmentID` değerini `Course` varlık.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

İçin bir işleyici ekleyin `DetailsView` denetimin `Inserting` olay:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Kullanıcı tıkladığında `Insert`, `Inserting` yeni kayıt eklenmeden önce olayı oluşturulur. İşleyici kod alır `DepartmentID` gelen `DropDownList` denetlemek ve için kullanılan değer ayarlamak için kullanan `DepartmentID` özelliği `Course` varlık.

Entity Framework almak için bu indirmelere ekleme belirleyeceğini `Courses` ilişkilendirilen gezinme özelliği `Department` varlık. Ayrıca departmanına ekler `Department` Gezinti özelliğinin `Course` varlık.

Sayfayı çalıştırın.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Bir kimliği, bir başlığı, kredisi sayısını girin ve bir bölüm seçin ve ardından **Ekle**.

Çalıştırma *Courses.aspx* sayfasında ve yeni indirmelere görmek için aynı departmanı seçin.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Çok-çok ilişkileri ile çalışma

Arasındaki ilişkiyi `Courses` varlık kümesini ve `People` varlık bir çok-çok ilişkisi kümesidir. A `Course` varlık sahip adlı bir gezinti özelliği `People` sıfır, bir veya daha fazla ilgili içerebilir `Person` varlıkları (Bu indirmelere öğretmeyi atanan Eğitmen temsil eder). Ve `Person` varlık sahip adlı bir gezinti özelliği `Courses` sıfır, bir veya daha fazla ilgili içerebilir `Course` varlıkları (kurslar temsil eden Bu eğitmen öğretmeyi atanır). Bir eğitmen birden çok kurslar öğretmek ve bir indirmelere tarafından birden çok Eğitmen öğrettin. Kılavuzun bu bölümünde, ekleyip arasındaki ilişkileri `Person` ve `Course` ilgili varlık Gezinti özellikleri güncelleştirerek varlıklar.

Adlı yeni bir web sayfası oluşturun *InstructorsCourses.aspx* kullanan *Site.Master* ana sayfa ve aşağıdaki biçimlendirmeleri eklemek `Content` adlı Denetim `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Bu biçimlendirme oluşturur bir `EntityDataSource` denetim adını alır ve `PersonID` , `Person` eğitmen için varlıklar. A `DropDrownList` denetimin bağlı `EntityDataSource` denetim. `DropDownList` Denetim için bir işleyici belirtir `DataBound` olay. Bu işleyici kurslar görüntülemek databind iki aşağı açılır listeler için kullanacaksınız.

Biçimlendirme bir indirmelere seçili eğitmen atamak için kullanılacak denetimleri aşağıdaki grubunu da oluşturur:

- A `DropDownList` atamak için bir indirmelere seçmek için denetim. Bu denetim, şu anda seçili Eğitmen atanmamış kurslar ile doldurulur.
- A `Button` atama başlatmak için denetim.
- A `Label` denetim atama başarısız olursa bir hata iletisi görüntüler.

Son olarak, işaretleme denetimlerin bir indirmelere seçili Eğitmen kaldırmak için kullanmak üzere bir grup oluşturur.

İçinde *InstructorsCourses.aspx.cs*, kullanarak bir ekleme deyimi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Kurslar görüntülemek iki açılan listeleri doldurmak için bir yöntem ekleyin:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Bu kod tüm kursları alır `Courses` varlık ayarlamak ve kursları alır `Courses` Gezinti özelliğinin `Person` seçili Eğitmen varlık. Ardından hangi kurslar Bu eğitmen atanan belirler ve açılan listeleri uygun şekilde doldurur.

İçin bir işleyici ekleyin `Assign` düğmenin `Click` olay:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Bu kod alır `Person` seçili Eğitmen varlığı alır `Course` seçili Elbette, varlık ve seçili indirmelere ekler `Courses` Eğitmen gezinme özelliğinin `Person` varlık. Değişiklikler veritabanına kaydeder ve sonuçları hemen görülebilir için aşağı açılır listeler yeniden doldurur.

İçin bir işleyici ekleyin `Remove` düğmenin `Click` olay:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Bu kod alır `Person` seçili Eğitmen varlığı alır `Course` seçili Elbette, varlık ve seçili indirmelere alanından kaldırır `Person` varlığın `Courses` gezinti özelliği. Değişiklikler veritabanına kaydeder ve sonuçları hemen görülebilir için aşağı açılır listeler yeniden doldurur.

Kodu ekleyin `Page_Load` hata iletileri emin olur yöntemi görülemez rapor ve işleyicileri eklemek için hata olduğunda `DataBound` ve `SelectedIndexChanged` kurslar açılan listeleri doldurmak için Eğitmen aşağı açılan listesinin olayları:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Sayfayı çalıştırın.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Bir eğitmen seçin. **Bir indirmelere atamak** aşağı açılan liste Eğitmen öğretmek değildir, kurslar görüntüler ve **bir indirmelere kaldırmak** aşağı açılan liste Eğitmen zaten atandığı kurslar görüntüler. İçinde **bir indirmelere atamak** bölümünde, bir indirmelere seçin ve ardından **atamak**. İndirmelere taşır **bir indirmelere kaldırmak** aşağı açılan liste. Bir indirmelere seçin **bir indirmelere kaldırmak** 'ye tıklayın **Kaldır ***.* İndirmelere taşır **bir indirmelere atamak** aşağı açılan liste.

Şimdi ilgili verilerle çalışmak için bazı yöntemlerle gördünüz. Aşağıdaki öğreticide devralma veri modelinde uygulamanızın devamlılığını iyileştirmek için nasıl kullanılacağını öğreneceksiniz.

>[!div class="step-by-step"]
[Önceki](the-entity-framework-and-aspnet-getting-started-part-4.md)
[sonraki](the-entity-framework-and-aspnet-getting-started-part-6.md)
