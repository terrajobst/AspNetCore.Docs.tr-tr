---
title: "Razor sayfalarının EF temel - eşzamanlılık - 8 8"
author: rick-anderson
description: "Bu öğretici, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir."
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: b36fb71cba058a3409b30a1d9469159fcd027375
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
en-us/

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a>Eşzamanlılık çakışmalarını - EF çekirdek Razor sayfaları (8 8) ile işleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [zel Dykstra](https://github.com/tdykstra), ve [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Bu öğretici, birden çok kullanıcı aynı anda (aynı anda) bir varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir. Olamaz çözmek sorunlarla karşılaşırsanız, indirme [Bu aşama için tamamlanan uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).

## <a name="concurrency-conflicts"></a>Eşzamanlılık çakışmaları

Bir eşzamanlılık çakışması ortaya çıkar zaman:

* Bir kullanıcıyı bir varlığın düzenleme sayfasına götürür.
* İlk kullanıcının değişiklik DB yazılmadan önce başka bir kullanıcı aynı varlık güncelleştirir.

Eşzamanlılık algılama etkin değil, eş zamanlı güncelleştirmeler olduğunda:

* Son güncelleştirme WINS. Diğer bir deyişle, son güncelleştirme değerleri Veritabanına kaydedilir.
* Geçerli güncelleştirmeleri ilk kaybolur.

### <a name="optimistic-concurrency"></a>İyimser eşzamanlılık

İyimser eşzamanlılık eşzamanlılık çakışması olmasını sağlar ve tepki verdiğini uygun olduğunda bunlar yapın. Örneğin, Jane departmanı Düzen sayfasını ziyaret ve bütçe İngilizce departmanı için 350,000.00 0,00 için değiştirir.

![Bütçe 0 olarak değiştirme](concurrency/_static/change-budget.png)

Jane tıklar önce **kaydetmek**, John aynı sayfasını ziyaret ve başlangıç tarihi alanı 9/1/2007'den 9/1/2013'e değiştirir.

![Başlangıç tarihi 2013 için değiştirme](concurrency/_static/change-date.png)

Jane tıklar **kaydetmek** ilk ve her tarayıcı dizin sayfası görüntülendiğinde değiştirmek görür.

![Bütçe sıfır olarak değiştirildi](concurrency/_static/budget-zero.png)

John tıklar **kaydetmek** bir düzenleme sayfasında $350,000.00 bütçe görüntülenmeye devam eder. Ne olacağını eşzamanlılık çakışmaları nasıl işleneceğini tarafından belirlenir.

İyimser eşzamanlılık aşağıdaki seçenekleri içerir:

* Bir kullanıcı değiştirdi hangi özelliğinin izlemek ve yalnızca karşılık gelen sütunlara DB'de güncelleştirin.

 Senaryoda, hiçbir veri kaybı olacaktır. Farklı özellikler iki kullanıcı tarafından güncelleştirildi. Birisi İngilizce departmanı gözatar sonraki açışınızda Jane'nın ve Can'ın değişiklikleri görürler. Güncelleştirme bu yöntem, veri kaybına sebep çakışmaları sayısını azaltabilirsiniz. Bu yaklaşım: * aynı özelliğe rakip bir değişiklik yaptıysanız veri kaybını önlemek olamaz.
        * Olan bir web uygulamasında pratik genellikle. Tüm getirilen ve yeni değerleri izlemek için önemli durum koruma gerektiriyor. Büyük miktarlarda durumu koruma uygulama performansını etkileyebilir.
        * Bir varlık eşzamanlılık algılamayı karşılaştırılan uygulama karmaşıklığını artırabilir.

* Jane'nın değişiklik üzerine Can'ın değişiklik izin verebilirsiniz.

 Sonraki birisi İngilizce departmanı gözatar, 1/9/2013 görürler ve getirilen $350,000.00 değeri. Bu yaklaşım adlı bir *istemci WINS* veya *WINS'de son* senaryo. (İstemci tüm değerleri veri deposunda nedir üzerinden önceliklidir.) Hiçbir eşzamanlılık işleme için kodlama yapmazsanız, istemci WINS otomatik olarak gerçekleşir.

* DB güncelleştirilmiş Can'ın değişiklik engelleyebilir. Uygulama zamanki: * bir hata iletisi görüntülenir.
        * Verilerin geçerli durumunu gösterir.
        * Değişiklikleri uygulamak verin.

 Bu adlı bir *deposu WINS* senaryo. (Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.) Bu öğreticide deposu WINS senaryo uygulayın. Bu yöntem, hiçbir değişiklik uyarı bir kullanıcı üzerine yazılır sağlar.

## <a name="handling-concurrency"></a>Eşzamanlılık işleme 

Olarak bir özellik yapılandırıldığında bir [eşzamanlılık belirteci](https://docs.microsoft.com/ef/core/modeling/concurrency):

* EF çekirdek getirildi sonra'nın özellik değiştirilmemiş doğrular. Onay oluşur zaman [SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) veya [SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) olarak adlandırılır.
* Bunu getirildikten sonra özelliği değiştirildiğinde, bir [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) atılır. 

Veritabanını ve veri modeli atma desteklemek için yapılandırılması gerekir `DbUpdateConcurrencyException`.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Bir özelliğe eşzamanlılık çakışmalarını algılama

Eşzamanlılık çakışması algılanan özelliği düzeyindeki [ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) özniteliği. Öznitelik, model üzerinde birden çok özellikleri uygulanabilir. Daha fazla bilgi için bkz: [veri ek açıklamaları-ConcurrencyCheck](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations).

`[ConcurrencyCheck]` Özniteliği, bu öğreticide kullanılan değil.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Bir satırda eşzamanlılık çakışmalarını algılama

Eşzamanlılık çakışması algılamak için bir [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) sütun izleme modele eklenir.  `rowversion` :

* SQL Server özeldir. Diğer veritabanlarını benzer bir özellik sağlamayabilir.
* DB'den getirildikten sonra'nın bir varlık değiştirilmedi belirlemek için kullanılır. 

DB bir sıralı oluşturur `rowversion` her satırın artar numarası güncelleştirilir. İçinde bir `Update` veya `Delete` komutu, `Where` yan tümcesi içeren getirilen değeri `rowversion`. Güncelleştirilen satır değiştiyse:

 * `rowversion`getirilen değerle eşleşmiyor.
 * `Update` Veya `Delete` olduğundan, komutları bir satır bulmak yok `Where` yan tümcesi içeren getirilen `rowversion`.
 * A `DbUpdateConcurrencyException` atılır.

EF tarafından hiçbir satır güncelleştirildiğinde çekirdek içinde bir `Update` veya `Delete` komutu, bir eşzamanlılık özel durumu oluşur.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Departman varlığa izleme özellik ekleme

İçinde *Models/Department.cs*, RowVersion adlı bir izleme özelliği ekleyin:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

[Zaman damgası](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) özniteliği belirtir. Bu sütunda yer `Where` yan tümcesi `Update` ve `Delete` komutları. Öznitelik adı verilen `Timestamp` bir SQL SQL Server'ın önceki sürümlerinde kullanılan çünkü `timestamp` önce SQL veri türü `rowversion` türü değiştirildi.

Fluent API izleme özelliği de belirtebilirsiniz:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

Aşağıdaki kod bir bölüm adı güncelleştirildiğinde EF çekirdek tarafından oluşturulan T-SQL bölümünü gösterir:

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

Vurgulanmış kodu gösterir önceki `WHERE` yan tümcesi içeren `RowVersion`. Varsa DB `RowVersion` eşit olmayan `RowVersion` parametre (`@p2`), satır güncelleştirildi.

Aşağıdaki vurgulanmış kodu tam olarak bir satır güncelleştirildi doğrular T-SQL gösterir:

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT ](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql) son deyiminden etkilenen satırların sayısını döndürür. Hayır, satırları güncelleştirilir, EF çekirdek oluşturur bir `DbUpdateConcurrencyException`.

Visual Studio çıktı penceresinde T-SQL EF çekirdeği oluşturur görebilirsiniz.

### <a name="update-the-db"></a>DB güncelleştir

Ekleme `RowVersion` özelliğini bir geçiş gerektirir DB modeli değiştirir.

Projeyi oluşturun. Bir komut penceresinde aşağıdakileri girin:

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

Yukarıdaki komutlar:

* Ekler *geçişleri / {zaman stamp}_RowVersion.cs* geçiş dosyası.
* Güncelleştirmeleri *Migrations/SchoolContextModelSnapshot.cs* dosya. Bu güncelleştirme aşağıdaki vurgulanmış kodu ekler `BuildModel` yöntemi:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* DB güncelleştirmek için geçiş çalışır.

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a>İskele Departmanlar modeli

* Visual Studio'dan çıkın.
* Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).
* Şu komutu çalıştırın:

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

