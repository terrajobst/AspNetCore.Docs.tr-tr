---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: ASP.NET 4 Entity Framework 4.0 Database First çalışmaya başlama ve Web Forms - 6. Bölüm | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması, Entity Framework kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Örnek uygulamayı ediyor...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 57ba4a47dcc33a1fcc449bc32e9ce186e76cfb5b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755824"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Entity Framework 4.0 Database First çalışmaya başlama ve ASP.NET 4 Web Forms - 6. Bölüm
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Contoso University örnek web uygulaması Entity Framework 4.0 ve Visual Studio 2010 kullanarak ASP.NET Web Forms uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Tablo başına hiyerarşi devralma uygulama

Önceki öğreticide ile ilgili verileri ekleyerek ve ilişkileri silme ve var olan bir varlık ilişkisi olan yeni bir varlık ekleyerek çalışmıştır. Bu öğreticide, veri modelinde devralma uygulanması gösterilmektedir.

Nesne yönelimli programlama, devralma, iş ile ilgili sınıflar daha kolay hale getirmek için kullanabilirsiniz. Örneğin, aşağıdakileri oluşturabilirsiniz `Instructor` ve `Student` öğesinden türetilen sınıfların bir `Person` temel sınıfı. Varlık Çerçevesi'nde, aynı tür varlıklar arasında devralma yapılar oluşturabilirsiniz.

Öğreticinin bu bölümünde, yeni web sayfalarını oluşturmaz. Bunun yerine, varlık veri modeline türetilmiş ekleyin ve yeni varlıklar kullanmak için mevcut sayfalarını değiştirin.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tablo başına hiyerarşi tablo başına tür devralma karşılaştırması

