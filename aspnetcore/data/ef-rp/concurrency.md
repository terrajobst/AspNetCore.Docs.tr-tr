---
title: ASP.NET core'da - eşzamanlılık - 8 8 EF çekirdekli Razor sayfaları
author: tdykstra
description: Bu öğreticide, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/concurrency
ms.openlocfilehash: c9cbf8fd3ed85f32b3c166bf2df702fd26df4fc3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080983"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a>ASP.NET core'da - eşzamanlılık - 8 8 EF çekirdekli Razor sayfaları

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), ve [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

Bu öğreticide, birden çok kullanıcı aynı anda (aynı anda) bir varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.

## <a name="concurrency-conflicts"></a>Eşzamanlılık çakışmaları

Bir eşzamanlılık çakışması ortaya olduğunda:

* Bir kullanıcı bir varlığın düzenleme sayfasına götürür.
* İlk kullanıcının değişikliği veritabanına yazılmadan önce, başka bir kullanıcı aynı varlığı güncelleştirir.

Eşzamanlılık algılama etkinleştirilmemişse, veritabanını güncelleştirme son olarak diğer kullanıcının değişikliklerinin üzerine yazılır. Bu risk kabul edilebilir ise, eşzamanlılık için programlama maliyeti avantaja aykırı bir ücret verebilir.

### <a name="pessimistic-concurrency-locking"></a>Kötümser eşzamanlılık (kilitleme)

Eşzamanlılık çakışmalarını önlemenin bir yolu veritabanı kilitlerini kullanmaktır. Bu, Kötümser eşzamanlılık olarak adlandırılır. Uygulama, güncelleştirmeyi amaçladığı bir veritabanı satırını okumadan önce bir kilit ister. Bir satır, güncelleştirme erişimi için kilitlendiğinde, ilk kilit yayımlanıncaya kadar başka hiçbir kullanıcının satırı kilitlemesine izin verilmez.

Kilitleri yönetmek dezavantajlara sahiptir. Program, karmaşık olabilir ve Kullanıcı sayısı arttıkça performans sorunlarına neden olabilir. Entity Framework Core, BT için yerleşik destek sağlamaz ve bu öğretici Bu öğreticinin nasıl uygulanacağını göstermez.

### <a name="optimistic-concurrency"></a>İyimser eşzamanlılık

İyimser eşzamanlılık eşzamanlılık çakışmalarını olmasını sağlar ve tepki verdiğini uygun olduğunda bunlar yapın. Örneğin, Jane departmanı Düzen sayfasını ziyaret ve İngilizce departmanı için bütçe 350,000.00 $0,00 değiştirir.

![Bütçe 0 olarak değiştirme](concurrency/_static/change-budget30.png)

Jane tıkladığında önce **Kaydet**, John aynı sayfayı ziyaret eder ve alanın başlangıç tarihi 1/9/2013 1/9/2007'deki değiştirir.

![2013'e başlangıç tarihini değiştirme](concurrency/_static/change-date30.png)

Gamze önce **Kaydet** ' i tıklatır ve değişiklik etkin duruma geçer çünkü tarayıcı Dizin sayfasını bütçe miktarı olarak sıfır ile görüntülüyor.

John tıkladığında **Kaydet** yine de bir bütçe $350,000.00 birini gösteren bir Düzenle sayfasında. Sonraki durum eşzamanlılık çakışmalarını nasıl işleydiğinize göre belirlenir:

* Bir kullanıcının hangi özelliği değiştirdiği ve yalnızca ilgili sütunları veritabanında güncelleştirdiğinden haberdar olabilirsiniz.

  Bu senaryoda, veri kaybolacak. Farklı özellikler iki kullanıcı tarafından güncelleştirildi. Biri İngilizce, departman gözatar sonraki açışınızda Gamze'nin hem Can'ın değişiklikleri görürler. Bu güncelleştirme yöntemini, veri kaybına neden olabilecek çakışmaları sayısını azaltabilirsiniz. Bu yaklaşım bazı dezavantajlara sahiptir:
 
  * Aynı özelliğe rakip bir değişiklik yaptıysanız, veri kaybını önlemek olamaz.
  * Genellikle bir web uygulaması pratik değildir. Tüm getirilen ve yeni değerleri izlemek için önemli durum koruma gerektiriyor. Büyük miktarlarda durumu bakımını yapma, uygulama performansını etkileyebilir.
  * Bir varlıkta eşzamanlılık algılama ile karşılaştırıldığında app karmaşıklığı artırabilirsiniz.

* Gamze'nin değişikliğinin üzerine Can'ın değişiklik sağlayabilirsiniz.

  Sonraki biri İngilizce departmanı gözatar, 1/9/2013 görürler ve getirilen $350,000.00 değeri. Bu yaklaşım olarak adlandırılan bir *istemci WINS* veya *WINS'te son* senaryo. (Tüm istemci değerlerinden veri deposunda nedir üzerinde önceliklidir.) Eşzamanlılık işleme için kodlama yapmazsanız, istemci WINS otomatik olarak gerçekleşir.

* John 'un değişikliğini veritabanında güncelleştirilmesini engelleyebilirsiniz. Genellikle, bir uygulamayı atadığınız:

  * Bir hata iletisi görüntüler.
  * Verilerin geçerli durumunu gösterir.
  * Değişiklikleri uygulamak verin.

  Bu adlı bir *Store WINS* senaryo. (Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.) Bu öğreticide Store WINS senaryo uygulamanız. Bu yöntem, hiçbir değişiklik uyarı bir kullanıcı kılınmamasını sağlar.

## <a name="conflict-detection-in-ef-core"></a>EF Core çakışma algılama

EF Core, `DbConcurrencyException` çakışmalar algıladığında özel durum oluşturur. Çakışma algılamayı etkinleştirmek için veri modeli yapılandırılmalıdır. Çakışma algılamayı etkinleştirme seçenekleri şunlardır:

* EF Core, Update ve DELETE komutlarının WHERE yan tümcesinde [eşzamanlılık belirteçleri](/ef/core/modeling/concurrency) olarak yapılandırılan sütunların özgün değerlerini içerecek şekilde yapılandırın.

  Çağrıldığında, WHERE yan tümcesi ConcurrencyCheck özniteliğiyle açıklama eklenmiş herhangi bir özelliğin özgün değerlerini arar. [](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute) `SaveChanges` Update deyimleri, satır ilk okunduğundan bu yana eşzamanlılık belirteci özelliklerinden herhangi biri değiştiyse güncelleştirilecek bir satır bulmayacaktır. EF Core eşzamanlılık çakışması olarak yorumlar. Birçok sütunu olan veritabanı tablolarında bu yaklaşım çok büyük WHERE yan tümceleriyle sonuçlanabilir ve büyük miktarlarda durum gerektirebilir. Bu nedenle bu yaklaşım genellikle önerilmez ve bu öğreticide kullanılan yöntem değildir.

* Veritabanı tablosunda, bir satırın ne zaman değiştirildiğini belirlemede kullanılabilecek bir izleme sütunu ekleyin.

  SQL Server veritabanında, izleme sütununun veri türü olur `rowversion`. `rowversion` Değer, satır her güncelleştirildiği zaman artılan sıralı bir sayıdır. Bir Update veya delete komutunda WHERE yan tümcesi, izleme sütununun (orijinal satır sürüm numarası) orijinal değerini içerir. Güncelleştirilmekte olan satır başka bir kullanıcı tarafından değiştirilmişse `rowversion` sütunundaki değer özgün değerden farklı olur. Bu durumda, Update veya DELETE deyimi WHERE yan tümcesi nedeniyle güncelleştirilecek satırı bulamıyor. Bir Update veya delete komutundan hiçbir satır etkilenmeden EF Core eşzamanlılık özel durumu oluşturur.

## <a name="add-a-tracking-property"></a>İzleme özelliği Ekle

İçinde *Models/Department.cs*, RowVersion adlı izleme özelliği ekleyin:

[!code-csharp[](intro/samples/cu30/Models/Department.cs?highlight=26,27)]

[Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) özniteliği sütunu eşzamanlılık izleme sütunu olarak tanımlar. Fluent API, izleme özelliğini belirtmenin alternatif bir yoludur:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Bir SQL Server veritabanı için, `[Timestamp]` bir varlık özelliği üzerindeki özniteliği byte dizisi olarak tanımlanır:

* Sütunun WHERE yan tümceleri DELETE ve UPDATE 'e dahil edilmesini sağlar.
* Veritabanındaki sütun türünü [rowversion](/sql/t-sql/data-types/rowversion-transact-sql)olarak ayarlar.

Veritabanı, satır her güncelleştirildiği zaman artılan sıralı bir satır sürüm numarası oluşturur. `Update` Veya komutunda`Delete` , yan`Where` tümce getirilen satır sürümü değerini içerir. Güncelleştirilmekte olan satır getirilmesinden sonra değiştiyse:

* Geçerli satır sürümü değeri getirilen değerle eşleşmiyor.
* Yan tümce `Delete` getirilen satır sürümü değerini aradığı için veyakomutlarıbirsatırbulmayın.`Update` `Where`
* A `DbUpdateConcurrencyException` oluşturulur.

Aşağıdaki kod, T-SQL bölüm adını güncelleştirildiğinde EF Core tarafından oluşturulan bir bölümü gösterilmektedir:

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=2-3)]