Yukarıdaki komut iskelesini kurar `Department` modeli. Projesini Visual Studio'da açın.

Projeyi oluşturun. Derleme hataları aşağıdaki gibi oluşturur:

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Genel olarak değiştirmek `_context.Department` için `_context.Departments` ("s" eklemek diğer bir deyişle, `Department`). 7 oluşumu bulundu ve güncelleştirildi.

### <a name="update-the-departments-index-page"></a>Güncelleştirme Departmanlar dizin sayfası

Oluşturulan iskele altyapısı bir `RowVersion` dizin sayfası, ancak bu alan için sütun döndürmemelidir görüntülenmesi. Bu öğreticide son baytını `RowVersion` eşzamanlılık anlamanıza yardımcı olması için görüntülenir. Son bayta kalan benzersiz olması garanti değil. Gerçek bir uygulama görüntüle olmayacaktır `RowVersion` veya son baytını `RowVersion`.

Dizin Sayfası güncelleştirin:

* Dizin Departmanlar ile değiştirin.
* Biçimlendirme içeren Değiştir `RowVersion` son baytını ile `RowVersion`.
* FirstMidName FullName ile değiştirin.

Aşağıdaki biçimlendirmede güncelleştirilmiş sayfası gösterir:

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Güncelleştirmeyi Düzenle sayfası modeli

