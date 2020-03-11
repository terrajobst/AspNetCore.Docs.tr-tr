---
title: ASP.NET core'da - eşzamanlılık - 8 8 EF çekirdekli Razor sayfaları
author: rick-anderson
description: Bu öğreticide, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/concurrency
ms.openlocfilehash: c4d43f26ba80e7922c3cbd37d9a5f8e1561b11ad
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656914"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a>ASP.NET core'da - eşzamanlılık - 8 8 EF çekirdekli Razor sayfaları

By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)ve [Jon P Smith](https://twitter.com/thereformedprog)

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

Kemal, **Kaydet**' i tıklamadan önce, John aynı sayfayı ziyaret ettiğinde başlangıç tarihi alanını 9/1/2007 ' den 9/1/2013 ' e değiştirir.

![2013'e başlangıç tarihini değiştirme](concurrency/_static/change-date30.png)

Gamze önce **Kaydet** ' i tıklatır ve değişiklik etkin duruma geçer çünkü tarayıcı Dizin sayfasını bütçe miktarı olarak sıfır ile görüntülüyor.

John, hala $350.000,00 bütçesini gösteren bir düzenleme sayfasında **Kaydet** ' i tıklatır. Sonraki durum eşzamanlılık çakışmalarını nasıl işleydiğinize göre belirlenir:

* Bir kullanıcının hangi özelliği değiştirdiği ve yalnızca ilgili sütunları veritabanında güncelleştirdiğinden haberdar olabilirsiniz.

  Bu senaryoda, veri kaybolacak. Farklı özellikler iki kullanıcı tarafından güncelleştirildi. Biri İngilizce, departman gözatar sonraki açışınızda Gamze'nin hem Can'ın değişiklikleri görürler. Bu güncelleştirme yöntemini, veri kaybına neden olabilecek çakışmaları sayısını azaltabilirsiniz. Bu yaklaşım bazı dezavantajlara sahiptir:
 
  * Aynı özelliğe rakip bir değişiklik yaptıysanız, veri kaybını önlemek olamaz.
  * Genellikle bir web uygulaması pratik değildir. Tüm getirilen ve yeni değerleri izlemek için önemli durum koruma gerektiriyor. Büyük miktarlarda durumu bakımını yapma, uygulama performansını etkileyebilir.
  * Bir varlıkta eşzamanlılık algılama ile karşılaştırıldığında app karmaşıklığı artırabilirsiniz.

* Gamze'nin değişikliğinin üzerine Can'ın değişiklik sağlayabilirsiniz.

  Sonraki biri İngilizce departmanı gözatar, 1/9/2013 görürler ve getirilen $350,000.00 değeri. Bu yaklaşım, *Istemci WINS* veya *son WINS* senaryosu olarak adlandırılır. (İstemciden gelen tüm değerler veri deposunda yer alacak şekilde önceliklidir.) Eşzamanlılık işleme için herhangi bir kodlama yapmazsanız, Istemci WINS otomatik olarak gerçekleşir.

* John 'un değişikliğini veritabanında güncelleştirilmesini engelleyebilirsiniz. Genellikle, bir uygulamayı atadığınız:

  * Bir hata iletisi görüntüler.
  * Verilerin geçerli durumunu gösterir.
  * Değişiklikleri uygulamak verin.

  Buna *Mağaza WINS* senaryosu denir. (Veri deposu değerleri, istemci tarafından gönderilen değerlere göre önceliklidir.) Bu öğreticide mağaza WINS senaryosunu uygulamanız gerekir. Bu yöntem, hiçbir değişiklik uyarı bir kullanıcı kılınmamasını sağlar.

## <a name="conflict-detection-in-ef-core"></a>EF Core çakışma algılama

EF Core, çakışmalar algıladığında `DbConcurrencyException` özel durum oluşturur. Çakışma algılamayı etkinleştirmek için veri modeli yapılandırılmalıdır. Çakışma algılamayı etkinleştirme seçenekleri şunlardır:

* EF Core, Update ve DELETE komutlarının WHERE yan tümcesinde [eşzamanlılık belirteçleri](/ef/core/modeling/concurrency) olarak yapılandırılan sütunların özgün değerlerini içerecek şekilde yapılandırın.

  `SaveChanges` çağrıldığında WHERE yan tümcesi [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute) özniteliğiyle açıklama eklenmiş herhangi bir özelliğin özgün değerlerini arar. Update deyimleri, satır ilk okunduğundan bu yana eşzamanlılık belirteci özelliklerinden herhangi biri değiştiyse güncelleştirilecek bir satır bulmayacaktır. EF Core eşzamanlılık çakışması olarak yorumlar. Birçok sütunu olan veritabanı tablolarında bu yaklaşım çok büyük WHERE yan tümceleriyle sonuçlanabilir ve büyük miktarlarda durum gerektirebilir. Bu nedenle bu yaklaşım genellikle önerilmez ve bu öğreticide kullanılan yöntem değildir.

* Veritabanı tablosunda, bir satırın ne zaman değiştirildiğini belirlemede kullanılabilecek bir izleme sütunu ekleyin.

  SQL Server veritabanında, izleme sütununun veri türü `rowversion`. `rowversion` değeri, satır her güncelleştirildiği zaman artılan sıralı bir sayıdır. Bir Update veya delete komutunda WHERE yan tümcesi, izleme sütununun (orijinal satır sürüm numarası) orijinal değerini içerir. Güncelleştirilmekte olan satır başka bir kullanıcı tarafından değiştirilmişse, `rowversion` sütunundaki değer özgün değerden farklıdır. Bu durumda, Update veya DELETE deyimi WHERE yan tümcesi nedeniyle güncelleştirilecek satırı bulamıyor. Bir Update veya delete komutundan hiçbir satır etkilenmeden EF Core eşzamanlılık özel durumu oluşturur.

## <a name="add-a-tracking-property"></a>İzleme özelliği Ekle

*Modeller/departman. cs*' de, rowversion adlı bir izleme özelliği ekleyin:

[!code-csharp[](intro/samples/cu30/Models/Department.cs?highlight=26,27)]

[Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) özniteliği sütunu eşzamanlılık izleme sütunu olarak tanımlar. Fluent API, izleme özelliğini belirtmenin alternatif bir yoludur:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Bir SQL Server veritabanı için, bayt dizisi olarak tanımlanan bir varlık özelliğindeki `[Timestamp]` özniteliği:

* Sütunun WHERE yan tümceleri DELETE ve UPDATE 'e dahil edilmesini sağlar.
* Veritabanındaki sütun türünü [rowversion](/sql/t-sql/data-types/rowversion-transact-sql)olarak ayarlar.

Veritabanı, satır her güncelleştirildiği zaman artılan sıralı bir satır sürüm numarası oluşturur. Bir `Update` veya `Delete` komutunda, `Where` yan tümcesi getirilen satır sürümü değerini içerir. Güncelleştirilmekte olan satır getirilmesinden sonra değiştiyse:

* Geçerli satır sürümü değeri getirilen değerle eşleşmiyor.
* `Where` yan tümcesi getirilen satır sürümü değerini aradığı için `Update` veya `Delete` komutları bir satır bulamaz.
* Bir `DbUpdateConcurrencyException` oluşturulur.

Aşağıdaki kod, T-SQL bölüm adını güncelleştirildiğinde EF Core tarafından oluşturulan bir bölümü gösterilmektedir:

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=2-3)]

