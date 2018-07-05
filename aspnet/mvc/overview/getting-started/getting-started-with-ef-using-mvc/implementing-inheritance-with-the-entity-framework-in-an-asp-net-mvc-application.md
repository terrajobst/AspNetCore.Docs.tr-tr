---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Bir ASP.NET MVC 5 uygulamasında (11, 12) devralma Entity Framework 6 ile uygulama | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d10656f5ce0479e1290fa6921f2d1a99d1081b57
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382984"
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>Bir ASP.NET MVC 5 uygulamasında (11, 12) devralma Entity Framework 6 ile uygulama
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Tamamlanmış projeyi indirmek](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) veya [PDF olarak indirin](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio 2013 kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Önceki öğreticide eşzamanlılık özel durumları işlenir. Bu öğreticide, veri modelinde devralma uygulanması gösterilmektedir.

Nesne yönelimli programlama, kullandığınız [devralma](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) kolaylaştırmak için [yeniden kod](http://en.wikipedia.org/wiki/Code_reuse). Bu öğreticide, değiştireceksiniz `Instructor` ve `Student` oldukları türetilmesi sınıflara bir `Person` temel gibi özellikler içeren sınıf `LastName` eğitmenler ve öğrenciler için ortak olan. Ekleyebilir veya herhangi bir web sayfalarını değiştirmesine olmaz ancak bazı kodları değiştireceksiniz ve bu değişiklikleri veritabanında otomatik olarak yansıtılır.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Veritabanı tabloları devralma eşleme seçenekleri

`Instructor` Ve `Student` sınıfları `School` veri modeline sahip aynı olan çeşitli özellikler:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Gereksiz kod tarafından paylaşılan özellikleri için kaldırmak istediğiniz varsayalım `Instructor` ve `Student` varlıklar. Veya adın bir eğitmen ya da bir öğrenci gelmediğini caring olmadan adları biçimlendirebilen hizmet yazmak istediğiniz. Oluşturabilirsiniz bir `Person` temel sınıfı, yalnızca bu paylaşılan özelliklerini içeren ve ardından olun `Instructor` ve `Student` varlıklar aşağıdaki çizimde gösterildiği gibi o temel sınıftan devralınan:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Bu devralma yapı veritabanında gösterilebilir birkaç yolu vardır. Sahip olabilir bir `Person` Öğrenciler hem tek bir tabloyu eğitmenlerini hakkında bilgi içeren tablo. Bazı sütunların yalnızca eğitmen için geçerli olabilir (`HireDate`), bazı yalnızca Öğrenciler (`EnrollmentDate`), bazı her ikisine de (`LastName`, `FirstName`). Genellikle, olurdu bir *ayrıştırıcı* her satır türü belirtmek için sütunu temsil eder. Örneğin, ayrıştırıcı sütununu "Eğitmen" eğitmenler ve "Öğrenci" Öğrenciler için olabilir.

![Tablo başına hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Bu düzen, tek veritabanı tablosundan bir varlık devralma yapısı oluşturma adlı *tablo başına hiyerarşi* (TPH) devralma.

Devralma yapısı gibi daha veritabanını yapmak için kullanılan bir alternatiftir. Yalnızca ad alanları gibi olabilir `Person` tablo ve ayrı sahip `Instructor` ve `Student` tablolarla birlikte tarih.

![Tablo başına type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Her varlık sınıfı adı için bir veritabanı tablosu yaparak bu deseni *tablo başına tür* (TPT) devralma.

Henüz başka bir seçenek tüm soyut olmayan türler için tek tek tablolar eşlemektir. Devralınan özellikler dahil olmak üzere, bir sınıfın tüm özellikler için karşılık gelen bir tablonun sütunlarını eşleyin. Bu düzen (TPC) tablo başına somut sınıf devralma çağrılır. TPC devralma için uygulanırsa `Person`, `Student`, ve `Instructor` sınıfları daha önce gösterildiği gibi `Student` ve `Instructor` tabloları benzeyecektir farklı Öncekine göre devralma uyguladıktan sonra.

Karmaşık birleştirme sorgularda TPT desenleri sağladığından TPC ve TPH devralma desenler genellikle daha iyi performans varlık çerçevesi TPT devralma desenleri daha sunar.

Bu öğreticide, TPH devralma uygulanması gösterilmektedir. TPH olan Entity Framework varsayılan devralma desende yapmanız gereken tek şey, bu nedenle oluşturma bir `Person` sınıfı, değişiklik `Instructor` ve `Student` sınıfların türetilmesi için `Person`, yeni sınıfa eklemek `DbContext`, oluşturup bir geçiş. (Bir devralma desenlerinin nasıl uygulandığını hakkında daha fazla bilgi için bkz. [tablo başına tür (TPT) devralma eşleme](https://msdn.microsoft.com/data/jj591617#2.5) ve [tablo başına somut sınıf (TPC) devralma eşleme](https://msdn.microsoft.com/data/jj591617#2.6) MSDN'de Entity Framework belgeleri.)

## <a name="create-the-person-class"></a>Kişi sınıfı oluşturma

İçinde *modelleri* klasör oluşturma *Person.cs* ve şablon kodunu aşağıdaki kodla değiştirin:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Öğrenci ve Eğitmenler sınıfları kişiden devral olun

İçinde *Instructor.cs*, türetilen `Instructor` gelen sınıfı `Person` sınıfı ve anahtar ve ad alanlarını kaldırın. Kod, aşağıdaki örnekteki gibi görünür:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Benzer değişiklik *Student.cs*. `Student` Sınıfı aşağıdaki örnekteki gibi görünür:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>Kişi varlık türü için modeli ekleme

İçinde *SchoolContext.cs*, ekleme bir `DbSet` özelliği `Person` varlık türü:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Entity Framework tablo başına hiyerarşi devralmayı yapılandırmak için gereken budur. Veritabanı güncelleştirildiğinde, anlatıldığı gibi sahip bir `Person` yerine tablo `Student` ve `Instructor` tablolar.

## <a name="create-and-update-a-migrations-file"></a>Oluşturma ve geçişler dosyasını güncelleştirme

Paket Yöneticisi Konsolu (PMC'de), aşağıdaki komutu girin:

`Add-Migration Inheritance`

Çalıştırma `Update-Database` PMC'yi komutunu. Geçişleri ne yapılacağını bildiğiniz olmayan mevcut verileri aldık komutu bu noktada başarısız olur. Bir hata iletisi aşağıdakine benzer olursunuz:

> *Nesne bırakılamadı ' dbo. Eğitmen ' yabancı anahtar kısıtlaması tarafından başvurulduğundan.*


Açık *geçişler\&lt; zaman damgası&gt;\_Inheritance.cs* değiştirin `Up` yöntemini aşağıdaki kod ile:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Bu kod aşağıdaki veritabanı güncelleştirme görevleri üstlenir:

- Yabancı anahtar kısıtlamaları ve Öğrenci tabloya noktası dizinleri kaldırır.
- Eğitmen tablo kişi olarak yeniden adlandırır ve Öğrenci verileri depolamak gerekli değişiklikleri yapar:

    - Öğrenciler için boş değer atanabilir EnrollmentDate ekler.
    - Bir satır için bir öğrenci ya da bir eğitmen olup olmadığını belirtmek için ayrıştırıcı sütununu ekler.
    - Öğrenci satırları seferde olmaz beri İşeAlmaTarihi boş değer atanabilir bir hale getirir.
    - Öğrenciler için işaret yabancı anahtarlar güncelleştirmek için kullanılan geçici bir alan ekler. Kişi tabloya Öğrenciler kopyaladığınızda yeni birincil anahtar değerlerini elde edersiniz.
- Kişi tabloya Öğrenci tablodan veri kopyalar. Bu, Öğrenciler, yeni birincil anahtar değerlerini atanan neden olur.
- Öğrenciler için işaret yabancı anahtar değerlerine düzeltir.
- Yabancı anahtar kısıtlamaları ve artık kişi tabloya işaret eden, dizinleri yeniden oluşturur.

(GUID yerine tamsayı birincil anahtar türü kullansaydınız, Öğrenci birincil anahtar değerlerini değiştirmek zorunda mıydı ve bu adımların birçok atlandı.)

Çalıştırma `update-database` yeniden komutu.

(Önceki veritabanı sürümüne geri dönmek için kullanan gerekiyordu durumunda bir üretim sisteminde karşılık gelen aşağı yöntemi değişiklik. Bu öğretici için aşağıya yöntemi kullanıyor olmanız gerekmez.)

> [!NOTE]
> Veri ve yapma şema değişiklikleri geçiş sırasında diğer hatalarıyla mümkündür. Geçiş hatalarla karşılaşırsanız, çözümleyemiyor, devam ederek öğreticiyle bağlantı dizesini değiştirerek *Web.config* dosya veya veritabanı silerek. Veritabanında yeniden adlandırmak için en basit yaklaşımdır *Web.config* dosya. Örneğin, veritabanı adını ContosoUniversity2 için aşağıdaki örnekte gösterildiği gibi değiştirin:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> Yeni bir veritabanı ile geçirmek için veri yoktur ve `update-database` hatasız tamamlanması çok daha büyük olasılıkla komutu. Veritabanı silme hakkında yönergeler için bkz: [Visual Studio 2012'den bir veritabanını bırakmak nasıl](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Öğretici ile devam etmek için bu yaklaşımı benimsemeniz durumunda, bu öğreticinin sonunda dağıtım adımı atlayın veya yeni site ve veritabanı'na dağıtın. Bir güncelleştirme zaten dağıtım siteye dağıtırsanız, EF geçişleri otomatik olarak çalıştığında aynı hatayı alırsınız. Geçişleri hatayı gidermek, en iyi bir Entity Framework forumları veya StackOverflow.com kaynaktır.


## <a name="testing"></a>Sınama

Siteyi çalıştırın ve çeşitli sayfalar deneyin. Önce yaptığınız gibi her şey aynı çalışır.

İçinde **Sunucu Gezgini** genişletin **veri Connections\SchoolContext** ardından **tabloları**, ve gördüğünüz **Öğrenci** ve **Eğitmen** tablolar tarafından değiştirildi bir **kişi** tablo. Genişletin **kişi** tablo ve bkz tüm bulunması için kullanılan sütunları olan **Öğrenci** ve **Eğitmen** tablolar.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Kişi tabloya sağ tıklayıp ardından **tablo verilerini Göster** ayrıştırıcı sütununu görmek için.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Aşağıdaki diyagram, yeni School veritabanını yapısını gösterir:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Bu bölümde isteğe bağlı tamamlamış olmanız gerekir **uygulamasını Azure'a dağıtma** konusundaki [bölüm 3, sıralama, filtreleme ve sayfalama](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) Bu öğretici serisinin. Veritabanı yerel projenizdeki silerek çözümlenen geçişleri hatasız tamamlanırsa, bu adımı atlayın; veya yeni site ve veritabanı oluşturun ve yeni ortama dağıtın.

1. Visual Studio'da projeye sağ **Çözüm Gezgini** seçip **Yayımla** bağlam menüsünden.  
  
    ![Proje bağlam menüsünde Yayımla](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. Tıklayın **yayımlama**.  
  
    ![Yayımlama](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
   Web uygulamasını varsayılan tarayıcınızda açılır.
3. Bunu doğrulamak için uygulamayı test etme çalışmaktadır.

    Bir sayfa, ilk kez çalıştırdığınızda, veritabanına erişir, Entity Framework tüm geçişlerde çalıştırır `Up` veritabanı geçerli bir veri modeli ile güncel duruma getirmek için gerekli yöntemleri.

## <a name="summary"></a>Özet

Tablo başına hiyerarşi devralma için uyguladık `Person`, `Student`, ve `Instructor` sınıfları. Bu ve diğer devralma yapıları hakkında daha fazla bilgi için bkz. [TPT devralma deseni](https://msdn.microsoft.com/data/jj618293) ve [TPH devralma deseni](https://msdn.microsoft.com/data/jj618292) MSDN'de. Sonraki öğreticide, bir göreceli olarak Gelişmiş Entity Framework senaryoları işlemek nasıl görürsünüz.

Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET veri erişimi - önerilen kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Önceki](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [İleri](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
