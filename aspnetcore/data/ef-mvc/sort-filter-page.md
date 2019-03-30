---
title: 'Öğretici: Sıralama, filtreleme ve sayfalama - EF çekirdekli ASP.NET MVC Ekle'
description: Bu öğreticide, Öğrenciler dizin sayfasına sıralama, filtreleme ve sayfalama işlevselliğinin ekleyeceksiniz. Basit gruplandırma yapan bir sayfa da oluşturacaksınız.
author: rick-anderson
ms.author: tdykstra
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: dff5a5b1ba3c8ed07ccc8d134f8cfeb25b9f6689
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58751044"
---
# <a name="tutorial-add-sorting-filtering-and-paging---aspnet-mvc-with-ef-core"></a>Öğretici: Sıralama, filtreleme ve sayfalama - EF çekirdekli ASP.NET MVC Ekle

Önceki öğreticide, bir dizi web sayfaları için Öğrenci varlıkları temel CRUD işlemleri için uygulanır. Bu öğreticide, Öğrenciler dizin sayfasına sıralama, filtreleme ve sayfalama işlevselliğinin ekleyeceksiniz. Basit gruplandırma yapan bir sayfa da oluşturacaksınız.

Aşağıdaki çizim, hazır olduğunuzda sayfanın nasıl görüneceğini gösterir. Sütun başlıkları, kullanıcının sütuna göre sıralamak için tıklayabileceği bağlantılar verilmiştir. Bir sütun başlığına tekrar tekrar tıklayarak, artan veya azalan sıralama düzeni arasında geçiş yapar.

![Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Sütun sıralama bağlantılar ekleme
> * Bir arama kutusu ekleme
> * Disk belleği Öğrenciler dizine Ekle
> * Disk belleği dizin yöntemine ekleyin.
> * Disk belleği bağlantılar ekleme
> * Hakkında sayfası oluşturma

## <a name="prerequisites"></a>Önkoşullar

* [CRUD işlevselliği uygulama](crud.md)

## <a name="add-column-sort-links"></a>Sütun sıralama bağlantılar ekleme

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

## <a name="add-a-search-box"></a>Bir arama kutusu ekleme

Öğrenciler dizin sayfasına filtre eklemek için görünümü için metin kutusu ve bir Gönder düğmesi ekleyin ve karşılık gelen değişiklik `Index` yöntemi. Metin kutusunda, ad ve soyadı alanları aramak için bir dize girin olanak tanır.

### <a name="add-filtering-functionality-to-the-index-method"></a>Filtreleme işlevselliği dizin yöntemine ekleyin.

İçinde *StudentsController.cs*, değiştirin `Index` yöntemini aşağıdaki kodla (değişiklikleri vurgulanır).

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Eklediğiniz bir `searchString` parametresi `Index` yöntemi. Dizin görünümüne ekleyeceksiniz bir metin kutusundan arama dizesi değeri alındı. LINQ deyime where ekledik, ad ve Soyadı arama dizesini içeren Öğrenciler seçer yan tümcesi. Where ekler deyimi yan tümcesi yalnızca aramak için bir değer ise yürütülür.

> [!NOTE]
> Burada, aradığınız `Where` metodunda bir `IQueryable` nesne ve filtre, sunucuda işlenir. Bazı senaryolarda, çağırma `Where` yöntemi olarak bir genişletme yöntemi bir bellek içi koleksiyonu. (Örneğin, başvuru değiştirmeniz varsayalım `_context.Students` bir EF, bu nedenle, bunun yerine `DbSet` döndüren bir depo yöntemin başvurduğu bir `IEnumerable` koleksiyonu.) Sonuç, normalde aynı kalır ancak bazı durumlarda farklı olabilir.
>
>Örneğin, .NET Framework uygulamasını `Contains` yöntemi varsayılan olarak büyük küçük harfe duyarlı bir karşılaştırma yapar, ancak SQL Server'da bu SQL Server örneğinin harmanlama ayarı tarafından belirlenir. Bu ayar için büyük küçük harf duyarsız varsayar. Çağırabilir `ToUpper` yöntemini test açıkça büyük küçük harf duyarsız sağlamak için:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*. Emin döndüren bir depoyu daha sonra kullanmak için kodu değiştirirseniz sonuçları aynı kalmasını bir `IEnumerable` koleksiyonu yerine bir `IQueryable` nesne. (Çağırdığınızda `Contains` metodunda bir `IEnumerable` koleksiyonu, .NET Framework uygulaması alın; çağırdığınızda, üzerinde bir `IQueryable` nesne veritabanı sağlayıcısı uygulamasını edinin.) Ancak, bu çözüm için bir performans cezası yoktur. `ToUpper` Kod TSQL seçin deyiminin WHERE yan tümcesinde bir işlev yerleştirecek. Bu, iyileştirici dizin kullanmasını önler. SQL çoğunlukla olarak büyük küçük harf duyarsız yüklü olduğu düşünüldüğünde, kaçınmanız en iyisidir `ToUpper` büyük/küçük harfe veri deposuna aktarana kadar kodu.

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

## <a name="add-paging-to-students-index"></a>Disk belleği Öğrenciler dizine Ekle

Disk belleği Öğrenciler dizin sayfasına eklemek için oluşturacağınız bir `PaginatedList` kullanan sınıf `Skip` ve `Take` yerine her zaman tablonun tüm satırlarının alınırken sunucu üzerindeki verileri filtrelemek için deyimleri. Sonra ek değişiklik yapacaksınız `Index` yöntemi ve disk belleği düğmeleri ekleme `Index` görünümü. Aşağıdaki çizim, disk belleği düğme gösterilmektedir.

![disk belleği bağlantılarla Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

Proje klasöründe oluşturma `PaginatedList.cs`ve sonra şablon kodunu aşağıdaki kodla değiştirin.

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

`CreateAsync` Yöntemi bu kodda sayfa boyutu ve sayfa numarası alır ve uygun geçerlidir `Skip` ve `Take` deyimleriyle `IQueryable`. Zaman `ToListAsync` üzerinde çağrılır `IQueryable`, yalnızca istenen sayfayı içeren bir liste döndürür. Özellikleri `HasPreviousPage` ve `HasNextPage` etkinleştirmek veya devre dışı bırakmak için kullanılabilir **önceki** ve **sonraki** düğmeleri sayfalama.

A `CreateAsync` yöntemi oluşturmak için bir oluşturucu yerine kullanılır `PaginatedList<T>` oluşturucular zaman uyumsuz kod çalıştırılamaz nesne.

## <a name="add-paging-to-index-method"></a>Disk belleği dizin yöntemine ekleyin.

İçinde *StudentsController.cs*, değiştirin `Index` yöntemini aşağıdaki kod ile.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

Bu kod, yöntem imzası için sayfa sayı parametresi, geçerli bir sıralama sipariş parametresi ve geçerli bir filtre parametresi ekler.

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? pageNumber)
```

Kullanıcı bir disk belleği veya bağlantı sıralama taşınmadığından seçeneğine tıkladıysanız, tüm parametreleri null veya ilk kez sayfası görüntülenir.  Disk belleği bağlantıya tıkladıysanız, sayfa değişkenini görüntülemek için sayfa numarasını içerir.

`ViewData` CurrentSort adlı bir öğe sıralama sırasında disk belleği aynı tutulabilmesi için disk belleği bağlantıları bu eklenmelidir çünkü geçerli sıralama düzenini görünümüyle sağlar.

`ViewData` GeçerliFiltre adlı öğesi, geçerli bir filtre dizesi görünümüyle sağlar. Bu değer sırasında disk belleği filtre ayarlarını sürdürmek için disk belleği bağlantıları eklenmelidir ve sayfası görüntülendiğinde, metin kutusuna geri yüklenmelidir.

Arama dizesi sırasında disk belleği değiştirilirse, yeni filtre görüntülemek için farklı veri kaybına neden çünkü sayfa 1'e sıfırlanması gereken. Arama dizesi, metin kutusuna girilen değer ve Gönder düğmesine basıldığında değiştirilir. Bu durumda, `searchString` parametresi null değil.

```csharp
if (searchString != null)
{
    pageNumber = 1;
}
else
{
    searchString = currentFilter;
}
```

Sonunda `Index` yöntemi `PaginatedList.CreateAsync` yöntemi disk belleği destekleyen bir koleksiyon türü Öğrenci tek sayfalık Öğrenci sorgu dönüştürür. Öğrenciler, tek sayfalık ardından görünüme iletilir.

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), pageNumber ?? 1, pageSize));
```

`PaginatedList.CreateAsync` Yöntemi, bir sayfa numarasını alır. İki soru işareti null birleşim işleci temsil eder. Boş değer atanabilir bir tür için varsayılan bir değer null birleşim işleci tanımlar; ifade `(pageNumber ?? 1)` anlamına gelir dönüş değerini `pageNumber` bir değere sahip veya 1 döndürür, `pageNumber` null.

## <a name="add-paging-links"></a>Disk belleği bağlantılar ekleme

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
   asp-route-pageNumber="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

Uygulamayı çalıştırın ve öğrenciler sayfasına gidin.

![disk belleği bağlantılarla Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

Disk belleği works emin olmak için farklı sıralamalar sayfalama bağlantıları tıklatın. Ardından bir arama dizesi girin ve yeniden disk belleği de doğru sıralama ve filtreleme ile çalıştığını doğrulamak için disk belleği'ni deneyin.

## <a name="create-an-about-page"></a>Hakkında sayfası oluşturma

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

Ekleme bir `About` yöntemini aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

LINQ deyiminden Öğrenci varlıkları kayıt tarihe göre gruplar, her grupta varlık sayısını hesaplar ve sonuçları bir koleksiyonda depolar `EnrollmentDateGroup` model nesneleri görüntüleyin.
> [!NOTE]
> Entity Framework Core 1.0 sürümünde, sonuç kümesinin tamamı istemciye döndürülür ve gruplandırma istemcide gerçekleştirilir. Bazı senaryolarda bu performans sorunlarını oluşturabilirsiniz. Veri hacimleri üretim performansını test edin ve gerekirse, ham SQL sunucu üzerinde gruplandırma yapmak için kullanın emin olun. Ham SQL kullanma hakkında daha fazla bilgi için bkz: [bu serinin son öğreticide](advanced.md).

### <a name="modify-the-about-view"></a>Değiştirme görünümü hakkında

Ekleme bir *Views/Home/About.cshtml* dosyasındaki kodu aşağıdaki kodla:

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

Uygulamayı çalıştırın ve hakkında sayfasına gidin. Bir tablodaki her kayıt tarihi için Öğrenci sayısı görüntülenir.

## <a name="get-the-code"></a>Kodu alma

[İndirme veya tamamlanmış uygulamanın görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Eklenen sütun sıralama bağlantıları
> * Bir arama kutusu eklendi
> * Öğrenciler dizine eklenen disk belleği
> * Dizin yönteme eklenen disk belleği
> * Disk belleği bağlantılar eklendi
> * Hakkında bir sayfa oluşturuldu

Veri modeli değişikliklerini migrations'ı kullanarak işleme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Sonraki: Veri modeli değişikliklerini işlemek](migrations.md)
