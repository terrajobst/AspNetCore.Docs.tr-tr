---
title: -Eşzamanlılık - 8, 10 EF çekirdekli ASP.NET Core MVC
author: rick-anderson
description: Bu öğreticide, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 0ae566a76a2ef656843452ed537b8fdfbddaed22
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090907"
---
# <a name="aspnet-core-mvc-with-ef-core---concurrency---8-of-10"></a>-Eşzamanlılık - 8, 10 EF çekirdekli ASP.NET Core MVC

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması, Entity Framework Core ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](intro.md).

Önceki öğreticilerde, verileri güncelleştirmek hakkında bilgi edindiniz. Bu öğreticide, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.

Departman varlık ile çalışma ve eşzamanlılık hatalarını işleme web sayfaları oluşturacaksınız. Aşağıdaki çizimler bir eşzamanlılık çakışması ortaya çıkarsa, gösterilen bazı iletileri de dahil olmak üzere düzenleme ve silme sayfalar gösterir.

![Departman düzenleme sayfası](concurrency/_static/edit-error.png)

![Bölüm silme sayfası](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a>Eşzamanlılık çakışmaları

Düzenlemek için bir kullanıcı bir varlığın verilerini görüntüler ve ilk kullanıcının değişiklik veritabanına yazılmadan önce başka bir kullanıcı aynı varlığın veri güncelleştirmeleri bir eşzamanlılık çakışması gerçekleşir. Böyle bir çakışma algılanması etkinleştirmezseniz, kişi veritabanını güncelleştiren son diğer kullanıcının yaptığı değişikliklerin üzerine yazar. Birçok uygulamada, bu riski kabul edilebilir: Bazı kullanıcılara veya birkaç güncelleştirmeleri varsa veya bazı değişikliklerin üzerine yazılır, programlama için eşzamanlılık maliyeti gereksinimlerine ağır bastığı avantajı gerçekten önemli değildir. Bu durumda, eşzamanlılık çakışmalarını işleme için uygulamayı yapılandırmanız gerekmez.

### <a name="pessimistic-concurrency-locking"></a>Kötümser eşzamanlılık (kilitleme)

Uygulamanızı eşzamanlılık senaryolarda yanlışlıkla veri kaybı önleme ihtiyacınız varsa, bunu yapmanın bir yolu veritabanı kilitlerini kullanmaktır. Bu, eşzamanlılık çağrılır. Örneğin, bir veritabanından satır okumadan önce kilit isteği salt okunur veya güncelleştirme erişim için. Güncelleştirme erişimi için bir satır kilitlerseniz, başka hiçbir kullanıcı için ya da satırın kilitlemek için izin salt okunur veya değiştirilen sürecinde olan verilerin bir kopyasını elde edecekleri çünkü erişim güncelleştirin. Salt okunur erişim için bir satır kilitlerseniz, diğerleri de salt okunur erişim için kullanılabilir ancak güncelleştirme kilitleyebilirsiniz.

Kilitleri yönetmek dezavantajları vardır. Programa karmaşık olabilir. Önemli bir veritabanı yönetim kaynakları gerektirir ve bu uygulamanın kullanıcı sayısı performans sorunlarına neden olabilir artırır. Bu nedenle, tüm veritabanı yönetim sistemleri kötümser eşzamanlılık destekler. Entity Framework Core yerleşik desteği yok sağlar ve Bu öğreticide nasıl göstermez.

### <a name="optimistic-concurrency"></a>İyimser eşzamanlılık

İyimser eşzamanlılık kötümser eşzamanlılık alternatiftir. İyimser eşzamanlılık, eşzamanlılık çakışmalarını olmasını sağlar ve eğer uygun şekilde tepki anlamına gelir. Örneğin, Jane departmanı Düzen sayfasını ziyaret ve bütçe tutar İngilizce bölümü için 350,000.00 $0,00 değiştirir.

![Bütçe 0 olarak değiştirme](concurrency/_static/change-budget.png)

Jane tıkladığında önce **Kaydet**, John aynı sayfayı ziyaret eder ve alanın başlangıç tarihi 1/9/2013 1/9/2007'deki değiştirir.

![2013'e başlangıç tarihini değiştirme](concurrency/_static/change-date.png)

Jane tıkladığında **Kaydet** ilk ve her tarayıcı dizin sayfasına geri döndüğünde değiştirme görür.

![Bütçe sıfır olarak değiştirildi](concurrency/_static/budget-zero.png)

John tıklattığında **Kaydet** yine de bir bütçe $350,000.00 birini gösteren bir Düzenle sayfasında. Sonraki işlemin ne eşzamanlılık çakışmalarını nasıl ele tarafından belirlenir.

Bazı seçenekleri şunlardır:

* Bir kullanıcı değiştirmiş hangi özelliğinin izlemek ve yalnızca ilgili sütunları veritabanında güncelleştirin.

     Farklı özellikleri iki kullanıcı tarafından güncelleştirildiği için yer alan örnek senaryoda, veri kaybı, olacaktır. Gamze'nin hem Can'ın değişiklikleri--başlangıç tarihi 1/9/2013 / ve sıfır dolarlık bir bütçe biri İngilizce, departman gözatar sonraki açışınızda görürler. Bu güncelleştirme yöntemini veri kaybına neden olabilecek çakışmaları sayısını azaltabilir, ancak bir varlığın aynı özelliğe rakip bir değişiklik yaptıysanız, veri kaybını önlemek olamaz. Varlık Çerçevesi'Bu şekilde çalışıp çalışmadığını güncelleştirme kodunuzu nasıl uygulayacağınıza bağlıdır. Bir varlığın özgün tüm özellik değerlerini yanı sıra yeni değerleri izlemek için büyük miktarlarda durumu bakımını gerektirebilir içinde bir web uygulaması pratik değildir, çünkü genellikle. Koruma durumu büyük miktarlarda uygulama performansı etkileyebilir sunucu kaynaklarını gerektirir ya da web sayfasında kendisi (örneğin, gizli alanlar) dahil edilmesi için veya bir tanımlama bilgisinde.

* Gamze'nin değişikliğinin üzerine Can'ın değişiklik sağlayabilirsiniz.

     Sonraki biri İngilizce departmanı gözatar, 1/9/2013 görürler ve geri yüklenen $350,000.00 değeri. Bu adlı bir *istemci WINS* veya *WINS'te son* senaryo. (Tüm istemci değerlerinden veri deposunda nedir üzerinde önceliklidir.) Bu bölüm için giriş eşzamanlılık işleme için kodlama yapmazsanız belirtildiği gibi bu otomatik olarak gerçekleşir.

* Veritabanında güncelleştirilen Can'ın değişiklik engelleyebilirsiniz.

     Genellikle, bir hata iletisi görüntüler, ona verilerin geçerli durumunu gösterir ve yine de bunları yapmak istediği, peter'in değişikliklerini uygulamak kendisine izin. Bu adlı bir *Store WINS* senaryo. (Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.) Bu öğreticide Store WINS senaryo uygulayacaksınız. Bu yöntem, neler olduğunu için uyarı bir kullanıcı olmaksızın herhangi bir değişiklik kılınmamasını sağlar.

### <a name="detecting-concurrency-conflicts"></a>Eşzamanlılık çakışmaları algılama

İşleyerek çakışmalarını çözebilirsiniz `DbConcurrencyException` Entity Framework oluşturduğu özel durumları. Bu özel durumlar ne zaman öğrenmek için Entity Framework algılayamayabilir çakışmalar olması gerekir. Bu nedenle, veritabanı ve veri modeline uygun şekilde yapılandırmanız gerekir. Çakışma algılamasını etkinleştirmek için bazı seçenekler şunlardır:

* Veritabanı tablosunda bir satıra değiştirildiğinde belirlemek için kullanılan bir izleme sütunu içerir. Daha sonra bu sütun nerede eklemek için Entity Framework yapılandırabilirsiniz SQL güncelleştirme veya silme komutlarını yan tümcesi.

     İzleme sütunun veri türü genellikle `rowversion`. `rowversion` Satır güncelleştirilir her zaman artan sıralı bir sayı değeridir. Bir Update veya Delete komutu, Where yan tümcesi izleme sütunu (özgün satır sürümü) özgün değeri içerir. Güncelleştirilen satır değeri başka bir kullanıcı tarafından değiştirildiyse `rowversion` Update veya Delete deyiminin Where nedeniyle güncelleştirilecek satırın bulmanız sütun özgün değerinden farklı yan tümcesi. Update veya Delete tarafından hiçbir satırın güncelleştirilmediği Entity Framework bulur (diğer bir deyişle, etkilenen satır sayısı sıfır olduğunda) komut olduğunda, bir eşzamanlılık çakışması yorumlar.

* Where tablosunda her sütun öğesinin özgün değerleri eklemek için Entity Framework yapılandırma Update ve Delete komutlarını yan tümcesi.

     İlk seçenek satırın ilk okunuşundan bu yana, sıradaki herhangi bir şey değiştiyse, olduğu gibi Where yan tümcesi bir satırı güncelleştirmek için iade kalmaz, Entity Framework bir eşzamanlılık çakışması yorumlar. Birçok sütunları olan veritabanı tabloları için bu yaklaşım çok büyük nerede sonuçlanabilir yan tümcesi ve büyük miktarlarda durumu bakımını gerektirebilir. Daha önce belirtildiği gibi büyük miktarlarda durumu bakımını yapma, uygulama performansını etkileyebilir. Bu nedenle bu yaklaşım genellikle önerilmez ve Bu öğreticide kullanılan yöntem değildir.

     Eşzamanlılık için bu yaklaşımı uygulamak istiyorsanız, tüm birincil anahtar özellikleri ekleyerek eşzamanlılık için izlemek istediğiniz varlık işaretlemek sahip `ConcurrencyCheck` öznitelik. Bu değişiklik tüm sütunları SQL Where yan tümcesinde Update ve Delete deyimleri dahil etmek Entity Framework sağlar.

Bu öğreticinin geri kalanında, ekleyeceksiniz bir `rowversion` özelliği departmanı varlık izleme, bir denetleyici ve Görünüm ve her şeyin düzgün çalıştığını doğrulamak için sınama.

## <a name="add-a-tracking-property-to-the-department-entity"></a>Departman varlığa izleme özelliği ekleme

İçinde *Models/Department.cs*, RowVersion adlı izleme özelliği ekleyin:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

`Timestamp` Özniteliği, bu sütun dahil olacağını belirtir Where yan tümcesi, Update ve Delete komutlarını veritabanına gönderilir. Adlandırılan öznitelik `Timestamp` bir SQL SQL Server'ın önceki sürümlerinde kullanılan çünkü `timestamp` veri türü SQL önce `rowversion` değiştirildi. .NET türü `rowversion` bir bayt dizisidir.

Fluent API'sini kullanmayı tercih ederseniz kullanabilirsiniz `IsConcurrencyToken` yöntemi (içinde *Data/SchoolContext.cs*) aşağıdaki örnekte gösterildiği gibi izleme özelliği belirtmek için:

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

Başka bir geçiş gerçekleştirmeniz gereken şekilde bir özellik ekleyerek veritabanı modeli değişti.

Yaptığınız değişiklikleri kaydedin ve projeyi oluşturun ve sonra komut penceresinde aşağıdaki komutları girin:

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a>Departmanlar denetleyici ve görünümler oluşturma

Öğrenciler, kurslara ve Eğitmenler için daha önce yaptığınız gibi Departmanlar denetleyici ve görünüm iskelesini.

![İskele departmanı](concurrency/_static/add-departments-controller.png)

İçinde *DepartmentsController.cs* dosya, departman Yöneticisi açılır listede Eğitmen tam adı yerine yalnızca son adını içerecek şekilde "tam"adı "FirstMidName" dört tüm oluşumlarını değiştirin.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a>Departmanlar dizini görünümünü güncelleştirme

Yapı iskelesi altyapısı RowVersion sütun dizini Görünümü'nde oluşturulur, ancak bu alan görüntülenen olmamalıdır.

Değiştirin *Views/Departments/Index.cshtml* aşağıdaki kod ile.

[!code-html[](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

Bu "Bölümler" başlığı değiştirir, RowVersion sütun siler ve yönetici için tam adı ad yerine gösterir.

## <a name="update-the-edit-methods-in-the-departments-controller"></a>Düzenleme metotlarını Departmanlar denetleyicisindeki güncelleştir

Her iki HttpGet içinde `Edit` yöntemi ve `Details` yöntemi ekleme `AsNoTracking`. İçinde HttpGet `Edit` yöntemi istekli yükleme için yönetici ekleyin.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

HttpPost için mevcut kodu değiştirin `Edit` yöntemini aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

Kod, güncelleştirilecek departman okumaya çalışarak başlar. Varsa `SingleOrDefaultAsync` yöntemi null değerini döndürür, departman, başka bir kullanıcı tarafından silindi. Bu durumda kod düzenleme sayfası, bir hata iletisi ile yeniden, böylece bir departman varlık oluşturmak için gönderilen form değerleri kullanır. Alternatif olarak, departman alanları yeniden görüntüleme olmadan yalnızca bir hata iletisi görüntüler, departman varlık yeniden oluşturmak zorunda mıydı.

Görünümün özgün depolar `RowVersion` değerini gizli bir alan ve bu yöntem değeri alan `rowVersion` parametresi. Çağırmadan önce `SaveChanges`, özgün moduna sahip `RowVersion` özellik değeri `OriginalValues` varlık için koleksiyon.

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

Entity Framework SQL güncelleştirme komut oluşturduğunda, bu komut, özgün sahip bir satır için görünen bir WHERE yan tümcesi içerecektir sonra `RowVersion` değeri. Hiçbir satır güncelleştirme komutu tarafından etkileniyorsanız (hiçbir satır özgün sahip `RowVersion` değer), Entity Framework oluşturur bir `DbUpdateConcurrencyException` özel durum.

Bu özel durum için catch bloğu içindeki kod güncelleştirilmiş değerleri olan etkilenen departmanı varlık alır `Entries` özelliği özel durum nesnesi.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

`Entries` Koleksiyon yalnızca bir tane olacaktır `EntityEntry` nesne.  Bu nesne, kullanıcı tarafından girilen yeni değerler ve geçerli veritabanı değerlerini almak için kullanabilirsiniz.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

Özel hata iletisi düzenleme girilen kullanıcı sayfasında veritabanı değerleri farklı olan her sütun için kod ekler (yalnızca bir alan burada gösterilmiştir kısa tutmak için).

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

Son olarak, kod ayarlar `RowVersion` değerini `departmentToUpdate` yeni değere veritabanından alınır. Bu yeni `RowVersion` değeri depolanan gizli alanı sayfasını yeniden düzenleme ve sonraki kullanıcı çalıştırdığında **Kaydet**, yeniden düzenleme sayfası, yakalanan bu yana gerçekleşen eşzamanlılık hataları.

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

`ModelState.Remove` Deyimi, çünkü gereklidir `ModelState` eski olan `RowVersion` değeri. Görünümü'nde `ModelState` değerinin ikisi de mevcut olduğunda alan üzerinde model özellik değerlerini öncelik kazanır.

## <a name="update-the-department-edit-view"></a>Departman düzenleme görünümü güncelleştirme

İçinde *Views/Departments/Edit.cshtml*, aşağıdaki değişiklikleri yapın:

* Kaydetmek için gizli bir alan ekleme `RowVersion` özellik değeri, gizli bir alan için takip `DepartmentID` özelliği.

* Bir "Yönetici Seç" seçeneği aşağı açılan listesine ekleyin.

[!code-html[](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a>Test eşzamanlılık çakışmaları düzenleme sayfası

Uygulamayı çalıştırın ve Departmanlar dizin sayfasına gidin. Sağ **Düzenle** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**, ardından **Düzenle** İngilizce departmanı için köprü. İki tarayıcı sekmeleri artık aynı bilgiyi görüntüler.

İlk tarayıcı sekmesine bir alana değiştirin ve tıklatın **Kaydet**.

![Departman düzenleme değişikliğinden sonra sayfa 1](concurrency/_static/edit-after-change-1.png)

Tarayıcı değişmiş değer ile dizin sayfası gösterilir.

İkinci bir tarayıcı sekmesinde bir alanı değiştirin.

![Departman düzenleme değişikliğinden sonra sayfa 2](concurrency/_static/edit-after-change-2.png)

**Kaydet**'e tıklayın. Bir hata iletisi görürsünüz:

![Departman düzenleme sayfa hata iletisi](concurrency/_static/edit-error.png)

Tıklayın **Kaydet** yeniden. İkinci tarayıcı sekmesinde girdiğiniz değer kaydedilir. Dizin Sayfası göründüğünde, kaydedilen değerler görürsünüz.

## <a name="update-the-delete-page"></a>Silme sayfası

Silme sayfası için Entity Framework, birisi başka benzer bir şekilde bölüm düzenleme nedeni eşzamanlılık çakışmalarını algılar. Zaman HttpGet `Delete` yöntemi görüntüler onay görünümü, görünümün özgün içerir `RowVersion` gizli bir alan değeri. Değer ardından HttpPost için kullanılabilir olduğunu `Delete` kullanıcının silmeyi onaylaması çağrılan yöntem. Entity Framework SQL DELETE komutu oluşturduğunda orijinal bir WHERE yan tümcesi içerdiğinden `RowVersion` değeri. Sıfır satır komutu sonuçları (satır silme onayı sayfasında görüntülenen sonra değiştirildiği anlamına gelir) etkileniyorsa, bir eşzamanlılık özel durum oluşturulur ve HttpGet `Delete` yöntemi görüntülemek için true olarak hata bayrak kümesi ile çağrılır bir hata iletisiyle onay sayfası. Bu durumda, hata iletisi görüntülenir, böylece satırın başka bir kullanıcı tarafından silindiğinden sıfır satır etkilenmiştir mümkündür.

### <a name="update-the-delete-methods-in-the-departments-controller"></a>Güncelleştirme Departmanlar denetleyicideki silme yöntemleri

İçinde *DepartmentsController.cs*, HttpGet değiştirin `Delete` yöntemini aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

Yöntemi, bir eşzamanlılık hatası sonra sayfayı yeniden olup olmadığını belirten isteğe bağlı bir parametre kabul eder. Bu bayrağı true olarak ayarlandığında ve artık departman var, başka bir kullanıcı tarafından silinmiş olabilir. Bu durumda, kod dizin sayfasına yönlendirir.  Bu bayrağı true olarak ayarlandığında ve departman mevcut değilse, başka bir kullanıcı tarafından değiştirildi. Bu durumda, kod görünümünü kullanarak bir hata iletisi gönderir `ViewData`.

HttpPost değiştirin `Delete` yöntemi (adlı `DeleteConfirmed`) aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

Yalnızca değiştirilen iskele kurulmuş kod içinde bu yöntem yalnızca bir kayıt kimliği kabul:

```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

Model bağlayıcı tarafından oluşturulan bir bölüm varlık örneği için bu parametreyi değiştirdiniz. Bu kayıt anahtarı yanı sıra RowVersion özellik değerinin EF erişim sağlar.

```csharp
public async Task<IActionResult> Delete(Department department)
```

Eylem yöntemi adından de değiştirdiğiniz `DeleteConfirmed` için `Delete`. İskele kurulan kodu kullanılan adı `DeleteConfirmed` HttpPost yöntem benzersiz bir imza vermek için. (CLR'nin farklı yöntem parametreleri sağlamak için aşırı yüklenmiş yöntemler gerektirir.) İmzaları benzersiz, MVC kuralı takılıyor ve HttpPost ve HttpGet silme yöntemleri için aynı adı kullanın.

Departman zaten silinmişse `AnyAsync` yöntem false döndürür ve uygulama yalnızca dizin yöntemine döner.

Bir eşzamanlılık hatası yakalanmışsa kod silme onayı sayfası görüntüler ve bunu belirten bir bayrak eşzamanlılık hata iletisi görüntülenmelidir sağlar.

### <a name="update-the-delete-view"></a>Delete Görünümü güncelleştirme

İçinde *Views/Departments/Delete.cshtml*, iskele kurulan kodu bir hata iletisi alan ve gizli alanları DepartmentID ve RowVersion özellikleri ekleyen aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-html[](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

Bu, aşağıdaki değişiklikleri yapar:

* Bir hata iletisi arasında ekler `h2` ve `h3` başlıkları.

* FullName FirstMidName değiştirir **yönetici** alan.

* RowVersion alanını kaldırır.

* İçin gizli bir alan ekler `RowVersion` özelliği.

Uygulamayı çalıştırın ve Departmanlar dizin sayfasına gidin. Sağ tıklayın **Sil** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**, birinci sekmede ardından **Düzenle** İngilizce departmanı için köprü.

İlk penceresinde değerlerden birini değiştirin ve tıklayın **Kaydet**:

![Bölüm silme önce değişiminin düzenleme sayfası](concurrency/_static/edit-after-change-for-delete.png)

İkinci sekmesini tıklatın **Sil**. Eşzamanlılık hata iletisini görüntüleyin ve departman değerlerini şu anda veritabanında nedir ile yenilenir.

![Eşzamanlılık hatası ile bölüm silme onay sayfası](concurrency/_static/delete-error.png)

Tıklarsanız **Sil** yeniden departman silindiğini gösterir dizin sayfasına yönlendirilirsiniz.

## <a name="update-details-and-create-views"></a>Güncelleştirme ayrıntıları ve görünümler oluşturma

İsteğe bağlı olarak, iskele kurulan kodu ayrıntıları temizlemek ve görünümler oluşturun.

Değiştirin *Views/Departments/Details.cshtml* RowVersion sütunu silin ve yöneticinin tam adı.

[!code-html[](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

Değiştirin *Views/Departments/Create.cshtml* Seç seçeneğini aşağı açılan listesine eklenecek.

[!code-html[](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a>Özet

Bu, eşzamanlılık çakışmalarını işleme giriş tamamlar. EF Core eşzamanlılık nasıl ele alınacağını hakkında daha fazla bilgi için bkz. [eşzamanlılık çakışmalarını](/ef/core/saving/concurrency). Sonraki öğreticiye Eğitmen ve Öğrenci varlıklar için tablo başına hiyerarşi devralma uygulanması gösterilmektedir.

::: moniker-end

> [!div class="step-by-step"]
> [Önceki](update-related-data.md)
> [İleri](inheritance.md)
