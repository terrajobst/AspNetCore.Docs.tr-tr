---
title: "EF çekirdek - devralma - 9, 10 ile ASP.NET Core MVC"
author: tdykstra
description: "Bu öğretici ASP.NET Core uygulamada Entity Framework Çekirdek kullanarak veri modelindeki devralma uygulamak nasıl yapacağınızı gösterir."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 985cc38b10ef830b8274e40ad5f7050157fd4d86
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a>Devralma - EF çekirdek ASP.NET Core MVC Öğreticisi (9, 10)

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).

Önceki öğreticide eşzamanlılık işlenir. Bu öğreticide veri modelinde devralma uygulamak nasıl yapacağınızı gösterir.

Nesne odaklı programlama kodu yeniden kullanma kolaylaştırmak için devralma kullanabilirsiniz. Bu öğreticide, değiştireceğiz `Instructor` ve `Student` öğesinden türetilen sınıflara bir `Person` temel gibi özellikler içeren sınıfı `LastName` Eğitmen ve öğrenciler için ortak olan. Ekleme veya tüm web sayfalarını değiştirme olmaz ancak bazı kodları değiştireceksiniz ve bu değişiklikleri otomatik olarak veritabanında yansıtılır.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Veritabanı tablolarında devralma eşleme seçenekleri

`Instructor` Ve `Student` sınıfları Okul veri modelindeki özdeş çeşitli özellikleri vardır:

![Öğrenci ve Eğitmen sınıfları](inheritance/_static/no-inheritance.png)

Tarafından paylaşılan özellikler için yedek kod ortadan kaldırmak istediğinizi varsayalım `Instructor` ve `Student` varlıklar. Veya adları adı bir eğitmen veya öğrencinin gelmediğini caring olmadan biçimlendirebilirsiniz bir hizmet yazmak istiyorsanız. Oluşturabilirsiniz bir `Person` temel yalnızca bu özellikleri paylaşılan içeren sınıfı sonra olun `Instructor` ve `Student` sınıfları aşağıdaki çizimde gösterildiği gibi taban sınıfından devralan:

![Kişi sınıfından türetilen Öğrenci ve Eğitmen sınıfları](inheritance/_static/inheritance.png)

Bu devralma yapı veritabanında gösterilebilir birkaç yolu vardır. Öğrenciler ve tek bir tablodaki Eğitmen hakkında bilgi içeren bir kişinin Tablo olabilir. Bazı sütunları yalnızca Eğitmen (İşeAlmaTarihi), bazı yalnızca Öğrenciler (EnrollmentDate), bazı hem (Soyadı, adı) için geçerli olabilir. Genellikle, her satır temsil eden tür belirtmek için ayrıştırıcı sütunun gerekir. Örneğin, ayrıştırıcı sütunun "Eğitmen" Eğitmen ve "Öğrenci" Öğrenciler için olabilir.

![Tablo başına hiyerarşisi örneği](inheritance/_static/tph.png)

Bir varlığın devralma yapısı tek veritabanı tablosundan oluşturmanın bu deseni tablo başına hiyerarşisi (TPH) devralma adı verilir.

Devralma yapısı gibi daha fazla ara veritabanını yapmak için kullanılan bir alternatiftir. Örneğin, kişi tablosu yalnızca ad alanlarına sahip ve tarih alanları ayrı Eğitmen ve Öğrenci tablolarla sahip.

![Tür başına Tablo Devralma](inheritance/_static/tpt.png)

Bu desen her varlık sınıfı için bir veritabanı tablosu hale getirme türü (birleştirilmiş TPT) devralma her tablo adı verilir.

Henüz başka bir tek tek tablolar için tüm Özet olmayan türlerine eşlemek için bir seçenektir. Devralınan özellikler de dahil olmak üzere, bir sınıfın tüm özellikleri ilgili tablo sütunlarına eşleyin. Bu desen tablo başına somut sınıfı (TPC) devralma adı verilir. Daha önce gösterildiği gibi kişi, Öğrenci ve Eğitmen sınıfları için TPC devralma uygulanırsa Öğrenci ve Eğitmen tablolarını farklı Öncekine göre devralma kullanıldıktan sonra görünür.

Birleştirilmiş TPT desenleri karmaşık JOIN sorguları sağladığından TPC ve TPH devralma desenleri genellikle birleştirilmiş TPT devralma desenleri, daha iyi performans sunar.

Bu öğretici nasıl TPH devralma uygulanacağını gösterir. TPH Entity Framework Çekirdek destekleyen tek devralma deseni ortaya çıkar.  Ne siz gerçekleştirirsiniz oluşturmaktır bir `Person` sınıfı, değişiklik `Instructor` ve `Student` öğesinden türetilen sınıflar `Person`, yeni sınıf ekleyin `DbContext`ve bir geçiş oluşturun.

> [!TIP] 
> Aşağıdaki değişiklik yapmadan önce bir kopyasını proje kaydetme göz önünde bulundurun.  Ardından, sorunları ve baştan başlamak gerek alıyorsanız, Bu öğretici için yapılan adımları ters çevirme veya devam eden yerine kaydedilmiş projeden geri tüm dizileri başlangıcına başlatmak daha kolay olacaktır.

## <a name="create-the-person-class"></a>Kişi sınıfı oluşturma

