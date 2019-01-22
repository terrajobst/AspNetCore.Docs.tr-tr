---
title: ASP.NET core'da - EF çekirdekli Razor sayfaları sıralama, filtreleme, sayfalama - 8'in 3
author: rick-anderson
description: Bu öğreticide, sıralama, filtreleme ve sayfalama ASP.NET Core ve Entity Framework Core kullanan sayfa işlevselliği ekleyeceksiniz.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 350243fb94b4798293a5a61b580c3b3b4d8c6d4a
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444304"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---sort-filter-paging---3-of-8"></a>ASP.NET core'da - EF çekirdekli Razor sayfaları sıralama, filtreleme, sayfalama - 8'in 3

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Tarafından [Tom Dykstra](https://github.com/tdykstra), [Rick Anderson](https://twitter.com/RickAndMSFT), ve [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

Bu öğretici, sıralama, filtreleme, gruplama ve disk belleği, işlevselliği eklendi.

Aşağıda, tamamlanan bir sayfa görüntülenir. Sütun başlıkları sütunu sıralamak için tıklanabilir bağlantılar verilmiştir. Bir sütun başlığına tekrar tekrar tıklayarak, artan veya azalan sıralama düzeni arasında geçiş yapar.

![Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

Olamaz çözmek sorunlarla karşılaşırsanız, indirme [tamamlanan uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

## <a name="add-sorting-to-the-index-page"></a>Dizin sayfasına sıralama Ekle

Dizelere ekleme *Students/Index.cshtml.cs* `PageModel` sıralama parametrelerini içerecek:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]

Güncelleştirme *Students/Index.cshtml.cs* `OnGetAsync` aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

Yukarıdaki kod alır bir `sortOrder` URL'ye sorgu dizesi parametresi. URL (sorgu dizesi dahil) tarafından oluşturulan [yer işareti etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
)

`sortOrder` Parametresi, "Name" veya "Tarih." `sortOrder` Parametresi isteğe bağlı olarak "azalan belirtmek için _desc" tarafından izlenen. Varsayılan sıralama artan düzendedir.

Ne zaman dizin sayfası istenen gelen **Öğrenciler** bağlantı, sorgu dizesi yok. Öğrenciler soyadına göre artan düzende görüntülenir. Soyadına göre artan düzende, varsayılan (başarısızlık harf) içinde `switch` deyimi. Kullanıcı uygun sütun başlığına bağlantı tıkladığında `sortOrder` değeri, sorgu dizesi değerini sağlanır.

`NameSort` ve `DateSort` Razor sayfası tarafından sütun başlığı köprüler uygun sorgu dize değerleri ile yapılandırmak için kullanılır:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

Aşağıdaki kodu içeren C# koşullu [?: işleci](/dotnet/csharp/language-reference/operators/conditional-operator):

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

İlk satır belirtir `sortOrder` null veya boş `NameSort` "name_desc" ayarlayın Varsa `sortOrder` olduğu **değil** null veya boş `NameSort` boş bir dizeye ayarlayın.

`?: operator` Olarak da bilinen Üçlü işleçtir.

Bu iki deyimden sütun başlığı köprüler şu şekilde ayarlamak sayfayı etkinleştir:

| Geçerli bir sıralama düzeni | Son adı köprü | Tarih köprü |
|:--------------------:|:-------------------:|:--------------:|
| Adı artan en son | descending        | ascending      |
| Son azalan düzende ad | ascending           | ascending      |
| Artan tarihi       | ascending           | descending     |
| Azalan düzende tarihi      | ascending           | ascending      |

Yöntemi, LINQ to Entities göre sıralamak için sütun belirtmek için kullanır. Kod başlatan bir `IQueryable<Student>` switch deyiminden önce ve switch deyimi içinde değiştirir:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-999)]

 Olduğunda bir`IQueryable` oluşturulduğunda veya değiştirildiğinde, hiçbir sorgu veritabanına gönderilir. Sorgu kadar yürütülen değil `IQueryable` nesne bir koleksiyona dönüştürüldü. `IQueryable` bir koleksiyon için bir yöntem çağırarak dönüştürülür `ToListAsync`. Bu nedenle, `IQueryable` kod aşağıdaki deyimi kadar yürütülmez tek bir sorgu sonuçları:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync` çok sayıda sıralanabilir sütunları ayrıntılı alabilir.

### <a name="add-column-heading-hyperlinks-to-the-student-index-page"></a>Öğrenci dizin sayfasına sütun başlığı köprüler ekleme

Değiştirin *Students/Index.cshtml*, aşağıdaki vurgulanmış kodu:

[!code-html[](intro/samples/cu21/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

Yukarıdaki kod:

* Köprüler ekler `LastName` ve `EnrollmentDate` sütun başlıkları.
* Bilgileri kullanan `NameSort` ve `DateSort` köprüler geçerli sıralama sipariş değerleri ayarlamak için.

Bu sıralama çalıştığını doğrulamak için:

* Uygulamayı çalıştırmak ve seçmek **Öğrenciler** sekmesi.
* Tıklayın **Soyadı**.
* Tıklayın **kayıt tarihi**.

Daha iyi anlamak kodunu almak için:

* İçinde *Students/Index.cshtml.cs*, bir kesme noktası ayarlamak `switch (sortOrder)`.
* İçin bir izleme Ekle `NameSort` ve `DateSort`.
* İçinde *Students/Index.cshtml*, bir kesme noktası ayarlamak `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Hata ayıklayıcı ile adım.

