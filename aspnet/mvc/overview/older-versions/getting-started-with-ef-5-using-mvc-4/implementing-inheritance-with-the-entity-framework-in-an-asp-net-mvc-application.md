---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Bir ASP.NET MVC uygulamasındaki (8, 10) Entity Framework kalıtım uygulama | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamaları oluşturmak nasıl gösteren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: ee088f841bdb68f4806b0b62be7d379b9eab9f8c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Bir ASP.NET MVC uygulamasındaki (8, 10) Entity Framework kalıtım uygulama
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Eğitmen serisi baştan başlayabilirsiniz veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Olamaz gidermek, bir sorunla çalıştırırsanız [tamamlanmış bölüm karşıdan](building-the-ef5-mvc4-chapter-downloads.md) ve sorunu yeniden deneyin. Tamamlanan kodu kodunuzu karşılaştırarak genellikle soruna çözüm bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümler.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki öğreticide eşzamanlılık işlenir. Bu öğreticide veri modelinde devralma uygulamak nasıl yapacağınızı gösterir.

Nesne odaklı programlama içinde devralma yedekli kod ortadan kaldırmak için kullanabilirsiniz. Bu öğreticide, değiştireceğiz `Instructor` ve `Student` öğesinden türetilen sınıflara bir `Person` temel gibi özellikler içeren sınıfı `LastName` Eğitmen ve öğrenciler için ortak olan. Ekleme veya tüm web sayfalarını değiştirme olmaz ancak bazı kodları değiştireceksiniz ve bu değişiklikleri otomatik olarak veritabanında yansıtılır.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tablo başına hiyerarşisi tablo başına türü devralma karşılaştırması

Nesne odaklı programlama ile ilgili sınıflar iş kolaylaştırmak için devralma kullanabilirsiniz. Örneğin, `Instructor` ve `Student` sınıfları `School` veri modeli, hangi yedek kodda sonuçları birkaç özellikleri paylaşır:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Tarafından paylaşılan özellikler için yedek kod ortadan kaldırmak istediğinizi varsayalım `Instructor` ve `Student` varlıklar. Oluşturabilirsiniz bir `Person` temel yalnızca bu özellikleri paylaşılan içeren sınıfı sonra olun `Instructor` ve `Student` varlıklar aşağıdaki çizimde gösterildiği gibi bu taban sınıfından devral:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Bu devralma yapı veritabanında gösterilebilir birkaç yolu vardır. Sahip olabilir bir `Person` Öğrenciler ve tek bir tablodaki Eğitmen hakkında bilgi içeren tablo. Bazı sütunları yalnızca eğitmen için geçerli olabilir (`HireDate`), bazı yalnızca Öğrenciler (`EnrollmentDate`), bazı iki (`LastName`, `FirstName`). Genellikle, olurdu bir *Ayrıştırıcıyı* her satır tür belirtmek için sütun temsil eder. Örneğin, ayrıştırıcı sütunun "Eğitmen" Eğitmen ve "Öğrenci" Öğrenciler için olabilir.