Güncelleştirme *pages\departments\edit.cshtml.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Bir eşzamanlılık sorunu algılamak için [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) ile güncelleştirilmiş `rowVersion` alınan varlık değeri. EF çekirdek özgün içeren bir WHERE yan tümcesi ile SQL güncelleştirme komut oluşturur `RowVersion` değeri. Hiçbir satır güncelleştirme komutu tarafından etkilenen varsa (özgün hiçbir satır sahip `RowVersion` değeri), bir `DbUpdateConcurrencyException` özel durumu oluşur.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

Önceki kod `Department.RowVersion` varlık getirildi değeri olur. `OriginalValue`DB değer olduğunda `FirstOrDefaultAsync` bu yöntemi çağrıldı.

Aşağıdaki kod, istemci (Bu yönteme gönderilen değerler) ve DB değerlerini alır:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

DB sahip her bir sütunun ne için deftere farklı değerler için aşağıdaki kod bir özel hata iletisi ekler `OnPostAsync`:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

Aşağıda vurgulanan kod kümeleri `RowVersion` değerden yeni değere Veritabanından alınır. Kullanıcı, sonraki açışınızda **kaydetmek**, düzenleme sayfasını son görüntüsünü yakalanan bu yana, eşzamanlılık hataları.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

`ModelState.Remove` Deyimi, çünkü gereklidir `ModelState` eski sahip `RowVersion` değeri. Razor sayfasındaki `ModelState` her ikisi de mevcut olduğunda bir alan modeli özellik değerlerini önceliklidir için bir değer.

## <a name="update-the-edit-page"></a>Güncelleştirmeyi Düzenle sayfası

Güncelleştirme *Pages/Departments/Edit.cshtml* aşağıdaki biçimlendirme ile:

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Önceki biçimlendirme:

* Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.
* Gizli satır sürümü ekler. `RowVersion`POST geri değere bağlar şekilde eklenmesi gerekir.
* Son baytını görüntüler `RowVersion` hata ayıklama amacıyla.
* Değiştirir `ViewData` kesin türü belirtilmiş ile `InstructorNameSL`.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Test eşzamanlılık düzenleme sayfasını çakışıyor

İki tarayıcılar örneklerinde düzenleme İngilizce departmanı açın:

* Uygulamayı çalıştırın ve Departmanlar seçin.
* Sağ **Düzenle** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**.
* İlk sekmesini tıklatın **Düzenle** İngilizce departmanı için köprü.

