---
title: ASP.NET core'da - eşzamanlılık - 8 8 EF çekirdekli Razor sayfaları
author: rick-anderson
description: Bu öğreticide, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2018
uid: data/ef-rp/concurrency
ms.openlocfilehash: da854bafdb9d3741358fd69c2f862c3cdf1ee406
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121654"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a>ASP.NET core'da - eşzamanlılık - 8 8 EF çekirdekli Razor sayfaları

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), ve [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

Bu öğreticide, birden çok kullanıcı aynı anda (aynı anda) bir varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir. Olamaz çözmenize, sorunlarla karşılaşırsanız, [indirin veya tamamlanmış uygulamayı görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yükleme yönergeleri](xref:index#how-to-download-a-sample).

## <a name="concurrency-conflicts"></a>Eşzamanlılık çakışmaları

Bir eşzamanlılık çakışması ortaya olduğunda:

* Bir kullanıcı bir varlığın düzenleme sayfasına götürür.
* İlk kullanıcının değişiklik DB'ye yazılmadan önce başka bir kullanıcı aynı varlık güncelleştirir.

Eşzamanlılık algılama etkin değil, eş zamanlı güncelleştirmeler olduğunda:

* Son güncelleştirme WINS. Diğer bir deyişle, son güncelleştirme değerleri Veritabanına kaydedilir.
* Geçerli güncelleştirme ilk kaybolur.

### <a name="optimistic-concurrency"></a>İyimser eşzamanlılık

İyimser eşzamanlılık eşzamanlılık çakışmalarını olmasını sağlar ve tepki verdiğini uygun olduğunda bunlar yapın. Örneğin, Jane departmanı Düzen sayfasını ziyaret ve İngilizce departmanı için bütçe 350,000.00 $0,00 değiştirir.

![Bütçe 0 olarak değiştirme](concurrency/_static/change-budget.png)

Jane tıkladığında önce **Kaydet**, John aynı sayfayı ziyaret eder ve alanın başlangıç tarihi 1/9/2013 1/9/2007'deki değiştirir.

![2013'e başlangıç tarihini değiştirme](concurrency/_static/change-date.png)

Jane tıkladığında **Kaydet** ilk ve her tarayıcı dizin sayfası görüntülendiğinde değiştirme görür.

![Bütçe sıfır olarak değiştirildi](concurrency/_static/budget-zero.png)

John tıkladığında **Kaydet** yine de bir bütçe $350,000.00 birini gösteren bir Düzenle sayfasında. Sonraki işlemin ne eşzamanlılık çakışmalarını nasıl ele tarafından belirlenir.

İyimser eşzamanlılık aşağıdaki seçenekleri içerir:

* Bir kullanıcı değiştirmiş hangi özelliğinin kaydını ve yalnızca ilgili sütunları DB'de güncelleştirin.

  Bu senaryoda, veri kaybolacak. Farklı özellikler iki kullanıcı tarafından güncelleştirildi. Biri İngilizce, departman gözatar sonraki açışınızda Gamze'nin hem Can'ın değişiklikleri görürler. Bu güncelleştirme yöntemini, veri kaybına neden olabilecek çakışmaları sayısını azaltabilirsiniz. Bu yaklaşım:
 
  * Aynı özelliğe rakip bir değişiklik yaptıysanız, veri kaybını önlemek olamaz.
  * Genellikle bir web uygulaması pratik değildir. Tüm getirilen ve yeni değerleri izlemek için önemli durum koruma gerektiriyor. Büyük miktarlarda durumu bakımını yapma, uygulama performansını etkileyebilir.
  * Bir varlıkta eşzamanlılık algılama ile karşılaştırıldığında app karmaşıklığı artırabilirsiniz.

* Gamze'nin değişikliğinin üzerine Can'ın değişiklik sağlayabilirsiniz.

  Sonraki biri İngilizce departmanı gözatar, 1/9/2013 görürler ve getirilen $350,000.00 değeri. Bu yaklaşım olarak adlandırılan bir *istemci WINS* veya *WINS'te son* senaryo. (Tüm istemci değerlerinden veri deposunda nedir üzerinde önceliklidir.) Eşzamanlılık işleme için kodlama yapmazsanız, istemci WINS otomatik olarak gerçekleşir.

* Can'ın değişiklik DB'de güncelleştirilmesini engelleyebilir. Genellikle, bir uygulamayı atadığınız:

  * Bir hata iletisi görüntüler.
  * Verilerin geçerli durumunu gösterir.
  * Değişiklikleri uygulamak verin.

  Bu adlı bir *Store WINS* senaryo. (Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.) Bu öğreticide Store WINS senaryo uygulamanız. Bu yöntem, hiçbir değişiklik uyarı bir kullanıcı kılınmamasını sağlar.

## <a name="handling-concurrency"></a>Eşzamanlılığı işleme 

Ne zaman bir özellik olarak yapılandırıldığında bir [eşzamanlılık belirteci](/ef/core/modeling/concurrency):

* EF Core getirildi sonra'nın özellik değiştirilmedi doğrular. Onay gerçekleşir, [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) veya [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) çağrılır.
* Özelliği, getirildikten sonra değiştirilmişse, bir [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) oluşturulur. 

DB ve veri modeli oluşturma destekleyecek şekilde yapılandırılması gerekir `DbUpdateConcurrencyException`.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Bir özellik eşzamanlılık çakışmaları algılama

Eşzamanlılık çakışmaları ile özellik düzeyinde algılanamıyor [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) özniteliği. Öznitelik, birden çok modelin özellikleri için uygulanabilir. Daha fazla bilgi için [veri ek açıklamaları-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).

`[ConcurrencyCheck]` Özniteliği, bu öğreticide kullanılmaz.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Bir satırda eşzamanlılık çakışmaları algılama

Eşzamanlılık çakışmalarını algılamak için bir [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) sütun izleme modele eklenir.  `rowversion` :

* SQL Server özeldir. Diğer veritabanlarının benzer bir özellik sağlamayabilir.
* Veritabanından getirildikten sonra'nın bir varlık değiştirilmediğinden belirlemek için kullanılır. 

Bir sıralı bir veritabanı oluşturur `rowversion` her zaman satır artan sayısı güncelleştirilir. İçinde bir `Update` veya `Delete` komutu `Where` yan tümcesi içeren getirilen değeri `rowversion`. Güncelleştirilen satır değiştiyse:

 * `rowversion` getirilen değeri ile eşleşmiyor.
 * `Update` Veya `Delete` olduğundan, komut satır Bul yok `Where` yan tümcesi içeren getirilen `rowversion`.
 * A `DbUpdateConcurrencyException` oluşturulur.

EF Core tarafından hiçbir satır güncelleştirildiğinde içinde bir `Update` veya `Delete` komutu, bir eşzamanlılık özel durumu oluşturulur.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Departman varlığa izleme özelliği ekleme

İçinde *Models/Department.cs*, RowVersion adlı izleme özelliği ekleyin:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

[Zaman damgası](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) özniteliği, bu sütunda yer belirtir `Where` yan tümcesi `Update` ve `Delete` komutları. Adlandırılan öznitelik `Timestamp` bir SQL SQL Server'ın önceki sürümlerinde kullanılan çünkü `timestamp` veri türü SQL önce `rowversion` türü değiştirildi.

Fluent API'si izleme özelliği de belirtebilirsiniz:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

Aşağıdaki kod, T-SQL bölüm adını güncelleştirildiğinde EF Core tarafından oluşturulan bir bölümü gösterilmektedir:

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

Önceki kodun gösterdiği vurgulanmış `WHERE` yan tümcesi içeren `RowVersion`. Varsa DB `RowVersion` eşit değildir `RowVersion` parametre (`@p2`), satır güncelleştirilir.

Aşağıdaki vurgulanmış kodu tam olarak bir satır güncellenmedi doğrular T-SQL gösterir:

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT ](/sql/t-sql/functions/rowcount-transact-sql) son deyiminden etkilenen satır sayısını döndürür. Hayır, satır güncelleştirilir, EF Core oluşturur bir `DbUpdateConcurrencyException`.

T-SQL EF Core oluşturur Visual Studio çıktı penceresinde görebilirsiniz.

### <a name="update-the-db"></a>DB update

Ekleme `RowVersion` geçiş gerektiren DB modeli özelliğini değiştirir.

Projeyi oluşturun. Bir komut penceresinde aşağıdakileri girin:

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

Yukarıdaki komutlar:

* Ekler *geçişleri / {zaman stamp}_RowVersion.cs* geçiş dosyası.
* Güncelleştirmeleri *Migrations/SchoolContextModelSnapshot.cs* dosya. Bu güncelleştirme aşağıdaki vurgulanmış kodu ekler `BuildModel` yöntemi:

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* DB güncelleştirilemedi geçişlerini çalıştırır.

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a>İskele Departmanlar modeli

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Bölümündeki yönergeleri [Öğrenci modeli iskelesini](xref:data/ef-rp/intro#scaffold-the-student-model) ve `Department` model sınıfı için.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

 Şu komutu çalıştırın:

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

------

Önceki komut iskelesini kurar `Department` modeli. Projeyi Visual Studio'da açın.

Projeyi oluşturun.

### <a name="update-the-departments-index-page"></a>Departmanlar dizin sayfası

Oluşturulan yapı iskelesi altyapısı bir `RowVersion` sütunu için dizin sayfasını, ancak bu alanı olmamalıdır görüntülenecek. Bu öğreticide, son baytı `RowVersion` eşzamanlılık anlamanıza yardımcı olması için görüntülenir. Son bayta kalan benzersiz olması garanti yoktur. Gerçek bir uygulamada görüntüleyemiyordu `RowVersion` veya son baytı `RowVersion`.

Dizin Sayfası güncelleştirin:

* Dizin bölümlerine değiştirin.
* Biçimlendirme içeren değiştirin `RowVersion` son baytı ile `RowVersion`.
* FirstMidName tam adı ile değiştirin.

Güncelleştirilen sayfaya aşağıdaki işaretlemeyi gösterir:

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Düzenleme sayfa modeli güncelleştirme

Güncelleştirme *pages\departments\edit.cshtml.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Bir eşzamanlılık algılanması [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) güncelleştirilmesi `rowVersion` alınan varlık değeri. EF Core özgün içeren bir WHERE yan tümcesi ile SQL güncelleştirme komut oluşturur `RowVersion` değeri. Hiçbir satır güncelleştirme komutu tarafından etkileniyorsanız (hiçbir satır özgün sahip `RowVersion` değer), `DbUpdateConcurrencyException` özel durumu oluşturulur.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

Önceki kodda, `Department.RowVersion` varlık getirildi değeri olur. `OriginalValue` DB değer olduğunda `FirstOrDefaultAsync` bu yöntemi çağrıldı.

Aşağıdaki kod, istemci değerler (Bu yönteme gönderilen değerler) ve DB değerleri alır:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

DB sahip her bir sütunun ne deftere nakledilen farklı değerler için aşağıdaki kod bir özel hata iletisi ekler `OnPostAsync`:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

Aşağıdaki vurgulanmış kodu kümeleri `RowVersion` değerden yeni değere Veritabanından alınır. Kullanıcının bir sonraki tıklayışında **Kaydet**, düzenleme sayfası son görüntülenmesini yakalandı beri gerçekleşen eşzamanlılık hataları.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

`ModelState.Remove` Deyimi, çünkü gereklidir `ModelState` eski olan `RowVersion` değeri. Razor Sayfası'nda `ModelState` değerinin ikisi de mevcut olduğunda alan üzerinde model özellik değerlerini öncelik kazanır.

## <a name="update-the-edit-page"></a>Güncelleştirme düzenleme sayfası

Güncelleştirme *Pages/Departments/Edit.cshtml* aşağıdaki işaretlemeyle:

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Önceki işaretlemesi:

* Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.
* Gizli satır sürümü ekler. `RowVersion` POST geri değere bağlar, böylece eklenmesi gerekir.
* Son baytı görüntüler `RowVersion` hata ayıklama amacıyla.
* Değiştirir `ViewData` türü kesin belirlenmiş ile `InstructorNameSL`.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Eşzamanlılık çakışmalarını düzenleme sayfası ile test

İki tarayıcılar örneklerinde düzenleme İngilizce departmanı açın:

* Uygulamayı çalıştırın ve bölümleri seçin.
* Sağ **Düzenle** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**.
* Birinci sekmede tıklayın **Düzenle** İngilizce departmanı için köprü.

Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.

' A tıklayın ve ilk tarayıcı sekmesine adını değiştirmek **Kaydet**.

![Departman düzenleme değişikliğinden sonra sayfa 1](concurrency/_static/edit-after-change-1.png)

Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir. Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.

İkinci bir tarayıcı sekmesinde farklı bir alana değiştirin.

![Departman düzenleme değişikliğinden sonra sayfa 2](concurrency/_static/edit-after-change-2.png)

**Kaydet**'e tıklayın. DB değerleri eşleşmeyen tüm alanlar için hata iletileri görürsünüz:

![Departman düzenleme sayfa hata iletisi](concurrency/_static/edit-error.png)

Ad alanı değiştirmek bu tarayıcı penceresini düşünmediğiniz. Kopyalama ve geçerli değerin (dil) adı alanına yapıştırın. Çıkış sekmesi. İstemci tarafı doğrulama hata iletisi kaldırır.

![Departman düzenleme sayfa hata iletisi](concurrency/_static/cv.png)

Tıklayın **Kaydet** yeniden. İkinci tarayıcı sekmesinde girdiğiniz değer kaydedilir. Dizin sayfasında kaydedilen değerler görürsünüz.

## <a name="update-the-delete-page"></a>Silme sayfası

Delete sayfa modeli aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

Varlık getirildi sonra değiştiğinde silme sayfası eşzamanlılık çakışmalarını algılar. `Department.RowVersion` Varlık getirildi satır sürümü andır. EF Core SQL DELETE komutu oluşturduğunda, WHERE yan tümcesi ile içerdiği `RowVersion`. Sıfır satır SQL DELETE komutu sonuçları etkileniyorsa:

* `RowVersion` SQL DELETE komutu eşleşmiyor `RowVersion` DB'de.
* DbUpdateConcurrencyException özel durum oluşturulur.
* `OnGetAsync` çağrılır `concurrencyError`.

### <a name="update-the-delete-page"></a>Silme sayfası

Güncelleştirme *Pages/Departments/Delete.cshtml* aşağıdaki kod ile:

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]


Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:

* Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.
* Bir hata iletisi ekler.
* FullName FirstMidName değiştirir **yönetici** alan.
* Değişiklikleri `RowVersion` son bayt görüntülenecek.
* Gizli satır sürümü ekler. `RowVersion` POST geri değere bağlar, böylece eklenmesi gerekir.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Eşzamanlılık çakışmalarını silme sayfası ile test

Test bölümü oluşturun.

İki tarayıcılar örneklerinde DELETE test departmanı açın:

* Uygulamayı çalıştırın ve bölümleri seçin.
* Sağ **Sil** seçin ve test departmanı için köprü **yeni sekmede aç**.
* Tıklayın **Düzenle** köprü test bölümü için.

Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.

İlk tarayıcı sekmesine bütçede değiştirip'ı **Kaydet**.

Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir. Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.

Test bölümü ikinci sekmesinden silin. Bir eşzamanlılık hatası DB geçerli değerlerle görüntüdür. Tıklayarak **Sil** sürece varlığını siler `RowVersion` updated.department silinmiş olmuştur.

Bkz: [devralma](xref:data/ef-mvc/inheritance) nasıl bir veri modeli devralır.

### <a name="additional-resources"></a>Ek kaynaklar

* [EF Core eşzamanlılık belirteçleri](/ef/core/modeling/concurrency)
* [EF Core eşzamanlılık işleme](/ef/core/saving/concurrency)

> [!div class="step-by-step"]
> [Önceki](xref:data/ef-rp/update-related-data)