Modeller klasörü Person.cs oluşturun ve şablon kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Kişiden devral Öğrenci ve Eğitmen sınıflarının

İçinde *Instructor.cs*, kişi sınıfından Eğitmen sınıf türetin ve anahtar ve ad alanlarını kaldırın. Kod aşağıdaki gibi görünür:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

İçinde aynı değişiklik *Student.cs*.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>Kişi varlık türü veri modeline Ekle

Kişi varlık türüne ekleme *SchoolContext.cs*. Yeni satırlar vurgulanır.

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

Entity Framework tablo başına hiyerarşisi devralma yapılandırmak için gereken budur. Veritabanı güncelleştirildiğinde, göreceğiniz gibi bir kişinin Tablo Öğrenci ve Eğitmen tabloları yerine sahip olur.

## <a name="create-and-customize-migration-code"></a>Oluşturma ve geçiş kodu özelleştirme

Yaptığınız değişiklikleri kaydedin ve projeyi oluşturun. Sonra proje klasöründe komut penceresi açın ve aşağıdaki komutu girin:

```console
dotnet ef migrations add Inheritance
```

Çalıştırma `database update` henüz komutu. Bu komut, eğitmen tablo bırakma ve Öğrenci tablosunu kişiye yeniden adlandırmak için veri kaybına neden olur. Mevcut verilerinizi korumak için özel kod sağlamanız gerekir.

Açık *geçişleri /\<zaman damgası > _Inheritance.cs* ve değiştirme `Up` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

Bu kod, aşağıdaki veritabanı güncelleştirme görevleri mvc'deki:

* Yabancı anahtar kısıtlamaları ve Öğrenci tabloya noktası dizinleri kaldırır.

* Eğitmen tablo kişi olarak yeniden adlandırır ve Öğrenci verileri depolamak gerektiğinde değişiklikleri yapar:

* Öğrenciler için boş değer atanabilir EnrollmentDate ekler.

* Bir satır öğrencinin veya bir eğitmen olup olmadığını belirtmek için ayrıştırıcı sütunun ekler.

* Öğrenci satırları seferde olmayacaktır beri İşeAlmaTarihi null atanamaz hale getirir.

* Öğrenciler için noktası yabancı anahtarlar güncelleştirmek için kullanılan geçici bir alan ekler. Kişi tablosuna Öğrenciler kopyaladığınızda, yeni birincil anahtar değerlerinin alır.

* Kişi tabloya Öğrenci tablodan veri kopyalar. Bu, yeni birincil anahtar değerlerinin atanan Öğrenciler neden olur.

* Öğrenciler için noktası yabancı anahtar değerleri giderir.

* Yabancı anahtar kısıtlamaları ve şimdi kişi tabloya işaret eden dizinleri yeniden oluşturur.

(Birincil anahtar türü olarak tamsayı yerine GUID kullandıysanız, Öğrenci birincil anahtar değerlerinin değiştirmek zorunda olmayacaktır ve bu adımların çoğunun atlandı.)

Çalıştırma `database update` komutu:

```console
dotnet ef database update
```

(Bir üretim sisteminde karşılık gelen değişiklikler yapacağınız `Down` yöntemi şimdiye kadar olan, önceki veritabanı sürümüne geri dönmek için kullanılacak durumda. Bu öğretici, olmaz kullanıyor `Down` yöntemi.)

> [!NOTE] 
> Şema değişiklikleri var olan verileri içeren bir veritabanına yaparken diğer hatalarıyla mümkündür. Çözümlenemiyor Geçiş hataları alırsanız, bağlantı dizesindeki veritabanı adını değiştirin veya veritabanını silin. Yeni bir veritabanı ile geçirmek için veri yok ve update-database komutunu hatasız tamamlamak daha yüksektir. Veritabanını silmek için SSOX kullanın veya çalıştırın `database drop` CLI komutu.

## <a name="test-with-inheritance-implemented"></a>Uygulanan devralma ile test

Uygulamayı çalıştırın ve çeşitli sayfalar deneyin. Önce yaptığınız gibi her şey aynı çalışır.

İçinde **SQL Server Nesne Gezgini**, genişletin **veri bağlantıları/SchoolContext** ve ardından **tabloları**, ve Öğrenci ve Eğitmen tabloları almıştır gördüğünüz bir Kişi tablo. Kişi Tablo Tasarımcısı'nı açın ve tüm Öğrenci ve Eğitmen tablolarında olarak kullanılan sütunlar olduğunu görürsünüz.

![SSOX Kişi tablosunda](inheritance/_static/ssox-person-table.png)

Kişi tabloyu sağ tıklatın ve ardından **Show Table Data** ayrıştırıcı sütunun görmek için.

![Kişi tablosunda SSOX - tablo verileri](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>Özet

Tablo başına hiyerarşisi devralma uyguladık `Person`, `Student`, ve `Instructor` sınıfları. Entity Framework Çekirdek devralma hakkında daha fazla bilgi için bkz: [devralma](https://docs.microsoft.com/ef/core/modeling/inheritance). Sonraki öğreticide çeşitli göreceli olarak Gelişmiş Entity Framework senaryolarda nasıl ele alınacağını görürsünüz.

>[!div class="step-by-step"]
[Önceki](concurrency.md)
[sonraki](advanced.md)  
