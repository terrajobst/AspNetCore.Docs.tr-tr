---
title: "ASP.NET Core MVC EF çekirdek - sıralama, filtre, disk belleği - 10 3 ile"
author: tdykstra
description: "Bu öğreticide, sıralama, filtreleme ve ASP.NET Core ve Entity Framework Çekirdek kullanarak sayfa için işlevsellik disk belleği eklemeniz."
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 6da2073b18f6fff9738808c84441e59240caefe3
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a>Sıralama, filtreleme, disk belleği ve gruplandırma - EF çekirdek ASP.NET Core MVC Öğreticisi (3 / 10)

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir. Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).

Önceki öğreticide, bir dizi web sayfası Öğrenci varlıklar için temel CRUD işlemleri için uygulanır. Bu öğreticide Öğrenciler dizin sayfasına sıralama, filtreleme ve disk belleği işlevselliği ekleyeceksiniz. Ayrıca, basit gruplandırma yapan bir sayfa oluşturacaksınız.

Aşağıdaki çizimde tamamladığınızda sayfa nasıl görüneceği gösterilmektedir. Sütun başlıkları kullanıcının sütuna göre sıralamak için tıklatabileceği bağlantı bulunmaktadır. Bir sütun başlığına sürekli olarak artan veya azalan sıralama düzeni arasında geçiş yapar.

![Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Öğrenciler dizin sayfasına Sütun sıralama bağlantılar ekleme

Öğrenci dizin sayfasına sıralama eklemek için değiştireceğiz `Index` yöntemi Öğrenciler denetleyicisinin ve kod Öğrenci dizin görünümüne ekleyin.

### <a name="add-sorting-functionality-to-the-index-method"></a>İşlevleri sıralama için dizin yöntemi ekleme

İçinde *StudentsController.cs*, yerine `Index` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

Bu kod alan bir `sortOrder` URL'deki sorgu dizesi parametresi. Sorgu dizesi değerini eylem yönteminin bir parametresi olarak ASP.NET Core MVC tarafından sağlanır. Parametre "Ad" veya "Tarih", ardından isteğe bağlı olarak bir alt çizgi ve azalan belirtmek için "desc" dizesi olan bir dize olur. Varsayılan sıralama düzeni artan.

Dizin sayfası istenen ilk kez hiçbir sorgu dizesi yok. Öğrenciler artan düzende başarısızlık durumunda tarafından belirlenen varsayılan değer olan son adıyla görüntülenir `switch` deyimi. Kullanıcı uygun sütun başlığını köprü tıkladığında `sortOrder` değeri, sorgu dizesinde sağlanır.

İki `ViewData` öğeleri (NameSortParm ve DateSortParm) uygun sorgu dizesi değerlerini sütun başlığını köprüler yapılandırmak için Görünüm tarafından kullanılır.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

Üçlü deyimleri bunlar. Birinci anlama gelir `sortOrder` parametresi null veya boş NameSortParm "name_desc" olarak ayarlanmalıdır; Aksi halde, boş bir dize olarak ayarlanmalıdır. Bu iki ifade sütun başlığını köprüler şu şekilde ayarlamak görünümü etkinleştirin:

|  Geçerli bir sıralama düzeni  | Son adı köprü | Tarih köprü |
|:--------------------:|:-------------------:|:--------------:|
| Ad artan en son  | descending          | ascending      |
| Ad azalan en son | ascending           | ascending      |
| Artan tarihi       | ascending           | descending     |
| Azalan tarihi      | ascending           | ascending      |

Yöntemi, LINQ to Entities göre sıralamak için sütun belirlemek için kullanır. Kod oluşturur bir `IQueryable` değişken switch deyimi önce değiştirir, bunu, switch deyimi ve çağrıları `ToListAsync` sonra yöntemi `switch` deyimi. Ne zaman oluşturma ve değiştirme `IQueryable` değişkenleri, sorgu veritabanına gönderilir. Dönüştürülünceye kadar sorgu yürütülmedi `IQueryable` gibi bir yöntemini çağırarak bir koleksiyon nesnesine `ToListAsync`. Bu nedenle, bu kod kadar yürütülmedi tek bir sorgu sonuçları `return View` deyimi.

Bu kod, çok sayıda sütun ayrıntılı alabilirsiniz. [Bu serideki son öğretici](advanced.md#dynamic-linq) adını geçirmenize olanak verir kodunun nasıl yazılacağını gösterir `OrderBy` bir dize değişkeni sütununda.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Sütun başlık köprüleri için Öğrenci dizini görünümü ekleme

Kodla *Views/Students/Index.cshtml*, sütun başlığını köprüler eklemek için aşağıdaki kod ile. Değiştirilen satırları vurgulanır.

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

Bu kod bilgileri kullanan `ViewData` uygun sorguyla köprüler ayarlamak için özellikler dize değerleri.

Uygulama, belirleyin **Öğrenciler** sekmesine ve tıklayın **Soyadı** ve **kayıt tarihi** Bu sıralama doğrulamak için sütun başlıkları çalışır.

![Ad sırayla Öğrenciler dizin sayfası](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Bir arama kutusu Öğrenciler dizin sayfasına ekleme

Öğrenciler dizin sayfasına filtre eklemek için metin kutusu ve bir gönderme düğmesi görünümüne ekleyin ve karşılık gelen değişiklik `Index` yöntemi. Metin kutusunda, ad ve soyadı alanları aramak için bir dize girin olanak tanır.

### <a name="add-filtering-functionality-to-the-index-method"></a>Filtreleme işlevselliği dizin yöntemine ekleyin

İçinde *StudentsController.cs*, yerine `Index` (değişiklikleri vurgulanır) aşağıdaki kod ile yöntemi.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Eklediğiniz bir `searchString` parametresi `Index` yöntemi. Bir metin kutusundan dizin görünümüne ekleyeceksiniz arama dizesi değeri alındı. LINQ ifadesi bir where eklediğiniz yalnızca, ad ve Soyadı arama dizesini içeren Öğrenciler seçer yan tümcesi. Where ekler deyimi yan tümcesi yalnızca arama için bir değer ise gerçekleştirilir.

> [!NOTE]
> Burada, aradığınız `Where` yöntemi bir `IQueryable` nesne ve filtre sunucuda işlenir. Bazı senaryolarda, çağırma `Where` yöntemi bir bellek içi koleksiyonda bir genişletme yöntemi olarak. (Örneğin, referansını değiştirme varsayalım `_context.Students` bir EF, bu nedenle, bunun yerine `DbSet` döndüren bir depo yöntemi başvurduğu bir `IEnumerable` koleksiyonu.) Sonuç normalde aynı kalır ancak bazı durumlarda farklı olabilir.
>
>Örneğin, .NET Framework uygulamasını `Contains` yöntemi, varsayılan olarak büyük küçük harfe duyarlı karşılaştırma gerçekleştirir, ancak SQL Server'da bu SQL Server örneği harmanlama ayarı tarafından belirlenir. Bu ayar varsayılan olarak çok büyük küçük harfe duyarsızdır. Çağrı `ToUpper` yöntemini test açıkça büyük küçük harf duyarsız sağlamak için: *nerede (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*. Emin kodunu döndüren bir depo daha sonra değiştirirseniz, sonuçları aynı kalmasını bir `IEnumerable` koleksiyon yerine bir `IQueryable` nesnesi. (Çağırdığınızda `Contains` yöntemi bir `IEnumerable` koleksiyonu, .NET Framework uygulamasını alın; çağırdığınızda, üzerinde bir `IQueryable` nesnesi, veritabanı sağlayıcısı uygulaması alın.) Ancak, bulunmaktadır performans Bu çözüm için. `ToUpper` Kod TSQL SELECT deyimi WHERE yan tümcesinde bir işlev yerleştirecek. Bir dizini kullanarak, iyileştirici engeller. SQL çoğunlukla olarak büyük küçük harf duyarsız yüklendi Bunu önlemek en iyi düşünüldüğünde `ToUpper` büyük küçük harfe duyarlı veri deposuna geçirene kadar kod.

### <a name="add-a-search-box-to-the-student-index-view"></a>Bir arama kutusu Öğrenci dizin görünümüne ekleyin

İçinde *Views/Student/Index.cshtml*, hemen açılış tablo önce etiketi resim yazısı, bir metin kutusu oluşturma için vurgulanmış kodu ekleyin ve bir **arama** düğmesi.

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

Bu kodu kullanır `<form>` [yardımcı etiketi](xref:mvc/views/tag-helpers/intro) düğmesi ve arama metin kutusuna eklemek için. Varsayılan olarak, `<form>` etiket Yardımcısı parametreleri HTTP ileti gövdesi yer alan ve URL sorgu dizeleri geçirilir, yani bir POST ile form verileri gönderir. HTTP GET belirttiğinizde, form verilerini URL'de sorgu dizeleri kullanıcıların URL yer işareti sağlayan geçirilir. Kullanmanız gereken W3C yönergeleri önerilen eylemi bir güncelleştirmede sonuçlanmaz zaman alın.

Uygulama, belirleyin **Öğrenciler** sekmesinde, bir arama dizesi girin ve filtreleme çalıştığını doğrulamak için Ara'yı tıklatın.

![Filtreleme ile Öğrenciler dizin sayfası](sort-filter-page/_static/filtering.png)

URL arama dizesi içerdiğine dikkat edin.

```html
http://localhost:5813/Students?SearchString=an
```

Bu sayfaya yer işareti varsa, yer işareti kullandığınızda filtrelenmiş liste elde edersiniz. Ekleme `method="get"` için `form` etiketi olan neyin neden olduğunu oluşturulacak sorgu dizesi.

Bu aşamada, bir sütun başlığını sıralama bağlantısını tıklatırsanız girdiğiniz filtre değeri kaybedersiniz **arama** kutusu. Sonraki bölümde düzeltmesi.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Sayfalama işlevselliğinin Öğrenciler dizin sayfasına ekleme

Disk belleği Öğrenciler dizin sayfasına eklemek için oluşturacağınız bir `PaginatedList` kullanan sınıfı `Skip` ve `Take` her zaman tablonun tüm satırlarının almak yerine sunucusundaki verileri filtrelemek için deyimleri. Ek değişiklikler hale getireceğiz sonra `Index` yöntemi ve disk belleği düğmelere ekleme `Index` görünümü. Aşağıdaki çizimde, disk belleği düğmeleri gösterir.

![disk belleği bağlantılarla Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

Proje klasöründe oluşturma `PaginatedList.cs`ve ardından şablon kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

`CreateAsync` Yöntemi bu kodda sayfa boyutu ve sayfa numarasını alır ve uygun geçerlidir `Skip` ve `Take` deyimlerini `IQueryable`. Zaman `ToListAsync` üzerinde adlı `IQueryable`, yalnızca istenen sayfa içeren bir liste döndürür. Özellikler `HasPreviousPage` ve `HasNextPage` etkinleştirmek veya devre dışı bırakmak için kullanılan **önceki** ve **sonraki** düğmeleri disk belleği.

A `CreateAsync` yöntemi yerine bir oluşturucu oluşturmak için kullanılan `PaginatedList<T>` zaman uyumsuz kod oluşturucuları çalışamadığı nesne.

## <a name="add-paging-functionality-to-the-index-method"></a>Sayfalama işlevselliğinin dizin yöntemine ekleyin

İçinde *StudentsController.cs*, yerine `Index` aşağıdaki kod ile yöntemi.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

Bu kod, yöntem imzası sayfa numarası parametre, geçerli bir sıralama sipariş parametresi ve geçerli bir filtre parametresi ekler.

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

Kullanıcı bir disk belleği veya bağlantı sıralama kurmadı tıkladıysanız, tüm parametreleri null veya ilk kez sayfası görüntülenir.  Disk belleği bağlantıya tıkladıysanız sayfa değişkenini görüntülemek için sayfa numarasını içerir.

`ViewData` CurrentSort adlı öğe bu disk belleği bağlantıları sıralama sırasında disk belleği aynı tutmak için dahil edilmesi için geçerli sıralama düzenini görünümüyle sağlar.

`ViewData` GeçerliFiltre adlı öğe geçerli filtre dizesi görünümüyle sağlar. Bu değer, disk belleği bağlantıları sırasında disk belleği filtre ayarlarını korumak için eklenmelidir ve sayfası görüntülendiğinde, metin kutusuna geri yüklenmelidir.

Arama dizesi sırasında disk belleği değiştirdiyseniz, 1'e sıfırlamak için sayfanın yeni filtre görüntülemek için farklı veri kaybına neden çünkü gerekir. Arama dizesi, metin kutusuna girilen değer ve Gönder düğmesine basıldığında değiştirilir. Bu durumda, `searchString` parametresi null değil.

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

Sonunda `Index` yöntemi, `PaginatedList.CreateAsync` yöntemi disk belleği destekleyen bir koleksiyon türü öğrencinin tek sayfalık Öğrenci sorgu dönüştürür. Bu sayfada Öğrenciler daha sonra görünümüne geçirilir.

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

`PaginatedList.CreateAsync` Yöntemi bir sayfa numarasını alır. İki soru işaretleri null birleşim işlecinin temsil eder. Null birleşim işleci, null atanabilir bir tür için varsayılan bir değer tanımlar; ifade `(page ?? 1)` anlamına gelir dönüş değerini `page` değerine sahip veya 1 döndürür, `page` null.

## <a name="add-paging-links-to-the-student-index-view"></a>Disk belleği bağlantılar için Öğrenci dizini görünümü ekleme

İçinde *Views/Students/Index.cshtml*, var olan kodu aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

`@model` Sayfanın üst kısmındaki deyimi belirtir görünümü şimdi alır bir `PaginatedList<T>` yerine Nesne bir `List<T>` nesnesi.

Sütun başlığı bağlantıları sorgu dizesi kullanıcı içinde filtre sonuçlarını sıralayabilmesi geçerli arama dizesi denetleyiciye geçirmek için kullanın:

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

Disk belleği düğmeler etiketi Yardımcılar tarafından görüntülenir:

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

![disk belleği bağlantılarla Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

Disk belleği çalıştığından emin olmak için farklı sıralamalar disk belleği bağlantıları tıklatın. Ardından bir arama dizesi girin ve yeniden disk belleği de doğru sıralama ve filtreleme ile çalıştığını doğrulamak için disk belleği deneyin.

## <a name="create-an-about-page-that-shows-student-statistics"></a>Öğrenci istatistiklerini gösterir hakkında sayfası oluşturma

Contoso University Web sitesinin için **hakkında** sayfasında, kaç tane Öğrenciler her kayıt tarihi için kayıtlı olan görüntülersiniz. Bu grupları gruplandırma ve basit hesaplamaları gerektirir. Bunu gerçekleştirmek için şunları yapmanız:

* Görünüme iletmek için gereken verileri için bir görünüm model sınıfı oluşturun.

* Giriş Denetleyicisi hakkında yönteminde değiştirin.

* Hakkında görünümü değiştirin.

### <a name="create-the-view-model"></a>Görünüm modeli oluşturma

Oluşturma bir *SchoolViewModels* klasöründe *modelleri* klasör.

Yeni klasör içinde bir sınıf dosyası ekleyin *EnrollmentDateGroup.cs* ve şablon kodu aşağıdaki kodla değiştirin:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>Ev denetleyicisi değiştirme

İçinde *HomeController.cs*, aşağıdaki using deyimlerini dosyanın üst kısmında:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

Veritabanı bağlamı için bir sınıf değişkeni parantezinden sınıfı için hemen sonra ekleyin ve ASP.NET Core dı bağlamının bir örneği elde:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

Değiştir `About` aşağıdaki kod ile yöntemi:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

LINQ ifadesi Öğrenci varlıklar kayıt tarihe göre gruplar, her grup içindeki varlıkların sayısı hesaplar ve sonuçları bir koleksiyondaki depolar `EnrollmentDateGroup` model nesneleri görüntüleyin.
> [!NOTE] 
> Entity Framework Çekirdek 1.0 sürümü, istemciye tüm sonuç kümesi döndürdü ve gruplandırma istemcide yapılır. Bazı senaryolarda bu performans sorunlarını oluşturabilirsiniz. Veri üretim birimleri ile performansı test edin ve gerekirse ham SQL sunucusunda gruplandırma yapmak için kullanın emin olun. Ham SQL kullanma hakkında daha fazla bilgi için bkz: [bu serideki son Öğreticisi](advanced.md).

### <a name="modify-the-about-view"></a>Değiştirme Görünüm hakkında

Kodla *Views/Home/About.cshtml* aşağıdaki kod ile dosya:

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

Uygulamayı çalıştırın ve hakkında sayfasına gidin. Öğrenciler için her kayıt tarihi sayısı bir tabloda görüntülenir.

![Sayfa hakkında](sort-filter-page/_static/about.png)

## <a name="summary"></a>Özet

Bu öğreticide, sıralama, filtreleme, disk belleği ve gruplandırma gerçekleştirmeyi öğrendiniz. Sonraki öğreticide geçişler kullanarak veri modeli değişikliklerini işlemek öğreneceksiniz.

>[!div class="step-by-step"]
[Önceki](crud.md)
[sonraki](migrations.md)  