Önceki vurgulanan kod, `RowVersion`içeren `WHERE` yan tümcesini gösterir. Veritabanı `RowVersion` `RowVersion` parametresine eşit değilse (`@p2`), hiçbir satır güncellenmez.

Aşağıdaki vurgulanmış kodu tam olarak bir satır güncellenmedi doğrular T-SQL gösterir:

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=4-6)]

[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) , son deyimden etkilenen satır sayısını döndürür. Hiçbir satır güncellenmemişse, EF Core bir `DbUpdateConcurrencyException`oluşturur.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Bir SQLite veritabanı için, bir varlık özelliğindeki `[Timestamp]` özniteliği byte dizisi olarak tanımlanır:

* Sütunun WHERE yan tümceleri DELETE ve UPDATE 'e dahil edilmesini sağlar.
* BLOB sütun türüyle eşlenir.

Veritabanı Tetikleyicileri satır her güncelleştirildiğinde, RowVersion sütununu yeni bir rastgele bayt dizisiyle güncelleştirir. Bir `Update` veya `Delete` komutunda, `Where` yan tümcesi RowVersion sütununun getirilen değerini içerir. Güncelleştirilmekte olan satır getirilmesinden sonra değiştiyse:

* Geçerli satır sürümü değeri getirilen değerle eşleşmiyor.
* `Where` yan tümcesi orijinal satır sürümü değerini aradığı için `Update` veya `Delete` komutu bir satır bulamaz.
* Bir `DbUpdateConcurrencyException` oluşturulur.

