---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: "Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - Bölüm 6 | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamasıdır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 164c2002a119420555d2c7065c5a79a5f433a725
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Entity Framework 4.0 veritabanı ile ilk Başlarken ve ASP.NET 4 Web Forms - Bölüm 6
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Tablo başına hiyerarşisi kalıtım uygulama

Önceki öğreticide ile ilgili veriler, ekleme ve silme ilişkileri ve var olan bir varlığa bir ilişkiye sahip yeni bir varlık ekleyerek çalışmıştır. Bu öğreticide veri modelinde devralma uygulamak nasıl yapacağınızı gösterir.

Nesne odaklı programlama ile ilgili sınıflar iş kolaylaştırmak için devralma kullanabilirsiniz. Örneğin, oluşturabilirsiniz `Instructor` ve `Student` öğesinden türetilen sınıflar bir `Person` temel sınıfı. Entity Framework devralma yapıları varlık arasında aynı tür oluşturabilirsiniz.

Öğreticinin bu bölümünde, tüm yeni web sayfalarını oluşturmaz. Bunun yerine, varlık veri modeli türetilmiş eklemek ve varolan sayfalarını yeni varlıklar kullanmak üzere değiştirin.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tablo başına hiyerarşisi tablo başına türü devralma karşılaştırması

Bir veritabanı, bir tablo veya birden çok tablo ilgili nesneler hakkında bilgi depolayabilir. Örneğin, `School` veritabanı `Person` tablo Öğrenciler ve tek bir tablodaki Eğitmen hakkında bilgi içerir. Bazı sütunları yalnızca Eğitmen uygulamak (`HireDate`), bazı yalnızca Öğrenciler (`EnrollmentDate`) ve bazı her ikisine de (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Oluşturmak için Entity Framework yapılandırabilirsiniz `Instructor` ve `Student` devralınmalıdır varlıklar `Person` varlık. Bir varlığın devralma yapısı tek veritabanı tablosundan oluşturmanın bu deseni adlı *tablo başına hiyerarşisi* (TPH) devralma.

Kurslara `School` veritabanı farklı bir desen kullanır. Çevrimiçi kurslar ve yerinde kurslar her birinin işaret eden bir yabancı anahtar olan ayrı tablolarda depolanır `Course` tablo. Her iki indirmelere türleri için ortak bilgiler yalnızca depolanan `Course` tablo.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Entity Framework veri modeli yapılandırabilirsiniz böylece `OnlineCourse` ve `OnsiteCourse` varlıklar devral `Course` varlık. Bir varlığın devralma yapısı geri tüm türleri için ortak verileri depolayan bir tabloya başvuran her ayrı bir tablo ile her türü için ayrı tablolardan oluşturmanın bu deseni adlı *tablo türü başına* (birleştirilmiş TPT) devralma.

Birleştirilmiş TPT desenleri karmaşık JOIN sorguları sağladığından TPH devralma desenleri birleştirilmiş TPT devralma düzenleri daha Entity Framework içinde daha iyi performans genellikle sunar. Bu kılavuz, TPH devralma uygulamak gösterilmiştir. Bu, aşağıdaki adımları gerçekleştirerek gerçekleştirirsiniz:

- Oluşturma `Instructor` ve `Student` öğesinden türetilen varlık türleri `Person`.
- Türetilen varlıkları ilgilidir taşıma özellikleri `Person` türetilen varlık için varlık.
- Özellikler kısıtlamalar türetilmiş türler ayarlayın.
- Olun `Person` varlığı soyut bir varlık.
- Harita her türetilmiş varlığa `Person` nasıl belirleneceği belirten bir koşul tabloyla olup olmadığını bir `Person` satır türü türetilen temsil eder.

## <a name="adding-instructor-and-student-entities"></a>Eğitmen ve Öğrenci varlıklar ekleme

Açık *SchoolModel.edmx* dosya, select Tasarımcısı'nda boş bir alanı sağ **Ekle**seçeneğini belirleyip **varlık***.*

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

İçinde **varlık Ekle** iletişim kutusunda, varlık adı `Instructor` ve kendi **temel türü** için seçenek `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

**Tamam**'ı tıklatın. Tasarımcı oluşturur bir `Instructor` türetilen varlık `Person` varlık. Yeni varlık henüz herhangi bir özellik yok.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Oluşturmak için yordamı yineleyin bir `Student` de türetilen varlık `Person`.

Bu özellikten taşımanız gerekir böylece Eğitmen seferde yalnızca `Person` varlığa `Instructor` varlık. İçinde `Person` varlık, sağ `HireDate` özelliği ve tıklatın **Kes**. Ardından sağ **özellikleri** içinde `Instructor` varlık ve tıklatın **Yapıştır**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

İşe alma tarihi bir `Instructor` varlık null olamaz. Sağ `HireDate` özelliği,'ı tıklatın **özellikleri**ve ardından **özellikleri** penceresi değişiklik `Nullable` için `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Taşıma için yordamı yineleyin `EnrollmentDate` özelliğinden `Person` varlığa `Student` varlık. Ayrıca ayarladığınızdan emin olun `Nullable` için `False` için `EnrollmentDate` özelliği.

Artık bir `Person` varlık için ortak olan özelliklerine sahip `Instructor` ve `Student` varlıkları (değil taşıma Gezinti özellikleri yanı sıra,), varlığın devralma yapısındaki temel bir varlık olarak yalnızca kullanılabilir. Bu nedenle, bağımsız bir varlık olarak hiçbir zaman davranılır emin olmak gerekir. Sağ `Person` varlık, select **özellikleri**ve ardından **özellikleri** penceresi değerini değiştirin **soyut** özelliğine  **Doğru**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Eğitmen ve Öğrenci varlıkları kişi Tablo eşleme

Entity Framework birbirinden ayırmak nasıl bildirmeniz gerekir artık `Instructor` ve `Student` veritabanındaki varlıklar.

Sağ `Instructor` varlık ve select **Tablo eşleme**. İçinde **eşleme ayrıntıları** penceresinde tıklatın **bir tablo veya görünüm ekleyin** seçip **kişi**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Tıklatın **bir koşul eklemek**ve ardından **İşeAlmaTarihi**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Değişiklik **işleci** için **olan** ve **değeri / özellik** için **Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

İçin yordamı yineleyin `Students` varlık, bu varlık için eşlemeleri belirtme `Person` tablosundan `EnrollmentDate` sütun null değil. Ardından kaydedin ve veri modelinin kapatın.

Sınıflar olarak yeni varlıklar oluşturmak ve onları Tasarımcısı'nda kullanılabilir hale getirmek için projeyi oluşturun.

## <a name="using-the-instructor-and-student-entities"></a>Eğitmen ve Öğrenci varlıkları kullanma

Öğrenci ve Eğitmen verileriyle, veriye bağlı onlara çalışma web sayfaları oluşturduğunuzda `Person` üzerinde filtrelenmiş ve varlık kümesi `HireDate` veya `EnrollmentDate` Öğrenciler veya Eğitmen döndürülen verileri kısıtlamak için özellik. Bununla birlikte, artık bağladığınızda her veri kaynağı denetimi `Person` varlık kümesi, yalnızca belirtebilirsiniz `Student` veya `Instructor` varlık türleri seçilmelidir. Entity Framework Öğrenciler ve Eğitmen içinde ayırt etmek nasıl bildiği için `Person` varlık kümesini kaldırabilirsiniz `Where` girdiğiniz el ile bunu yapmak için özellik ayarları.

Visual Studio Tasarımcısı'nda, varlık türü belirtebilirsiniz bir `EntityDataSource` denetim seçmelisiniz **EntityTypeFilter** açılan kutusunda `Configure Data Source` Sihirbazı, aşağıdaki örnekte gösterildiği gibi.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

Ve **özellikleri** kaldırabilirsiniz penceresi `Where` artık, aşağıdaki örnekte gösterildiği gibi gerekmeyen yan tümcesi değerleri.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Ancak, için biçimlendirme değiştirdiyseniz çünkü `EntityDataSource` kullanılacak denetimleri `ContextTypeName` özniteliği çalıştıramaz **Configure Data Source** üzerinde Sihirbazı `EntityDataSource` önceden oluşturduğunuz denetimleri. Bu nedenle, bunun yerine biçimlendirme değiştirerek gerekli değişiklikleri yapın.

Açık *Students.aspx* sayfası. İçinde `StudentsEntityDataSource` denetlemek, Kaldır `Where` özniteliği ve ekleme bir `EntityTypeFilter="Student"` özniteliği. Biçimlendirme, aşağıdaki örnekte şimdi benzeyecektir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Ayarı `EntityTypeFilter` özniteliği sağlar `EntityDataSource` denetimi yalnızca belirtilen varlık türü seçin. Her ikisi de almak istiyorsanız `Student` ve `Instructor` varlık türleri, bu öznitelik değil ayarlardı. (Birden çok varlık türleri biriyle alma seçeneğiniz `EntityDataSource` yalnızca salt okunur veri erişimi için Denetim kullanıyorsanız denetim. Kullanıyorsanız, bir `EntityDataSource` Ekle, Güncelleştir veya varlıkları silmek için denetlemek ve bağlı için varlık kümesini birden çok tür içeriyorsa, yalnızca tek bir varlık türü ile çalışabilirsiniz ve bu öznitelik ayarlamanız gerekir.)

İçin yordamı yineleyin `SearchEntityDataSource` , yalnızca bir bölümü kaldırmak dışında kontrol `Where` seçer öznitelik `Student` özelliği tamamen kaldırmak yerine varlıklar. Denetimi açılış etiketinde şimdi aşağıdaki örnekte benzeyecektir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Uygulama önceden olduğu gibi yine çalışır durumda olduğunu doğrulamak için sayfayı çalıştırın.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Önceki eğitimlerine oluşturulan ve böylece yeni kullandıkları aşağıdaki sayfalarda güncelleştirme `Student` ve `Instructor` yerine varlıklar `Person` varlıklar, ardından çalıştırın bunları önceden olduğu gibi çalıştıklarını doğrulamak için:

- İçinde *StudentsAdd.aspx*, ekleme `EntityTypeFilter="Student"` için `StudentsEntityDataSource` denetim. Biçimlendirme, aşağıdaki örnekte şimdi benzeyecektir: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- İçinde *About.aspx*, ekleme `EntityTypeFilter="Student"` için `StudentStatisticsEntityDataSource` denetlemek ve kaldırma `Where="it.EnrollmentDate is not null"`. Biçimlendirme, aşağıdaki örnekte şimdi benzeyecektir: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- İçinde *Instructors.aspx* ve *InstructorsCourses.aspx*, ekleme `EntityTypeFilter="Instructor"` için `InstructorsEntityDataSource` denetlemek ve kaldırma `Where="it.HireDate is not null"`. Biçimlendirme *Instructors.aspx* şimdi aşağıdaki örneğe benzer: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Biçimlendirme *InstructorsCourses.aspx* şimdi aşağıdaki örnekte benzeyecektir:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

Bu değişikliklerin sonucu olarak çeşitli şekillerde Contoso University uygulamanın bakım geliştirdik. Seçim ve Doğrulama mantığı UI katmanında dışında geçtiğinizi (*.aspx* biçimlendirme) ve veri erişim katmanı'nın ayrılmaz bir parçası yapılır. Bu uygulama kodunuzdan gelecekte veritabanı şeması veya veri modeli için yapabileceğiniz değişiklikleri ayırmaya yardımcı olur. Örneğin, Öğrenciler Öğretmen yardımları işe ve bu nedenle bir işe alma tarihi elde edebileceğiniz karar. Öğrenciler Eğitmen ayırt etmek ve veri modeli güncelleştirmek için yeni bir özellik daha sonra ekleyebilirsiniz. Web uygulamasındaki kod burada Öğrenciler için işe alma tarihi göstermek istediğiniz dışında değiştirmeniz gerekir. Başka bir yararı ekleme `Instructor` ve `Student` varlıklar olan kodunuzu zaman, başvurulan değerinden daha kolay anlaşılır `Person` gerçekten Öğrenciler olan nesneleri veya eğitmen.

Şimdi Entity Framework bir devralma desen uygulamak için bir yol da gördünüz. Aşağıdaki öğreticide Entity Framework veritabanı nasıl eriştiğini üzerinde daha fazla denetim olması için saklı yordamlar kullanmayı öğreneceksiniz.

>[!div class="step-by-step"]
[Önceki](the-entity-framework-and-aspnet-getting-started-part-5.md)
[sonraki](the-entity-framework-and-aspnet-getting-started-part-7.md)
