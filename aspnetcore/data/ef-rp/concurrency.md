---
title: Razor Pages ASP.NET Core eşzamanlılık-8 ' de EF Core ile
author: rick-anderson
description: Bu öğreticide, birden fazla kullanıcı aynı anda aynı varlığı güncelleştirilişinde çakışmaların nasıl işleneceği gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/concurrency
ms.openlocfilehash: 944e746624bf5fe7c586a521059fa4eb34b0f1e7
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259385"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a>Razor Pages ASP.NET Core eşzamanlılık-8 ' de EF Core ile

By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)ve [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

Bu öğreticide, birden çok kullanıcı bir varlığı eşzamanlı olarak (aynı zamanda) güncelleştirilişinde çakışmaların nasıl işleneceği gösterilmektedir.

## <a name="concurrency-conflicts"></a>Eşzamanlılık çakışmaları

Şu durumlarda bir eşzamanlılık çakışması oluşur:

* Bir Kullanıcı bir varlık için düzenleme sayfasına gider.
* İlk kullanıcının değişikliği veritabanına yazılmadan önce, başka bir kullanıcı aynı varlığı güncelleştirir.

Eşzamanlılık algılama etkinleştirilmemişse, veritabanını güncelleştirme son olarak diğer kullanıcının değişikliklerinin üzerine yazılır. Bu risk kabul edilebilir ise, eşzamanlılık için programlama maliyeti avantaja aykırı bir ücret verebilir.

### <a name="pessimistic-concurrency-locking"></a>Kötümser eşzamanlılık (kilitleme)

Eşzamanlılık çakışmalarını önlemenin bir yolu veritabanı kilitlerini kullanmaktır. Bu, Kötümser eşzamanlılık olarak adlandırılır. Uygulama, güncelleştirmeyi amaçladığı bir veritabanı satırını okumadan önce bir kilit ister. Bir satır, güncelleştirme erişimi için kilitlendiğinde, ilk kilit yayımlanıncaya kadar başka hiçbir kullanıcının satırı kilitlemesine izin verilmez.

Kilitleri yönetmek dezavantajlara sahiptir. Program, karmaşık olabilir ve Kullanıcı sayısı arttıkça performans sorunlarına neden olabilir. Entity Framework Core, BT için yerleşik destek sağlamaz ve bu öğretici Bu öğreticinin nasıl uygulanacağını göstermez.

### <a name="optimistic-concurrency"></a>İyimser eşzamanlılık

İyimser eşzamanlılık eşzamanlılık çakışmalarının gerçekleşmesini sağlar ve ardından, ne zaman uygun şekilde yeniden çalışır. Örneğin, Gamze departman düzenleme sayfasını ziyaret ettiğinde, Ingilizce bölümünün bütçesini $350.000,00 ' den $0,00 ' e değiştiriyor.

![Bütçeyi 0 olarak değiştirme](concurrency/_static/change-budget30.png)

Kemal, **Kaydet**' i tıklamadan önce, John aynı sayfayı ziyaret ettiğinde başlangıç tarihi alanını 9/1/2007 ' den 9/1/2013 ' e değiştirir.

![Başlangıç tarihini 2013 olarak değiştirme](concurrency/_static/change-date30.png)

Gamze önce **Kaydet** ' i tıklatır ve değişiklik etkin duruma geçer çünkü tarayıcı Dizin sayfasını bütçe miktarı olarak sıfır ile görüntülüyor.

John, hala $350.000,00 bütçesini gösteren bir düzenleme sayfasında **Kaydet** ' i tıklatır. Sonraki durum eşzamanlılık çakışmalarını nasıl işleydiğinize göre belirlenir:

* Bir kullanıcının hangi özelliği değiştirdiği ve yalnızca ilgili sütunları veritabanında güncelleştirdiğinden haberdar olabilirsiniz.

  Senaryosunda hiçbir veri kaybolmaz. Farklı özellikler iki kullanıcı tarafından güncelleştirildi. Ingilizce bölüme bir dahaki sefer gözattığında, hem gamze 'nin hem de John 'un değişikliklerini görürler. Bu güncelleştirme yöntemi, veri kaybına neden olabilecek çakışmaların sayısını azaltabilir. Bu yaklaşım bazı dezavantajlara sahiptir:
 
  * Aynı özellikte rekabet değişiklikleri yapılmışsa veri kaybını önleyebilirsiniz.
  * Genellikle bir Web uygulamasında pratik değildir. Tüm getirilen değerleri ve yeni değerleri izlemek için önemli durumun sürdürülmesi gerekir. Büyük miktarlarda durum bulundurma, uygulama performansını etkileyebilir.
  * , Bir varlıkta eşzamanlılık algılama ile karşılaştırıldığında uygulama karmaşıklığını artırabilir.

* John 'un değişikliğini kemal 'in değişikliğini geçersiz kılabilirsiniz.

  Ingilizce bölüme bir dahaki sefer gözattığında, 9/1/2013 ve getirilen $350.000,00 değerini görür. Bu yaklaşım, *Istemci WINS* veya *son WINS* senaryosu olarak adlandırılır. (İstemciden gelen tüm değerler veri deposunda yer alacak şekilde önceliklidir.) Eşzamanlılık işleme için herhangi bir kodlama yapmazsanız, Istemci WINS otomatik olarak gerçekleşir.

* John 'un değişikliğini veritabanında güncelleştirilmesini engelleyebilirsiniz. Uygulama genellikle şu şekilde olur:

  * Bir hata iletisi görüntüler.
  * Verilerin geçerli durumunu gösterir.
  * Kullanıcının değişiklikleri yeniden uygulamasına izin verin.

  Buna *Mağaza WINS* senaryosu denir. (Veri deposu değerleri, istemci tarafından gönderilen değerlere göre önceliklidir.) Bu öğreticide mağaza WINS senaryosunu uygulamanız gerekir. Bu yöntem, bir Kullanıcı uyarı vermeden hiçbir değişikliğin üzerine yazılmamasını sağlar.

## <a name="conflict-detection-in-ef-core"></a>EF Core çakışma algılama

EF Core, çakışmalar algıladığında @no__t 0 özel durum oluşturur. Çakışma algılamayı etkinleştirmek için veri modeli yapılandırılmalıdır. Çakışma algılamayı etkinleştirme seçenekleri şunlardır:

* EF Core, Update ve DELETE komutlarının WHERE yan tümcesinde [eşzamanlılık belirteçleri](/ef/core/modeling/concurrency) olarak yapılandırılan sütunların özgün değerlerini içerecek şekilde yapılandırın.

  @No__t-0 çağrıldığında WHERE yan tümcesi [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute) özniteliğiyle açıklama eklenmiş herhangi bir özelliğin özgün değerlerini arar. Update deyimleri, satır ilk okunduğundan bu yana eşzamanlılık belirteci özelliklerinden herhangi biri değiştiyse güncelleştirilecek bir satır bulmayacaktır. EF Core eşzamanlılık çakışması olarak yorumlar. Birçok sütunu olan veritabanı tablolarında bu yaklaşım çok büyük WHERE yan tümceleriyle sonuçlanabilir ve büyük miktarlarda durum gerektirebilir. Bu nedenle bu yaklaşım genellikle önerilmez ve bu öğreticide kullanılan yöntem değildir.

* Veritabanı tablosunda, bir satırın ne zaman değiştirildiğini belirlemede kullanılabilecek bir izleme sütunu ekleyin.

  SQL Server veritabanında, izleme sütununun veri türü `rowversion` ' dır. @No__t-0 değeri, satır her güncelleştirildiği zaman artılan ardışık bir sayıdır. Bir Update veya delete komutunda WHERE yan tümcesi, izleme sütununun (orijinal satır sürüm numarası) orijinal değerini içerir. Güncelleştirilmekte olan satır başka bir kullanıcı tarafından değiştirilmişse, `rowversion` sütunundaki değer özgün değerden farklıdır. Bu durumda, Update veya DELETE deyimi WHERE yan tümcesi nedeniyle güncelleştirilecek satırı bulamıyor. Bir Update veya delete komutundan hiçbir satır etkilenmeden EF Core eşzamanlılık özel durumu oluşturur.

## <a name="add-a-tracking-property"></a>İzleme özelliği Ekle

*Modeller/departman. cs*' de, rowversion adlı bir izleme özelliği ekleyin:

[!code-csharp[](intro/samples/cu30/Models/Department.cs?highlight=26,27)]

[Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) özniteliği sütunu eşzamanlılık izleme sütunu olarak tanımlar. Fluent API, izleme özelliğini belirtmenin alternatif bir yoludur:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Bir SQL Server veritabanı için, bayt dizisi olarak tanımlanan bir varlık özelliğindeki `[Timestamp]` özniteliği:

* Sütunun WHERE yan tümceleri DELETE ve UPDATE 'e dahil edilmesini sağlar.
* Veritabanındaki sütun türünü [rowversion](/sql/t-sql/data-types/rowversion-transact-sql)olarak ayarlar.

Veritabanı, satır her güncelleştirildiği zaman artılan sıralı bir satır sürüm numarası oluşturur. @No__t-0 veya `Delete` komutunda, `Where` yan tümcesi getirilen satır sürümü değerini içerir. Güncelleştirilmekte olan satır getirilmesinden sonra değiştiyse:

* Geçerli satır sürümü değeri getirilen değerle eşleşmiyor.
* @No__t-0 veya `Delete` komutları satır bulmadığından `Where` yan tümcesi getirilen satır sürümü değerini arar.
* @No__t-0 oluşturulur.

Aşağıdaki kod, Bölüm adı güncelleştirildiği zaman EF Core tarafından oluşturulan T-SQL ' in bir bölümünü gösterir:

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=2-3)]

