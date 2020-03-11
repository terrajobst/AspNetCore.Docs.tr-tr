---
title: 'Öğretici: EF Core sıralama, filtreleme ve sayfalama-ASP.NET MVC ekleme'
description: Bu öğreticide, öğrenciler dizin sayfasına sıralama, filtreleme ve sayfalama işlevselliği ekleyeceksiniz. Ayrıca basit gruplandırma yapan bir sayfa oluşturacaksınız.
author: rick-anderson
ms.author: riande
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 99bf9ed59b47e8fbba838b97c3e032b9808f6a94
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657138"
---
# <a name="tutorial-add-sorting-filtering-and-paging---aspnet-mvc-with-ef-core"></a>Öğretici: EF Core sıralama, filtreleme ve sayfalama-ASP.NET MVC ekleme

Önceki öğreticide, öğrenci varlıkları için temel CRUD işlemleri için bir dizi Web sayfası uyguladık. Bu öğreticide, öğrenciler dizin sayfasına sıralama, filtreleme ve sayfalama işlevselliği ekleyeceksiniz. Ayrıca basit gruplandırma yapan bir sayfa oluşturacaksınız.

Aşağıdaki çizimde, işiniz bittiğinde sayfanın nasıl görüneceği gösterilmektedir. Sütun başlıkları, kullanıcının sütuna göre sıralamak için tıkladığı bağlantılardır. Sütun başlığına tıklanması artan ve azalan sıralama düzeni arasında sürekli olarak geçiş yapar.

![Öğrenciler Dizin sayfası](sort-filter-page/_static/paging.png)

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Sütun sıralama bağlantıları Ekle
> * Arama kutusu ekleme
> * Öğrenciler dizinine sayfalama ekleme
> * Dizin yöntemine sayfalama ekleme
> * Sayfalama bağlantıları Ekle
> * Hakkında bir sayfa oluşturun

## <a name="prerequisites"></a>Önkoşullar

* [CRUD Işlevlerini uygulama](crud.md)

## <a name="add-column-sort-links"></a>Sütun sıralama bağlantıları Ekle

Öğrenci dizini sayfasına sıralama eklemek için, öğrenciler denetleyicisinin `Index` yöntemini değiştirecek ve öğrenci dizini görünümüne kod ekleyeceksiniz.

### <a name="add-sorting-functionality-to-the-index-method"></a>Dizin yöntemine sıralama Işlevi ekleme

*StudentsController.cs*' de `Index` yöntemini aşağıdaki kodla değiştirin:

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

Bu kod, URL 'deki sorgu dizesinden bir `sortOrder` parametresi alır. Sorgu dizesi değeri, ASP.NET Core MVC tarafından eylem yöntemine bir parametre olarak sağlanır. Parametresi, "Name" veya "Date" adlı bir dize, isteğe bağlı olarak bir alt çizgi ve azalan sıralama belirtmek için "desc" dizesi olacaktır. Varsayılan sıralama düzeni artan.

Dizin sayfası ilk kez istendiğinde sorgu dizesi yoktur. Öğrenciler, son ada göre artan sırada görüntülenir, bu, `switch` deyimindeki karşı bir durum tarafından kurulduğu varsayılan değer olan addır. Kullanıcı bir sütun başlığı köprüsüne tıkladığında, sorgu dizesinde uygun `sortOrder` değeri sağlanır.

İki `ViewData` öğesi (Namesortpara ve Datesortpard), sütun başlığı köprülerini uygun sorgu dizesi değerleriyle yapılandırmak için görünüm tarafından kullanılır.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

Bunlar Üçlü ifadelerdir. İlki `sortOrder` parametresi null veya boş ise, Namesortparı 'nin "name_desc" olarak ayarlanması gerektiğini belirtir; Aksi takdirde, boş bir dizeye ayarlanmalıdır. Bu iki deyim, görünümün sütun başlığı köprülerini şu şekilde ayarlamaya olanak tanır:

|  Geçerli sıralama düzeni  | Son ad Köprüsü | Tarih Köprüsü |
|:--------------------:|:-------------------:|:--------------:|
| Artan son ad  | descending          | ascending      |
| Azalan son ad | ascending           | ascending      |
| Artan Tarih       | ascending           | descending     |
| Azalan Tarih      | ascending           | ascending      |

Yöntemi, sıralama yapılacak sütunu belirtmek için LINQ to Entities kullanır. Kod, Switch ifadesinden önce bir `IQueryable` değişkeni oluşturur, Switch ifadesinde değiştirir ve `switch` deyimden sonra `ToListAsync` yöntemini çağırır. `IQueryable` değişkenleri oluşturup değiştirirken veritabanına hiçbir sorgu gönderilmez. Sorgu, `ToListAsync`gibi bir yöntemi çağırarak `IQueryable` nesnesini bir koleksiyona dönüştürene kadar yürütülmez. Bu nedenle, bu kod `return View` ifadeye kadar Yürütülmeyen tek bir sorgu ile sonuçlanır.

