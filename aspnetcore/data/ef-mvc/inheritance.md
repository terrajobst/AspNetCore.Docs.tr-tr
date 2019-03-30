---
title: 'Öğretici: Devralma - EF çekirdekli ASP.NET MVC uygulama'
description: Bu öğreticide, Entity Framework Core ASP.NET Core uygulamasını kullanarak veri modelinde, devralma uygulanması gösterilmektedir.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/inheritance
ms.openlocfilehash: ba688b259206f21045376b6a38a266ad7528aec1
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750773"
---
# <a name="tutorial-implement-inheritance---aspnet-mvc-with-ef-core"></a>Öğretici: Devralma - EF çekirdekli ASP.NET MVC uygulama

Önceki öğreticide eşzamanlılık özel durumları işlenir. Bu öğreticide, veri modelinde devralma uygulanması gösterilmektedir.

Nesne yönelimli programlama, devralma, kod yeniden kullanımını kolaylaştırmak için kullanabilirsiniz. Bu öğreticide, değiştireceksiniz `Instructor` ve `Student` oldukları türetilmesi sınıflara bir `Person` temel gibi özellikler içeren sınıf `LastName` eğitmenler ve öğrenciler için ortak olan. Ekleyebilir veya herhangi bir web sayfalarını değiştirmesine olmaz ancak bazı kodları değiştireceksiniz ve bu değişiklikleri veritabanında otomatik olarak yansıtılır.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Devralma için veritabanı eşleme
> * Kişi sınıfı oluşturma
> * Güncelleştirme Eğitmen ve Öğrenci
> * Modele Kişi Ekle
> * Oluşturma ve geçişler güncelleştirme
> * Uygulama testi

## <a name="prerequisites"></a>Önkoşullar

* [Eşzamanlılık işleme](concurrency.md)

## <a name="map-inheritance-to-database"></a>Devralma için veritabanı eşleme

`Instructor` Ve `Student` sınıflarının Okul veri modelinde aynı olan çeşitli özellikler vardır:

![Öğrenci ve Eğitmenler sınıfları](inheritance/_static/no-inheritance.png)

Gereksiz kod tarafından paylaşılan özellikleri için kaldırmak istediğiniz varsayalım `Instructor` ve `Student` varlıklar. Veya adın bir eğitmen ya da bir öğrenci gelmediğini caring olmadan adları biçimlendirebilen hizmet yazmak istediğiniz. Oluşturabilirsiniz bir `Person` temel sınıfının yalnızca bu paylaşılan özelliklerini içeren ve ardından olun `Instructor` ve `Student` sınıfları, aşağıdaki çizimde gösterildiği gibi o temel sınıftan devralınan:

![Kişi sınıftan türetme Öğrenci ve Eğitmenler sınıfları](inheritance/_static/inheritance.png)

Bu devralma yapı veritabanında gösterilebilir birkaç yolu vardır. Öğrenciler hem de tek bir tabloyu eğitmenlerini hakkında bilgi içeren bir kişi tablo olabilir. Bazı sütunların Eğitmenler (İşeAlmaTarihi), bazı yalnızca Öğrenciler (EnrollmentDate) bazı hem (Soyadı, FirstName) için yalnızca uygulayabilirsiniz. Genellikle, her satırı temsil eder. hangi türü belirtmek için bir ayrıştırıcı sütunu gerekir. Örneğin, ayrıştırıcı sütununu "Eğitmen" eğitmenler ve "Öğrenci" Öğrenciler için olabilir.

![Tablo başına hiyerarşi örneği](inheritance/_static/tph.png)

Bu düzen, tek veritabanı tablosundan bir varlık devralma yapısı oluşturma, tablo başına hiyerarşi (TPH) devralma çağrılır.

Devralma yapısı gibi daha veritabanını yapmak için kullanılan bir alternatiftir. Örneğin, kişi tablosu yalnızca ad alanlarına sahip ve tarih alanları ayrı Eğitmen ve Öğrenci tablolarla sahip.

![Tablo başına tür devralma](inheritance/_static/tpt.png)

Bu düzen, her varlık sınıfı için bir veritabanı tablosu oluşturma, tablo başına tür (TPT) devralma çağrılır.

Henüz başka bir seçenek tüm soyut olmayan türler için tek tek tablolar eşlemektir. Devralınan özellikler dahil olmak üzere, bir sınıfın tüm özellikler için karşılık gelen bir tablonun sütunlarını eşleyin. Bu düzen (TPC) tablo başına somut sınıf devralma çağrılır. TPC devralma kişi, Öğrenci ve Eğitmen sınıfları için daha önce gösterildiği gibi uygulanırsa, Öğrenci ve Eğitmenler tabloları farklı Öncekine göre devralma kullanıldıktan sonra görünür.

Karmaşık birleştirme sorgularda TPT desenleri sağladığından TPC ve TPH devralma desenleri genellikle TPT devralma desenleri daha iyi performans sunar.

Bu öğreticide, TPH devralma uygulanması gösterilmektedir. TPH Entity Framework Core destekleyen tek devralma modelidir.  Ne yaparsınız oluşturmaktır bir `Person` sınıfı, değişiklik `Instructor` ve `Student` sınıfların türetilmesi için `Person`, yeni sınıfa eklemek `DbContext`ve bir geçiş oluşturun.

> [!TIP]
> Aşağıdaki değişiklikler yapmadan önce bir kopyasını bir projeyi kaydetmeyi tercih edin.  Ardından, sorunları ve baştan gerek çalıştırdığınızda Bu öğretici için gerçekleştirilen adımlar ters veya devam eden yerine kaydedilmiş projeden tekrar başlangıcını tüm dizileri başlatmak daha kolay olacaktır.

