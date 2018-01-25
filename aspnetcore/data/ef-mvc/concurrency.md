---
title: "EF çekirdek - eşzamanlılık - 8, 10 ile ASP.NET Core MVC"
author: tdykstra
description: "Bu öğretici, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/concurrency
ms.openlocfilehash: eee84fe0fbec6ed772342d09931986994903906a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a>Eşzamanlılık çakışmalarını - EF çekirdek ASP.NET Core MVC Öğreticisi (8, 10) ile işleme

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).

Önceki eğitimlerine verileri güncelleştirmek öğrendiniz. Bu öğretici, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.

Departman varlıkla çalışan ve eşzamanlılık hataları işlemek web sayfaları oluşturacaksınız. Aşağıdaki çizimler bir eşzamanlılık çakışması olursa, görüntülenen bazı iletiler de dahil olmak üzere düzenleme ve silme sayfalarında gösterilir.

![Departman Düzenle sayfası](concurrency/_static/edit-error.png)

![Bölüm silme sayfası](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a>Eşzamanlılık çakışmaları

Bir eşzamanlılık çakışması düzenlemek için bir kullanıcı bir varlığın verileri görüntüler ve ilk kullanıcının değişiklik veritabanına yazılmadan önce başka bir kullanıcı aynı varlığın veri güncelleştirmeleri oluşur. Bu gibi çakışmaları algılama etkinleştirmezseniz aktaranın veritabanını güncelleştiren son diğer kullanıcının değişiklikleri geçersiz kılar. Birçok uygulamada, bu riski kabul edilebilir: birkaç kullanıcı ya da birkaç güncelleştirmeleri varsa veya bazı değişiklikleri üzerine yazılmışsa eşzamanlılık programlama maliyetini ağır avantajı gerçekten önemli değildir. Bu durumda, eşzamanlılık çakışmalarına için uygulamayı yapılandırma gerekmez.

### <a name="pessimistic-concurrency-locking"></a>Eşzamanlılık (kilitleme)

Uygulamanızı eşzamanlılık senaryolarda yanlışlıkla veri kaybını önlemek gerekiyorsa, bunu yapmanın bir yolu veritabanı kilitleri kullanmaktır. Bu eşzamanlılık çağrılır. Örneğin, bir veritabanından bir satır okumadan önce için bir kilit isteği salt okunur veya güncelleştirme erişim. Güncelleştirme erişim için bir satır kilitlemek, başka hiçbir kullanıcı için ya da satır kilitlemek için izin verilir salt okunur veya değiştirilen sürecinde olan verilerin bir kopyasını elde edecekleri çünkü erişimi güncelleştirin. Salt okunur erişim için bir satır kilitlemek, diğerleri de salt okunur erişim için kullanılabilir ancak güncelleştirme kilitleyebilirsiniz.

Kilitleri yönetmek dezavantajları vardır. Programa karmaşık olabilir. Önemli Veritabanı Yönetimi kaynaklarını gerektirir ve bu kullanıcı bir uygulamanın sayısı olarak performans sorunlarına neden olabilir artırır. Bu nedenlerle, tüm veritabanı yönetim sistemleri eşzamanlılık destekler. Entity Framework Çekirdek, yerleşik desteği sağlar ve bu öğreticiyi uyguladıktan nasıl göstermez.

### <a name="optimistic-concurrency"></a>İyimser eşzamanlılık

Eşzamanlılık iyimser eşzamanlılık alternatifidir. İyimser eşzamanlılık gerçekleşecek şekilde eşzamanlılık çakışması izin vererek ve içermiyorlarsa uygun şekilde tepki anlamına gelir. Örneğin, Jane departmanı düzenleme sayfasını ziyaret ve İngilizce departmanı bütçe tutarını 350,000.00 0,00 için değiştirir.

![Bütçe 0 olarak değiştirme](concurrency/_static/change-budget.png)

Jane tıklar önce **kaydetmek**, John aynı sayfasını ziyaret ve başlangıç tarihi alanı 9/1/2007'den 9/1/2013'e değiştirir.

![Başlangıç tarihi 2013 için değiştirme](concurrency/_static/change-date.png)

Jane tıklar **kaydetmek** ilk ve her tarayıcı dizin sayfasına geri döndüğünde değiştirmek görür.

![Bütçe sıfır olarak değiştirildi](concurrency/_static/budget-zero.png)

John tıklattığında **kaydetmek** bir düzenleme sayfasında $350,000.00 bütçe görüntülenmeye devam eder. Ne olacağını eşzamanlılık çakışmaları nasıl işleneceğini tarafından belirlenir.

Bazı seçenekleri şunlardır:

* Bir kullanıcı değiştirdi hangi özelliğinin izlemek ve yalnızca karşılık gelen sütunlara veritabanındaki güncelleştirin.

     Farklı özellikler iki kullanıcı tarafından güncelleştirildi olduğundan örnek senaryoda, hiçbir veri kaybı, olacaktır. Jane'nın ve Can'ın değişiklikleri--1/9/2013 başlangıç tarihi ve bütçe sıfır Doları birisi İngilizce departmanı gözatar sonraki açışınızda görürler. Güncelleştirme bu yöntem veri kaybına sebep çakışmaları sayısını azaltır, ancak bir varlık aynı özelliğe rakip bir değişiklik yaptıysanız, veri kaybını önlemek olamaz. Entity Framework bu şekilde çalışıp çalışmadığını güncelleştirme kodunuzu nasıl uygulayacağınıza bağlıdır. Bir varlık için tüm özgün özellik değerlerini yanı sıra yeni değerleri izlemek için büyük miktarlarda durumu bakımını gerektirebilir bir web uygulamasında pratik değildir, çünkü genellikle. Büyük miktarlarda durumu koruma uygulama performansı etkileyebilir sunucu kaynakları gerekir ya da web sayfasındaki kendisini (örneğin, gizli alanlar) dahil gerekir çünkü ya da bir tanımlama bilgisinde.

* Jane'nın değişiklik üzerine Can'ın değişiklik izin verebilirsiniz.

     Sonraki birisi İngilizce departmanı gözatar, 1/9/2013 görürler ve geri yüklenen $350,000.00 değeri. Bu adlı bir *istemci WINS* veya *WINS'de son* senaryo. (İstemci tüm değerleri veri deposunda nedir üzerinden önceliklidir.) Bu bölüm için giriş hiçbir eşzamanlılık işleme için kodlama yapmazsanız belirtildiği gibi bu otomatik olarak gerçekleşir.

* Veritabanında güncelleştirilen Can'ın değişiklik engelleyebilir.

     Genellikle, bir hata iletisi görüntüler ona verilerin geçerli durumunu gösterir ve ona kendisinin bunları olmak istiyorsa, bu değişiklikleri uygulamak izin. Bu adlı bir *deposu WINS* senaryo. (Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.) Bu öğreticide deposu WINS senaryo uygulama. Bu yöntem, hiçbir değişiklik olanlar için uyarı bir kullanıcı olmaksızın üzerine yazılır sağlar.

### <a name="detecting-concurrency-conflicts"></a>Eşzamanlılık çakışmalarını algılama

İşleyerek çakışmalarını çözme `DbConcurrencyException` Entity Framework oluşturur özel durumları. Bu özel durumlar oluşturma zamanı bilmek için Entity Framework çakışmaları algılayabilir olması gerekir. Bu nedenle, veritabanı ve veri modelinin uygun şekilde yapılandırmanız gerekir. Çakışma algılamasını etkinleştirmek için bazı seçenekler aşağıdakileri içerir:

* Veritabanı tablosunda bir satırı değiştiğinde belirlemek için kullanılan bir izleme sütun içerir. Daha sonra bu sütun nerede dahil etmek için Entity Framework yapılandırabilirsiniz SQL güncelleştirme veya silme komutlarını yan tümcesi.

     İzleme sütunun veri türünü genellikle `rowversion`. `rowversion` Satır güncelleştirilir her zaman artar bir sıralı sayı bir değerdir. Bir güncelleştirme veya silme komutunda Where yan tümcesi izleme sütunu (özgün satır sürümü) özgün değeri içerir. Güncelleştirilen satır başka bir kullanıcı tarafından değeri değiştirildiğinde, `rowversion` Update veya Delete deyiminde Where nedeniyle güncelleştirilecek satır bulmanız sütun özgün değerinden farklı yan tümcesi. (Diğer bir deyişle, etkilenen satırların sayısı sıfır olduğunda) güncelleştirme veya silme tarafından hiçbir satırın güncelleştirilmediği Entity Framework bulur komutu tıklattığınızda, bir eşzamanlılık çakışması yorumlar.

* Where tablodaki her sütunun özgün değerler eklemek için Entity Framework yapılandırma Update ve Delete komutları yan tümcesi.

     İlk seçenek satırın ilk okunuşundan bu yana, sıradaki herhangi bir şey değiştiyse, olduğu gibi Where yan tümcesi güncelleştirmek için bir satır dönüş kalmaz, Entity Framework bir eşzamanlılık çakışması yorumlar. Çok sayıda sütuna sahip veritabanı tabloları için bu yaklaşım çok büyük Where sonuçlanabilir yan tümceleri ve büyük miktarlarda durumu bakımını gerektirebilir. Daha önce belirtildiği gibi büyük miktarlarda durumu koruma uygulama performansını etkileyebilir. Bu nedenle bu yaklaşım genellikle önerilmez ve Bu öğreticide kullanılan yöntem değildir.

     Eşzamanlılık bu yaklaşımı uygulamak istiyorsanız, tüm birincil anahtar özellikleri'nde ekleyerek eşzamanlılık için izlemek istediğiniz varlık işaretleyin zorunda `ConcurrencyCheck` onlara özniteliği. Bu değişiklik, tüm sütunlar SQL Where yan tümcesinde güncelleştirme ve silme deyimleri dahil etmek Entity Framework sağlar.

Bu öğreticinin geri kalanında, ekleyeceksiniz bir `rowversion` özelliği departmanı varlığa izleme, bir denetleyici ve görünümler oluşturma ve her şeyin doğru şekilde çalıştığını doğrulamak için test.

## <a name="add-a-tracking-property-to-the-department-entity"></a>Departman varlığa izleme özellik ekleme

İçinde *Models/Department.cs*, RowVersion adlı bir izleme özelliği ekleyin:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

`Timestamp` Özniteliği belirtir. Bu sütun eklenecek Where yan tümcesi Update ve Delete komutların veritabanına gönderilen. Öznitelik adı verilen `Timestamp` bir SQL SQL Server'ın önceki sürümlerinde kullanılan çünkü `timestamp` önce SQL veri türü `rowversion` yerine. .NET türü için `rowversion` bir bayt dizisi.

Fluent API kullanmayı tercih ederseniz kullanabilirsiniz `IsConcurrencyToken` yöntemi (içinde *Data/SchoolContext.cs*) aşağıdaki örnekte gösterildiği gibi izleme özelliği belirtmek için:

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

Başka bir geçiş gerçekleştirmeniz gereken şekilde bir özelliği ekleyerek veritabanı modeli değişti.

Değişikliklerinizi kaydetmek ve projeyi oluşturun ve ardından komut penceresinde aşağıdaki komutları girin:

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a>Departmanlar denetleyicisi ve görünümler oluşturma

Öğrenciler, kurslar ve Eğitmen için daha önce yaptığınız gibi bir Departmanlar denetleyici ve görünüm iskelesi.

![İskele bölüm](concurrency/_static/add-departments-controller.png)

İçinde *DepartmentsController.cs* dosya, departman yönetici açılan listeleri Eğitmen tam adı yerine yalnızca son adını içerecek şekilde "FirstMidName" dört tüm oluşumlarını "FullName" değiştirin.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a>Departmanlar dizini görünümünü güncelleştirme

Yapı iskelesi altyapısı RowVersion sütun dizini görünümde oluşturulmuş, ancak bu alan görüntülenen döndürmemelidir.

Kodla *Views/Departments/Index.cshtml* aşağıdaki kod ile.

[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

Bu "Departman" başlığı değişikliklerini RowVersion sütunu siler ve yöneticisinin adı yerine tam adını gösterir.

## <a name="update-the-edit-methods-in-the-departments-controller"></a>Güncelleştirme Departmanlar denetleyicideki düzenleme yöntemleri

Her iki HttpGet içinde `Edit` yöntemi ve `Details` yöntemi Ekle `AsNoTracking`. HttpGet içinde `Edit` yöntemi, istekli yükleme için yönetici ekleyin.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

HttpPost için var olan kodu `Edit` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

Kod bölümünün güncelleştirilmesi okumaya çalışırken tarafından başlar. Varsa `SingleOrDefaultAsync` yöntemi null değerini döndürür, departman başka bir kullanıcı tarafından silindi. Bu durumda kod düzenleme sayfasını bir hata iletisi ile görünürler bir bölüm varlık oluşturmak üzere gönderilen form değerleri kullanır. Alternatif olarak, departman alanları yeniden görüntüleme olmadan yalnızca bir hata iletisi görüntülerseniz departmanı varlık yeniden oluşturmak zorunda olmayacaktır.

Özgün görünümü depolar `RowVersion` değere gizli bir alan ve bu yöntem alan değeri `rowVersion` parametresi. Çağırmadan önce `SaveChanges`, özgün put zorunda `RowVersion` özellik değeri `OriginalValues` varlık koleksiyonu.

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

Entity Framework SQL güncelleştirme komut oluşturduğunda, bu komut için özgün sahip bir satır benzeyen bir WHERE yan tümcesi içerecektir sonra `RowVersion` değeri. Hiçbir satır güncelleştirme komutu tarafından etkilenen varsa (özgün hiçbir satır sahip `RowVersion` değeri), Entity Framework oluşturur bir `DbUpdateConcurrencyException` özel durum.

Başka bir özel durum için catch bloğu kodda güncelleştirilmiş değerleri sahip etkilenen departman varlığı alır `Entries` özel durum nesnesi özelliği.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

`Entries` Koleksiyonu tek olacaktır `EntityEntry` nesnesi.  Kullanıcı tarafından girilen yeni ve geçerli veritabanı değerlerini almak için bu nesneyi kullanabilirsiniz.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

Kod Düzenle iletişim kutusunda girilen kullanıcı sayfasında veritabanı değerleri farklı olan her bir sütun için bir özel hata iletisi ekler (yalnızca bir alan burada gösterilmiştir okumanızdır).

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

Son olarak, kod ayarlar `RowVersion` değerini `departmentToUpdate` yeni değere veritabanından alınır. Bu yeni `RowVersion` değeri sayfa yeniden düzenleme ve sonraki kullanıcı çalıştırdığında gizli alanında depolanacağı **kaydetmek**, düzenleme sayfasını yeniden yakalanan bu yana, eşzamanlılık hataları.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

`ModelState.Remove` Deyimi, çünkü gereklidir `ModelState` eski sahip `RowVersion` değeri. Görünümünde `ModelState` her ikisi de mevcut olduğunda bir alan modeli özellik değerlerini önceliklidir için bir değer.

## <a name="update-the-department-edit-view"></a>Güncelleştirme görünümü departmanı Düzenle

İçinde *Views/Departments/Edit.cshtml*, aşağıdaki değişiklikleri yapın:

* Kaydetmek için gizli bir alan eklemek `RowVersion` hemen gizli alanı için aşağıdaki özellik değeri `DepartmentID` özelliği.

* Bir "Yönetici seçin" seçeneği, aşağı açılan listesine ekleyin.

[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a>Düzenleme sayfasını test eşzamanlılık çakışıyor

Uygulamayı çalıştırın ve Departmanlar dizin sayfasına gidin. Sağ **Düzenle** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**, ardından **Düzenle** İngilizce departmanı için köprü. İki tarayıcı sekmeleri artık aynı bilgileri görüntüler.

İlk tarayıcı sekmesinde alanında değiştirip'ı **kaydetmek**.

![Departman düzenleme değişiklikten sonra sayfa 1](concurrency/_static/edit-after-change-1.png)

Tarayıcı değiştirilen değerle dizin sayfası gösterilir.

İkinci bir tarayıcı sekmesinde alanında değiştirin.

![Departman düzenleme değişiklikten sonra sayfa 2](concurrency/_static/edit-after-change-2.png)

**Kaydet**'e tıklayın. Bir hata iletisi görüntülenir:

![Departman Düzenle sayfası hata iletisi](concurrency/_static/edit-error.png)

Tıklatın **kaydetmek** yeniden. İkinci bir tarayıcı sekmesinde girdiğiniz değer kaydedilir. Dizin Sayfası göründüğünde, kaydedilen değerler görürsünüz.

## <a name="update-the-delete-page"></a>Güncelleştirme Sil sayfası

Delete sayfa için Entity Framework birisi başka departman benzer bir şekilde düzenleyerek nedeni eşzamanlılık çakışması algılar. Zaman HttpGet `Delete` yöntemi onay görünümü görüntüler, özgün görünümü içerir `RowVersion` gizli bir alan değeri. Değer sonra HttpPost için kullanılabilir olduğunu `Delete` kullanıcı silme doğruladığında çağrılan yöntem. Entity Framework SQL DELETE komutu oluşturduğunda, özgün WHERE yan tümcesi içeren `RowVersion` değeri. Sıfır satır komutu sonuçlarında (satır silme onayı sayfası görüntülendikten sonra değiştirildiği anlamına gelir) etkilenen, bir eşzamanlılık özel durum oluşur ve HttpGet `Delete` yöntemi görüntülemek için true olarak hata bayrak kümesi ile çağrılır onay sayfasında bir hata iletisi ile. Bu durumda, hata iletisi görüntülenir şekilde satır başka bir kullanıcı tarafından silindiği için sıfır satır etkilendi mümkündür.

### <a name="update-the-delete-methods-in-the-departments-controller"></a>Departmanlar denetleyicideki Sil yöntemlerini güncelleştirme

İçinde *DepartmentsController.cs*, HttpGet Değiştir `Delete` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

Yöntemi, bir eşzamanlılık hatası sonra sayfanın görünürler olup olmadığını belirten isteğe bağlı bir parametre kabul eder. Bu bayrağı true olarak ayarlandığında ve artık departman mevcut değilse, başka bir kullanıcı tarafından silindi. Bu durumda, kod dizin sayfasına yönlendirir.  Bu bayrağı true olarak ayarlandığında ve departman mevcut değilse, başka bir kullanıcı tarafından değiştirildi. Bu durumda, görünümü kullanarak kodu bir hata iletisi gönderir `ViewData`.  

HttpPost kodla `Delete` yöntemi (adlı `DeleteConfirmed`) aşağıdaki kod ile:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

Yalnızca değiştirilen kurulmuş kodda, bu yöntem yalnızca bir kayıt kimliği kabul:


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

Model bağlayıcı tarafından oluşturulan bir bölüm varlık örneği için bu parametreyi değiştirdiniz. Bu kayıt anahtarı yanı sıra RowVersion özellik değerine EF erişim sağlar.

```csharp
public async Task<IActionResult> Delete(Department department)
```

Ayrıca eylem yönteminin adı değiştirilmiştir `DeleteConfirmed` için `Delete`. İskele kurulmuş kod kullanılan adı `DeleteConfirmed` HttpPost yöntemi benzersiz bir imza vermek için. (CLR farklı yöntem parametreleri sağlamak için aşırı yüklenmiş yöntemler gerektirir.) İmzaları benzersiz, MVC kuralı ile takılıyor ve HttpPost ve HttpGet silme yöntemleri için aynı adı kullanın.

Departman zaten silinmişse `AnyAsync` yöntemi false döndürür ve uygulamanın yalnızca geri dizin yönteme girer.

Bir eşzamanlılık hatası yakalanmışsa kodu silme onayı sayfası görüntüler ve bunu belirten bir bayrak bir eşzamanlılık hata iletisi görüntülenmelidir sağlar.

### <a name="update-the-delete-view"></a>Delete görünümünü güncelleştirme

İçinde *Views/Departments/Delete.cshtml*, kurulmuş kodu bir hata iletisini alan ve Gizli alanlar DepartmentID ve RowVersion özellikleri için ekleyen aşağıdaki kod ile değiştirin. Değişiklikler vurgulanır.

[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

Aşağıdaki değişiklikleri yapar:

* Arasında bir hata iletisi ekler `h2` ve `h3` başlıkları.

* FullName içinde FirstMidName değiştirir **yönetici** alan.

* RowVersion alan kaldırır.

* İçin gizli bir alan ekler `RowVersion` özelliği.

Uygulamayı çalıştırın ve Departmanlar dizin sayfasına gidin. Sağ **silmek** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**, ardından ilk sekmesini tıklatın **Düzenle** İngilizce departmanı için köprü.

İlk penceresinde değerlerden birini değiştirmek ve tıklatın **kaydetmek**:

![Delete önce değişiklikten sonra departman Düzenle sayfası](concurrency/_static/edit-after-change-for-delete.png)

İkinci sekmesini tıklatın **silmek**. Eşzamanlılık hata iletisine bakın ve departman değerleri şu anda veritabanında nedir ile yenilenir.

![Bölüm silme onayı sayfası eşzamanlılık hatası](concurrency/_static/delete-error.png)

Tıklatırsanız **silmek** yeniden departman silinip silinmediğini gösterir dizin sayfasına yönlendirilirsiniz.

## <a name="update-details-and-create-views"></a>Güncelleştirme ayrıntıları ve görünümler oluşturma

İsteğe bağlı olarak kurulmuş kodu ayrıntıları temizlemek ve görünümler oluşturabilirsiniz.

Kodla *Views/Departments/Details.cshtml* RowVersion sütun silip yönetici tam adını gösterir.

[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

Kodla *Views/Departments/Create.cshtml* Seç seçeneği aşağı açılan listeye eklemek için.

[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a>Özet

Bu, eşzamanlılık çakışmalarını işleme giriş tamamlar. EF çekirdek eşzamanlılık nasıl ele alınacağını hakkında daha fazla bilgi için bkz: [eşzamanlılık çakışması](https://docs.microsoft.com/ef/core/saving/concurrency). Sonraki öğretici nasıl Eğitmen ve Öğrenci varlıklar için tablo başına hiyerarşisi devralma uygulandığını gösterir.

>[!div class="step-by-step"]
[Önceki](update-related-data.md)
[sonraki](inheritance.md)  