Bu kod çok sayıda sütunla ayrıntı alabilir. [Bu serideki son öğretici](advanced.md#dynamic-linq) , bir dize değişkeninde `OrderBy` sütununun adını geçirmenize olanak sağlayan kodun nasıl yazılacağını gösterir.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Öğrenci dizini görünümüne sütun başlığı köprüleri ekleme

*Görünümler/öğrenciler/Index. cshtml*içindeki kodu, sütun başlığı köprüleri eklemek için aşağıdaki kodla değiştirin. Değiştirilen çizgiler vurgulanır.

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

Bu kod, uygun sorgu dizesi değerleriyle köprüler ayarlamak için `ViewData` özelliklerindeki bilgileri kullanır.

Uygulamayı çalıştırın, **öğrenciler** sekmesini seçin ve sıralamanın çalıştığını doğrulamak Için **son ad** ve **kayıt tarihi** sütun başlıklarına tıklayın.

![Ad düzeninde öğrenciler Dizin sayfası](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box"></a>Arama kutusu ekleme

Öğrenciler dizin sayfasına filtre eklemek için, görünüme bir metin kutusu ve Gönder düğmesi ekleyeceksiniz ve `Index` yönteminde ilgili değişiklikleri yapmanız gerekir. Metin kutusu, ilk adı ve soyadı alanlarını aramak için bir dize girmenizi sağlar.

### <a name="add-filtering-functionality-to-the-index-method"></a>Dizin yöntemine filtreleme işlevi ekleme

*StudentsController.cs*' de `Index` yöntemini aşağıdaki kodla değiştirin (değişiklikler vurgulanır).

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

`Index` yöntemine bir `searchString` parametresi eklediniz. Arama dizesi değeri, dizin görünümüne ekleyeceğiniz bir metin kutusundan alınır. Ayrıca, yalnızca adı veya soyadı arama dizesini içeren öğrencileri seçen bir where yan tümcesine LINQ deyimi de eklediniz. WHERE yan tümcesini ekleyen deyimi, yalnızca aranacak bir değer varsa yürütülür.

> [!NOTE]
> Burada `Where` yöntemi bir `IQueryable` nesnesi üzerinde arıyorsanız ve filtrenin sunucuda işlenmesi gerekir. Bazı senaryolarda, bellek içi bir koleksiyonda `Where` yöntemini genişletme yöntemi olarak çağırma olabilirsiniz. (Örneğin, `_context.Students` başvurusunu, bir `DbSet` EF yerine bir `IEnumerable` koleksiyonu döndüren bir depo yöntemine başvurduğu gibi) değiştirdiğinizi varsayın.) Sonuç normalde aynı olur, ancak bazı durumlarda farklı olabilir.
>
>Örneğin, `Contains` yönteminin .NET Framework uygulanması, varsayılan olarak büyük/küçük harfe duyarlı bir karşılaştırma gerçekleştirir, ancak SQL Server ' de SQL Server örneğinin harmanlama ayarı tarafından belirlenir. Bu ayar varsayılan olarak büyük/küçük harfe duyarsız olur. Testi açıkça büyük/küçük harfe duyarsız hale getirmek için `ToUpper` yöntemini çağırabilirsiniz: *burada (s = > s. LastName. ToUpper (). Contains (searchString. ToUpper ())* . Bu, kodu daha sonra bir `IQueryable` nesnesi yerine `IEnumerable` koleksiyonu döndüren bir depoyu kullanacak şekilde değiştirirseniz sonuçların aynı kalmasını sağlar. (Bir `IEnumerable` koleksiyonunda `Contains` yöntemini çağırdığınızda .NET Framework uygulamasını alırsınız; bunu bir `IQueryable` nesnesi üzerinde çağırdığınızda, veritabanı sağlayıcısı uygulamasını alırsınız.) Ancak, bu çözüm için bir performans cezası vardır. `ToUpper` kodu, TSQL SELECT ifadesinin WHERE yan tümcesine bir işlev koyar. Bu, iyileştiricinin bir dizin kullanmasını engelleyecek. SQL 'in çoğu büyük küçük harfe duyarsız olarak yüklendiği için, büyük/küçük harfe duyarlı bir veri deposuna geçiş yapılıncaya kadar `ToUpper` koddan kaçınmak en iyisidir.

### <a name="add-a-search-box-to-the-student-index-view"></a>Öğrenci dizini görünümüne arama kutusu ekleme

*Görünümler/öğrenci/Index. cshtml*'de, bir başlık, metin kutusu ve bir **arama** düğmesi oluşturmak için, açılan tablo etiketinden hemen önce vurgulanan kodu ekleyin.

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

Bu kod, arama metin kutusu ve düğmesini eklemek için `<form>` [Tag yardımcısını](xref:mvc/views/tag-helpers/intro) kullanır. Varsayılan olarak, `<form>` etiketi Yardımcısı, form verilerini bir GÖNDERIYLE gönderir, yani Parametreler, URL 'de sorgu dizeleri olarak değil, HTTP ileti gövdesinde geçirilir. HTTP GET belirttiğinizde, form verileri URL 'ye sorgu dizeleri olarak geçirilir ve bu da kullanıcıların URL 'ye yer işareti eklemesini sağlar. W3C yönergeleri, eylem bir güncelleştirme ile sonuçlanmazsa Al ' ın kullanılması önerilir.

Uygulamayı çalıştırın, **öğrenciler** sekmesini seçin, bir arama dizesi girin ve filtreleme 'nin çalıştığını doğrulamak için ara ' ya tıklayın.

![Bir filtreleme ile öğrenciler Dizin sayfası](sort-filter-page/_static/filtering.png)

URL 'nin arama dizesini içerdiğine dikkat edin.

```html
http://localhost:5813/Students?SearchString=an
```

Bu sayfaya yer işareti eklerseniz, yer işaretini kullandığınızda filtrelenmiş listeyi alırsınız. `form` etiketine `method="get"` eklemek, sorgu dizesinin oluşturulmasına neden olur.

Bu aşamada, bir sütun başlığı sıralama bağlantısını tıklatırsanız, **arama** kutusuna girdiğiniz filtre değerini kaybedersiniz. Sonraki bölümde bunu çözeceksiniz.

## <a name="add-paging-to-students-index"></a>Öğrenciler dizinine sayfalama ekleme

Öğrenciler dizin sayfasına sayfalama eklemek için, `Skip` ve `Take` deyimlerini kullanan bir `PaginatedList` sınıfını, tablodaki verileri her zaman almak yerine, sunucuda filtrelemek için oluşturacaksınız. Daha sonra `Index` yönteminde ek değişiklikler yapar ve `Index` görünümüne sayfalama düğmeleri eklersiniz. Aşağıdaki çizimde sayfalama düğmeleri gösterilmektedir.

![Sayfa bağlantılarıyla öğrenciler Dizin sayfası](sort-filter-page/_static/paging.png)

Proje klasöründe `PaginatedList.cs`oluşturun ve ardından şablon kodunu aşağıdaki kodla değiştirin.

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

Bu koddaki `CreateAsync` yöntemi sayfa boyutunu ve sayfa numarasını alır ve uygun `Skip` ve `Take` deyimlerini `IQueryable`uygular. `IQueryable``ToListAsync` çağrıldığında, yalnızca istenen sayfayı içeren bir liste döndürür. `HasPreviousPage` ve `HasNextPage` özellikleri, **önceki** ve **sonraki** sayfalama düğmelerini etkinleştirmek veya devre dışı bırakmak için kullanılabilir.

Oluşturucular zaman uyumsuz kod çalıştıramadığından `PaginatedList<T>` nesnesini oluşturmak için bir Oluşturucu yerine bir `CreateAsync` yöntemi kullanılır.

## <a name="add-paging-to-index-method"></a>Dizin yöntemine sayfalama ekleme

*StudentsController.cs*' de `Index` yöntemini aşağıdaki kodla değiştirin.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

Bu kod, bir sayfa numarası parametresi, geçerli bir sıralama düzeni parametresi ve Yöntem imzasına geçerli bir filtre parametresi ekler.

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? pageNumber)
```

Sayfa ilk kez görüntülenirken veya Kullanıcı bir sayfalama veya sıralama bağlantısına tıklamamışsa, tüm parametreler null olur.  Bir sayfalama bağlantısına tıklandıysanız, sayfa değişkeni görüntülenecek sayfa numarasını içerir.

CurrentSort adlı `ViewData` öğesi, sayfalama bağlantılarına dahil edilmesi gerektiğinden, bu sıralama düzeni geçerli sıralama düzeni ile birlikte sağlar.

CurrentFilter adlı `ViewData` öğesi, geçerli filtre dizesiyle görünüm sağlar. Bu değer, disk belleği sırasında filtre ayarlarını korumak için disk belleği bağlantılarına dahil olmalıdır ve sayfa yeniden görüntülenirken metin kutusuna geri yüklenmesi gerekir.

Arama dizesi sayfalama sırasında değiştirilmişse, yeni filtre farklı verilerin görüntülenmesini sağladığından sayfanın 1 olarak sıfırlanması gerekir. Metin kutusuna bir değer girildiğinde ve Gönder düğmesine basıldığında arama dizesi değişir. Bu durumda `searchString` parametresi null değildir.

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

`Index` yönteminin sonunda, `PaginatedList.CreateAsync` yöntemi öğrenci sorgusunu, sayfalama destekleyen bir koleksiyon türünde tek bir öğrenciye dönüştürür. Bu tek bir öğrenci sayfası daha sonra görünüme geçirilir.

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), pageNumber ?? 1, pageSize));
```