Önceki kodun gösterdiği vurgulanmış `WHERE` yan tümcesi içeren `RowVersion`. Veritabanı `RowVersion` `RowVersion` ()`@p2`parametresine eşit değilse, hiçbir satır güncellenmez.

Aşağıdaki vurgulanmış kodu tam olarak bir satır güncellenmedi doğrular T-SQL gösterir:

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=4-6)]

[@@ROWCOUNT ](/sql/t-sql/functions/rowcount-transact-sql) son deyiminden etkilenen satır sayısını döndürür. Hiçbir satır güncellenmemişse EF Core bir `DbUpdateConcurrencyException`oluşturur.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Bir SQLite veritabanı için, `[Timestamp]` bir varlık özelliği üzerindeki özniteliği byte dizisi olarak tanımlanır:

* Sütunun WHERE yan tümceleri DELETE ve UPDATE 'e dahil edilmesini sağlar.
* BLOB sütun türüyle eşlenir.

Veritabanı Tetikleyicileri satır her güncelleştirildiğinde, RowVersion sütununu yeni bir rastgele bayt dizisiyle güncelleştirir. `Update` Or komutunda`Delete` , yan`Where` tümce rowversion sütununun getirilen değerini içerir. Güncelleştirilmekte olan satır getirilmesinden sonra değiştiyse:

