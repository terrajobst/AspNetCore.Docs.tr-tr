---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Bir ASP.NET MVC uygulamasındaki (10 8) devralma Entity Framework ile uygulama | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 10ee0be62a1d601e323afc423e9022bed56f4f33
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752957"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Bir ASP.NET MVC uygulamasındaki (10 8) Entity Framework kalıtım uygulama
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Projeyi yükle](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Öğretici serisinin en baştan başlayın veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.
> 
> > [!NOTE] 
> > 
> > Çözümleyemiyor, bir sorunla karşılaştıysanız [tamamlanmış bölüm indirme](building-the-ef5-mvc4-chapter-downloads.md) ve sorununuzu yeniden oluşturmaya çalışın. Tamamlanan kodu kodunuza karşılaştırarak, sorunun çözümünü genellikle bulabilirsiniz. Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümleri bulabilirsiniz.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Önceki öğreticide eşzamanlılık özel durumları işlenir. Bu öğreticide, veri modelinde devralma uygulanması gösterilmektedir.

Nesne yönelimli programlama, devralma, gereksiz kod ortadan kaldırmak için kullanabilirsiniz. Bu öğreticide, değiştireceksiniz `Instructor` ve `Student` oldukları türetilmesi sınıflara bir `Person` temel gibi özellikler içeren sınıf `LastName` eğitmenler ve öğrenciler için ortak olan. Ekleyebilir veya herhangi bir web sayfalarını değiştirmesine olmaz ancak bazı kodları değiştireceksiniz ve bu değişiklikleri veritabanında otomatik olarak yansıtılır.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tablo başına hiyerarşi tablo başına tür devralma karşılaştırması

Nesne yönelimli programlama, devralma, iş ile ilgili sınıflar daha kolay hale getirmek için kullanabilirsiniz. Örneğin, `Instructor` ve `Student` sınıfları `School` veri modeli, yedekli kodda sonuçlanır çeşitli özellikleri paylaşır:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Gereksiz kod tarafından paylaşılan özellikleri için kaldırmak istediğiniz varsayalım `Instructor` ve `Student` varlıklar. Oluşturabilirsiniz bir `Person` temel sınıfı, yalnızca bu paylaşılan özelliklerini içeren ve ardından olun `Instructor` ve `Student` varlıklar aşağıdaki çizimde gösterildiği gibi o temel sınıftan devralınan:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Bu devralma yapı veritabanında gösterilebilir birkaç yolu vardır. Sahip olabilir bir `Person` Öğrenciler hem tek bir tabloyu eğitmenlerini hakkında bilgi içeren tablo. Bazı sütunların yalnızca eğitmen için geçerli olabilir (`HireDate`), bazı yalnızca Öğrenciler (`EnrollmentDate`), bazı her ikisine de (`LastName`, `FirstName`). Genellikle, olurdu bir *ayrıştırıcı* her satır türü belirtmek için sütunu temsil eder. Örneğin, ayrıştırıcı sütununu "Eğitmen" eğitmenler ve "Öğrenci" Öğrenciler için olabilir.

![Tablo başına hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Bu düzen, tek veritabanı tablosundan bir varlık devralma yapısı oluşturma adlı *tablo başına hiyerarşi* (TPH) devralma.

Devralma yapısı gibi daha veritabanını yapmak için kullanılan bir alternatiftir. Yalnızca ad alanları gibi olabilir `Person` tablo ve ayrı sahip `Instructor` ve `Student` tablolarla birlikte tarih.

![Tablo başına type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Her varlık sınıfı adı için bir veritabanı tablosu yaparak bu deseni *tablo başına tür* (TPT) devralma.

Karmaşık birleştirme sorgularda TPT desenleri sağladığından TPH devralma desenleri daha iyi performans varlık çerçevesi TPT devralma desenler, daha genel kullanıma sunun. Bu öğreticide, TPH devralma uygulanması gösterilmektedir. Bu, aşağıdaki adımları uygulayarak gerçekleştirirsiniz:

- Oluşturma bir `Person` sınıfı ve değiştirme `Instructor` ve `Student` sınıfların türetilmesi için `Person`.
- Model veritabanı eşleme kod veritabanı bağlam sınıfına ekleyin.
- Değişiklik `InstructorID` ve `StudentID` başvuruları projeye boyunca `PersonID`.

## <a name="creating-the-person-class"></a>Kişi sınıfı oluşturma

 Not: Bu sınıfları kullanan denetleyicileri güncelleştirilene kadar aşağıdaki sınıflar oluşturduktan sonra projeyi derlemek mümkün olmayacaktır. 

İçinde *modelleri* klasör oluşturma *Person.cs* ve şablon kodunu aşağıdaki kodla değiştirin:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

İçinde *Instructor.cs*, türetilen `Instructor` gelen sınıfı `Person` sınıfı ve anahtar ve ad alanlarını kaldırın. Kod, aşağıdaki örnekteki gibi görünür:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Benzer değişiklik *Student.cs*. `Student` Sınıfı aşağıdaki örnekteki gibi görünür:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Modele kişi varlık türü ekleme

İçinde *SchoolContext.cs*, ekleme bir `DbSet` özelliği `Person` varlık türü:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Entity Framework tablo başına hiyerarşi devralmayı yapılandırmak için gereken budur. Veritabanı yeniden oluşturulan olduğunda, anlatıldığı gibi sahip bir `Person` yerine tablo `Student` ve `Instructor` tablolar.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Personıd için Instructorıd ve StudentID değiştirme

İçinde *SchoolContext.cs*, kurs Eğitmen eşlemesini deyiminde değiştirme `MapRightKey("InstructorID")` için `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Bu değişiklik, gerekmez; yalnızca çoktan bire çok birleşme tablosundaki Instructorıd adını da değiştirir. Uygulama adı Instructorıd olarak bırakılırsa, yine de düzgün çalışır. İşte tamamlanmış *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Sonraki değiştirmenize gerek `InstructorID` için `PersonID` ve `StudentID` için `PersonID` proje boyunca ***dışında*** zaman damgalı geçişleri dosyalarında *geçişler* klasör. Bunu yapmak için bulun ve yalnızca değiştirilmesi gereken dosyaları açın ardından açılan dosyalarda genel bir değişiklik yapmak. Yalnızca dosyasında *geçişler* değiştirmeniz gerekir klasördür *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Visual Studio'da tüm açık dosyalar kapatarak başlayın.
2. Tıklayın **Bul ve Değiştir – tüm dosyaları bulur** içinde **Düzenle** menüsüne ve ardından arama projesinde içeren tüm dosyaları için `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Her dosyada açın **sonuçları Bul** penceresi ***dışında*** &lt;zaman damgası&gt;*\_.cs* geçişdosyalarında*Geçişler* klasöründe her dosya için bir satır çift tıklayın.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Açık **dosyalarda Değiştir** iletişim ve değişiklik **konum** için **tüm açık belgeleri**.
5. Kullanım **dosyalarda Değiştir** tüm değiştirmek için iletişim `InstructorID` için `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Tüm dosyaları içeren projede bulma `StudentID`.
7. Her dosyada açın **sonuçları Bul** penceresi ***dışında*** &lt;zaman damgası&gt;*\_\*.cs* geçiş dosyaları içinde *geçişler* klasöründe her dosya için bir satır çift tıklayın.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Açık **dosyalarda Değiştir** iletişim ve değişiklik **konum** için **tüm açık belgeleri**.
9. Kullanım **dosyalarda Değiştir** tüm değiştirmek için iletişim `StudentID` için `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Projeyi oluşturun.

(Bu gösterir Not bir *olumsuz* , `classnameID` birincil anahtarları adlandırma deseni. Birincil anahtarlar kimliği sınıf adını önek olmadan adlı *hiçbir* yeniden adlandırma olacak gerekli artık.)

## <a name="create-and-update-a-migrations-file"></a>Oluşturma ve geçişler dosyasını güncelleştirme

Paket Yöneticisi Konsolu (PMC'de), aşağıdaki komutu girin:

`Add-Migration Inheritance`

Çalıştırma `Update-Database` PMC'yi komutunu. Geçişleri ne yapılacağını bildiğiniz olmayan mevcut verileri aldık komutu bu noktada başarısız olur. Aşağıdaki hatayı alıyorsunuz:

*ALTER TABLE deyimini FOREIGN KEY kısıtlaması ile çakıştı. "FK\_dbo. Departman\_dbo. Kişi\_Personıd ". Veritabanı "ContosoUniversity" Tablo "dbo. çakışma oluştu Kişi", sütun 'Personıd'.*

Açık *geçişler\&lt; zaman damgası&gt;\_Inheritance.cs* değiştirin `Up` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Çalıştırma `update-database` yeniden komutu.

> [!NOTE]
> Veri ve yapma şema değişiklikleri geçiş sırasında diğer hatalarıyla mümkündür. Geçiş hatalarla karşılaşırsanız, çözümleyemiyor, devam ederek öğreticiyle bağlantı dizesini değiştirerek *Web.config* dosya veya veritabanı siliniyor. Veritabanında yeniden adlandırmak için en basit yaklaşımdır *Web.config* dosya. Örneğin, CU için veritabanı adını değiştirin\_aşağıdaki örnekte gösterildiği gibi test:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Yeni bir veritabanı ile geçirmek için veri yoktur ve `update-database` hatasız tamamlanması çok daha büyük olasılıkla komutu. Veritabanı silme hakkında yönergeler için bkz: [Visual Studio 2012'den bir veritabanını bırakmak nasıl](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Öğretici ile devam etmek için bu yaklaşımı benimsemeniz durumunda, dağıtım adımı geçişleri otomatik olarak çalıştığında dağıtılan site aynı hata elde edebileceğiniz olduğundan bu öğreticinin sonunda atlayın. Geçişleri hatayı gidermek, en iyi bir Entity Framework forumları veya StackOverflow.com kaynaktır.


## <a name="testing"></a>Sınama

Siteyi çalıştırın ve çeşitli sayfalar deneyin. Önce yaptığınız gibi her şey aynı çalışır.

İçinde **Sunucu Gezgini** genişletin **SchoolContext** ardından **tabloları**, ve gördüğünüz **Öğrenci** ve **Eğitmen**  tablolar tarafından değiştirildi bir **kişi** tablo. Genişletin **kişi** tablo ve bkz tüm bulunması için kullanılan sütunları olan **Öğrenci** ve **Eğitmen** tablolar.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Kişi tabloya sağ tıklayıp ardından **tablo verilerini Göster** ayrıştırıcı sütununu görmek için.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Aşağıdaki diyagram, yeni School veritabanını yapısını gösterir:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Özet

Tablo başına hiyerarşi devralma artık uygulandıktan için `Person`, `Student`, ve `Instructor` sınıfları. Bu ve diğer devralma yapıları hakkında daha fazla bilgi için bkz. [devralma eşleme stratejileri](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) Morteza Manavi'nın blogunda. Sonraki öğreticide, depo ve iş birimi desenleri uygulamak için bazı yollar görürsünüz.

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [İleri](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