Önceki vurgulanan kod, `RowVersion` içeren `WHERE` yan tümcesini gösterir. @No__t-0 veritabanı `RowVersion` parametresine eşit değilse (`@p2`), hiçbir satır güncellenmez.

Aşağıdaki vurgulanan kod, tam olarak bir satırın güncelleştirildiğini doğrulayan T-SQL ' i göstermektedir:

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=4-6)]

[@ @ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) , son deyimden etkilenen satır sayısını döndürür. Hiçbir satır güncellenmemişse, EF Core bir @no__t oluşturur-0.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Bir SQLite veritabanı için, bir varlık özelliğinde bayt dizisi olarak tanımlanan `[Timestamp]` özniteliği:

* Sütunun WHERE yan tümceleri DELETE ve UPDATE 'e dahil edilmesini sağlar.
* BLOB sütun türüyle eşlenir.

Veritabanı Tetikleyicileri satır her güncelleştirildiğinde, RowVersion sütununu yeni bir rastgele bayt dizisiyle güncelleştirir. @No__t-0 veya `Delete` komutunda, `Where` yan tümcesi RowVersion sütununun getirilen değerini içerir. Güncelleştirilmekte olan satır getirilmesinden sonra değiştiyse:

* Geçerli satır sürümü değeri getirilen değerle eşleşmiyor.
* @No__t-0 veya `Delete` komutu satır bulamaz çünkü `Where` yan tümcesi orijinal satır sürümü değerini arar.
* @No__t-0 oluşturulur.