* Geçerli satır sürümü değeri getirilen değerle eşleşmiyor.
* Or komutu bir satır`Where` bulmadığından, yan tümce orijinal satır sürümü değerini arar. `Delete` `Update`
* A `DbUpdateConcurrencyException` oluşturulur.

---

### <a name="update-the-database"></a>Veritabanını güncelleştirme

`RowVersion` Özelliği eklendiğinde geçiş gerektiren veri modeli değişir.

Projeyi oluşturun. 

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* PMC 'de şu komutu çalıştırın:

  ```powershell
  Add-Migration RowVersion
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Bir terminalde aşağıdaki komutu çalıştırın:

  ```dotnetcli
  dotnet ef migrations add RowVersion
  ```

---

Bu komut:

* *Geçişler/{zaman damgası} _RowVersion. cs* geçiş dosyasını oluşturur.
* Güncelleştirmeleri *Migrations/SchoolContextModelSnapshot.cs* dosya. Bu güncelleştirme aşağıdaki vurgulanmış kodu ekler `BuildModel` yöntemi:

  [!code-csharp[](intro/samples/cu30/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=15-17)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* PMC 'de şu komutu çalıştırın:

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* `Migrations/<timestamp>_RowVersion.cs` Dosyayı açın ve vurgulanmış kodu ekleyin:

  [!code-csharp[](intro/samples/cu30/MigrationsSQLite/20190722151951_RowVersion.cs?highlight=16-42)]

  Yukarıdaki kod:

  * Mevcut satırları rastgele blob değerleriyle güncelleştirir.
  * Satır her güncelleştirildiğinde ROWVERSION sütununu rastgele bir blob değerine ayarlamış veritabanı Tetikleyicileri ekler.

* Bir terminalde aşağıdaki komutu çalıştırın:

  ```dotnetcli
  dotnet ef database update
  ```

---

<a name="scaffold"></a>

## <a name="scaffold-department-pages"></a>Yapı iskelesi departmanı sayfaları

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Aşağıdaki özel durumlarla birlikte [Yapı Fkatlama öğrenci sayfalarındaki](xref:data/ef-rp/intro#scaffold-student-pages) yönergeleri izleyin:

* *Sayfalar/departmanlar* klasörü oluşturun.  
* Model `Department` sınıfı için kullanın.
  * Yeni bir tane oluşturmak yerine var olan bağlam sınıfını kullanın.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* *Sayfalar/departmanlar* klasörü oluşturun.

* Bölüm sayfalarını iskele almak için aşağıdaki komutu çalıştırın.

  **Windows üzerinde:**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

  **Linux veya macOS 'ta:**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages/Departments --referenceScriptLibraries
  ```