![Table-per-hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Bir varlığın devralma yapısı tek veritabanı tablosundan oluşturmanın bu deseni adlı *tablo başına hiyerarşisi* (TPH) devralma.

Devralma yapısı gibi daha fazla ara veritabanını yapmak için kullanılan bir alternatiftir. Yalnızca ad alanları gibi olabilir `Person` tablo ve ayrı sahip `Instructor` ve `Student` tarih alanları tablolarla.

![Tablo başına type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Her varlık sınıfı adlı için bir veritabanı tablosu yapma bu deseni *tablo türü başına* (birleştirilmiş TPT) devralma.

Birleştirilmiş TPT desenleri karmaşık JOIN sorguları sağladığından TPH devralma desenleri birleştirilmiş TPT devralma düzenleri daha Entity Framework içinde daha iyi performans genellikle sunar. Bu öğretici nasıl TPH devralma uygulanacağını gösterir. Bu, aşağıdaki adımları gerçekleştirerek gerçekleştirirsiniz:

- Oluşturma bir `Person` sınıfı ve değişiklik `Instructor` ve `Student` öğesinden türetilen sınıflar `Person`.
- Model veritabanı eşleme kodu veritabanı bağlamı sınıfına ekleyin.
- Değişiklik `InstructorID` ve `StudentID` başvuruları projeye boyunca `PersonID`.

## <a name="creating-the-person-class"></a>Kişi sınıfı oluşturma

 Not: Bu sınıfları kullanan denetleyicileri güncelleştirilene kadar sınıfları oluşturduktan sonra projeyi derlemek mümkün olmayacaktır. 

İçinde *modelleri* klasörü oluşturmak *Person.cs* ve şablon kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

İçinde *Instructor.cs*, türetilen `Instructor` sınıfıyla `Person` sınıfı ve anahtar ve ad alanlarını kaldırın. Kod aşağıdaki gibi görünür:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Benzer değişiklik *Student.cs*. `Student` Sınıfı, aşağıdaki gibi görünür:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Kişi varlık türü için Model ekleme

İçinde *SchoolContext.cs*, ekleme bir `DbSet` özelliği için `Person` varlık türü:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Entity Framework tablo başına hiyerarşisi devralma yapılandırmak için gereken budur. Veritabanı yeniden oluşturulan olduğunda, göreceğiniz gibi sahip bir `Person` yerine tablo `Student` ve `Instructor` tabloları.

## <a name="changing-instructorid-and-studentid-to-personid"></a>InstructorID ve StudentID PersonID için değiştirme

İçinde *SchoolContext.cs*, eğitmen indirmelere eşleme ifadesini değiştirin `MapRightKey("InstructorID")` için `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Bu değişikliği gerekli değildir; yalnızca, çoktan bire çok birleşme tablodaki InstructorID sütun adını değiştirir. Uygulama adı InstructorID olarak bırakılırsa, hala düzgün çalışır. İşte tamamlanan *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Sonraki değiştirmeye ihtiyaç `InstructorID` için `PersonID` ve `StudentID` için `PersonID` proje boyunca ***dışında*** zaman damgalı geçişler dosyalarında *geçişler* klasör. Bunu yapmak için bulmak ve açık yalnızca değiştirilmesi gereken dosyalar ardından açık dosyalarda genel değişiklik yapmak. Yalnızca dosyasında *geçişler* değiştirmek klasördür *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Visual Studio'da açık olan tüm dosyaları kapatarak başlar.
2. Tıklatın **Bul ve Değiştir – tüm dosyaları bulmak** içinde **Düzenle** menüsüne ve ardından arama içeren projedeki tüm dosyalar için `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Her dosya içine açmak **Bul sonuçları** penceresi ***dışında*** &lt;zaman damgası&gt;*\_.cs* geçişdosyalarında*Geçişler* her dosya için bir satırı çift tıklatarak klasör.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Açık **dosyalarda Değiştir** iletişim ve değişiklik **konum** için **tüm açık belgeleri**.
5. Kullanım **dosyalarda Değiştir** tüm değiştirmek için iletişim `InstructorID` için `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Tüm dosyaları içeren projeye bulmak `StudentID`.
7. Her dosya içine açmak **Bul sonuçları** penceresi ***dışında*** &lt;zaman damgası&gt;*\_\*.cs* geçiş dosyaları içinde *geçişler* her dosya için bir satırı çift tıklatarak klasör.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Açık **dosyalarda Değiştir** iletişim ve değişiklik **konum** için **tüm açık belgeleri**.
9. Kullanım **dosyalarda Değiştir** tüm değiştirmek için iletişim `StudentID` için `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Projeyi oluşturun.

(Bu gösterir Not bir *dezavantajı* , `classnameID` birincil anahtarlar adlandırma deseni. Sınıf adı önek olmadan birincil anahtarlar kimliği adlı değilse *hiçbir* yeniden adlandırma olacaktır gerekli şimdi.)

## <a name="create-and-update-a-migrations-file"></a>Oluşturma ve geçişler dosyasını güncelleştirme

Paket Yöneticisi Konsolu (PMC)'da, aşağıdaki komutu girin:

`Add-Migration Inheritance`

Çalıştırma `Update-Database` PMC komutu. Geçişleri nasıl ele alınacağını bilmeniz değil mevcut verileri sahibiz komutu bu noktada başarısız olur. Aşağıdaki hatayı alıyorsunuz:

*ALTER TABLE deyimi yabancı anahtar kısıtlaması ile çakıştı "FK\_dbo. Departman\_dbo. Kişi\_PersonID ". Veritabanı "ContosoUniversity" Tablo "dbo. çakışma oluştu Kişi", sütun 'PersonID'.*

Açık *geçişler\&lt; zaman damgası&gt;\_Inheritance.cs* ve değiştirme `Up` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Çalıştırma `update-database` yeniden komutu.

> [!NOTE]
> Veri ve yapma şema değişiklikleri geçirilirken diğer hatalarıyla mümkündür. Geçiş hataları alırsanız, olamaz gidermek, devam edebilirsiniz eğitici bağlantı dizesinde değiştirerek *Web.config* dosya veya veritabanı siliniyor. Veritabanında yeniden adlandırmak için basit yaklaşımdır *Web.config* dosya. Örneğin, CU için veritabanı adını değiştirin\_aşağıdaki örnekte gösterildiği gibi test edin:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Yeni bir veritabanı ile geçirmek için veri yok ve `update-database` komut hatasız tamamlamak çok daha büyük bir olasılıkla. Veritabanını silmek yönergeler için bkz: [Visual Studio 2012'den bir veritabanını bırakmak nasıl](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Bu öğreticiyi devam edebilmek için bu yaklaşımı benimsemeye varsa, geçişler otomatik çalıştırıldığında dağıtılan site aynı hata elde edebileceğiniz beri bu öğreticinin sonunda dağıtım adımı atlayın. Geçişler hata için sorun giderme istiyorsanız, en iyi kaynak Entity Framework forumları veya StackOverflow.com biridir.


## <a name="testing"></a>Sınama

Siteyi çalıştırın ve çeşitli sayfalar deneyin. Önce yaptığınız gibi her şey aynı çalışır.

İçinde **Sunucu Gezgini** genişletin **SchoolContext** ve ardından **tabloları**, ve, gördüğünüz **Öğrenci** ve **Eğitmen**  tabloları değiştirilir tarafından bir **kişi** tablo. Genişletme **kişi** tablo göreceksiniz bulunması için kullanılan sütunların tümünün sahip **Öğrenci** ve **Eğitmen** tabloları.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Kişi tabloyu sağ tıklatın ve ardından **Show Table Data** ayrıştırıcı sütunun görmek için.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Aşağıdaki diyagram yeni Okul veritabanının yapısını gösterir:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Özet

Tablo başına hiyerarşisi devralma şimdi uygulandıktan için `Person`, `Student`, ve `Instructor` sınıfları. Bu ve diğer devralma yapıları hakkında daha fazla bilgi için bkz: [devralma eşleme stratejileri](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) Morteza Manavi'nın blogunda. Sonraki öğreticide depo ve iş desenleri biriminin uygulamak için bazı yollar görürsünüz.

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [sonraki](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