---

### <a name="update-the-database"></a>Veritabanını güncelleştirme

@No__t-0 özelliği eklendiğinde, geçiş gerektiren veri modeli değişir.

Projeyi derleyin. 

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
* *Geçişler/SchoolContextModelSnapshot. cs* dosyasını güncelleştirir. Güncelleştirme, `BuildModel` yöntemine aşağıdaki vurgulanmış kodu ekler:

  [!code-csharp[](intro/samples/cu30/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=15-17)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* PMC 'de şu komutu çalıştırın:

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* @No__t-0 dosyasını açın ve vurgulanan kodu ekleyin:

  [!code-csharp[](intro/samples/cu30/MigrationsSQLite/20190722151951_RowVersion.cs?highlight=16-42)]

  Önceki kod:

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
* Model sınıfı için `Department` kullanın.
  * Yeni bir tane oluşturmak yerine var olan bağlam sınıfını kullanın.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

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

Projeyi derleyin.

## <a name="update-the-index-page"></a>Dizin sayfasını Güncelleştir

Scafkatlama aracı Dizin sayfası için bir `RowVersion` sütunu oluşturdu, ancak bu alan bir üretim uygulamasında görüntülenmemelidir. Bu öğreticide, eşzamanlılık işlemenin nasıl çalıştığını göstermeye yardımcı olmak için `RowVersion` ' ın son baytı görüntülenir. Son baytın kendi kendine benzersiz olması garanti edilmez.

*Pages\Departments\Index.cshtml* sayfasını güncelleştir:

* Dizini departmanlar ile değiştirin.
* @No__t-0 içeren kodu, bayt dizisinin yalnızca son baytını gösterecek şekilde değiştirin.
* FirstMidName öğesini FullName ile değiştirin.

Aşağıdaki kod, güncelleştirilmiş sayfayı gösterir:

[!code-html[](intro/samples/cu30/Pages/Departments/Index.cshtml?highlight=5,8,29,48,51)]

## <a name="update-the-edit-page-model"></a>Düzenleme sayfası modelini güncelleştirme

Aşağıdaki kodla *Pages\Departments\Edit.cshtml.cs* güncelleştirin:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_All)]

[OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) , `OnGet` yönteminde alındığı sırada varlıktaki `rowVersion` değeriyle güncelleştirilir. EF Core özgün `RowVersion` değerini içeren WHERE yan tümcesi içeren bir SQL UPDATE komutu oluşturur. UPDATE komutundan hiçbir satır etkilenmemesi durumunda (orijinal @no__t 0 değeri yoksa), bir `DbUpdateConcurrencyException` özel durumu oluşturulur.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_RowVersion&highlight=17-18)]

Vurgulanan kodda:

* @No__t-0 ' daki değer, ilk olarak düzenleme sayfasına yönelik GET isteğine getirilen varlıkta yer alır. Değer, düzenlenecek varlığı görüntüleyen Razor sayfasındaki gizli bir alan tarafından `OnPost` yöntemine sağlanır. Gizli alan değeri model cildi tarafından `Department.RowVersion` ' a kopyalanır.
* `OriginalValue`, WHERE yan tümcesinde kullanılacak EF Core şeydir. Vurgulanan kod satırı çalıştırılmadan önce, bu yöntemde `FirstOrDefaultAsync` çağrıldığında, düzenleme sayfasında görüntülendiklerden farklı olabilecek `OriginalValue` veritabanında bulunan değere sahip olur.
* Vurgulanan kod, EF Core SQL UPDATE ifadesinin WHERE yan tümcesindeki görüntülenen `Department` varlığındaki orijinal `RowVersion` değerini kullandığından emin olur.