`PaginatedList.CreateAsync` yöntemi bir sayfa numarası alır. İki soru işareti, null birleşim işlecini temsil eder. Null birleşim işleci, Nullable bir tür için varsayılan değeri tanımlar; `(pageNumber ?? 1)` ifadesi bir değere sahipse `pageNumber` değerini döndürür veya `pageNumber` null ise 1 döndürür.

## <a name="add-paging-links"></a>Sayfalama bağlantıları Ekle

*Görünümler/öğrenciler/Index. cshtml*'de, mevcut kodu aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

Sayfanın üst kısmındaki `@model` ifade, görünümün artık `List<T>` nesnesi yerine `PaginatedList<T>` bir nesne aldığından emin olarak belirtir.

Sütun üst bilgisi bağlantıları, kullanıcının filtre sonuçları içinde sıralama yapabilmesi için geçerli arama dizesini denetleyiciye geçirmek için sorgu dizesini kullanır:

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

Sayfalama düğmeleri etiket yardımcıları tarafından görüntülenir:

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

![Sayfa bağlantılarıyla öğrenciler Dizin sayfası](sort-filter-page/_static/paging.png)

Disk belleğinin çalıştığından emin olmak için farklı sıralama emirlerindeki disk belleği bağlantılarına tıklayın. Daha sonra bir arama dizesi girin ve sayfalama ve filtreleme ile doğru şekilde çalıştığını doğrulamak için sayfalama işlemi yeniden deneyin.