---

### <a name="update-the-database"></a>Veritabanını güncelleştirme

`RowVersion` özelliği eklendiğinde geçiş gerektiren veri modeli değişir.

Projeyi oluşturun. 

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* PMC 'de şu komutu çalıştırın:

  ```powershell
  Add-Migration RowVersion
  ```

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Bir terminalde aşağıdaki komutu çalıştırın:

  ```dotnetcli
  dotnet ef migrations add RowVersion
  ```

---

Bu komut:

* *Geçişleri/{zaman damgası} _RowVersion. cs* geçiş dosyasını oluşturur.
* *Geçişler/SchoolContextModelSnapshot. cs* dosyasını güncelleştirir. Güncelleştirme, `BuildModel` yöntemine aşağıdaki vurgulanmış kodu ekler:

  [!code-csharp[](intro/samples/cu30/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=15-17)]

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* PMC 'de şu komutu çalıştırın:

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* `Migrations/<timestamp>_RowVersion.cs` dosyasını açın ve vurgulanan kodu ekleyin:

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

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Aşağıdaki özel durumlarla birlikte [Yapı Fkatlama öğrenci sayfalarındaki](xref:data/ef-rp/intro#scaffold-student-pages) yönergeleri izleyin:

* *Sayfalar/departmanlar* klasörü oluşturun.  
* Model sınıfı için `Department` kullanın.
  * Yeni bir tane oluşturmak yerine var olan bağlam sınıfını kullanın.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* *Sayfalar/departmanlar* klasörü oluşturun.

* Bölüm sayfalarını iskele almak için aşağıdaki komutu çalıştırın.

  **Windows'da:**

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

Scafkatlama aracı Dizin sayfası için bir `RowVersion` sütunu oluşturdu, ancak bu alan bir üretim uygulamasında gösterilmemelidir. Bu öğreticide, eşzamanlılık işlemenin nasıl çalıştığını göstermeye yardımcı olmak için `RowVersion` son baytı görüntülenir. Son baytın kendi kendine benzersiz olması garanti edilmez.

*Pages\Departments\Index.cshtml* sayfasını güncelleştir:

* Dizin bölümlerine değiştirin.
* Yalnızca bayt dizisinin son baytını göstermek için `RowVersion` içeren kodu değiştirin.
* FirstMidName tam adı ile değiştirin.

Aşağıdaki kod, güncelleştirilmiş sayfayı gösterir:

[!code-html[](intro/samples/cu30/Pages/Departments/Index.cshtml?highlight=5,8,29,48,51)]

## <a name="update-the-edit-page-model"></a>Düzenleme sayfa modeli güncelleştirme

Aşağıdaki kodla *Pages\Departments\Edit.cshtml.cs* güncelleştirin:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_All)]

[OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) , `OnGet` yönteminde alındığı sırada varlıktaki `rowVersion` değeriyle güncelleştirilir. EF Core, özgün `RowVersion` değerini içeren WHERE yan tümcesiyle bir SQL UPDATE komutu oluşturur. GÜNCELLEŞTIRME komutundan hiçbir satır etkilenmeden (hiçbir satır özgün `RowVersion` değerine sahip değilse), bir `DbUpdateConcurrencyException` özel durumu oluşturulur.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_RowVersion&highlight=17-18)]

Vurgulanan kodda:

* `Department.RowVersion` değeri, ilk olarak düzenleme sayfasına yönelik GET isteğine getirilen varlıkta yer alır. Değer, düzenlenecek varlığı görüntüleyen Razor sayfasındaki gizli bir alan tarafından `OnPost` yöntemine sağlanır. Gizli alan değeri model cildi tarafından `Department.RowVersion` ' a kopyalanır.
* `OriginalValue` WHERE yan tümcesinde kullanılacak EF Core. Vurgulanan kod satırı çalıştırılmadan önce, `FirstOrDefaultAsync` Bu yöntemde çağrıldığında, düzenleme sayfasında görüntülendiklerden farklı olabilen `OriginalValue` veritabanında bulunan değeri vardır.
* Vurgulanan kod, EF Core SQL UPDATE ifadesinin WHERE yan tümcesindeki görüntülenen `Department` varlığındaki özgün `RowVersion` değerini kullandığından emin olur.

Bir eşzamanlılık hatası oluştuğunda, aşağıdaki vurgulanmış kod istemci değerlerini (Bu yönteme gönderilen değerler) ve veritabanı değerlerini alır.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=14,23)]