Bir eşzamanlılık hatası oluştuğunda, aşağıdaki vurgulanmış kod istemci değerlerini (Bu yönteme gönderilen değerler) ve veritabanı değerlerini alır.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=14,23)]

Aşağıdaki kod, `OnPostAsync` ' a gönderilen değerden farklı veritabanı değerleri olan her bir sütun için özel bir hata iletisi ekler:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_Error)]

Aşağıdaki vurgulanan kod, `RowVersion` değerini veritabanından alınan yeni değere ayarlar. Kullanıcının bir dahaki sefer **Kaydet**' i tıklatması durumunda, yalnızca düzenleme sayfasının son görüntüsü bu yana gerçekleşen eşzamanlılık hataları yakalanacaktır.

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=28)]

@No__t-0 ifadesinde, `ModelState` ' in eski `RowVersion` değeri bulunduğundan gereklidir. Razor sayfasında, bir alan için `ModelState` değeri, her ikisi de varsa Model Özellik değerlerinden önceliklidir.

### <a name="update-the-razor-page"></a>Razor sayfasını güncelleştirme

*Sayfaları/departmanları/Düzenle. cshtml* 'yi aşağıdaki kodla güncelleştirin:

[!code-html[](intro/samples/cu30/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Önceki kod:

* @No__t-0 yönergesini `@page` ' den `@page "{id:int}"` ' ye güncelleştirir.
* Gizli satır sürümü ekler. `RowVersion` eklenmelidir, bu yüzden geri gönder değeri bağlar.
* Hata ayıklama amacıyla `RowVersion` ' ın son baytını görüntüler.
* @No__t-0 ' yı kesin türü belirtilmiş `InstructorNameSL` ile değiştirir.

### <a name="test-concurrency-conflicts-with-the-edit-page"></a>Düzenleme sayfasıyla eşzamanlılık çakışmalarını test etme

Ingilizce bölümünde düzenleme öğesinin iki tarayıcı örneğini açın:

* Uygulamayı çalıştırın ve Departmanlar ' ı seçin.
* Ingilizce departman için **düzenleme** köprüsüne sağ tıklayın ve **Yeni sekmede aç**' ı seçin.
* İlk sekmede, Ingilizce bölümünün **düzenleme** Köprüsü ' ne tıklayın.

İki tarayıcı sekmesi aynı bilgileri görüntüler.

İlk tarayıcı sekmesinde adı değiştirin ve **Kaydet**' e tıklayın.

![Bölüm Düzenle sayfa 1 değişiklikten sonra](concurrency/_static/edit-after-change-130.png)

Tarayıcı, değiştirilen değeri olan dizin sayfasını gösterir ve rowVersion göstergesi güncellenir. Güncelleştirilmiş ROWVERSION göstergesinin, diğer sekmede ikinci geri göndermede görüntülendiğini aklınızda edin.

İkinci tarayıcı sekmesinden farklı bir alanı değiştirin.

![Değişiklik sonrasında bölüm düzenleme sayfası 2](concurrency/_static/edit-after-change-230.png)

**Kaydet** düğmesine tıklayın. Veritabanı değerleriyle eşleşmeyen tüm alanlar için hata iletileri görürsünüz:

![Bölüm düzenleme sayfası hata iletisi](concurrency/_static/edit-error30.png)

Bu tarayıcı penceresi, ad alanını değiştirmeyi planlamadı. Geçerli değeri (diller) kopyalayın ve ad alanına yapıştırın. Sekme genişletme. İstemci tarafı doğrulaması hata iletisini kaldırır.

Yeniden **Kaydet** ' e tıklayın. İkinci tarayıcı sekmesine girdiğiniz değer kaydedilir. Kaydedilen değerleri Dizin sayfasında görürsünüz.

## <a name="update-the-delete-page"></a>Silme sayfasını Güncelleştir

*Sayfaları/departmanları/delete. cshtml. cs* öğesini aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu30/Pages/Departments/Delete.cshtml.cs)]

Sil sayfası, varlık alındıktan sonra değiştirildiğinde eşzamanlılık çakışmalarını algılar. `Department.RowVersion`, varlık getirilirken satır sürümüdür. EF Core, SQL DELETE komutunu oluşturduğunda, `RowVersion` içeren bir WHERE yan tümcesi içerir. SQL DELETE komutu sıfır satırla etkileniyorsa:

* SQL DELETE komutunda `RowVersion`, veritabanındaki `RowVersion` ile eşleşmiyor.
* Bir DbUpdateConcurrencyException özel durumu oluşturulur.
* `OnGetAsync` `concurrencyError` ile çağrılır.

### <a name="update-the-delete-razor-page"></a>Razor Sil sayfasını Güncelleştir

*Sayfaları/departmanları/delete. cshtml* 'yi aşağıdaki kodla güncelleştirin:

[!code-html[](intro/samples/cu30/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

Yukarıdaki kod aşağıdaki değişiklikleri yapar:

* @No__t-0 yönergesini `@page` ' den `@page "{id:int}"` ' ye güncelleştirir.
* Bir hata iletisi ekler.
* FirstMidName öğesini, **yönetici** alanındaki FullName ile değiştirir.
* Son baytı göstermek için `RowVersion` değişir.
* Gizli satır sürümü ekler. `RowVersion` eklenmelidir, bu nedenle postgit ekleme geri eklemesi değeri bağlar.

### <a name="test-concurrency-conflicts"></a>Eşzamanlılık çakışmalarını test et

Test departmanı oluşturun.

Test departmanı üzerinde silmenin iki tarayıcı örneğini açın:

* Uygulamayı çalıştırın ve Departmanlar ' ı seçin.
* Test departmanı için **silme** köprüsünü sağ tıklayın ve **Yeni sekmede aç**' ı seçin.
* Test departmanı için **düzenleme** köprüsüne tıklayın.

İki tarayıcı sekmesi aynı bilgileri görüntüler.

İlk tarayıcı sekmesinde bütçeyi değiştirin ve **Kaydet**' e tıklayın.

Tarayıcı, değiştirilen değeri olan dizin sayfasını gösterir ve rowVersion göstergesi güncellenir. Güncelleştirilmiş ROWVERSION göstergesinin, diğer sekmede ikinci geri göndermede görüntülendiğini aklınızda edin.

İkinci sekmeden test departmanını silin. Veritabanında geçerli değerlerle birlikte bir eşzamanlılık hatası görüntülenir. @No__t-1 güncellenmemişse **Sil** ' i tıklattığınızda varlık silinir. departman silindi.

## <a name="additional-resources"></a>Ek kaynaklar

* [EF Core eşzamanlılık belirteçleri](/ef/core/modeling/concurrency)
* [EF Core eşzamanlılık işle](/ef/core/saving/concurrency)
* [2. x kaynağı ASP.NET Core hata ayıklaması](https://github.com/aspnet/AspNetCore.Docs/issues/4155)

## <a name="next-steps"></a>Sonraki adımlar

Bu, serideki son öğreticidir. [Bu öğretici serisinin MVC sürümünde](xref:data/ef-mvc/index)ek konular ele alınmıştır.

> [!div class="step-by-step"]
> [Önceki öğretici](xref:data/ef-rp/update-related-data)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu öğreticide, birden çok kullanıcı bir varlığı eşzamanlı olarak (aynı zamanda) güncelleştirilişinde çakışmaların nasıl işleneceği gösterilmektedir. Sorun yaşıyorsanız, bu [uygulamayı indiremez veya görüntüleyemezsiniz.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Yönergeleri indirin](xref:index#how-to-download-a-sample).

## <a name="concurrency-conflicts"></a>Eşzamanlılık çakışmaları

Şu durumlarda bir eşzamanlılık çakışması oluşur:

* Bir Kullanıcı bir varlık için düzenleme sayfasına gider.
* İlk kullanıcının değişikliği VERITABANıNA yazılmadan önce, başka bir kullanıcı aynı varlığı güncelleştirir.

Eşzamanlılık algılama etkin değilse, eşzamanlı güncelleştirmeler gerçekleştiğinde:

* Son güncelleştirme kazanır. Diğer bir deyişle, son güncelleştirme değerleri VERITABANıNA kaydedilir.
* Geçerli güncelleştirmelerin ilki kaybedilir.

### <a name="optimistic-concurrency"></a>İyimser eşzamanlılık

İyimser eşzamanlılık eşzamanlılık çakışmalarının gerçekleşmesini sağlar ve ardından, ne zaman uygun şekilde yeniden çalışır. Örneğin, Gamze departman düzenleme sayfasını ziyaret ettiğinde, Ingilizce bölümünün bütçesini $350.000,00 ' den $0,00 ' e değiştiriyor.

![Bütçeyi 0 olarak değiştirme](concurrency/_static/change-budget.png)

Kemal, **Kaydet**' i tıklamadan önce, John aynı sayfayı ziyaret ettiğinde başlangıç tarihi alanını 9/1/2007 ' den 9/1/2013 ' e değiştirir.

![Başlangıç tarihini 2013 olarak değiştirme](concurrency/_static/change-date.png)

Gamze önce **Kaydet** ' i tıklatır ve tarayıcı Dizin sayfasını görüntülediğinde değişikliği görür.

![Bütçe sıfıra değişti](concurrency/_static/budget-zero.png)

John, hala $350.000,00 bütçesini gösteren bir düzenleme sayfasında **Kaydet** ' i tıklatır. Sonraki durum eşzamanlılık çakışmalarını nasıl işleydiğinize göre belirlenir.

İyimser eşzamanlılık aşağıdaki seçenekleri içerir:

* Bir kullanıcının hangi özelliği değiştirmiş olduğunu izleyebilir ve yalnızca VERITABANıNDAKI ilgili sütunları güncelleştirebilirsiniz.

  Senaryosunda hiçbir veri kaybolmaz. Farklı özellikler iki kullanıcı tarafından güncelleştirildi. Ingilizce bölüme bir dahaki sefer gözattığında, hem gamze 'nin hem de John 'un değişikliklerini görürler. Bu güncelleştirme yöntemi, veri kaybına neden olabilecek çakışmaların sayısını azaltabilir. Bu yaklaşım:
 
  * Aynı özellikte rekabet değişiklikleri yapılmışsa veri kaybını önleyebilirsiniz.
  * Genellikle bir Web uygulamasında pratik değildir. Tüm getirilen değerleri ve yeni değerleri izlemek için önemli durumun sürdürülmesi gerekir. Büyük miktarlarda durum bulundurma, uygulama performansını etkileyebilir.
  * , Bir varlıkta eşzamanlılık algılama ile karşılaştırıldığında uygulama karmaşıklığını artırabilir.

* John 'un değişikliğini kemal 'in değişikliğini geçersiz kılabilirsiniz.

  Ingilizce bölüme bir dahaki sefer gözattığında, 9/1/2013 ve getirilen $350.000,00 değerini görür. Bu yaklaşım, *Istemci WINS* veya *son WINS* senaryosu olarak adlandırılır. (İstemciden gelen tüm değerler veri deposunda yer alacak şekilde önceliklidir.) Eşzamanlılık işleme için herhangi bir kodlama yapmazsanız, Istemci WINS otomatik olarak gerçekleşir.

* John 'un değişikliğini VERITABANıNDA güncelleştirilmesini engelleyebilirsiniz. Uygulama genellikle şu şekilde olur:

  * Bir hata iletisi görüntüler.
  * Verilerin geçerli durumunu gösterir.
  * Kullanıcının değişiklikleri yeniden uygulamasına izin verin.

  Buna *Mağaza WINS* senaryosu denir. (Veri deposu değerleri, istemci tarafından gönderilen değerlere göre önceliklidir.) Bu öğreticide mağaza WINS senaryosunu uygulamanız gerekir. Bu yöntem, bir Kullanıcı uyarı vermeden hiçbir değişikliğin üzerine yazılmamasını sağlar.

## <a name="handling-concurrency"></a>Eşzamanlılık işleme 

Bir özellik [eşzamanlılık belirteci](/ef/core/modeling/concurrency)olarak yapılandırıldığında:

* EF Core, özelliğin getirildikten sonra değiştirilmediğini doğrular. Bu denetim, [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) veya [Savechangesasync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) çağrıldığında gerçekleşir.
* Özellik alındıktan sonra değiştirilmişse, bir [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) oluşturulur. 

DB ve veri modeli `DbUpdateConcurrencyException` ' nın üretilmesini destekleyecek şekilde yapılandırılmalıdır.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Bir özellikte eşzamanlılık çakışmalarını algılama

Eşzamanlılık çakışmaları özellik düzeyinde [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) özniteliğiyle algılanabilir. Öznitelik, modeldeki birden çok özelliğe uygulanabilir. Daha fazla bilgi için bkz. [veri ek açıklamaları-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).

@No__t-0 özniteliği bu öğreticide kullanılmıyor.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Bir satırda eşzamanlılık çakışmalarını algılama

Eşzamanlılık çakışmalarını algılamak için, modele bir [ROWVERSION](/sql/t-sql/data-types/rowversion-transact-sql) izleme sütunu eklenir.  `rowversion` hatasıyla başarısız oldu:

* SQL Server özeldir. Diğer veritabanları benzer bir özellik sağlayamayabilir.
* , DB 'den getirilmesinden sonra bir varlığın değiştirilmediğini tespit etmek için kullanılır. 

DB, satır her güncelleştirildiği zaman artılan sıralı bir @no__t 0 sayı üretir. @No__t-0 veya `Delete` komutunda, `Where` yan tümcesi `rowversion` ' ün getirilen değerini içerir. Güncelleştirilmekte olan satır değiştiyse:

* `rowversion` getirilen değerle eşleşmiyor.
* @No__t-0 veya `Delete` komutları bir satır bulmadığından `Where` yan tümcesi getirilen `rowversion` ' i içerir.
* @No__t-0 oluşturulur.

EF Core, hiçbir satır bir `Update` veya `Delete` komutuyla güncellenmemişse, bir eşzamanlılık özel durumu oluşturulur.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Bölüm varlığına bir izleme özelliği ekleyin

*Modeller/departman. cs*' de, rowversion adlı bir izleme özelliği ekleyin:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

[Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) özniteliği, bu sütunun `Update` ve `Delete` komutlarının `Where` yan tümcesine ekleneceğini belirtir. Bu öznitelik `Timestamp` olarak adlandırılır çünkü önceki SQL Server sürümleri SQL `rowversion` türü değiştirilmeden önce SQL `timestamp` veri türü kullandı.

Fluent API izleme özelliğini de belirtebilir:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

Aşağıdaki kod, Bölüm adı güncelleştirildiği zaman EF Core tarafından oluşturulan T-SQL ' in bir bölümünü gösterir:

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=2-3)]

Önceki vurgulanan kod, `RowVersion` içeren `WHERE` yan tümcesini gösterir. DB `RowVersion` `RowVersion` parametresine eşit değilse (`@p2`), hiçbir satır güncellenmez.

Aşağıdaki vurgulanan kod, tam olarak bir satırın güncelleştirildiğini doğrulayan T-SQL ' i göstermektedir:

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=4-6)]