---

Projeyi oluşturun.

## <a name="update-the-index-page"></a>Dizin sayfasını Güncelleştir

Scafkatlama aracı Dizin sayfası `RowVersion` için bir sütun oluşturdu, ancak bu alan bir üretim uygulamasında gösterilmemelidir. Bu öğreticide, eşzamanlılık işlemenin nasıl çalıştığını göstermeye `RowVersion` yardımcı olmak için öğesinin son baytı görüntülenir. Son baytın kendi kendine benzersiz olması garanti edilmez.

*Pages\Departments\Index.cshtml* sayfasını güncelleştir:

* Dizin bölümlerine değiştirin.
* Yalnızca bayt dizisinin son `RowVersion` baytını göstermek için içeren kodu değiştirin.
* FirstMidName tam adı ile değiştirin.

Aşağıdaki kod, güncelleştirilmiş sayfayı gösterir:

[!code-html[](intro/samples/cu30/Pages/Departments/Index.cshtml?highlight=5,8,29,48,51)]

## <a name="update-the-edit-page-model"></a>Düzenleme sayfa modeli güncelleştirme

Aşağıdaki kodla *Pages\Departments\Edit.cshtml.cs* güncelleştirin:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_All)]

[OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) , `OnGet` yöntemde alındığı sırada varlıktaki `rowVersion` değerle güncelleştirilir. EF Core özgün içeren bir WHERE yan tümcesi ile SQL güncelleştirme komut oluşturur `RowVersion` değeri. Hiçbir satır güncelleştirme komutu tarafından etkileniyorsanız (hiçbir satır özgün sahip `RowVersion` değer), `DbUpdateConcurrencyException` özel durumu oluşturulur.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_RowVersion&highlight=17-18)]

Vurgulanan kodda:

* İçindeki `Department.RowVersion` değeri, ilk olarak düzenleme sayfasına yönelik GET isteğine getirilen varlıkta olduğu şeydir. Değer, düzenlenecek varlığı görüntüleyen Razor `OnPost` sayfasındaki gizli bir alana göre yöntemine sağlanır. Gizli alan değeri model cildi tarafından öğesine `Department.RowVersion` kopyalanır.
* `OriginalValue`WHERE yan tümcesinde EF Core kullanılacak şeydir. Vurgulanan kod satırından önce, `OriginalValue` Bu yöntemde çağrıldığında `FirstOrDefaultAsync` veritabanında bulunan değeri, düzenleme sayfasında görüntülendiklerden farklı olabilir.
* Vurgulanan kod, EF Core SQL Update ifadesinin WHERE yan tümcesinde `RowVersion` görüntülenen `Department` varlıktaki özgün değeri kullandığından emin olur.

Bir eşzamanlılık hatası oluştuğunda, aşağıdaki vurgulanmış kod istemci değerlerini (Bu yönteme gönderilen değerler) ve veritabanı değerlerini alır.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=14,23)]

Aşağıdaki kod, ' a gönderilen `OnPostAsync`değerden farklı veritabanı değerleri olan her bir sütun için özel bir hata iletisi ekler:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_Error)]

Aşağıdaki vurgulanan kod, `RowVersion` değeri veritabanından alınan yeni değer olarak ayarlar. Kullanıcının bir sonraki tıklayışında **Kaydet**, düzenleme sayfası son görüntülenmesini yakalandı beri gerçekleşen eşzamanlılık hataları.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=28)]

`ModelState.Remove` Deyimi, çünkü gereklidir `ModelState` eski olan `RowVersion` değeri. Razor Sayfası'nda `ModelState` değerinin ikisi de mevcut olduğunda alan üzerinde model özellik değerlerini öncelik kazanır.

### <a name="update-the-razor-page"></a>Razor sayfasını güncelleştirme

*Sayfaları/departmanları/Düzenle. cshtml* 'yi aşağıdaki kodla güncelleştirin:

[!code-html[](intro/samples/cu30/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Yukarıdaki kod:

* Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.
* Gizli satır sürümü ekler. `RowVersion` POST geri değere bağlar, böylece eklenmesi gerekir.
* Son baytı görüntüler `RowVersion` hata ayıklama amacıyla.
* Değiştirir `ViewData` türü kesin belirlenmiş ile `InstructorNameSL`.

### <a name="test-concurrency-conflicts-with-the-edit-page"></a>Eşzamanlılık çakışmalarını düzenleme sayfası ile test

İki tarayıcılar örneklerinde düzenleme İngilizce departmanı açın:

* Uygulamayı çalıştırın ve bölümleri seçin.
* Sağ **Düzenle** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**.
* Birinci sekmede tıklayın **Düzenle** İngilizce departmanı için köprü.

Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.

' A tıklayın ve ilk tarayıcı sekmesine adını değiştirmek **Kaydet**.

![Departman düzenleme değişikliğinden sonra sayfa 1](concurrency/_static/edit-after-change-130.png)

Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir. Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.

İkinci bir tarayıcı sekmesinde farklı bir alana değiştirin.

![Departman düzenleme değişikliğinden sonra sayfa 2](concurrency/_static/edit-after-change-230.png)

**Kaydet**'e tıklayın. Veritabanı değerleriyle eşleşmeyen tüm alanlar için hata iletileri görürsünüz:

![Departman düzenleme sayfa hata iletisi](concurrency/_static/edit-error30.png)

Ad alanı değiştirmek bu tarayıcı penceresini düşünmediğiniz. Kopyalama ve geçerli değerin (dil) adı alanına yapıştırın. Çıkış sekmesi. İstemci tarafı doğrulama hata iletisi kaldırır.

Tıklayın **Kaydet** yeniden. İkinci tarayıcı sekmesinde girdiğiniz değer kaydedilir. Dizin sayfasında kaydedilen değerler görürsünüz.

## <a name="update-the-delete-page"></a>Silme sayfası

*Sayfaları/departmanları/delete. cshtml. cs* öğesini aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Delete.cshtml.cs)]

Varlık getirildi sonra değiştiğinde silme sayfası eşzamanlılık çakışmalarını algılar. `Department.RowVersion` Varlık getirildi satır sürümü andır. EF Core SQL DELETE komutu oluşturduğunda, WHERE yan tümcesi ile içerdiği `RowVersion`. Sıfır satır SQL DELETE komutu sonuçları etkileniyorsa:

* SQL `RowVersion` Delete komutunda bulunan, veritabanında eşleşmiyor. `RowVersion`
* DbUpdateConcurrencyException özel durum oluşturulur.
* `OnGetAsync` çağrılır `concurrencyError`.

### <a name="update-the-delete-razor-page"></a>Razor Sil sayfasını Güncelleştir

Güncelleştirme *Pages/Departments/Delete.cshtml* aşağıdaki kod ile:

[!code-html[](intro/samples/cu30/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

Yukarıdaki kod aşağıdaki değişiklikleri yapar:

* Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.
* Bir hata iletisi ekler.
* FullName FirstMidName değiştirir **yönetici** alan.
* Değişiklikleri `RowVersion` son bayt görüntülenecek.
* Gizli satır sürümü ekler. `RowVersion`, postgit ekleme geri eklemesi değeri bağlamalıdır.

### <a name="test-concurrency-conflicts"></a>Eşzamanlılık çakışmalarını test et

Test bölümü oluşturun.

İki tarayıcılar örneklerinde DELETE test departmanı açın:

* Uygulamayı çalıştırın ve bölümleri seçin.
* Sağ **Sil** seçin ve test departmanı için köprü **yeni sekmede aç**.
* Tıklayın **Düzenle** köprü test bölümü için.

Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.

İlk tarayıcı sekmesine bütçede değiştirip'ı **Kaydet**.

Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir. Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.

Test bölümü ikinci sekmesinden silin. Veritabanında geçerli değerlerle birlikte bir eşzamanlılık hatası görüntülenir. Tıklayarak **Sil** sürece varlığını siler `RowVersion` updated.department silinmiş olmuştur.

## <a name="additional-resources"></a>Ek kaynaklar

* [EF Core eşzamanlılık belirteçleri](/ef/core/modeling/concurrency)
* [EF Core eşzamanlılık işleme](/ef/core/saving/concurrency)
* [2. x kaynağı ASP.NET Core hata ayıklaması](https://github.com/aspnet/AspNetCore.Docs/issues/4155)

## <a name="next-steps"></a>Sonraki adımlar

Bu, serideki son öğreticidir. [Bu öğretici serisinin MVC sürümünde](xref:data/ef-mvc/index)ek konular ele alınmıştır.

> [!div class="step-by-step"]
> [Önceki öğretici](xref:data/ef-rp/update-related-data)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu öğreticide, birden çok kullanıcı aynı anda (aynı anda) bir varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir. Olamaz çözmenize, sorunlarla karşılaşırsanız, [indirin veya tamamlanmış uygulamayı görüntüleyin.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yükleme yönergeleri](xref:index#how-to-download-a-sample).

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

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=2-3)]

Önceki kodun gösterdiği vurgulanmış `WHERE` yan tümcesi içeren `RowVersion`. Varsa DB `RowVersion` eşit değildir `RowVersion` parametre (`@p2`), satır güncelleştirilir.

Aşağıdaki vurgulanmış kodu tam olarak bir satır güncellenmedi doğrular T-SQL gösterir:

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=4-6)]