## <a name="add-a-search-box-to-the-students-index-page"></a>Bir arama kutusu Öğrenciler dizin sayfasına ekleme

Öğrenciler dizin sayfasına filtre eklemek için:

* Bir metin kutusu ve bir Gönder düğmesi için Razor sayfası eklenir. Metin kutusuna bir arama dizesi ilk veya son adı sağlar.
* Sayfa modeli, metin kutusunun değerini kullanmak için güncelleştirilir.

### <a name="add-filtering-functionality-to-the-index-method"></a>Filtreleme işlevselliği dizin yöntemine ekleyin.

Güncelleştirme *Students/Index.cshtml.cs* `OnGetAsync` aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Yukarıdaki kod:

* Ekler `searchString` parametresi `OnGetAsync` yöntemi. Sonraki bölümde eklenen bir metin kutusundan arama dizesi değeri alındı.
* LINQ deyime eklenen bir `Where` yan tümcesi. `Where` Yan tümcesi yalnızca Öğrenciler, ad ve Soyadı arama dizesini içeren seçer. Aramak için bir değer yoksa, LINQ ifade yürütülür.

Not: Önceki kod çağrıları `Where` metodunda bir `IQueryable` nesne ve filtre sunucuda işlenir. Bazı senaryolarda, uygulama çağırma `Where` yöntemi olarak bir genişletme yöntemi bir bellek içi koleksiyonu. Örneğin, varsayalım `_context.Students` değişiklikleri EF Core `DbSet` döndüren bir depo yönteme bir `IEnumerable` koleksiyonu. Sonuç, normalde aynı kalır ancak bazı durumlarda farklı olabilir.

Örneğin, .NET Framework uygulamasını `Contains` varsayılan olarak büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirir. SQL Server'da `Contains` büyük küçük harf duyarlılığı, SQL Server örneğinin harmanlama ayarı tarafından belirlenir. SQL Server için büyük küçük harf duyarsız olarak varsayılır. `ToUpper` test açıkça duyarlı hale getirmek için çağrılabilir:

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

Kullanmak için kodu değişirse önceki kod sonuçları büyük/küçük harfe olmasını sağlamak `IEnumerable`. Zaman `Contains` üzerinde çağrılır bir `IEnumerable` koleksiyonu, .NET Core uygulaması kullanılır. Zaman `Contains` üzerinde çağrılır bir `IQueryable` nesnesi, veritabanı uygulama kullanılır. Döndüren bir `IEnumerable` depodan önemli performans penality olabilir:

1. DB sunucudan tüm satırlar döndürülür.
1. Filtre uygulama döndürülen tüm satırları uygulanır.

Arama için bir performans cezası yoktur `ToUpper`. `ToUpper` Kod TSQL seçin deyiminin WHERE yan tümcesinde bir işlev ekler. Eklenen işlev, bir dizin kullanarak iyileştirici engeller. SQL olarak büyük küçük harf duyarsız yüklü olduğu düşünüldüğünde, kaçınmanız en iyisidir `ToUpper` gerekli olmadığından çağırın.

### <a name="add-a-search-box-to-the-student-index-page"></a>Bir arama kutusu Öğrenci dizin sayfasına ekleme

İçinde *Pages/Students/Index.cshtml*, oluşturmak için aşağıdaki vurgulanmış kodu ekleyin bir **arama** düğmesi ve çeşitli chrome.