Aşağıdaki kod, `OnPostAsync`gönderilen değerden farklı veritabanı değerleri olan her bir sütun için özel bir hata iletisi ekler:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_Error)]

Aşağıdaki vurgulanan kod, `RowVersion` değerini veritabanından alınan yeni değer olarak ayarlar. Kullanıcının bir dahaki sefer **Kaydet**' i tıklatması durumunda, yalnızca düzenleme sayfasının son görüntüsü bu yana gerçekleşen eşzamanlılık hataları yakalanacaktır.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=28)]

`ModelState` eski `RowVersion` değerine sahip olduğundan `ModelState.Remove` deyimin olması gerekir. Razor sayfasında, bir alan için `ModelState` değeri, her ikisi de varsa Model özelliği değerlerinin üzerine gelir.

### <a name="update-the-razor-page"></a>Razor sayfasını güncelleştirme

*Sayfaları/departmanları/Düzenle. cshtml* 'yi aşağıdaki kodla güncelleştirin:

[!code-html[](intro/samples/cu30/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Yukarıdaki kod:

* `@page` `page` yönergesini `@page "{id:int}"`olarak güncelleştirir.
* Gizli satır sürümü ekler. `RowVersion` eklenmesi gerekir, bu yüzden geri gönder değeri bağlar.
* Hata ayıklama amacıyla `RowVersion` son baytını görüntüler.
* `ViewData`, türü kesin belirlenmiş `InstructorNameSL`ile değiştirir.

### <a name="test-concurrency-conflicts-with-the-edit-page"></a>Eşzamanlılık çakışmalarını düzenleme sayfası ile test

İki tarayıcılar örneklerinde düzenleme İngilizce departmanı açın:

* Uygulamayı çalıştırın ve bölümleri seçin.
* Ingilizce departman için **düzenleme** köprüsüne sağ tıklayın ve **Yeni sekmede aç**' ı seçin.
* İlk sekmede, Ingilizce bölümünün **düzenleme** Köprüsü ' ne tıklayın.

Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.

İlk tarayıcı sekmesinde adı değiştirin ve **Kaydet**' e tıklayın.

![Departman düzenleme değişikliğinden sonra sayfa 1](concurrency/_static/edit-after-change-130.png)

Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir. Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.

İkinci bir tarayıcı sekmesinde farklı bir alana değiştirin.

![Departman düzenleme değişikliğinden sonra sayfa 2](concurrency/_static/edit-after-change-230.png)

**Kaydet** düğmesine tıklayın. Veritabanı değerleriyle eşleşmeyen tüm alanlar için hata iletileri görürsünüz:

![Departman düzenleme sayfa hata iletisi](concurrency/_static/edit-error30.png)

Ad alanı değiştirmek bu tarayıcı penceresini düşünmediğiniz. Kopyalama ve geçerli değerin (dil) adı alanına yapıştırın. Sekme genişletme. İstemci tarafı doğrulaması hata iletisini kaldırır.

Yeniden **Kaydet** ' e tıklayın. İkinci tarayıcı sekmesinde girdiğiniz değer kaydedilir. Dizin sayfasında kaydedilen değerler görürsünüz.

## <a name="update-the-delete-page"></a>Silme sayfası

*Sayfaları/departmanları/delete. cshtml. cs* öğesini aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Delete.cshtml.cs)]

Varlık getirildi sonra değiştiğinde silme sayfası eşzamanlılık çakışmalarını algılar. `Department.RowVersion`, varlık getirilen satır sürümüdür. EF Core, SQL DELETE komutunu oluşturduğunda, bir WHERE yan tümcesini `RowVersion`içerir. Sıfır satır SQL DELETE komutu sonuçları etkileniyorsa:

* SQL DELETE komutundaki `RowVersion` veritabanındaki `RowVersion` eşleşmiyor.
* DbUpdateConcurrencyException özel durum oluşturulur.
* `OnGetAsync` `concurrencyError`çağırılır.

### <a name="update-the-delete-razor-page"></a>Razor Sil sayfasını Güncelleştir

*Sayfaları/departmanları/delete. cshtml* 'yi aşağıdaki kodla güncelleştirin:

[!code-html[](intro/samples/cu30/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

Yukarıdaki kod aşağıdaki değişiklikleri yapar:

* `@page` `page` yönergesini `@page "{id:int}"`olarak güncelleştirir.
* Bir hata iletisi ekler.
* FirstMidName öğesini, **yönetici** alanındaki FullName ile değiştirir.
* Son baytın görüntüleneceği `RowVersion` değiştirir.
* Gizli satır sürümü ekler. `RowVersion` eklenmesi gerekir, çünkü postgit ekleme geri eklemesi değeri bağlar.

### <a name="test-concurrency-conflicts"></a>Eşzamanlılık çakışmalarını test et

Test bölümü oluşturun.

İki tarayıcılar örneklerinde DELETE test departmanı açın:

* Uygulamayı çalıştırın ve bölümleri seçin.
* Test departmanı için **silme** köprüsünü sağ tıklayın ve **Yeni sekmede aç**' ı seçin.
* Test departmanı için **düzenleme** köprüsüne tıklayın.

Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.

İlk tarayıcı sekmesinde bütçeyi değiştirin ve **Kaydet**' e tıklayın.

Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir. Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.

İkinci sekmeden test departmanını silin. Veritabanında geçerli değerlerle birlikte bir eşzamanlılık hatası görüntülenir. `RowVersion` güncelleştirilmemiş olmadığı takdirde **Sil** ' i tıklattığınızda varlık silinir. departman silindi.

## <a name="additional-resources"></a>Ek kaynaklar

* [EF Core eşzamanlılık belirteçleri](/ef/core/modeling/concurrency)
* [EF Core eşzamanlılık işle](/ef/core/saving/concurrency)
* [2. x kaynağı ASP.NET Core hata ayıklaması](https://github.com/dotnet/AspNetCore.Docs/issues/4155)

## <a name="next-steps"></a>Sonraki adımlar

Bu, serideki son öğreticidir. [Bu öğretici serisinin MVC sürümünde](xref:data/ef-mvc/index)ek konular ele alınmıştır.

> [!div class="step-by-step"]
> [Önceki öğretici](xref:data/ef-rp/update-related-data)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu öğreticide, birden çok kullanıcı aynı anda (aynı anda) bir varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir. Sorun yaşıyorsanız, bu [uygulamayı indiremez veya görüntüleyemezsiniz.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yönergeleri indirin](xref:index#how-to-download-a-sample).

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

Kemal, **Kaydet**' i tıklamadan önce, John aynı sayfayı ziyaret ettiğinde başlangıç tarihi alanını 9/1/2007 ' den 9/1/2013 ' e değiştirir.

![2013'e başlangıç tarihini değiştirme](concurrency/_static/change-date.png)

Gamze önce **Kaydet** ' i tıklatır ve tarayıcı Dizin sayfasını görüntülediğinde değişikliği görür.

![Bütçe sıfır olarak değiştirildi](concurrency/_static/budget-zero.png)

John, hala $350.000,00 bütçesini gösteren bir düzenleme sayfasında **Kaydet** ' i tıklatır. Sonraki işlemin ne eşzamanlılık çakışmalarını nasıl ele tarafından belirlenir.

İyimser eşzamanlılık aşağıdaki seçenekleri içerir:

* Bir kullanıcı değiştirmiş hangi özelliğinin kaydını ve yalnızca ilgili sütunları DB'de güncelleştirin.

  Bu senaryoda, veri kaybolacak. Farklı özellikler iki kullanıcı tarafından güncelleştirildi. Biri İngilizce, departman gözatar sonraki açışınızda Gamze'nin hem Can'ın değişiklikleri görürler. Bu güncelleştirme yöntemini, veri kaybına neden olabilecek çakışmaları sayısını azaltabilirsiniz. Bu yaklaşım:
 
  * Aynı özelliğe rakip bir değişiklik yaptıysanız, veri kaybını önlemek olamaz.
  * Genellikle bir web uygulaması pratik değildir. Tüm getirilen ve yeni değerleri izlemek için önemli durum koruma gerektiriyor. Büyük miktarlarda durumu bakımını yapma, uygulama performansını etkileyebilir.
  * Bir varlıkta eşzamanlılık algılama ile karşılaştırıldığında app karmaşıklığı artırabilirsiniz.

* Gamze'nin değişikliğinin üzerine Can'ın değişiklik sağlayabilirsiniz.

  Sonraki biri İngilizce departmanı gözatar, 1/9/2013 görürler ve getirilen $350,000.00 değeri. Bu yaklaşım, *Istemci WINS* veya *son WINS* senaryosu olarak adlandırılır. (İstemciden gelen tüm değerler veri deposunda yer alacak şekilde önceliklidir.) Eşzamanlılık işleme için herhangi bir kodlama yapmazsanız, Istemci WINS otomatik olarak gerçekleşir.

* Can'ın değişiklik DB'de güncelleştirilmesini engelleyebilir. Genellikle, bir uygulamayı atadığınız:

  * Bir hata iletisi görüntüler.
  * Verilerin geçerli durumunu gösterir.
  * Değişiklikleri uygulamak verin.

  Buna *Mağaza WINS* senaryosu denir. (Veri deposu değerleri, istemci tarafından gönderilen değerlere göre önceliklidir.) Bu öğreticide mağaza WINS senaryosunu uygulamanız gerekir. Bu yöntem, hiçbir değişiklik uyarı bir kullanıcı kılınmamasını sağlar.

## <a name="handling-concurrency"></a>Eşzamanlılığı işleme 

Bir özellik [eşzamanlılık belirteci](/ef/core/modeling/concurrency)olarak yapılandırıldığında:

* EF Core getirildi sonra'nın özellik değiştirilmedi doğrular. Bu denetim, [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) veya [Savechangesasync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) çağrıldığında gerçekleşir.
* Özellik alındıktan sonra değiştirilmişse, bir [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) oluşturulur. 

DB ve veri modelinin `DbUpdateConcurrencyException`üretilmesini destekleyecek şekilde yapılandırılması gerekir.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Bir özellik eşzamanlılık çakışmaları algılama

Eşzamanlılık çakışmaları özellik düzeyinde [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) özniteliğiyle algılanabilir. Öznitelik, birden çok modelin özellikleri için uygulanabilir. Daha fazla bilgi için bkz. [veri ek açıklamaları-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).

`[ConcurrencyCheck]` özniteliği bu öğreticide kullanılmıyor.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Bir satırda eşzamanlılık çakışmaları algılama

Eşzamanlılık çakışmalarını algılamak için, modele bir [ROWVERSION](/sql/t-sql/data-types/rowversion-transact-sql) izleme sütunu eklenir.  `rowversion` hatasıyla başarısız oldu:

* SQL Server özeldir. Diğer veritabanlarının benzer bir özellik sağlamayabilir.
* Veritabanından getirildikten sonra'nın bir varlık değiştirilmediğinden belirlemek için kullanılır. 

DB, satır her güncelleştirildiği sırada artan bir sıralı `rowversion` numarası oluşturur. Bir `Update` veya `Delete` komutunda, `Where` yan tümcesi `rowversion`getirilen değerini içerir. Güncelleştirilen satır değiştiyse:

* `rowversion` getirilen değerle eşleşmiyor.
* `Where` yan tümcesi getirilen `rowversion`içerdiğinden `Update` veya `Delete` komutları bir satır bulamaz.
* Bir `DbUpdateConcurrencyException` oluşturulur.

EF Core, hiçbir satır bir `Update` veya `Delete` komutuyla güncellenmemişse, bir eşzamanlılık özel durumu oluşturulur.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Departman varlığa izleme özelliği ekleme

*Modeller/departman. cs*' de, rowversion adlı bir izleme özelliği ekleyin:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

[Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) özniteliği, bu sütunun `Update` ve `Delete` komutlarının `Where` yan tümcesine dahil edildiğini belirtir. Önceki SQL Server sürümleri, SQL `rowversion` türü değiştirilmeden önce bir SQL `timestamp` veri türü kullandığından özniteliğe `Timestamp` denir.

Fluent API'si izleme özelliği de belirtebilirsiniz:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

Aşağıdaki kod, T-SQL bölüm adını güncelleştirildiğinde EF Core tarafından oluşturulan bir bölümü gösterilmektedir:

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=2-3)]

Önceki vurgulanan kod, `RowVersion`içeren `WHERE` yan tümcesini gösterir. DB `RowVersion` `RowVersion` parametresine eşit değilse (`@p2`), hiçbir satır güncellenmez.

Aşağıdaki vurgulanmış kodu tam olarak bir satır güncellenmedi doğrular T-SQL gösterir:

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=4-6)]

[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) , son deyimden etkilenen satır sayısını döndürür. Hiçbir satır güncelleştirilmemiş EF Core, bir `DbUpdateConcurrencyException`oluşturur.

T-SQL EF Core oluşturur Visual Studio çıktı penceresinde görebilirsiniz.

### <a name="update-the-db"></a>DB update

`RowVersion` özelliği eklendiğinde, geçiş gerektiren DB modeli değişir.

Projeyi oluşturun. Bir komut penceresinde aşağıdakileri girin:

```dotnetcli
dotnet ef migrations add RowVersion
dotnet ef database update
```

Yukarıdaki komutlar:

* *Geçişler/{zaman damgası} _RowVersion. cs* geçiş dosyasını ekler.
* *Geçişler/SchoolContextModelSnapshot. cs* dosyasını güncelleştirir. Güncelleştirme, `BuildModel` yöntemine aşağıdaki vurgulanmış kodu ekler:

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=14-16)]