[@ @ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) , son deyimden etkilenen satır sayısını döndürür. Hiçbir satır güncelleştirilmemiş EF Core, bir @no__t oluşturur.

T-SQL EF Core, Visual Studio 'nun çıkış penceresinde görebilirsiniz.

### <a name="update-the-db"></a>DB 'yi güncelleştirme

@No__t-0 özelliği eklendiğinde, geçiş gerektiren DB modeli değişir.

Projeyi derleyin. Komut penceresine şunu girin:

```dotnetcli
dotnet ef migrations add RowVersion
dotnet ef database update
```

Önceki komutlar:

* *Geçişler/{zaman damgası} _RowVersion. cs* geçiş dosyasını ekler.
* *Geçişler/SchoolContextModelSnapshot. cs* dosyasını güncelleştirir. Güncelleştirme, `BuildModel` yöntemine aşağıdaki vurgulanmış kodu ekler:

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=14-16)]

* VERITABANıNı güncelleştirmek için geçişleri çalıştırır.

<a name="scaffold"></a>

## <a name="scaffold-the-departments-model"></a>Departmanlar modelini temklesi

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

[Öğrenci modelini Scafkatlama](xref:data/ef-rp/intro#scaffold-student-pages) ve model sınıfı için `Department` kullanın bölümündeki yönergeleri izleyin.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

 Şu komutu çalıştırın:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

---

Yukarıdaki komut, `Department` modelini tanılar. Projeyi Visual Studio'da açın.

Projeyi derleyin.

### <a name="update-the-departments-index-page"></a>Departmanlar Dizin sayfasını Güncelleştir

Yapı iskelesi altyapısı Dizin sayfası için bir `RowVersion` sütunu oluşturdu, ancak bu alan gösterilmemelidir. Bu öğreticide, eşzamanlılık kavramasına yardımcı olmak için `RowVersion` ' ın son baytı görüntülenir. Son baytın benzersiz olması garanti edilmez. Gerçek bir uygulama `RowVersion` veya son bayt `RowVersion` ' i görüntülemez.

Dizin sayfasını güncelleştir:

* Dizini departmanlar ile değiştirin.
* @No__t-0 içeren biçimlendirmeyi `RowVersion` ' in son baytla değiştirin.
* FirstMidName öğesini FullName ile değiştirin.

Aşağıdaki biçimlendirme güncelleştirilmiş sayfayı göstermektedir:

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Düzenleme sayfası modelini güncelleştirme

Aşağıdaki kodla *Pages\Departments\Edit.cshtml.cs* güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Bir eşzamanlılık sorunu tespit etmek için [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) , getirilen varlıktaki `rowVersion` değeriyle güncelleştirilir. EF Core özgün `RowVersion` değerini içeren WHERE yan tümcesi içeren bir SQL UPDATE komutu oluşturur. UPDATE komutundan hiçbir satır etkilenmemesi durumunda (orijinal @no__t 0 değeri yoksa), bir `DbUpdateConcurrencyException` özel durumu oluşturulur.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

Yukarıdaki kodda `Department.RowVersion`, varlık getirilirken değeridir. Bu yöntemde `FirstOrDefaultAsync` çağrıldığında, `OriginalValue`, VERITABANıNDAKI değerdir.

Aşağıdaki kod, istemci değerlerini (Bu yönteme gönderilen değerler) ve DB değerlerini alır:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

Aşağıdaki kod, `OnPostAsync` ' a gönderilen değerden farklı olan DB değerleri olan her bir sütun için özel bir hata iletisi ekler:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

Aşağıdaki vurgulanan kod, `RowVersion` değerini veritabanından alınan yeni değere ayarlar. Kullanıcının bir dahaki sefer **Kaydet**' i tıklatması durumunda, yalnızca düzenleme sayfasının son görüntüsü bu yana gerçekleşen eşzamanlılık hataları yakalanacaktır.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

@No__t-0 ifadesinde, `ModelState` ' in eski `RowVersion` değeri bulunduğundan gereklidir. Razor sayfasında, bir alan için `ModelState` değeri, her ikisi de varsa Model Özellik değerlerinden önceliklidir.

## <a name="update-the-edit-page"></a>Düzenleme sayfasını Güncelleştir

*Sayfaları/departmanları/Düzenle. cshtml* 'yi şu biçimlendirmeyle güncelleştirin:

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Yukarıdaki biçimlendirme:

* @No__t-0 yönergesini `@page` ' den `@page "{id:int}"` ' ye güncelleştirir.
* Gizli satır sürümü ekler. `RowVersion` eklenmelidir, bu yüzden geri gönder değeri bağlar.
* Hata ayıklama amacıyla `RowVersion` ' ın son baytını görüntüler.
* @No__t-0 ' yı kesin türü belirtilmiş `InstructorNameSL` ile değiştirir.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Düzenleme sayfasıyla eşzamanlılık çakışmalarını test etme

Ingilizce bölümünde düzenleme öğesinin iki tarayıcı örneğini açın:

* Uygulamayı çalıştırın ve Departmanlar ' ı seçin.
* Ingilizce departman için **düzenleme** köprüsüne sağ tıklayın ve **Yeni sekmede aç**' ı seçin.
* İlk sekmede, Ingilizce bölümünün **düzenleme** Köprüsü ' ne tıklayın.

İki tarayıcı sekmesi aynı bilgileri görüntüler.

İlk tarayıcı sekmesinde adı değiştirin ve **Kaydet**' e tıklayın.

![Bölüm Düzenle sayfa 1 değişiklikten sonra](concurrency/_static/edit-after-change-1.png)

Tarayıcı, değiştirilen değeri olan dizin sayfasını gösterir ve rowVersion göstergesi güncellenir. Güncelleştirilmiş ROWVERSION göstergesinin, diğer sekmede ikinci geri göndermede görüntülendiğini aklınızda edin.

İkinci tarayıcı sekmesinden farklı bir alanı değiştirin.

![Değişiklik sonrasında bölüm düzenleme sayfası 2](concurrency/_static/edit-after-change-2.png)

**Kaydet** düğmesine tıklayın. DB değerleriyle eşleşmeyen tüm alanlar için hata iletileri görürsünüz:

![Bölüm düzenleme sayfası hata iletisi](concurrency/_static/edit-error.png)

Bu tarayıcı penceresi, ad alanını değiştirmeyi planlamadı. Geçerli değeri (diller) kopyalayın ve ad alanına yapıştırın. Sekme genişletme. İstemci tarafı doğrulaması hata iletisini kaldırır.

![Bölüm düzenleme sayfası hata iletisi](concurrency/_static/cv.png)

Yeniden **Kaydet** ' e tıklayın. İkinci tarayıcı sekmesine girdiğiniz değer kaydedilir. Kaydedilen değerleri Dizin sayfasında görürsünüz.

## <a name="update-the-delete-page"></a>Silme sayfasını Güncelleştir

Silme sayfası modelini aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

Sil sayfası, varlık alındıktan sonra değiştirildiğinde eşzamanlılık çakışmalarını algılar. `Department.RowVersion`, varlık getirilirken satır sürümüdür. EF Core, SQL DELETE komutunu oluşturduğunda, `RowVersion` içeren bir WHERE yan tümcesi içerir. SQL DELETE komutu sıfır satırla etkileniyorsa:

* SQL DELETE komutunda `RowVersion`, DB 'de `RowVersion` ile eşleşmiyor.
* Bir DbUpdateConcurrencyException özel durumu oluşturulur.
* `OnGetAsync` `concurrencyError` ile çağrılır.

### <a name="update-the-delete-page"></a>Silme sayfasını Güncelleştir

*Sayfaları/departmanları/delete. cshtml* 'yi aşağıdaki kodla güncelleştirin:

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

Yukarıdaki kod aşağıdaki değişiklikleri yapar:

* @No__t-0 yönergesini `@page` ' den `@page "{id:int}"` ' ye güncelleştirir.
* Bir hata iletisi ekler.
* FirstMidName öğesini, **yönetici** alanındaki FullName ile değiştirir.
* Son baytı göstermek için `RowVersion` değişir.
* Gizli satır sürümü ekler. `RowVersion` eklenmelidir, bu yüzden geri gönder değeri bağlar.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Silme sayfasıyla eşzamanlılık çakışmalarını test etme

Test departmanı oluşturun.

Test departmanı üzerinde silmenin iki tarayıcı örneğini açın:

* Uygulamayı çalıştırın ve Departmanlar ' ı seçin.
* Test departmanı için **silme** köprüsünü sağ tıklayın ve **Yeni sekmede aç**' ı seçin.
* Test departmanı için **düzenleme** köprüsüne tıklayın.

İki tarayıcı sekmesi aynı bilgileri görüntüler.

İlk tarayıcı sekmesinde bütçeyi değiştirin ve **Kaydet**' e tıklayın.

Tarayıcı, değiştirilen değeri olan dizin sayfasını gösterir ve rowVersion göstergesi güncellenir. Güncelleştirilmiş ROWVERSION göstergesinin, diğer sekmede ikinci geri göndermede görüntülendiğini aklınızda edin.

İkinci sekmeden test departmanını silin. Bir eşzamanlılık hatası, VERITABANıNDAKI geçerli değerlerle birlikte görüntülenir. @No__t-1 güncellenmemişse **Sil** ' i tıklattığınızda varlık silinir. departman silindi.

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