## <a name="create-an-about-page"></a>Hakkında bir sayfa oluşturun

Contoso Üniversitesi web sitesinin **hakkında** sayfasında, her bir kayıt tarihi için kaç öğrenciye kaydolduğunu görüntüleyeceksiniz. Bu, gruplar üzerinde gruplandırma ve basit hesaplamalar gerektirir. Bunu gerçekleştirmek için aşağıdakileri yapmanız gerekir:

* Görünüme geçirmeniz gereken veriler için bir görünüm modeli sınıfı oluşturun.
* Giriş denetleyicisinde hakkında yöntemini oluşturun.
* Hakkında görünümünü oluşturun.

### <a name="create-the-view-model"></a>Görünüm modeli oluşturma

*Modeller* klasöründe bir *SchoolViewModels* klasörü oluşturun.

Yeni klasörde, *EnrollmentDateGroup.cs* bir sınıf dosyası ekleyin ve şablon kodunu şu kodla değiştirin:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>Ana denetleyiciyi değiştirme

*HomeController.cs*' de, aşağıdaki using deyimlerini dosyanın en üstüne ekleyin:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

Sınıf için açma küme ayracından hemen sonra veritabanı bağlamı için bir sınıf değişkeni ekleyin ve ASP.NET Core DI 'den bağlamın bir örneğini alın:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

Aşağıdaki kodla bir `About` yöntemi ekleyin:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

LINQ deyimleri, öğrenci varlıklarını kayıt tarihine göre gruplandırır, her bir gruptaki varlıkların sayısını hesaplar ve sonuçları bir `EnrollmentDateGroup` View model nesneleri koleksiyonunda depolar.

### <a name="create-the-about-view"></a>Hakkında görünüm oluştur

Aşağıdaki kodla bir *Görünümler/Home/about. cshtml* dosyası ekleyin:

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

Uygulamayı çalıştırın ve hakkında sayfasına gidin. Her kayıt tarihi için öğrenci sayısı bir tabloda görüntülenir.

## <a name="get-the-code"></a>Kodu alma

[Tamamlanmış uygulamayı indirin veya görüntüleyin.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Sütun sıralama bağlantıları eklendi
> * Arama kutusu eklendi
> * Öğrenciler dizinine sayfalama eklendi
> * Dizin oluşturma yöntemine sayfalama eklendi
> * Sayfalama bağlantıları eklendi
> * Hakkında bir sayfa oluşturuldu

Geçiş kullanarak veri modeli değişikliklerini nasıl işleyebileceğinizi öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Sonraki: veri modeli değişikliklerini Işle](migrations.md)