## <a name="create-the-person-class"></a>Kişi sınıfı oluşturma

Modeller klasörü Person.cs oluşturma ve şablon kodunu aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="update-instructor-and-student"></a>Güncelleştirme Eğitmen ve Öğrenci

İçinde *Instructor.cs*, kişi Eğitmen sınıf türetin ve anahtar ve ad alanlarını kaldırın. Kod, aşağıdaki örnekteki gibi görünür:

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

İçinde aynı değişiklik *Student.cs*.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-person-to-the-model"></a>Modele Kişi Ekle

Kişi varlık türüne eklemek *SchoolContext.cs*. Yeni satırlar vurgulanır.

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

Entity Framework tablo başına hiyerarşi devralmayı yapılandırmak için gereken budur. Veritabanı güncelleştirildiğinde, göreceğiniz gibi Öğrenci ve Eğitmenler tabloları yerine kişi tabloya sahip olur.

## <a name="create-and-update-migrations"></a>Oluşturma ve geçişler güncelleştirme

Yaptığınız değişiklikleri kaydedin ve projeyi derleyin. Ardından proje klasöründe komut penceresi açın ve aşağıdaki komutu girin:

```console
dotnet ef migrations add Inheritance
```

Çalıştırma `database update` henüz komutu. Bu eğitmen tabloyu bırakmak ve kişiye Öğrenci tabloyu yeniden adlandırmak için bu komut, veri kaybına neden olur. Mevcut verilerinizi korumak için özel kod girmenize gerek.

Açık *geçişleri /\<zaman damgası > _Inheritance.cs* değiştirin `Up` yöntemini aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

Bu kod aşağıdaki veritabanı güncelleştirme görevleri üstlenir:

* Yabancı anahtar kısıtlamaları ve Öğrenci tabloya noktası dizinleri kaldırır.

* Eğitmen tablo kişi olarak yeniden adlandırır ve Öğrenci verileri depolamak gerekli değişiklikleri yapar:

* Öğrenciler için boş değer atanabilir EnrollmentDate ekler.

* Bir satır için bir öğrenci ya da bir eğitmen olup olmadığını belirtmek için ayrıştırıcı sütununu ekler.

* Öğrenci satırları seferde olmaz beri İşeAlmaTarihi boş değer atanabilir bir hale getirir.

* Öğrenciler için işaret yabancı anahtarlar güncelleştirmek için kullanılan geçici bir alan ekler. Kişi tabloya Öğrenciler kopyaladığınızda, yeni birincil anahtar değerlerini alırlar.

* Kişi tabloya Öğrenci tablodan veri kopyalar. Bu, Öğrenciler, yeni birincil anahtar değerlerini atanan neden olur.

* Öğrenciler için işaret yabancı anahtar değerlerine düzeltir.

* Yabancı anahtar kısıtlamaları ve artık kişi tabloya işaret eden, dizinleri yeniden oluşturur.

(GUID yerine tamsayı birincil anahtar türü kullansaydınız, Öğrenci birincil anahtar değerlerini değiştirmek zorunda mıydı ve bu adımların birçok atlandı.)

Çalıştırma `database update` komutu:

```console
dotnet ef database update
```

(Üretim sisteminde karşılık gelen değişiklikleri yapacağınız `Down` durumda daha önce yaptığı, önceki veritabanı sürümüne geri dönmek için kullanılacak yöntem. Bu öğretici, olmaz kullanıyor `Down` yöntemi.)

> [!NOTE]
> Diğer hatalar mevcut veriler varsa bir veritabanında şema değişiklik yaparken almak mümkündür. Gideremezsiniz Geçiş hataları alırsanız, bağlantı dizesi içinde veritabanı adını değiştirin veya veritabanını silin. Yeni bir veritabanı geçirmek için veri yok ve veritabanını güncelleştir komut hatasız tamamlanması daha olasıdır. Veritabanını silmek için SSOX kullandığınızda veya `database drop` CLI komutu.

## <a name="test-the-implementation"></a>Uygulama testi

Uygulamayı çalıştırın ve çeşitli sayfalar deneyin. Önce yaptığınız gibi her şey aynı çalışır.

İçinde **SQL Server Nesne Gezgini**, genişletin **veri bağlantıları/SchoolContext** ardından **tabloları**, ve Öğrenci ve Eğitmenler tabloları almıştır gördüğünüz bir Kişi tablo. Kişi Tablo Tasarımcısı'nı açın ve tüm Öğrenci ve Eğitmenler tablolarında kullanılabilir sütunların olduğunu görürsünüz.

![Kişi SSOX tablosunda](inheritance/_static/ssox-person-table.png)

Kişi tabloya sağ tıklayıp ardından **tablo verilerini Göster** ayrıştırıcı sütununu görmek için.

![Kişi tablosunda SSOX - tablo verileri](inheritance/_static/ssox-person-data.png)

## <a name="get-the-code"></a>Kodu alma

[İndirme veya tamamlanmış uygulamanın görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="additional-resources"></a>Ek kaynaklar

Entity Framework Core içinde devralma hakkında daha fazla bilgi için bkz. [devralma](/ef/core/modeling/inheritance).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Veritabanı için eşlenen devralma
> * Kişi Sınıf oluşturuldu
> * Güncelleştirilmiş bir eğitmen ve Öğrenci
> * Eklenen kişi modeli
> * Oluşturulan ve güncelleştirme geçişleri
> * Test uygulaması

Çeşitli oldukça gelişmiş Entity Framework senaryoları ele öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Sonraki: Gelişmiş konular](advanced.md)