İki tarayıcı sekmeleri aynı bilgileri görüntüler.

İlk tarayıcı sekmesinde adını değiştirip tıklatın **kaydetmek**.

![Departman düzenleme değişiklikten sonra sayfa 1](concurrency/_static/edit-after-change-1.png)

Tarayıcı değiştirilen değeri ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir. Güncelleştirilmiş rowVersion göstergesi dikkat edin diğer sekmesinde ikinci geri gönderme üzerinde görüntülenir.

İkinci bir tarayıcı sekmesinde başka bir alanı değiştirin.

![Departman düzenleme değişiklikten sonra sayfa 2](concurrency/_static/edit-after-change-2.png)

**Kaydet**'e tıklayın. DB değerleri eşleşmiyor tüm alanlar için hata iletilerine bakın:

![Departman Düzenle sayfası hata iletisi](concurrency/_static/edit-error.png)

Bu tarayıcı penceresini ad alanı değiştirmek istiyorsanız alamadık. Kopyalama ve geçerli değeri (dilleri) ad alanına yapıştırabilirsiniz. Out sekmesi. İstemci tarafı doğrulama hata iletisi kaldırır.

![Departman Düzenle sayfası hata iletisi](concurrency/_static/cv.png)

Tıklatın **kaydetmek** yeniden. İkinci bir tarayıcı sekmesinde girdiğiniz değer kaydedilir. Dizin Sayfası kaydedilen değerler bakın.

## <a name="update-the-delete-page"></a>Güncelleştirme Sil sayfası

Delete sayfa modeli aşağıdaki kod ile güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

Varlık getirildi sonra değiştiğinde Sil sayfasında eşzamanlılık çakışması algılar. `Department.RowVersion`satır sürümü varlık getirildi durumdur. EF çekirdek SQL DELETE komutu oluşturduğunda, bir WHERE yan tümcesi ile içeren `RowVersion`. Sıfır satır SQL DELETE komutu sonuçları etkilenen ise:

* `RowVersion` SQL DELETE komutu eşleşmeyen `RowVersion` DB'de.
* DbUpdateConcurrencyException özel durum oluşur.
* `OnGetAsync`çağrılır `concurrencyError`.

### <a name="update-the-delete-page"></a>Güncelleştirme Sil sayfası

Güncelleştirme *Pages/Departments/Delete.cshtml* aşağıdaki kod ile:

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:

* Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.
* Bir hata iletisi ekler.
* FullName içinde FirstMidName değiştirir **yönetici** alan.
* Değişiklikleri `RowVersion` son bayta kalan görüntülemek için.
* Gizli satır sürümü ekler. `RowVersion`POST geri değere bağlar şekilde eklenmesi gerekir.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Test eşzamanlılık Delete sayfa çakışıyor

Bir test bölüm oluşturun.

İki tarayıcılar örneklerinde Delete test departmanı açın:

* Uygulamayı çalıştırın ve Departmanlar seçin.
* Sağ **silmek** seçin ve test departmanı için köprü **yeni sekmede aç**.
* Tıklatın **Düzenle** test departmanı için köprü.

İki tarayıcı sekmeleri aynı bilgileri görüntüler.

İlk tarayıcı sekmesinde bütçe değiştirip'ı **kaydetmek**.

Tarayıcı değiştirilen değeri ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir. Güncelleştirilmiş rowVersion göstergesi dikkat edin diğer sekmesinde ikinci geri gönderme üzerinde görüntülenir.

Test departmanı ikinci sekmesinden silin. Bir eşzamanlılık hatası DB'den geçerli değerlerle görüntülenir. Tıklatarak **silmek** sürece varlığı silen `RowVersion` updated.department silinmiş olmuştur.

Bkz: [devralma](xref:data/ef-mvc/inheritance) nasıl bir veri modeli devralır.

### <a name="additional-resources"></a>Ek kaynaklar

* [EF çekirdek eşzamanlılık belirteçleri](https://docs.microsoft.com/ef/core/modeling/concurrency)
* [EF çekirdek eşzamanlılık işleme](https://docs.microsoft.com/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[Önceki](xref:data/ef-rp/update-related-data)