[@@ROWCOUNT ](/sql/t-sql/functions/rowcount-transact-sql) son deyiminden etkilenen satır sayısını döndürür. Hayır, satır güncelleştirilir, EF Core oluşturur bir `DbUpdateConcurrencyException`.

T-SQL EF Core oluşturur Visual Studio çıktı penceresinde görebilirsiniz.

### <a name="update-the-db"></a>DB update

Ekleme `RowVersion` geçiş gerektiren DB modeli özelliğini değiştirir.

Projeyi oluşturun. Bir komut penceresinde aşağıdakileri girin:

```dotnetcli
dotnet ef migrations add RowVersion
dotnet ef database update
```

Yukarıdaki komutlar:

* Ekler *geçişleri / {zaman stamp}_RowVersion.cs* geçiş dosyası.
* Güncelleştirmeleri *Migrations/SchoolContextModelSnapshot.cs* dosya. Bu güncelleştirme aşağıdaki vurgulanmış kodu ekler `BuildModel` yöntemi:

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=14-16)]

* DB güncelleştirilemedi geçişlerini çalıştırır.

<a name="scaffold"></a>

## <a name="scaffold-the-departments-model"></a>İskele Departmanlar modeli

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Bölümündeki yönergeleri [Öğrenci modeli iskelesini](xref:data/ef-rp/intro#scaffold-student-pages) ve `Department` model sınıfı için.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

 Şu komutu çalıştırın:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

---

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

Aşağıdaki kodla *Pages\Departments\Edit.cshtml.cs* güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Bir eşzamanlılık algılanması [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) güncelleştirilmesi `rowVersion` alınan varlık değeri. EF Core özgün içeren bir WHERE yan tümcesi ile SQL güncelleştirme komut oluşturur `RowVersion` değeri. Hiçbir satır güncelleştirme komutu tarafından etkileniyorsanız (hiçbir satır özgün sahip `RowVersion` değer), `DbUpdateConcurrencyException` özel durumu oluşturulur.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

Önceki kodda, `Department.RowVersion` varlık getirildi değeri olur. `OriginalValue` DB değer olduğunda `FirstOrDefaultAsync` bu yöntemi çağrıldı.

Aşağıdaki kod, istemci değerler (Bu yönteme gönderilen değerler) ve DB değerleri alır:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

Aşağıdaki kod, ' den postalandıklarından farklı olarak `OnPostAsync`DB değerleri olan her bir sütun için özel bir hata iletisi ekler:

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

Yukarıdaki kod aşağıdaki değişiklikleri yapar:

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
* [Bu öğreticinin YouTube sürümü (eşzamanlılık çakışmalarını Işleme)](https://youtu.be/EosxHTFgYps)
* [Bu öğreticinin YouTube sürümü (Bölüm 2)](https://www.youtube.com/watch?v=kcxERLnaGO0)
* [Bu öğreticinin YouTube sürümü (Bölüm 3)](https://www.youtube.com/watch?v=d4RbpfvELRs)

> [!div class="step-by-step"]
> [Önceki](xref:data/ef-rp/update-related-data)

::: moniker-end