[!code-html[](intro/samples/cu21/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

Önceki kod `<form>` [etiket Yardımcısı](xref:mvc/views/tag-helpers/intro) arama metin kutusu ve düğme. Varsayılan olarak, `<form>` etiketi Yardımcısı form verilerini bir posta ile gönderir. POST ile HTTP ileti gövdesini ve URL'yi parametreleri geçirilir. HTTP GET kullanıldığında, form verileri sorgu dizeleri URL'de geçirilir. Sorgu dizeleri ile veri geçirme kullanıcıların yer işareti URL'si olanak tanır. [W3C yönergeleri](https://www.w3.org/2001/tag/doc/whenToUseGet.html) eylemi bir güncelleştirmeye neden olmaz, GET kullanılması gereken önerilir.

Uygulamayı test edin:

* Seçin **Öğrenciler** sekmesinde ve arama dizesini girin.
* Seçin **arama**.

URL arama dizesi içerdiğine dikkat edin.

```html
http://localhost:5000/Students?SearchString=an
```

Sayfa atanmışsa, yer işareti sayfasının URL'sini içeren ve `SearchString` sorgu dizesi. `method="get"` İçinde `form` etikettir neyin neden oluşturulacak sorgu dizesi.

Şu anda bir sütun başlığını sıralama bağlantı seçildiğinde, filtre değeri **arama** kutusu kaybolur. Sonraki bölümde kayıp filtre değeri sabittir.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Disk belleği işlevsellik Öğrenciler dizin sayfasına ekleme

Bu bölümde, bir `PaginatedList` sınıfı, disk belleği desteklemek için oluşturulur. `PaginatedList` Sınıfının kullandığı `Skip` ve `Take` yerine tablonun tüm satırlarının alınırken sunucu üzerindeki verileri filtrelemek için deyimleri. Aşağıdaki çizim, disk belleği düğme gösterilmektedir.

![disk belleği bağlantılarla Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

Proje klasöründe oluşturma `PaginatedList.cs` aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/PaginatedList.cs)]

`CreateAsync` Yöntemi önceki kodda sayfa boyutu ve sayfa numarası alır ve uygun geçerlidir `Skip` ve `Take` deyimleriyle `IQueryable`. Zaman `ToListAsync` üzerinde çağrılır `IQueryable`, yalnızca istenen sayfayı içeren bir liste döndürür. Özellikleri `HasPreviousPage` ve `HasNextPage` etkinleştirmek veya devre dışı bırakmak için kullanılan **önceki** ve **sonraki** düğmeleri sayfalama.

`CreateAsync` Yöntemi oluşturmak için kullanılan `PaginatedList<T>`. Bir oluşturucu oluşturulamıyor `PaginatedList<T>` nesne Oluşturucu, zaman uyumsuz kod çalıştıramaz.

## <a name="add-paging-functionality-to-the-index-method"></a>Sayfalama işlevselliğinin dizin yöntemine ekleyin.

İçinde *Students/Index.cshtml.cs*, türünü güncelleştirme `Student` gelen `IList<Student>` için `PaginatedList<Student>`:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

Güncelleştirme *Students/Index.cshtml.cs* `OnGetAsync` aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-999)]

Yukarıdaki kod, geçerli sayfa dizini ekler `sortOrder`ve `currentFilter` metodu imzası.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

Tüm parametreler ne zaman null şunlardır:

* Sayfa çağrılır **Öğrenciler** bağlantı.
* Kullanıcı, bir disk belleği veya bağlantı sıralama tıkladı edilmemiş.

Bir disk belleği bağlantısına tıklandığında sayfası dizin değişkeni görüntülemek için sayfa numarasını içerir.

`CurrentSort` Razor sayfası, geçerli sıralama düzeni sağlar. Disk belleği sırasında sıralama düzenini korumak için disk belleği bağlantıları geçerli sıralama düzenini yeniden eklenmesi gerekir.

`CurrentFilter` Razor sayfası ile geçerli filtre dizesi sağlar. `CurrentFilter` Değeri:

* Filtre ayarları sırasında disk belleği sürdürmek için disk belleği bağlantıları yeniden eklenmesi gerekir.
* Sayfası görüntülendiğinde metin kutusuna geri yüklenmelidir.

Arama dizesi çalışırken disk belleği değiştirilirse, sayfa 1 olarak ayarlanır. Sayfayı yeni filtre görüntülemek için farklı veri kaybına neden çünkü 1 sıfırlanması gereken. Bir arama değeri zaman girilir ve **Gönder** seçilir:

* Arama dizesi değiştirilir.
* `searchString` Parametresi null değil.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

