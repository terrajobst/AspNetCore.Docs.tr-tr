---
title: EF Core - sıralama, filtreleme, sayfalama - 3, 10 ile ASP.NET Core MVC
author: rick-anderson
description: Bu öğreticide, sıralama, filtreleme ve sayfalama ASP.NET Core ve Entity Framework Core kullanan sayfa işlevselliği ekleyeceksiniz.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 1f80faf0e36332c28e8337ddc331cc8b4c4970d7
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38193955"
---
# <a name="aspnet-core-mvc-with-ef-core---sort-filter-paging---3-of-10"></a>EF Core - sıralama, filtreleme, sayfalama - 3, 10 ile ASP.NET Core MVC

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması, Entity Framework Core ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](intro.md).

Önceki öğreticide, bir dizi web sayfaları için Öğrenci varlıkları temel CRUD işlemleri için uygulanır. Bu öğreticide, Öğrenciler dizin sayfasına sıralama, filtreleme ve sayfalama işlevselliğinin ekleyeceksiniz. Basit gruplandırma yapan bir sayfa da oluşturacaksınız.

Aşağıdaki çizim, hazır olduğunuzda sayfanın nasıl görüneceğini gösterir. Sütun başlıkları, kullanıcının sütuna göre sıralamak için tıklayabileceği bağlantılar verilmiştir. Bir sütun başlığına tekrar tekrar tıklayarak, artan veya azalan sıralama düzeni arasında geçiş yapar.

![Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Öğrenciler dizin sayfasına Sütun sıralama bağlantılar ekleme

Öğrenci dizin sayfasına sıralama eklemek için değiştireceksiniz `Index` Öğrenciler denetleyicinin yöntemi ve kodu Öğrenci dizin görünümüne ekleyin.

### <a name="add-sorting-functionality-to-the-index-method"></a>İşlevleri sıralama için dizin yöntemi ekleme

İçinde *StudentsController.cs*, değiştirin `Index` yöntemini aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

Bu kod alır bir `sortOrder` URL'ye sorgu dizesi parametresi. Sorgu dizesi değerini eylem yönteminin bir parametresi olarak ASP.NET Core MVC tarafından sağlanır. Parametre "Name" veya "Tarih", ardından isteğe bağlı olarak bir alt çizgi ve azalan düzende belirtmek için "desc" dizesi bir dize olur. Varsayılan sıralama artan düzendedir.

Dizin Sayfası istendi, ilk kez hiçbir sorgu dizesi yoktur. Öğrenciler artan düzende başarısızlık durumunda tarafından belirlenen varsayılan olan soyadına göre görüntülenen `switch` deyimi. Kullanıcı uygun sütun başlığına köprü tıkladığında `sortOrder` değeri, sorgu dizesinde sağlanır.

İki `ViewData` öğelerini (NameSortParm ve DateSortParm) sütun başlığı köprüler uygun sorgu dize değerleri ile yapılandırmak için Görünüm tarafından kullanılır.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

Üçlü deyimleri şunlardır. Olmadığını birincinin belirtir `sortOrder` parametresi null veya boş NameSortParm "name_desc için" olarak ayarlanmalıdır; Aksi takdirde, boş bir dize olarak ayarlanmalıdır. Bu iki deyimden sütun başlığı köprüler şu şekilde ayarlayın görünümün etkinleştir:

|  Geçerli bir sıralama düzeni  | Son adı köprü | Tarih köprü |
|:--------------------:|:-------------------:|:--------------:|
| Adı artan en son  | descending          | ascending      |
| Son azalan düzende ad | ascending           | ascending      |
| Artan tarihi       | ascending           | descending     |
| Azalan düzende tarihi      | ascending           | ascending      |

Yöntemi, LINQ to Entities göre sıralamak için sütun belirtmek için kullanır. Kod oluşturur bir `IQueryable` switch deyiminden önce değişkeni değiştirir, bu, switch ifadesi ve çağrıları `ToListAsync` sonrasına `switch` deyimi. Ne zaman oluşturma ve değiştirme `IQueryable` değişkenleri, sorgu veritabanına gönderilir. Sorgu dönüştürmeniz kadar yürütülen değil `IQueryable` nesne gibi bir yöntem çağırarak koleksiyonuna `ToListAsync`. Bu nedenle, bu kod, kadar yürütülmez tek bir sorgu sonuçları `return View` deyimi.

Bu kod, çok sayıda sütun ayrıntılı alabilir. [Bu serideki son öğretici](advanced.md#dynamic-linq) adını geçirmenize olanak tanıyan kodunun nasıl yazılacağını gösterir `OrderBy` bir dize değişkeni sütunu.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Öğrenci dizini görünümü için sütun başlığını köprüler ekleme

Değiştirin *Views/Students/Index.cshtml*, sütun başlığını köprüler eklemek için aşağıdaki kod ile. Değiştirilen satırlar vurgulanır.

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

Bilgiler, bu kodu kullanan `ViewData` uygun sorgu köprülerle ayarlamak için özellikler dize değerleri.

Uygulamayı çalıştırın, seçin **Öğrenciler** sekmesine ve tıklayın **Soyadı** ve **kayıt tarihi** sütun başlıkları, sıralama doğrulamak için çalışır.

![Öğrenciler dizin sayfası adı sırayla](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Bir arama kutusu Öğrenciler dizin sayfasına ekleme

Öğrenciler dizin sayfasına filtre eklemek için görünümü için metin kutusu ve bir Gönder düğmesi ekleyin ve karşılık gelen değişiklik `Index` yöntemi. Metin kutusunda, ad ve soyadı alanları aramak için bir dize girin olanak tanır.

### <a name="add-filtering-functionality-to-the-index-method"></a>Filtreleme işlevselliği dizin yöntemine ekleyin.

İçinde *StudentsController.cs*, değiştirin `Index` yöntemini aşağıdaki kodla (değişiklikleri vurgulanır).

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Eklediğiniz bir `searchString` parametresi `Index` yöntemi. Dizin görünümüne ekleyeceksiniz bir metin kutusundan arama dizesi değeri alındı. LINQ deyime where ekledik, ad ve Soyadı arama dizesini içeren Öğrenciler seçer yan tümcesi. Where ekler deyimi yan tümcesi yalnızca aramak için bir değer ise yürütülür.

> [!NOTE]
> Burada, aradığınız `Where` metodunda bir `IQueryable` nesne ve filtre, sunucuda işlenir. Bazı senaryolarda, çağırma `Where` yöntemi olarak bir genişletme yöntemi bir bellek içi koleksiyonu. (Örneğin, başvuru değiştirmeniz varsayalım `_context.Students` bir EF, bu nedenle, bunun yerine `DbSet` döndüren bir depo yöntemin başvurduğu bir `IEnumerable` koleksiyonu.) Sonuç, normalde aynı kalır ancak bazı durumlarda farklı olabilir.
>
>Örneğin, .NET Framework uygulamasını `Contains` yöntemi varsayılan olarak büyük küçük harfe duyarlı bir karşılaştırma yapar, ancak SQL Server'da bu SQL Server örneğinin harmanlama ayarı tarafından belirlenir. Bu ayar için büyük küçük harf duyarsız varsayar. Çağırabilir `ToUpper` test açıkça duyarlı hale getirmek için yöntemi: *nerede (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*. Emin döndüren bir depoyu daha sonra kullanmak için kodu değiştirirseniz sonuçları aynı kalmasını bir `IEnumerable` koleksiyonu yerine bir `IQueryable` nesne. (Çağırdığınızda `Contains` metodunda bir `IEnumerable` koleksiyonu, .NET Framework uygulaması alın; çağırdığınızda, üzerinde bir `IQueryable` nesne veritabanı sağlayıcısı uygulamasını edinin.) Ancak, bu çözüm için bir performans cezası yoktur. `ToUpper` Kod TSQL seçin deyiminin WHERE yan tümcesinde bir işlev yerleştirecek. Bu, iyileştirici dizin kullanmasını önler. SQL çoğunlukla olarak büyük küçük harf duyarsız yüklü olduğu düşünüldüğünde, kaçınmanız en iyisidir `ToUpper` büyük/küçük harfe veri deposuna aktarana kadar kodu.

### <a name="add-a-search-box-to-the-student-index-view"></a>Bir arama kutusu Öğrenci dizini görünümü ekleme

İçinde *Views/Student/Index.cshtml*, resim yazısı, bir metin kutusu oluşturmak için bir etiket hemen açılış tablo önce vurgulanmış kodu ekleyin ve bir **arama** düğmesi.

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

Bu kod `<form>` [etiket Yardımcısı](xref:mvc/views/tag-helpers/intro) arama metin kutusu ve düğme. Varsayılan olarak, `<form>` etiketi Yardımcısı parametreleri HTTP ileti gövdesini ve URL'yi içinde değil sorgu dizeleri geçirilir, yani bir GÖNDERİ ile form verileri gönderir. HTTP GET belirttiğinizde, form verilerini URL'ye sorgu dizeleri kullanıcıların yer işareti URL'si sağlayan geçirilir. Kullanmanız gereken W3C yönergeleri önerilen eylemi bir güncelleştirmeye neden olmaz, alın.

Uygulamayı çalıştırın, seçin **Öğrenciler** sekmesinde, bir arama dizesi girin ve filtreleme çalışıp çalışmadığını doğrulamak için Ara'yı tıklatın.

![Filtreleme ile Öğrenciler dizin sayfası](sort-filter-page/_static/filtering.png)

URL arama dizesi içerdiğine dikkat edin.

```html
http://localhost:5813/Students?SearchString=an
```

Bu sayfaya yer işareti, yer işareti kullandığınızda filtrelenmiş liste elde edersiniz. Ekleme `method="get"` için `form` etikettir neyin neden oluşturulacak sorgu dizesi.

Bu aşamada bir sütun başlığını sıralama bağlantı tıklarsanız girdiğiniz filtre değeri kaybedeceksiniz **arama** kutusu. Sonraki bölümde, çözeceksiniz.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Disk belleği işlevsellik Öğrenciler dizin sayfasına ekleme

Disk belleği Öğrenciler dizin sayfasına eklemek için oluşturacağınız bir `PaginatedList` kullanan sınıf `Skip` ve `Take` yerine her zaman tablonun tüm satırlarının alınırken sunucu üzerindeki verileri filtrelemek için deyimleri. Sonra ek değişiklik yapacaksınız `Index` yöntemi ve disk belleği düğmeleri ekleme `Index` görünümü. Aşağıdaki çizim, disk belleği düğme gösterilmektedir.

![Disk belleği bağlantılarla sayfası Öğrenciler dizin](sort-filter-page/_static/paging.png)

Proje klasöründe oluşturma `PaginatedList.cs`ve sonra şablon kodunu aşağıdaki kodla değiştirin.

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

`CreateAsync` Yöntemi bu kodda sayfa boyutu ve sayfa numarası alır ve uygun geçerlidir `Skip` ve `Take` deyimleriyle `IQueryable`. Zaman `ToListAsync` üzerinde çağrılır `IQueryable`, yalnızca istenen sayfayı içeren bir liste döndürür. Özellikleri `HasPreviousPage` ve `HasNextPage` etkinleştirmek veya devre dışı bırakmak için kullanılabilir **önceki** ve **sonraki** düğmeleri sayfalama.

A `CreateAsync` yöntemi oluşturmak için bir oluşturucu yerine kullanılır `PaginatedList<T>` oluşturucular zaman uyumsuz kod çalıştırılamaz nesne.

## <a name="add-paging-functionality-to-the-index-method"></a>Sayfalama işlevselliğinin dizin yöntemine ekleyin.

İçinde *StudentsController.cs*, değiştirin `Index` yöntemini aşağıdaki kod ile.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

Bu kod, yöntem imzası için sayfa sayı parametresi, geçerli bir sıralama sipariş parametresi ve geçerli bir filtre parametresi ekler.

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

Kullanıcı bir disk belleği veya bağlantı sıralama taşınmadığından seçeneğine tıkladıysanız, tüm parametreleri null veya ilk kez sayfası görüntülenir.  Disk belleği bağlantıya tıkladıysanız, sayfa değişkenini görüntülemek için sayfa numarasını içerir.

`ViewData` CurrentSort adlı bir öğe sıralama sırasında disk belleği aynı tutulabilmesi için disk belleği bağlantıları bu eklenmelidir çünkü geçerli sıralama düzenini görünümüyle sağlar.

`ViewData` GeçerliFiltre adlı öğesi, geçerli bir filtre dizesi görünümüyle sağlar. Bu değer sırasında disk belleği filtre ayarlarını sürdürmek için disk belleği bağlantıları eklenmelidir ve sayfası görüntülendiğinde, metin kutusuna geri yüklenmelidir.

Arama dizesi sırasında disk belleği değiştirilirse, yeni filtre görüntülemek için farklı veri kaybına neden çünkü sayfa 1'e sıfırlanması gereken. Arama dizesi, metin kutusuna girilen değer ve Gönder düğmesine basıldığında değiştirilir. Bu durumda, `searchString` parametresi null değil.

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

Sonunda `Index` yöntemi `PaginatedList.CreateAsync` yöntemi disk belleği destekleyen bir koleksiyon türü Öğrenci tek sayfalık Öğrenci sorgu dönüştürür. Öğrenciler, tek sayfalık ardından görünüme iletilir.

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

`PaginatedList.CreateAsync` Yöntemi, bir sayfa numarasını alır. İki soru işareti null birleşim işleci temsil eder. Boş değer atanabilir bir tür için varsayılan bir değer null birleşim işleci tanımlar; ifade `(page ?? 1)` anlamına gelir dönüş değerini `page` bir değere sahip veya 1 döndürür, `page` null.

## <a name="add-paging-links-to-the-student-index-view"></a>Öğrenci dizini görünümü için disk belleği bağlantılar ekleme

İçinde *Views/Students/Index.cshtml*, mevcut kodu şu kodla değiştirin. Değişiklikler vurgulanır.

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

`@model` Sayfanın üstündeki deyimi belirtir görünüme artık alır bir `PaginatedList<T>` yerine Nesne bir `List<T>` nesne.

Sütun üst bilgisi bağlantıları, kullanıcının içinde filtre sonuçlarını sıralayabilirsiniz, böylece geçerli arama dizesinin denetleyiciye geçirilecek sorgu dizesi kullanın:

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

Disk belleği düğmeler tarafından etiket Yardımcıları görüntülenir:

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

Uygulamayı çalıştırın ve öğrenciler sayfasına gidin.

![Disk belleği bağlantılarla sayfası Öğrenciler dizin](sort-filter-page/_static/paging.png)

Disk belleği works emin olmak için farklı sıralamalar sayfalama bağlantıları tıklatın. Ardından bir arama dizesi girin ve yeniden disk belleği de doğru sıralama ve filtreleme ile çalıştığını doğrulamak için disk belleği'ni deneyin.

## <a name="create-an-about-page-that-shows-student-statistics"></a>Öğrenci istatistiklerini gösteren bir hakkında sayfası oluşturma

Contoso University Web sitesinin için **hakkında** sayfasında, her kayıt tarihi için kaç Öğrenciler kaydedilmiş görüntüleyeceksiniz. Bu gruplar üzerinde gruplandırma ve basit hesaplama gerektirir. Bunu yapmak için aşağıdakileri:

* Görünüme iletmek için gereken verileri için bir görünüm modeli sınıfı oluşturun.

* Giriş denetleyicisine hakkında yöntemi değiştirin.

* Hakkında görünümü değiştirin.

### <a name="create-the-view-model"></a>Görünüm modeli oluşturun

Oluşturma bir *SchoolViewModels* klasöründe *modelleri* klasör.

Yeni klasörde bir sınıf dosyası ekleyin *EnrollmentDateGroup.cs* ve şablon kodunu aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>Giriş denetleyicisini değiştirmek

İçinde *HomeController.cs*, aşağıdaki using deyimlerini dosyanın üstüne:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

Hemen sınıfı için açılış kaşlı ayracından sonra veritabanı bağlamı için bir sınıf değişkeni ekleyin ve ASP.NET Core DI bağlam örneğini alın:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

Değiştirin `About` yöntemini aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

LINQ deyiminden Öğrenci varlıkları kayıt tarihe göre gruplar, her grupta varlık sayısını hesaplar ve sonuçları bir koleksiyonda depolar `EnrollmentDateGroup` model nesneleri görüntüleyin.
> [!NOTE]
> Entity Framework Core 1.0 sürümünde, sonuç kümesinin tamamı istemciye döndürülür ve gruplandırma istemcide gerçekleştirilir. Bazı senaryolarda bu performans sorunlarını oluşturabilirsiniz. Veri hacimleri üretim performansını test edin ve gerekirse, ham SQL sunucu üzerinde gruplandırma yapmak için kullanın emin olun. Ham SQL kullanma hakkında daha fazla bilgi için bkz: [bu serinin son öğreticide](advanced.md).

### <a name="modify-the-about-view"></a>Değiştirme görünümü hakkında

Değiştirin *Views/Home/About.cshtml* dosyasındaki kodu aşağıdaki kodla:

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

Uygulamayı çalıştırın ve hakkında sayfasına gidin. Bir tablodaki her kayıt tarihi için Öğrenci sayısı görüntülenir.

![Sayfa hakkında](sort-filter-page/_static/about.png)

## <a name="summary"></a>Özet

Bu öğreticide, sıralama, filtreleme, sayfalama ve gruplandırma gerçekleştirmeyi öğrendiniz. Sonraki öğreticide, veri modeli değişikliklerini geçişleri kullanarak nasıl ele alınacağını öğreneceksiniz.

::: moniker-end

> [!div class="step-by-step"]
> [Önceki](crud.md)
> [İleri](migrations.md)
