---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: ASP.NET 4 Entity Framework 4.0 Database First çalışmaya başlama ve Web Forms - 7. Bölüm | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması, Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamayı ediyor...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: bb1abe52b913785f47b7f8ec9822ed9db5ee8a05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823413"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Entity Framework 4.0 Database First çalışmaya başlama ve ASP.NET 4 Web Forms - 7. Bölüm
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>Saklı Yordamları Kullanma

Önceki öğreticide tablo başına hiyerarşi devralma deseni uygulanır. Bu öğreticide, veritabanı erişimi üzerinde daha fazla denetim kazanmak için saklı yordamlar kullanma gösterilmektedir.

Entity Framework, saklı yordamlar için veritabanı erişimi kullanması gerektiğini belirtmenize olanak sağlar. Herhangi bir varlık türü için bir saklı yordam oluşturma, güncelleştirme veya silme varlık türü için kullanılacak belirtebilirsiniz. Daha sonra veri modeli varlık kümeleri alma gibi görevleri gerçekleştirmek için kullanabileceğiniz saklı yordamlarına yönelik başvuruları ekleyebilirsiniz.

Saklı yordamları kullanarak, veritabanı erişimi için ortak bir gereksinimdir. Bazı durumlarda, bir veritabanı yöneticisi, tüm veritabanı erişimi, güvenlik nedenleriyle depolanmış yordamlar yoluyla Git gerektirebilir. Diğer durumlarda veritabanı güncelleştirdiğinde, Entity Framework kullanan işlemlerin bazıları iş mantığı oluşturmak isteyebilirsiniz. Örneğin, bir varlık silindiğinde bir arşiv veritabanına kopyalamak isteyebilirsiniz. Veya bir satır güncelleştirildiğinde, değişikliği yapan kayıt günlüğü tabloya bir satır yazmak isteyebilirsiniz. Entity Framework bir varlığı silen veya bir varlığı güncelleştiren çağrılan saklı yordam, bu tür görevleri gerçekleştirebilirsiniz.

Önceki öğreticide olduğu gibi yeni sayfalar oluşturursunuz değil. Bunun yerine, veritabanı, zaten oluşturduğunuz sayfalardan bazıları için Entity Framework erişen şekilde değiştireceksiniz.

Bu öğreticide, saklı yordamlar eklemek için veritabanında oluşturursunuz `Student` ve `Instructor` varlıklar. Veri modeline ekleyeceksiniz ve Entity Framework bunları eklemek için kullanması gerektiğini belirtirsiniz `Student` ve `Instructor` veritabanına varlıklar. Ayrıca almak için kullanabileceğiniz bir saklı yordam oluşturacaksınız `Course` varlıklar.

## <a name="creating-stored-procedures-in-the-database"></a>Veritabanında saklı yordamlar oluşturma

(Kullanıyorsanız *School.mdf* dosyayı projeden Bu öğretici ile indirilebilir, saklı yordamları zaten mevcut olduğundan bu bölümü atlayabilirsiniz.)