`PaginatedList.CreateAsync` Yöntemi disk belleği destekleyen bir koleksiyon türü Öğrenci tek sayfalık Öğrenci sorgu dönüştürür. Öğrenciler, tek sayfalık Razor sayfasına geçirilir.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

İki soru işareti `PaginatedList.CreateAsync` temsil [null birleşim işleci](/dotnet/csharp/language-reference/operators/null-conditional-operator). Null birleşim işleci, null yapılabilir bir tür için varsayılan bir değer tanımlar. İfade `(pageIndex ?? 1)` anlamına gelir dönüş değerini `pageIndex` bir değere sahip olursa. Varsa `pageIndex` bir değere sahip değil, 1 döndürür.

## <a name="add-paging-links-to-the-student-razor-page"></a>Disk belleği bağlantılar Öğrenci Razor sayfası ekleme

Biçimlendirme içinde güncelleştirme *Students/Index.cshtml*. Değişiklikleri vurgulanmıştır:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-999)]

Sütun üst bağlantılar için geçerli arama dizesinin geçirilecek sorgu dizesini kullanın. `OnGetAsync` yöntemi, böylece kullanıcı içinde filtre sonuçlarını sıralayabilirsiniz:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=28-31)]

Disk belleği düğmeler tarafından etiket Yardımcıları görüntülenir:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=72-)]

Uygulamayı çalıştırın ve öğrenciler sayfasına gidin.

* Disk belleği works emin olmak için farklı sıralamalar sayfalama Bağlantıları'ı tıklatın.
* Disk belleği sıralama ve filtreleme ile düzgün çalıştığını doğrulamak için bir arama dizesi girin ve disk belleği'ni deneyin.

![disk belleği bağlantılarla Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

Daha iyi anlamak kodunu almak için:

* İçinde *Students/Index.cshtml.cs*, bir kesme noktası ayarlamak `switch (sortOrder)`.
* İçin bir izleme Ekle `NameSort`, `DateSort`, `CurrentSort`, ve `Model.Student.PageIndex`.
* İçinde *Students/Index.cshtml*, bir kesme noktası ayarlamak `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Hata ayıklayıcı ile adım.

## <a name="update-the-about-page-to-show-student-statistics"></a>Öğrenci istatistikleri gösterilecek hakkında sayfası

Bu adımda, *Pages/About.cshtml* kaç Öğrenciler her kayıt tarihi için kayıtlı olan görüntülemek için güncelleştirilir. Güncelleştirme gruplandırma kullanır ve aşağıdaki adımları içerir:

* Tarafından kullanılan veriler için bir görünüm modeli oluşturun **hakkında** sayfası.
* Görünüm modeli kullanılacak hakkında sayfası güncelleştirin.

### <a name="create-the-view-model"></a>Görünüm modeli oluşturun

Oluşturma bir *SchoolViewModels* klasöründe *modelleri* klasör.

İçinde *SchoolViewModels* klasör ekleme bir *EnrollmentDateGroup.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-page-model"></a>Güncelleştirme hakkında sayfa modeli

ASP.NET Core 2.2 içinde web şablonları hakkında sayfası dahil değildir. ASP.NET Core 2.2 kullanıyorsanız hakkında Razor sayfası oluşturun.

Güncelleştirme *Pages/About.cshtml.cs* dosyasındaki kodu aşağıdaki kodla:

[!code-csharp[](intro/samples/cu21/Pages/About.cshtml.cs)]

LINQ deyiminden Öğrenci varlıkları kayıt tarihe göre gruplar, her grupta varlık sayısını hesaplar ve sonuçları bir koleksiyonda depolar `EnrollmentDateGroup` model nesneleri görüntüleyin.

### <a name="modify-the-about-razor-page"></a>Değiştirme hakkında Razor sayfası

Değiştirin *Pages/About.cshtml* dosyasındaki kodu aşağıdaki kodla:

[!code-html[](intro/samples/cu21/Pages/About.cshtml)]

Uygulamayı çalıştırın ve hakkında sayfasına gidin. Bir tablodaki her kayıt tarihi için Öğrenci sayısı görüntülenir.

Olamaz çözmek sorunlarla karşılaşırsanız, indirme [Bu aşama için tamamlanan uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

![Sayfa hakkında](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core 2.x kaynak hata ayıklama](https://github.com/aspnet/Docs/issues/4155)

Sonraki öğreticide uygulama, veri modeli güncelleştirmek için geçişleri kullanır.

::: moniker-end

> [!div class="step-by-step"]
> [Önceki](xref:data/ef-rp/crud)
> [İleri](xref:data/ef-rp/migrations)