Bir veritabanı, bir tablodaki veya birden çok tablo ilgili nesneler hakkında bilgi depolayabilirsiniz. Örneğin, `School` veritabanı `Person` tablo, Öğrenciler hem de tek bir tabloyu eğitmenlerini hakkında bilgi içerir. Bazı sütunların yalnızca eğitmen için geçerlidir (`HireDate`), bazı yalnızca Öğrenciler (`EnrollmentDate`) ve her ikisi de bazı (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Oluşturmak için Entity Framework yapılandırabileceğiniz `Instructor` ve `Student` devralacak varlıkları `Person` varlık. Bu düzen, tek veritabanı tablosundan bir varlık devralma yapısı oluşturma adlı *tablo başına hiyerarşi* (TPH) devralma.

Kurslara `School` veritabanı farklı bir desen kullanır. Çevrimiçi kurslar ve yerinde kursları işaret eden bir yabancı anahtar olan her biri ayrı tablolarda depolanır `Course` tablo. Her iki kursu türleri için ortak bilgiler yalnızca depolanan `Course` tablo.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Entity Framework veri modeli yapılandırabilirsiniz. böylece `OnlineCourse` ve `OnsiteCourse` varlıkları devralmanız `Course` varlık. Bu düzen ayrı tablolarda geri tüm türleri için ortak olan veri depolayan bir tabloya başvuran her ayrı bir tablo ile her türü için bir varlık devralma yapısı oluşturma adlı *tablo başına tür* (TPT) devralma.

Karmaşık birleştirme sorgularda TPT desenleri sağladığından TPH devralma desenleri daha iyi performans varlık çerçevesi TPT devralma desenler, daha genel kullanıma sunun. Bu izlenecek yol, nasıl TPH devralma uygulanacağını gösterir. Bu, aşağıdaki adımları uygulayarak gerçekleştirirsiniz:

- Oluşturma `Instructor` ve `Student` öğesinden türetilen varlık türleri `Person`.
- Türetilmiş varlıkların ilgilidir taşıma özellikleri `Person` türetilmiş varlıklarla varlık.
- Türetilmiş türlerdeki özellikler kısıtlamaları ayarlayın.
- Olun `Person` varlığı soyut bir varlık.
- Harita her varlık için türetilmiş `Person` belirleme belirten bir koşul ile tablo olup olmadığını bir `Person` satır türetilmiş türü temsil eder.

## <a name="adding-instructor-and-student-entities"></a>Eğitmen ve Öğrenci varlıklar ekleme

Açık <em>SchoolModel.edmx</em> tasarımcısında seçim boş bir alana sağ tıklayın, dosya <strong>Ekle</strong>, ardından <strong>varlık</strong><em>.</em>

[![Image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

İçinde **varlık Ekle** iletişim kutusu, varlık adı `Instructor` ve kendi **temel türü** seçeneğini `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

**Tamam**'ı tıklatın. Tasarımcı oluşturur bir `Instructor` türetilen varlık `Person` varlık. Yeni varlığın tüm özellikleri henüz sahip değil.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Oluşturmak için yordamı yineleyin bir `Student` da türetilen varlık `Person`.

Bu özellikten taşımak gereken şekilde Eğitmenler seferde sadece `Person` varlığa `Instructor` varlık. İçinde `Person` varlığı, sağ `HireDate` özelliği ve tıklayın **Kes**. Ardından sağ tıklayarak **özellikleri** içinde `Instructor` varlık ve tıklatın **Yapıştır**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

İşe alım tarihi bir `Instructor` varlığı null olamaz. Sağ `HireDate` özelliği tıklayın **özellikleri**ve ardından **özellikleri** penceresinde değişiklik `Nullable` için `False`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Taşıma için yordamı yineleyin `EnrollmentDate` özelliğinden `Person` varlığa `Student` varlık. Ayrıca ayarladığınızdan emin olun `Nullable` için `False` için `EnrollmentDate` özelliği.

Artık bir `Person` varlık için ortak olan özellikleri olan `Instructor` ve `Student` varlıklar (değil Taşımakta olduğunuz Gezinti özellikleri dışında), varlık devralma yapısında temel bir varlık olarak yalnızca kullanılabilir. Bu nedenle, bağımsız bir varlık olarak hiçbir zaman değerlendirilir sağlamak gerekir. Sağ `Person` varlığı seçin **özellikleri**ve ardından **özellikleri** değerini değiştirme penceresi **soyut** özelliğini  **True**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Eğitmen ve Öğrenci varlıkları kişi tabloya eşleme

Artık, Entity Framework'ı arasındaki farkı anlamak nasıl bağlanacaklarını kullanıcılarınıza anlatmanız `Instructor` ve `Student` veritabanında varlıklar.

Sağ `Instructor` varlık ve select **Tablo eşleme**. İçinde **eşleşme ayrıntıları** penceresinde tıklayın **bir tablo veya Görünüm Ekle** seçip **kişi**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Tıklayın **koşul Ekle**ve ardından **İşeAlmaTarihi**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Değişiklik **işleci** için **olduğu** ve **değeri / özellik** için **değil Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

İçin yordamı yineleyin `Students` varlık, bu varlık için eşler belirtme `Person` tablosundan `EnrollmentDate` sütunu null değil. Ardından kaydedin ve veri modelini kapatın.

Sınıflar olarak yeni varlıklar oluşturmak ve Tasarımcısı'nda kullanılabilmesi için projeyi derleyin.

## <a name="using-the-instructor-and-student-entities"></a>Eğitmen ve Öğrenci varlıklar kullanma

Öğrenci ve Eğitmenler verilerle, veri bağlama için çalışan web sayfaları oluştururken `Person` varlık kümesi ve üzerinde filtre `HireDate` veya `EnrollmentDate` Öğrenciler veya Eğitmenler döndürülen verileri kısıtlamak için özellik. Bununla birlikte, artık bağladığınızda her veri kaynak denetimine `Person` varlık kümesi, yalnızca belirtebilirsiniz `Student` veya `Instructor` varlık türleri seçili olmalıdır. Entity Framework Öğrenciler ve eğitmenlerini ayırt etmek nasıl bildiğinden `Person` varlık kümesini kaldırabilirsiniz `Where` girdiğiniz el ile bunu yapmak için özellik ayarları.

Visual Studio Tasarımcısı'nda, varlık türü belirtebilirsiniz bir `EntityDataSource` denetim seçmelisiniz **EntityTypeFilter** açılan kutusunda `Configure Data Source` Sihirbazı, aşağıdaki örnekte gösterildiği gibi.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

Ve **özellikleri** kaldırabilirsiniz penceresi `Where` artık aşağıdaki örnekte gösterildiği gibi gerekli değerleri yan tümcesi.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Ancak, için biçimlendirme değiştirdiğinizi çünkü `EntityDataSource` kullanılacak denetimler `ContextTypeName` özniteliği çalıştıramazsınız **veri kaynağı yapılandırma** Sihirbazı yükleyerek `EntityDataSource` önceden oluşturduğunuz denetimleri. Bu nedenle, biçimlendirme değiştirerek gerekli değişiklikleri yaptırın.

Açık *Students.aspx* sayfası. İçinde `StudentsEntityDataSource` denetlemek, Kaldır `Where` ekleyin ve özniteliği bir `EntityTypeFilter="Student"` özniteliği. Biçimlendirme, artık aşağıdaki örneğe benzer:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Ayarı `EntityTypeFilter` öznitelik sağlar `EntityDataSource` denetimi yalnızca belirtilen varlık türü seçin. Her ikisi de almak istediyseniz `Student` ve `Instructor` varlık türleri bu özniteliği olmayan ayarlayın. (Birden çok varlık türleri biriyle alma seçeneğiniz `EntityDataSource` yalnızca salt okunur veri erişimi denetim kullanıyorsanız, denetim. Kullanıyorsanız, bir `EntityDataSource` eklemek, güncelleştirmek veya varlıklarını silme için Denetim ve birden çok ilişkili için varlık kümesini içerebilir, yalnızca bir varlık türüyle çalışabilir ve bu öznitelik ayarlamanız.)

İçin yordamı yineleyin `SearchEntityDataSource` denetimi yalnızca parçası kaldırmak dışında `Where` seçer özniteliği `Student` özelliği tamamen kaldırmak yerine varlıklar. Denetimin etiketiyle artık aşağıdaki örneğe benzer:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Önce yaptığınız gibi hala çalışır durumda olduğunu doğrulamak için sayfayı çalıştırın.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Önceki öğreticilerde, oluşturduğunuz ve böylece yeni kullandıkları sayfalarda güncelleştirme `Student` ve `Instructor` varlıkları yerine `Person` varlıklar, ardından bunları Öncekine şekilde çalıştıklarını doğrulamak için:

- İçinde *StudentsAdd.aspx*, ekleme `EntityTypeFilter="Student"` için `StudentsEntityDataSource` denetimi. Biçimlendirme, artık aşağıdaki örneğe benzer: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- İçinde *About.aspx*, ekleme `EntityTypeFilter="Student"` için `StudentStatisticsEntityDataSource` denetlemek ve kaldırma `Where="it.EnrollmentDate is not null"`. Biçimlendirme, artık aşağıdaki örneğe benzer: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- İçinde *Instructors.aspx* ve *InstructorsCourses.aspx*, ekleme `EntityTypeFilter="Instructor"` için `InstructorsEntityDataSource` denetlemek ve kaldırma `Where="it.HireDate is not null"`. İşaretlemede *Instructors.aspx* artık aşağıdaki örneğe benzer: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    İşaretlemede *InstructorsCourses.aspx* artık şu örnekteki gibi:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

Bu değişikliklerin bir sonucu olarak çeşitli şekillerde Contoso University uygulamanın bakım geliştirdik. UI yerleşiminde dışında seçimi ve Doğrulama mantığı taşıdığınıza (*.aspx* biçimlendirme) ve bir veri erişim katmanı parçası yapılır. Bu uygulama kodunuzu gelecekte veritabanı şemasını veya veri modeli için yapabileceğiniz değişikliklere karşı ayrılabilmesine yardımcı olur. Örneğin, Öğrenciler Öğretmenler Yardımcıları işe ve bu nedenle bir işe alım tarihi elde edebileceğiniz karar. Ardından, veri modelini güncelleştirme ve öğrenciler eğitmenlerimiz sizlerle mutlaka ayırt etmek için yeni bir özellik de ekleyebilirsiniz. Web uygulamasındaki kod burada öğrencilere yönelik bir işe alım tarihi göstermek istediğiniz dışında değiştirmeniz gerekir. Ekleme başka bir faydası `Instructor` ve `Student` varlıklar olduğunu, kodunuzun ne zaman, başvurulan değerinden daha kolay anlaşılır `Person` gerçekten Öğrenciler nesneler veya Eğitmenleri.

Şimdi, varlık Çerçevesi'nde bir devralma deseni uygulamak için bir yol gördünüz. Aşağıdaki öğreticide Entity Framework veritabanı nasıl eriştiğini hakkında daha fazla denetime sahip olmak için saklı yordamlar kullanmayı öğreneceksiniz.

> [!div class="step-by-step"]
> [Önceki](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [İleri](the-entity-framework-and-aspnet-getting-started-part-7.md)
