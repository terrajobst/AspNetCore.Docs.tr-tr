---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: "Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - Bölüm 7 | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamasıdır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: aeea122636f5235364e6a40cb6e041b1fe221317
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - Bölüm 7
====================
by [Tom Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>Saklı Yordamları Kullanma

Önceki öğreticide bir tablo başına hiyerarşisi devralma deseni uygulanmadı. Bu öğretici saklı yordamlar veritabanı erişimi üzerinde daha fazla denetim kazanmak için nasıl kullanılacağını gösterir.

Entity Framework bunu veritabanı erişimi için saklı yordamlar kullanmalısınız belirtmenize olanak sağlar. Herhangi bir varlık türü için oluşturma, güncelleştirme veya silme varlık türü için kullanılacak bir saklı yordam belirtebilirsiniz. Daha sonra veri modelinde varlık kümesi alma gibi görevleri gerçekleştirmek için kullanabileceğiniz saklı yordam başvurularını ekleyebilirsiniz.

Saklı yordamları kullanarak, veritabanı erişimi için ortak bir gereksinimdir. Bazı durumlarda, bir veritabanı Yöneticisi tüm veritabanı erişimi güvenlik nedenleriyle saklı yordamlar aracılığıyla Git gerektirebilir. Diğer durumlarda, iş mantığı bazı Entity Framework veritabanı güncelleştirdiğinde kullanan işlemler oluşturmak isteyebilirsiniz. Örneğin, bir varlık silindiğinde bir arşiv veritabanına kopyalamak isteyebilirsiniz. Veya bir satır güncelleştirildiğinde değişikliği yapan kaydeden günlük tabloya bir satır yazmak isteyebilirsiniz. Entity Framework bir varlığı silen veya bir varlık güncelleştirmeler olduğunda çağrılan saklı yordam, bu tür görevleri gerçekleştirebilirsiniz.

Önceki öğretici olduğu gibi yeni bir sayfa oluşturacağınız değil. Bunun yerine, Entity Framework veritabanı zaten oluşturulan sayfaları bazıları için erişen şekilde değiştireceksiniz.

Bu öğreticide eklemek için veritabanında depolanan yordamları oluşturacaksınız `Student` ve `Instructor` varlıklar. Veri modeline ekleyeceksiniz ve Entity Framework bunları eklemek için kullanması gerektiğini belirtirsiniz `Student` ve `Instructor` veritabanına varlıklar. Ayrıca almak için kullanabileceğiniz bir saklı yordam oluşturacaksınız `Course` varlıklar.

## <a name="creating-stored-procedures-in-the-database"></a>Saklı yordamlar veritabanında oluşturuluyor

(Kullanıyorsanız, *School.mdf* dosyası Bu öğretici ile İndirilebilecek projeden saklı yordamları zaten mevcut olduğundan bu bölümü atlayabilirsiniz.)

İçinde **Sunucu Gezgini**, genişletin *School.mdf*, sağ **saklı yordamlar**seçip **yeni saklı yordam eklemek**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Aşağıdaki SQL deyimlerini kopyalayın ve bunları iskelet saklı yordam değiştirme saklı yordam penceresine yapıştırın.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student`varlıkları dört özellikleri vardır: `PersonID`, `LastName`, `FirstName`, ve `EnrollmentDate`. Veritabanı kimliği değeri otomatik olarak oluşturur ve saklı yordam için diğer üç parametre kabul eder. Böylece Entity Framework, bellekte tutar varlık sürümünde izlemek saklı yordamı yeni sıranın kayıt anahtarının değerini döndürür.

Kaydet ve saklı yordam penceresini kapatın.

Oluşturma bir `InsertInstructor` saklı yordamı aşağıdaki SQL deyimlerini kullanarak aynı şekilde:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Oluşturma `Update` saklı yordamlar için `Student` ve `Instructor` varlıklar de. (Veritabanında zaten var. bir `DeletePerson` depolanan her ikisi için çalışır yordam `Instructor` ve `Student` varlıklar.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

Bu öğreticide tüm üç işlevleri--INSERT, update ve delete--her varlık türü için eşleme. Entity Framework sürüm 4, yalnızca bir eşlemenizi sağlar ya da bunların ikisini başkalarıyla, bir özel durum eşleme olmadan saklı yordamlar işlevleri: güncelleştirme işlevi ancak silme işlevini eşlerseniz Entity Framework bir özel durum olduğunda, bir varlığı silme girişimi. Entity Framework sürüm 3.5, saklı yordamlar eşleme bu kadar esneklik içermiyor: bir işlev eşlenen, tüm üç eşlemek için gerekli.

Veri güncelleştirmeleri yerine okuyan bir saklı yordam oluşturmak için tüm seçen bir oluşturun `Course` varlıklar, aşağıdaki SQL deyimlerini kullanarak:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Saklı yordamlar için veri modeli ekleme

Saklı yordamlar artık veritabanında tanımlı, ancak Entity Framework için kullanılabilir duruma getirmek için veri modeline eklenmelidir. Açık *SchoolModel.edmx*tasarım yüzeyine sağ tıklatın ve seçin **güncelleştirme modeli veritabanından**. İçinde **Ekle** sekmesinde **veritabanı nesnelerinizi** iletişim kutusunda, genişletin **saklı yordamlar**, yeni oluşturulan saklı yordamlar seçin ve `DeletePerson` saklı yordam ve ardından **son**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Saklı yordamlar eşleme

Veri modeli Tasarımcısı'nda sağ `Student` varlık ve select **depolanan yordamı eşleme**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

**Eşleme ayrıntıları** penceresi görüntülenir, ekleme, güncelleştirme ve bu tür varlıkları silmek için Entity Framework kullanması gereken saklı yordamlar belirtebilirsiniz.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Ayarlama **Ekle** için işlev **InsertStudent**. Pencerenin saklı yordam parametreleri, her biri bir varlık özelliğine eşleniyor gerekir listesini gösterir. Adları aynı olduğundan bu iki otomatik olarak eşlenir. Adlı bir varlık özellik yok `FirstName`, el ile seçmeniz gerekir böylece `FirstMidName` bir açılır listeden kullanılabilir varlık özellikleri gösterir. (Adının değişip olmasıdır `FirstName` özelliğine `FirstMidName` ilk öğreticide.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

Aynı **eşleme ayrıntıları** penceresinde, harita `Update` için işlev `UpdateStudent` saklı yordamı (belirttiğinizden emin olun `FirstMidName` için parametre değeri olarak `FirstName`yaptığınız gibi `Insert` saklı yordamı) ve `Delete` için işlev `DeletePerson` saklı yordamı.

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

INSERT, update ve delete eşlemek için aynı yordamı saklı yordamlar için eğitmen için izleme `Instructor` varlık.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Güncelleştirme veri yerine okuma saklı yordamlar için kullandığınız **modeli tarayıcısı** saklı yordamı varlığa eşlemek için pencere döndürür yazın. Veri modeli Tasarımcısı'nda tasarım yüzeyi ve select sağ **modeli tarayıcısı**. Açık **SchoolModel.Store** düğümünü ve ardından açın **saklı yordamlar** düğümü. Ardından sağ `GetCourses` saklı yordam ve Seç **eklemek işlev içeri aktarma**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

İçinde **eklemek işlev içeri aktarma** iletişim kutusunda **bir koleksiyon olarak döndürür** seçin **varlıklar**ve ardından `Course` varlık türü olarak döndürdü. İşiniz bittiğinde tıklatın **Tamam**. Kaydet ve Kapat *.edmx* dosya.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>INSERT kullanarak, güncelleştirme ve saklı yordamlar silme

Eklemek için saklı yordamlar güncelleştirmek ve verileri silmek için veri modeli eklendi ve uygun varlıklara eşlenen sonra Entity Framework tarafından otomatik olarak kullanılır. Şimdi Çalıştır *StudentsAdd.aspx* sayfasında ve her bir yeni Öğrenci oluşturduğunuzda, Entity Framework kullanacak `InsertStudent` saklı yordamı yeni satır eklemek için `Student` tablo.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Çalıştırma *Students.aspx* sayfası ve yeni Öğrenci listede görünür.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Güncelleştirme işlevi çalıştığını doğrulamak için adı değiştirin ve silme işlevini çalıştığını doğrulamak için Öğrenci silin.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Select saklı yordamları kullanma

Entity Framework otomatik olarak saklı yordamlar gibi çalışmaz `GetCourses`, ve bunlarla kullanamazsınız `EntityDataSource` denetim. Bunları kullanmak için bunları koddan çağırın.

Açık *InstructorsCourses.aspx.cs* dosya. `PopulateDropDownLists` Yöntemi, böylece döngü listede ilerleyin ve hangilerinin bir eğitmen atanır ve hangilerinin atanmamış belirlemek tüm indirmelere varlık almaya LINQ varlıkların sorgu kullanır:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Aşağıdaki kod ile değiştirin:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

Sayfa şimdi kullanıyorsa `GetCourses` saklı yordamı tüm kursları listesi alınamadı. Uygulama önceden olduğu gibi çalışır durumda olduğunu doğrulamak için sayfayı çalıştırın.

(Bir saklı yordam tarafından alınan varlık Gezinti özellikleri değil otomatik olarak doldurulur bağlı olarak bu varlıklarla ilgili verilerle `ObjectContext` varsayılan ayarlar. Daha fazla bilgi için bkz: [yüklenirken ilişkili nesneleri](https://msdn.microsoft.com/library/bb896272.aspx) MSDN Kitaplığı'nda.)

Sonraki öğreticide, program ve test veri biçimlendirmeyi ve doğrulama kurallarını kolaylaştırmak için dinamik veri işlevini kullanmayı öğreneceksiniz. Veri biçimi dizeleri gibi her web sayfası kuralları ve bir alan olup olmadığına gereklidir belirtme, yerine her sayfada otomatik olarak uygulanan ve veri modelinin meta verilerde bu tür bir kurallar belirtebilirsiniz.

>[!div class="step-by-step"]
[Önceki](the-entity-framework-and-aspnet-getting-started-part-6.md)
[sonraki](the-entity-framework-and-aspnet-getting-started-part-8.md)
