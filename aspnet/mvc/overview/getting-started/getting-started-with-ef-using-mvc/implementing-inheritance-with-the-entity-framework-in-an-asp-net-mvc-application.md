---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Bir ASP.NET MVC 5 uygulaması (11 / 12) Entity Framework 6 kalıtım uygulama | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1826659626106993d4796641492c62fcbd22a1b3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>Bir ASP.NET MVC 5 uygulaması (11 / 12) Entity Framework 6 kalıtım uygulama
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Önceki öğreticide eşzamanlılık işlenir. Bu öğreticide veri modelinde devralma uygulamak nasıl yapacağınızı gösterir.

Nesne odaklı programlama içinde kullandığınız [devralma](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) kolaylaştırmak için [yeniden kod](http://en.wikipedia.org/wiki/Code_reuse). Bu öğreticide, değiştireceğiz `Instructor` ve `Student` öğesinden türetilen sınıflara bir `Person` temel gibi özellikler içeren sınıfı `LastName` Eğitmen ve öğrenciler için ortak olan. Ekleme veya tüm web sayfalarını değiştirme olmaz ancak bazı kodları değiştireceksiniz ve bu değişiklikleri otomatik olarak veritabanında yansıtılır.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Veritabanı tablolarında devralma eşleme seçenekleri

`Instructor` Ve `Student` sınıfları `School` veri modeli özdeş çeşitli özellikleri vardır:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Tarafından paylaşılan özellikler için yedek kod ortadan kaldırmak istediğinizi varsayalım `Instructor` ve `Student` varlıklar. Veya adları adı bir eğitmen veya öğrencinin gelmediğini caring olmadan biçimlendirebilirsiniz bir hizmet yazmak istiyorsanız. Oluşturabilirsiniz bir `Person` temel yalnızca bu özellikleri paylaşılan içeren sınıfı sonra olun `Instructor` ve `Student` varlıklar aşağıdaki çizimde gösterildiği gibi bu taban sınıfından devral:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Bu devralma yapı veritabanında gösterilebilir birkaç yolu vardır. Sahip olabilir bir `Person` Öğrenciler ve tek bir tablodaki Eğitmen hakkında bilgi içeren tablo. Bazı sütunları yalnızca eğitmen için geçerli olabilir (`HireDate`), bazı yalnızca Öğrenciler (`EnrollmentDate`), bazı iki (`LastName`, `FirstName`). Genellikle, olurdu bir *Ayrıştırıcıyı* her satır tür belirtmek için sütun temsil eder. Örneğin, ayrıştırıcı sütunun "Eğitmen" Eğitmen ve "Öğrenci" Öğrenciler için olabilir.

![Table-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Bir varlığın devralma yapısı tek veritabanı tablosundan oluşturmanın bu deseni adlı *tablo başına hiyerarşisi* (TPH) devralma.

Devralma yapısı gibi daha fazla ara veritabanını yapmak için kullanılan bir alternatiftir. Yalnızca ad alanları gibi olabilir `Person` tablo ve ayrı sahip `Instructor` ve `Student` tarih alanları tablolarla.

![Tablo başına type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Her varlık sınıfı adlı için bir veritabanı tablosu yapma bu deseni *tablo türü başına* (birleştirilmiş TPT) devralma.

Henüz başka bir tek tek tablolar için tüm Özet olmayan türlerine eşlemek için bir seçenektir. Devralınan özellikler de dahil olmak üzere, bir sınıfın tüm özellikleri ilgili tablo sütunlarına eşleyin. Bu desen tablo başına somut sınıfı (TPC) devralma adı verilir. TPC devralma uygulanırsa `Person`, `Student`, ve `Instructor` sınıfları daha önce gösterildiği gibi `Student` ve `Instructor` tabloları uygulamamız görünecektir farklı Öncekine göre devralma kullanıldıktan sonra.

Birleştirilmiş TPT desenleri karmaşık JOIN sorguları sağladığından TPC ve TPH devralma desenleri genellikle daha iyi performans birleştirilmiş TPT devralma düzenleri daha Entity Framework sunar.

Bu öğretici nasıl TPH devralma uygulanacağını gösterir. TPH olduğundan Entity Framework varsayılan devralma desende yapmanız gereken tek şey oluşturma bir `Person` sınıfı, değişiklik `Instructor` ve `Student` öğesinden türetilen sınıflar `Person`, yeni sınıf ekleyin `DbContext`ve oluşturma bir geçiş. (Bir devralma desenlerini uygulama hakkında daha fazla bilgi için bkz: [birleştirilmiş tablo başına türü (TPT) devralma eşleme](https://msdn.microsoft.com/data/jj591617#2.5) ve [tablo başına somut sınıfı (TPC) devralma eşleme](https://msdn.microsoft.com/data/jj591617#2.6) MSDN'de Entity Framework belgelerine.)

## <a name="create-the-person-class"></a>Kişi sınıfı oluşturma

İçinde *modelleri* klasörü oluşturmak *Person.cs* ve şablon kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Kişiden devral Öğrenci ve Eğitmen sınıflarının

İçinde *Instructor.cs*, türetilen `Instructor` sınıfıyla `Person` sınıfı ve anahtar ve ad alanlarını kaldırın. Kod aşağıdaki gibi görünür:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Benzer değişiklik *Student.cs*. `Student` Sınıfı, aşağıdaki gibi görünür:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>Kişi varlık türü için Model ekleme

İçinde *SchoolContext.cs*, ekleme bir `DbSet` özelliği için `Person` varlık türü:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Entity Framework tablo başına hiyerarşisi devralma yapılandırmak için gereken budur. Veritabanı güncelleştirildiğinde, göreceğiniz gibi sahip bir `Person` yerine tablo `Student` ve `Instructor` tabloları.

## <a name="create-and-update-a-migrations-file"></a>Oluşturma ve geçişler dosyasını güncelleştirme

Paket Yöneticisi Konsolu (PMC)'da, aşağıdaki komutu girin:

`Add-Migration Inheritance`

Çalıştırma `Update-Database` PMC komutu. Geçişleri nasıl ele alınacağını bilmeniz değil mevcut verileri sahibiz komutu bu noktada başarısız olur. Aşağıdakine benzer bir hata iletisi alırsınız:

> *Nesne bırakılamıyor ' dbo. Eğitmen ' yabancı anahtar kısıtlaması tarafından başvurulduğu için.*


Açık *geçişler\&lt; zaman damgası&gt;\_Inheritance.cs* ve değiştirme `Up` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Bu kod, aşağıdaki veritabanı güncelleştirme görevleri mvc'deki:

- Yabancı anahtar kısıtlamaları ve Öğrenci tabloya noktası dizinleri kaldırır.
- Eğitmen tablo kişi olarak yeniden adlandırır ve Öğrenci verileri depolamak gerektiğinde değişiklikleri yapar:

    - Öğrenciler için boş değer atanabilir EnrollmentDate ekler.
    - Bir satır öğrencinin veya bir eğitmen olup olmadığını belirtmek için ayrıştırıcı sütunun ekler.
    - Öğrenci satırları seferde olmayacaktır beri İşeAlmaTarihi null atanamaz hale getirir.
    - Öğrenciler için noktası yabancı anahtarlar güncelleştirmek için kullanılan geçici bir alan ekler. Öğrenciler kişi tablosuna kopyaladığınızda, yeni birincil anahtar değerlerinin elde edersiniz.
- Kişi tabloya Öğrenci tablodan veri kopyalar. Bu, yeni birincil anahtar değerlerinin atanan Öğrenciler neden olur.
- Öğrenciler için noktası yabancı anahtar değerleri giderir.
- Yabancı anahtar kısıtlamaları ve şimdi kişi tabloya işaret eden dizinleri yeniden oluşturur.

(Birincil anahtar türü olarak tamsayı yerine GUID kullandıysanız, Öğrenci birincil anahtar değerlerinin değiştirmek zorunda olmayacaktır ve bu adımların çoğunun atlandı.)

Çalıştırma `update-database` yeniden komutu.

(Önceki veritabanı sürümüne geri dönmek için kullanan gerekiyordu durumunda bir üretim sisteminde karşılık gelen aşağı yöntemi değişiklik. Bu öğretici için aşağıya yöntemini kullanıyor olmanız çalışmaz.)

> [!NOTE]
> Veri ve yapma şema değişiklikleri geçirilirken diğer hatalarıyla mümkündür. Geçiş hataları alırsanız, olamaz gidermek, devam edebilirsiniz eğitici bağlantı dizesinde değiştirerek *Web.config* dosya veya göre veritabanı siliniyor. Veritabanında yeniden adlandırmak için basit yaklaşımdır *Web.config* dosya. Örneğin, veritabanı adını ContosoUniversity2 için aşağıdaki örnekte gösterildiği gibi değiştirin:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> Yeni bir veritabanı ile geçirmek için veri yok ve `update-database` komut hatasız tamamlamak çok daha büyük bir olasılıkla. Veritabanını silmek yönergeler için bkz: [Visual Studio 2012'den bir veritabanını bırakmak nasıl](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Bu öğreticiyi devam edebilmek için bu yaklaşımı benimsemeniz durumunda, bu öğreticinin sonunda dağıtım adımı atlamak veya yeni site ve veritabanı için dağıtın. Bir güncelleştirmeyi zaten dağıtım siteye dağıtırsanız, geçişler otomatik çalıştırıldığında EF aynı hatayı alırsınız. Geçişler hata için sorun giderme istiyorsanız, en iyi kaynak Entity Framework forumları veya StackOverflow.com biridir.


## <a name="testing"></a>Sınama

Siteyi çalıştırın ve çeşitli sayfalar deneyin. Önce yaptığınız gibi her şey aynı çalışır.

İçinde **Sunucu Gezgini** genişletin **veri Connections\SchoolContext** ve ardından **tabloları**, ve, gördüğünüz **Öğrenci** ve **Eğitmen** tabloları değiştirilir tarafından bir **kişi** tablo. Genişletme **kişi** tablo göreceksiniz bulunması için kullanılan sütunların tümünün sahip **Öğrenci** ve **Eğitmen** tabloları.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Kişi tabloyu sağ tıklatın ve ardından **Show Table Data** ayrıştırıcı sütunun görmek için.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Aşağıdaki diyagram yeni Okul veritabanının yapısını gösterir:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Azure'a dağıtma

Bu bölümde isteğe bağlı tamamladınız gerektirir **uygulamayı Azure'a dağıtan** bölümüne [bölümü sıralama, filtreleme ve disk belleği 3](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) Bu öğretici dizi. Yerel projenizi veritabanında silerek çözülmüş geçişler hatalar varsa, bu adımı atlayın; veya yeni site ve veritabanı oluşturun ve yeni ortamınıza dağıtın.

1. Visual Studio'da'nde projeye sağ **Çözüm Gezgini** seçip **Yayımla** ve bağlam menüsünden.  
  
    ![Proje bağlam menüsünde Yayımla](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. Tıklatın **yayımlama**.  
  
    ![Yayımlama](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
   Web uygulaması, varsayılan tarayıcıda açılır.
3. Bunu doğrulamak için uygulamayı test çalışıyor.

    Bir sayfa, ilk kez çalıştırdığınızda veritabanına erişir, Entity Framework tüm geçişler çalıştırılır `Up` veritabanı geçerli veri modeli ile güncel duruma getirmek için gereken yöntemleri.

## <a name="summary"></a>Özet

Tablo başına hiyerarşisi devralma uyguladık `Person`, `Student`, ve `Instructor` sınıfları. Bu ve diğer devralma yapıları hakkında daha fazla bilgi için bkz: [birleştirilmiş TPT devralma deseni](https://msdn.microsoft.com/data/jj618293) ve [TPH devralma deseni](https://msdn.microsoft.com/data/jj618292) konusuna bakın. Sonraki öğreticide çeşitli göreceli olarak Gelişmiş Entity Framework senaryolarda nasıl ele alınacağını görürsünüz.

Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access - kaynakları önerilen](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [sonraki](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