İçinde **Sunucu Gezgini**, genişletme *School.mdf*, sağ **saklı yordamlar**seçip **yeni saklı yordam Ekle**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Aşağıdaki SQL deyimlerini kopyalayın ve iskelet saklı yordamı değiştirerek saklı yordam penceresine yapıştırın.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` varlıkları dört özellikleri vardır: `PersonID`, `LastName`, `FirstName`, ve `EnrollmentDate`. Veritabanı kimliği değeri otomatik olarak oluşturur ve saklı yordam için diğer üç parametre kabul eder. Entity Framework, bellekte tutar varlık sürümünde izlemek için saklı yordam yeni sıranın kayıt anahtarının değerini döndürür.

Kaydet ve saklı yordam penceresini kapatın.

Oluşturma bir `InsertInstructor` aşağıdaki SQL deyimlerini kullanarak aynı şekilde, saklı yordam:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Oluşturma `Update` saklı yordamlar için `Student` ve `Instructor` varlıklar da. (Veritabanı zaten bir `DeletePerson` saklı yordamı için hem de çalışan `Instructor` ve `Student` varlıkları.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

Bu öğreticide üç işlev--INSERT, update ve delete--her varlık türü için eşleyebilirsiniz. Entity Framework sürüm 4 tek eşlemenizi sağlar veya Bunlardan ikisi başkalarıyla, bir özel durum eşleme olmadan saklı yordamlar için işlevleri: güncelleştirme işlevini ancak silme işlevini eşlerseniz, Entity Framework bir özel durum olduğunda, bir varlığı silme girişimi. Entity Framework sürüm 3.5, saklı yordamlar eşleme bu kadar esneklik yoktu: bir işlev eşlediyseniz üç eşlemek için gerekli.

Veri güncelleştirmeleri yerine okuyan bir saklı yordam oluşturmak için tüm seçer oluşturma `Course` varlıklar, aşağıdaki SQL deyimlerini kullanarak:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Saklı yordamları veri modeline ekleme

Saklı yordamları veritabanında artık tanımlanır, ancak Entity Framework için kullanılabilir hale getirmek için veri modeline eklenmelidir. Açık *SchoolModel.edmx*tasarım yüzeyine sağ tıklayın ve seçin **veritabanından bir güncelleştirme modeli**. İçinde **Ekle** sekmesinde **veritabanı nesnelerinizi seçin** iletişim kutusunda **saklı yordamlar**, yeni oluşturulan saklı yordamları seçin ve `DeletePerson` saklı yordam ve ardından **son**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Saklı yordamları eşleme

Veri modeli Tasarımcısı'nda sağ `Student` varlık ve select **saklı yordam eşlemesi**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

**Eşleşme ayrıntıları** penceresi görüntülenirse, Entity Framework, ekleme, güncelleştirme ve bu tür varlıkları silmek için kullanması gereken saklı yordamlar belirtebilirsiniz.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Ayarlama **Ekle** işlevi **InsertStudent**. Pencere, her biri bir varlık özelliğine eşleniyor gerekir, saklı yordam parametrelerinin listesini gösterir. Adları aynı olduğundan Bunlardan ikisi otomatik olarak eşlenir. Adlı bir varlık özelliği yok `FirstName`, el ile seçmeniz gerekir, böylece `FirstMidName` aşağı açılan listeden kullanılabilir varlık özelliklerini gösterir. (Adı değiştirilmiş olmasıdır `FirstName` özelliğini `FirstMidName` ilk öğreticide.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

Aynı **eşleşme ayrıntıları** penceresinde, harita `Update` işlevi `UpdateStudent` saklı yordamını (belirttiğinizden emin olun `FirstMidName` parametre değeri olarak `FirstName`yaptığınız gibi `Insert` saklı yordam) ve `Delete` işlevi `DeletePerson` saklı yordamı.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

INSERT, update ve delete eşlemek için aynı yordamı saklı yordamlar için eğitmen için izleme `Instructor` varlık.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Güncelleştirme verilerini yerine okuyun saklı yordamlar için kullandığınız **Model tarayıcı** varlığa saklı yordamı eşlemek için pencereyi döndürür yazın. Veri modeli Tasarımcısı'nda tasarım yüzeyi ve select sağ **Model tarayıcı**. Açık **SchoolModel.Store** düğümünü ve ardından açın **saklı yordamlar** düğümü. Ardından sağ tıklayarak `GetCourses` saklı yordam ve select **ekleme işlevi alma**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

İçinde **ekleme işlevi içeri aktarma** iletişim kutusunda, **bir koleksiyon, döndürür** seçin **varlıkları**ve ardından `Course` varlık türü olarak döndürdü. İşiniz bittiğinde tıklayın **Tamam**. Kaydet ve Kapat *.edmx* dosya.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>INSERT kullanarak, güncelleştirme ve saklı yordamlar silme

Saklı yordamlar eklemek için güncelleştirme ve verileri silmek varlık çerçevesi tarafından veri modeline ekleme ve bunları uygun varlıklara eşlenen sonra otomatik olarak kullanılır. Şimdi Çalıştır *StudentsAdd.aspx* sayfasında ve Entity Framework, yeni bir öğrenci her oluşturduğunuzda kullanacağınız `InsertStudent` saklı yordamı için yeni satır eklemek için `Student` tablo.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Çalıştırma *Students.aspx* sayfası ve yeni Öğrenci listesinde belirir.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Güncelleştirme işlevin çalıştığını doğrulamak için adı değiştirin ve ardından delete işlevin çalıştığını doğrulamak için Öğrenci silin.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Select saklı yordamları kullanma

Entity Framework otomatik olarak saklı yordamlar gibi çalışmaz `GetCourses`, ve bunları kullanamazsınız `EntityDataSource` denetimi. Bunları kullanmak için bunları koddan çağırın.

Açık *InstructorsCourses.aspx.cs* dosya. `PopulateDropDownLists` Yöntemi bir LINQ-varlıklar sorgu döngü listede ilerleyin ve hangilerinin bir eğitmen atanır ve hangilerinin atanmamış belirlemek kurs tüm varlıkları almak için kullanır:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Bunu, aşağıdaki kodla değiştirin:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

Sayfa artık kullanan `GetCourses` saklı yordamı tüm dersleri listesi alınamıyor. Önce yaptığınız gibi çalıştığını doğrulamak için sayfayı çalıştırın.

(Bir saklı yordam tarafından alınan varlık Gezinti özellikleri değil otomatik olarak doldurulur bağlı olarak, bu varlıklarla ilgili verilerle `ObjectContext` varsayılan ayarlar. Daha fazla bilgi için [ilgili nesneler Yükleniyor](https://msdn.microsoft.com/library/bb896272.aspx) MSDN Kitaplığı'ndaki.)

Sonraki öğreticide, program ve test verilerini biçimlendirme ve doğrulama kuralları daha kolay hale getirmek için dinamik veri işlevini kullanmayı öğreneceksiniz. Veri biçimi dizeleri gibi her bir web sayfası kuralları ve bir alanın gerekli olduğuna olup olmadığını belirtmek, yerine her sayfada otomatik olarak uygulanan ve veri modelinin meta verilerde bu tür bir kurallar belirtebilirsiniz.

> [!div class="step-by-step"]
> [Önceki](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [İleri](the-entity-framework-and-aspnet-getting-started-part-8.md)