* DB güncelleştirilemedi geçişlerini çalıştırır.

<a name="scaffold"></a>

## <a name="scaffold-the-departments-model"></a>İskele Departmanlar modeli

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio) 

[Öğrenci modelini Scafkatlama](xref:data/ef-rp/intro#scaffold-student-pages) ve model sınıfı için `Department` kullanma bölümündeki yönergeleri izleyin.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

 Şu komutu çalıştırın:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

---

Yukarıdaki komut, `Department` modelini yasaklıyor. Projeyi Visual Studio'da açın.

Projeyi oluşturun.

### <a name="update-the-departments-index-page"></a>Departmanlar dizin sayfası

Yapı iskelesi altyapısı Dizin sayfası için bir `RowVersion` sütunu oluşturdu, ancak bu alan gösterilmemelidir. Bu öğreticide, eşzamanlılık kavramasına yardımcı olmak için `RowVersion` son baytı görüntülenir. Son bayta kalan benzersiz olması garanti yoktur. Gerçek bir uygulama `RowVersion` veya `RowVersion`son baytını görüntülemez.

Dizin Sayfası güncelleştirin:

* Dizin bölümlerine değiştirin.
* `RowVersion` içeren biçimlendirmeyi `RowVersion`son baytla değiştirin.
* FirstMidName tam adı ile değiştirin.

Güncelleştirilen sayfaya aşağıdaki işaretlemeyi gösterir:

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Düzenleme sayfa modeli güncelleştirme

Aşağıdaki kodla *Pages\Departments\Edit.cshtml.cs* güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Bir eşzamanlılık sorunu tespit etmek için [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) , getirilen varlıktaki `rowVersion` değeri ile güncelleştirilir. EF Core, özgün `RowVersion` değerini içeren WHERE yan tümcesiyle bir SQL UPDATE komutu oluşturur. GÜNCELLEŞTIRME komutundan hiçbir satır etkilenmeden (hiçbir satır özgün `RowVersion` değerine sahip değilse), bir `DbUpdateConcurrencyException` özel durumu oluşturulur.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

Yukarıdaki kodda, `Department.RowVersion` varlık getirilirken değerdir. `OriginalValue`, bu yöntemde `FirstOrDefaultAsync` çağrıldığında VERITABANıNDAKI değerdir.

Aşağıdaki kod, istemci değerler (Bu yönteme gönderilen değerler) ve DB değerleri alır:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

Aşağıdaki kod, `OnPostAsync`gönderilen değerden farklı olan DB değerleri olan her bir sütun için özel bir hata iletisi ekler:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

Aşağıdaki vurgulanan kod, `RowVersion` değerini veritabanından alınan yeni değer olarak ayarlar. Kullanıcının bir dahaki sefer **Kaydet**' i tıklatması durumunda, yalnızca düzenleme sayfasının son görüntüsü bu yana gerçekleşen eşzamanlılık hataları yakalanacaktır.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

`ModelState` eski `RowVersion` değerine sahip olduğundan `ModelState.Remove` deyimin olması gerekir. Razor sayfasında, bir alan için `ModelState` değeri, her ikisi de varsa Model özelliği değerlerinin üzerine gelir.

## <a name="update-the-edit-page"></a>Güncelleştirme düzenleme sayfası

*Sayfaları/departmanları/Düzenle. cshtml* 'yi şu biçimlendirmeyle güncelleştirin:

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Önceki işaretlemesi:

* `@page` `page` yönergesini `@page "{id:int}"`olarak güncelleştirir.
* Gizli satır sürümü ekler. `RowVersion` eklenmesi gerekir, bu yüzden geri gönder değeri bağlar.
* Hata ayıklama amacıyla `RowVersion` son baytını görüntüler.
* `ViewData`, türü kesin belirlenmiş `InstructorNameSL`ile değiştirir.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Eşzamanlılık çakışmalarını düzenleme sayfası ile test

İki tarayıcılar örneklerinde düzenleme İngilizce departmanı açın:

* Uygulamayı çalıştırın ve bölümleri seçin.
* Ingilizce departman için **düzenleme** köprüsüne sağ tıklayın ve **Yeni sekmede aç**' ı seçin.
* İlk sekmede, Ingilizce bölümünün **düzenleme** Köprüsü ' ne tıklayın.

Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.

İlk tarayıcı sekmesinde adı değiştirin ve **Kaydet**' e tıklayın.

![Departman düzenleme değişikliğinden sonra sayfa 1](concurrency/_static/edit-after-change-1.png)

Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir. Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.

İkinci bir tarayıcı sekmesinde farklı bir alana değiştirin.

![Departman düzenleme değişikliğinden sonra sayfa 2](concurrency/_static/edit-after-change-2.png)

**Kaydet** düğmesine tıklayın. DB değerleri eşleşmeyen tüm alanlar için hata iletileri görürsünüz:

![Departman düzenleme sayfa hata iletisi](concurrency/_static/edit-error.png)

Ad alanı değiştirmek bu tarayıcı penceresini düşünmediğiniz. Kopyalama ve geçerli değerin (dil) adı alanına yapıştırın. Sekme genişletme. İstemci tarafı doğrulaması hata iletisini kaldırır.

![Departman düzenleme sayfa hata iletisi](concurrency/_static/cv.png)

Yeniden **Kaydet** ' e tıklayın. İkinci tarayıcı sekmesinde girdiğiniz değer kaydedilir. Dizin sayfasında kaydedilen değerler görürsünüz.

## <a name="update-the-delete-page"></a>Silme sayfası

Delete sayfa modeli aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

Varlık getirildi sonra değiştiğinde silme sayfası eşzamanlılık çakışmalarını algılar. `Department.RowVersion`, varlık getirilen satır sürümüdür. EF Core, SQL DELETE komutunu oluşturduğunda, bir WHERE yan tümcesini `RowVersion`içerir. Sıfır satır SQL DELETE komutu sonuçları etkileniyorsa:

* SQL DELETE komutundaki `RowVersion` VERITABANıNDAKI `RowVersion` eşleşmiyor.
* DbUpdateConcurrencyException özel durum oluşturulur.
* `OnGetAsync` `concurrencyError`çağırılır.

### <a name="update-the-delete-page"></a>Silme sayfası

*Sayfaları/departmanları/delete. cshtml* 'yi aşağıdaki kodla güncelleştirin:

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

Yukarıdaki kod aşağıdaki değişiklikleri yapar:

* `@page` `page` yönergesini `@page "{id:int}"`olarak güncelleştirir.
* Bir hata iletisi ekler.
* FirstMidName öğesini, **yönetici** alanındaki FullName ile değiştirir.
* Son baytın görüntüleneceği `RowVersion` değiştirir.
* Gizli satır sürümü ekler. `RowVersion` eklenmesi gerekir, bu yüzden geri gönder değeri bağlar.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Eşzamanlılık çakışmalarını silme sayfası ile test

Test bölümü oluşturun.

İki tarayıcılar örneklerinde DELETE test departmanı açın:

* Uygulamayı çalıştırın ve bölümleri seçin.
* Test departmanı için **silme** köprüsünü sağ tıklayın ve **Yeni sekmede aç**' ı seçin.
* Test departmanı için **düzenleme** köprüsüne tıklayın.

Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.

İlk tarayıcı sekmesinde bütçeyi değiştirin ve **Kaydet**' e tıklayın.

Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir. Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.

İkinci sekmeden test departmanını silin. Bir eşzamanlılık hatası, VERITABANıNDAKI geçerli değerlerle birlikte görüntülenir. `RowVersion` güncelleştirilmemiş olmadığı takdirde **Sil** ' i tıklattığınızda varlık silinir. departman silindi.

Veri modelinin nasıl devralınacağını öğrenmek için bkz. [Devralma](xref:data/ef-mvc/inheritance) .

### <a name="additional-resources"></a>Ek kaynaklar

* [EF Core eşzamanlılık belirteçleri](/ef/core/modeling/concurrency)
* [EF Core eşzamanlılık işle](/ef/core/saving/concurrency)
* [Bu öğreticinin YouTube sürümü (eşzamanlılık çakışmalarını Işleme)](https://youtu.be/EosxHTFgYps)
* [Bu öğreticinin YouTube sürümü (Bölüm 2)](https://www.youtube.com/watch?v=kcxERLnaGO0)
* [Bu öğreticinin YouTube sürümü (Bölüm 3)](https://www.youtube.com/watch?v=d4RbpfvELRs)

> [!div class="step-by-step"]
> [Öncekini](xref:data/ef-rp/update-related-data)

::: moniker-end

