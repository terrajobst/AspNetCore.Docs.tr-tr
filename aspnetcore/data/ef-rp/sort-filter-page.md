---
title: Razor sayfalarıyla EF çekirdek ASP.NET Core - sıralama, filtre, disk belleği - 8'in 3
author: rick-anderson
description: Bu öğreticide, sıralama, filtreleme ve ASP.NET Core ve Entity Framework Çekirdek kullanarak sayfa için işlevsellik disk belleği eklemeniz.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 5eb099ff630a01b55ac29e3ef30f6177409351af
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033270"
---
[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

# <a name="razor-pages-with-ef-core-in-aspnet-core---sort-filter-paging---3-of-8"></a>Razor sayfalarıyla EF çekirdek ASP.NET Core - sıralama, filtre, disk belleği - 8'in 3

Tarafından [zel Dykstra](https://github.com/tdykstra), [Rick Anderson](https://twitter.com/RickAndMSFT), ve [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

Bu öğretici, sıralama, filtreleme, gruplama ve disk belleği, işlevselliği eklenir.

Aşağıdaki çizimde bir tamamlandı sayfası gösterilir. Sütun başlıklarını sütunu sıralamak için tıklanabilir bağlantılardır. Bir sütun başlığına sürekli olarak artan veya azalan sıralama düzeni arasında geçiş yapar.

![Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

Olamaz çözmek sorunlarla karşılaşırsanız, indirme [tamamlanan uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

## <a name="add-sorting-to-the-index-page"></a>Dizin sayfasına sıralama Ekle

Dizelere ekleme *Students/Index.cshtml.cs* `PageModel` sıralama parametreleri içermesi için:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]

Güncelleştirme *Students/Index.cshtml.cs* `OnGetAsync` aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

Önceki kod alan bir `sortOrder` URL'deki sorgu dizesi parametresi. (Sorgu dizesi dahil) URL tarafından oluşturulan [yer işareti etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
)

`sortOrder` Parametredir "Name" veya "Date." `sortOrder` "Azalan belirtmek için tarafından _desc" parametresinin ardından isteğe bağlı. Varsayılan sıralama düzeni artan.

Ne zaman dizin sayfası istenen gelen **Öğrenciler** bağlantı, hiçbir sorgu dizesi. Öğrenciler son ada göre artan sırada görüntülenir. Son ada göre artan varsayılandır (başarısızlığı harf) içinde `switch` deyimi. Kullanıcı bir uygun sütun başlığını bağlantı tıkladığında `sortOrder` değeri, sorgu dizesi değerini sağlanır.

`NameSort` ve `DateSort` Razor sayfa tarafından uygun sorgu dizesi değerlerini sütun başlığını köprüler yapılandırmak için kullanılır:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

C# koşullu aşağıdaki kodu içeren [?: işleci](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/conditional-operator):

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

İlk satırı belirtir `sortOrder` null veya boş, `NameSort` "name_desc" ayarlayın Varsa `sortOrder` olan **değil** null veya boş, `NameSort` boş bir dize olarak ayarlayın.

`?: operator` Olarak da bilinen Üçlü işlecidir.

Bu iki ifade sütun başlığını köprüler şu şekilde ayarlamak sayfanın etkinleştirin:

| Geçerli bir sıralama düzeni | Son adı köprü | Tarih köprü |
|:--------------------:|:-------------------:|:--------------:|
| Ad artan en son | descending        | ascending      |
| Ad azalan en son | ascending           | ascending      |
| Artan tarihi       | ascending           | descending     |
| Azalan tarihi      | ascending           | ascending      |

Yöntemi, LINQ to Entities göre sıralamak için sütun belirlemek için kullanır. Kod başlatır bir `IQueryable<Student>` switch deyimi önce ve SWITCH deyiminde değiştirir:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-999)]

 Zaman bir`IQueryable` oluşturulan veya değiştirilen, hiçbir sorgu veritabanına gönderilir. Sorgu kadar yürütülür değil `IQueryable` nesnesi, bir koleksiyona dönüştürülür. `IQueryable` bir koleksiyon için bir yöntem çağırarak dönüştürülür `ToListAsync`. Bu nedenle, `IQueryable` kod şu deyimi kadar yürütülmedi tek bir sorgu sonuçları:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync` çok sayıda sıralanabilir sütunları ayrıntılı alabilirsiniz.

### <a name="add-column-heading-hyperlinks-to-the-student-index-page"></a>Sütun başlık köprüler Öğrenci dizin sayfasına ekleme

Kodla *Students/Index.cshtml*, aşağıdakilerle vurgulanmış kodu:

[!code-html[](intro/samples/cu21/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

Önceki kod:

* Köprüler ekler `LastName` ve `EnrollmentDate` sütun başlıkları.
* Bilgileri kullanan `NameSort` ve `DateSort` köprüler geçerli sıralama sipariş değerleri ayarlamak için.

Bu sıralama çalıştığını doğrulamak için:

* Uygulamayı çalıştırın ve seçin **Öğrenciler** sekmesi.
* Tıklatın **Soyadı**.
* Tıklatın **kayıt tarihi**.

Daha iyi anlamak kodunu almak için:

* İçinde *Student/Index.cshtml.cs*, bir kesme noktası ayarlayın `switch (sortOrder)`.
* Gözcü için Ekle `NameSort` ve `DateSort`.
* İçinde *Student/Index.cshtml*, bir kesme noktası ayarlayın `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Hata ayıklayıcı adım.

## <a name="add-a-search-box-to-the-students-index-page"></a>Bir arama kutusu Öğrenciler dizin sayfasına ekleme

Öğrenciler dizin sayfasına filtre eklemek için:

* Bir metin kutusu ve bir gönderme düğmesi Razor sayfasına eklenir. Metin kutusuna bir arama dizesi ilk veya son adı sağlar.
* Sayfa modeli metin kutusu değerini kullanacak şekilde güncelleştirilir.

### <a name="add-filtering-functionality-to-the-index-method"></a>Filtreleme işlevselliği dizin yöntemine ekleyin

Güncelleştirme *Students/Index.cshtml.cs* `OnGetAsync` aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Önceki kod:

* Ekler `searchString` parametresi `OnGetAsync` yöntemi. Sonraki bölümde eklenen bir metin kutusundan arama dizesi değeri alındı.
* LINQ ifadesi eklenen bir `Where` yan tümcesi. `Where` Yan tümcesi yalnızca, ad ve Soyadı arama dizesini içeren Öğrenciler seçer. Yalnızca arama için bir değer olduğunda LINQ ifadesi yürütülür.

Not: Yukarıdaki kod çağrıları `Where` yöntemi bir `IQueryable` nesne ve filtre sunucuda işlenir. Bazı senaryolarda, uygulama çağırma `Where` yöntemi bir bellek içi koleksiyonda bir genişletme yöntemi olarak. Örneğin, varsayalım `_context.Students` değişiklikleri EF çekirdek `DbSet` döndüren depo yönteme bir `IEnumerable` koleksiyonu. Sonuç normalde aynı kalır ancak bazı durumlarda farklı olabilir.

Örneğin, .NET Framework uygulamasını `Contains` varsayılan olarak büyük küçük harfe duyarlı karşılaştırma gerçekleştirir. SQL Server'da `Contains` büyük küçük harf duyarlılığı, SQL Server örneği harmanlama ayarı tarafından belirlenir. SQL Server varsayılan olarak çok büyük küçük harfe duyarsızdır. `ToUpper` test açıkça büyük küçük harf duyarsız yapmak için çağrılabilir:

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

Kullanmak için kodu değişirse önceki kod sonuçları duyarlı olduğundan emin olun `IEnumerable`. Zaman `Contains` üzerinde adlı bir `IEnumerable` koleksiyonu, .NET Core uygulama kullanılır. Zaman `Contains` üzerinde adlı bir `IQueryable` nesnesi, veritabanı uygulama kullanılır. Döndüren bir `IEnumerable` bir depodan önemli performans penality sahip olabilir:

1. Tüm satırları DB sunucudan döndürülür.
1. Filtre, uygulamadaki tüm döndürülen satırlar için uygulanır.

Bulunmaktadır performans çağırma `ToUpper`. `ToUpper` Kod TSQL SELECT deyimi WHERE yan tümcesinde bir işlev ekler. Eklenen işlev iyileştirici dizin kullanmalarını engeller. SQL olarak büyük küçük harf duyarsız yüklendi Bunu önlemek en iyi düşünüldüğünde `ToUpper` gerekli olmadığı zaman çağırın.

### <a name="add-a-search-box-to-the-student-index-page"></a>Bir arama kutusu Öğrenci dizin sayfasına ekleme

İçinde *Pages/Students/Index.cshtml*, oluşturmak için aşağıdaki vurgulanmış kodu ekleyin bir **arama** düğmesi ve getirilebilir chrome.

[!code-html[](intro/samples/cu21/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

Önceki kod kullanan `<form>` [yardımcı etiketi](xref:mvc/views/tag-helpers/intro) düğmesi ve arama metin kutusuna eklemek için. Varsayılan olarak, `<form>` etiket Yardımcısı bir POST ile form verileri gönderir. POST ile HTTP ileti gövdesi yer alan ve URL parametreleri geçirilir. HTTP GET kullanıldığında, form verileri sorgu dizeleri URL'de geçirilir. Sorgu dizeleri ile veri geçirme kullanıcıların URL yer işareti olanak tanır. [W3C yönergeleri](https://www.w3.org/2001/tag/doc/whenToUseGet.html) eylemi bir güncelleştirmede elde edemezseniz, GET kullanılması gereken öneririz.

Uygulamayı test etme:

* Seçin **Öğrenciler** sekmesinde ve arama dizesini girin.
* Seçin **arama**.

URL arama dizesi içerdiğine dikkat edin.

```html
http://localhost:5000/Students?SearchString=an
```

Sayfa atanmışsa, yer işareti sayfanın URL'sini içerir ve `SearchString` sorgu dizesi. `method="get"` İçinde `form` etiketi olan neyin neden olduğunu oluşturulacak sorgu dizesi.

Şu anda bir sütun başlığını sıralama bağlantı seçildiğinde, filtre değeri gelen **arama** kutusunu kaybolur. Kayıp filtre değeri, sonraki bölümde sabittir.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Sayfalama işlevselliğinin Öğrenciler dizin sayfasına ekleme

Bu bölümde, bir `PaginatedList` sınıfı, disk belleği desteklemek için oluşturulur. `PaginatedList` Sınıfını kullanan `Skip` ve `Take` tablonun tüm satırlarının almak yerine sunucusundaki verileri filtrelemek için deyimleri. Aşağıdaki çizimde, disk belleği düğmeleri gösterir.

![Disk belleği bağlantılar sayfasıyla Öğrenciler dizin](sort-filter-page/_static/paging.png)

Proje klasöründe oluşturma `PaginatedList.cs` aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/PaginatedList.cs)]

`CreateAsync` Önceki kod yönteminde sayfa boyutu ve sayfa numarasını alır ve uygun geçerlidir `Skip` ve `Take` deyimlerini `IQueryable`. Zaman `ToListAsync` üzerinde adlı `IQueryable`, yalnızca istenen sayfa içeren bir liste döndürür. Özellikler `HasPreviousPage` ve `HasNextPage` etkinleştirmek veya devre dışı bırakmak için kullanılan **önceki** ve **sonraki** düğmeleri disk belleği.

`CreateAsync` Yöntemi oluşturmak için kullanılan `PaginatedList<T>`. Bir oluşturucu oluşturulamıyor `PaginatedList<T>` nesne oluşturucuları zaman uyumsuz kod çalıştırılamaz.

## <a name="add-paging-functionality-to-the-index-method"></a>Sayfalama işlevselliğinin dizin yöntemine ekleyin

İçinde *Students/Index.cshtml.cs*, türü güncelleştirme `Student` gelen `IList<Student>` için `PaginatedList<Student>`:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

Güncelleştirme *Students/Index.cshtml.cs* `OnGetAsync` aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-999)]

Önceki kod geçerli sayfa dizini ekler `sortOrder`ve `currentFilter` yöntemi imzası.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

Tüm parametreleri ne zaman null şunlardır:

* Sayfa çağrılır **Öğrenciler** bağlantı.
* Kullanıcı, bir disk belleği veya bağlantı sıralama tıklattınız kurmadı.

Bir disk belleği bağlantıyı tıklattığında, sayfa dizini değişkeni görüntülemek için sayfa numarasını içerir.

`CurrentSort` Razor sayfasını geçerli sıralama düzeni sağlar. Disk belleği sırasında sıralama düzeni tutmak için disk belleği bağlantıların geçerli sıralama düzenini yeniden eklenmesi gerekir.

`CurrentFilter` Razor sayfasını ile geçerli filtre dizesini sağlar. `CurrentFilter` Değeri:

* Disk belleği sırasında filtre ayarlarını korumak için disk belleği bağlantıları eklenmesi gerekir.
* Sayfası görüntülendiğinde metin kutusuna geri yüklenmelidir.

Arama dizesi çalışırken disk belleği değiştirdiyseniz, sayfa 1 olarak ayarlanır. Sayfa yeni filtre görüntülemek için farklı veri kaybına neden çünkü 1 sıfırlanması gerekir. Arama değeri zaman girilir ve **gönderme** seçilir:

* Arama dizesi değiştirilir.
* `searchString` Parametresi null değil.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

`PaginatedList.CreateAsync` Yöntemi disk belleği destekleyen bir koleksiyon türü öğrencinin tek sayfalık Öğrenci sorgu dönüştürür. Bu sayfada Öğrenciler Razor sayfasına geçirilir.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

İki soru işaretleri `PaginatedList.CreateAsync` temsil eden [null birleşim işlecinin](https://docs.microsoft.com/ dotnet/csharp/language-reference/operators/null-conditional-operator). Null birleşim işleci, null atanabilir bir tür için varsayılan bir değer tanımlar. İfade `(pageIndex ?? 1)` anlamına gelir dönüş değerini `pageIndex` bir değer varsa. Varsa `pageIndex` bir değere sahip değil, 1 döndürür.

## <a name="add-paging-links-to-the-student-razor-page"></a>Disk belleği bağlantılar Öğrenci Razor sayfa ekleyin

Biçimlendirme güncelleştirme *Students/Index.cshtml*. Değişiklikleri vurgulanmıştır:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-999)]

Sütun başlığı bağlantıları, geçerli arama dizesi geçirmek için sorgu dizesini kullanın. `OnGetAsync` yöntemi böylece kullanıcı içinde filtre sonuçlarını sıralama yapabilirsiniz:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=28-31)]

Disk belleği düğmeler etiketi Yardımcılar tarafından görüntülenir:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=72-)]

Uygulamayı çalıştırın ve öğrenciler sayfasına gidin.

* Disk belleği çalıştığından emin olmak için farklı sıralamalar disk belleği bağlantıları tıklatın.
* Disk belleği sıralama ve filtreleme ile düzgün çalıştığını doğrulamak için bir arama dizesi girin ve disk belleği deneyin.

![disk belleği bağlantılarla Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

Daha iyi anlamak kodunu almak için:

* İçinde *Student/Index.cshtml.cs*, bir kesme noktası ayarlayın `switch (sortOrder)`.
* Gözcü için Ekle `NameSort`, `DateSort`, `CurrentSort`, ve `Model.Student.PageIndex`.
* İçinde *Student/Index.cshtml*, bir kesme noktası ayarlayın `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Hata ayıklayıcı adım.

## <a name="update-the-about-page-to-show-student-statistics"></a>Güncelleştirme hakkında sayfanın Öğrenci istatistiklerini göster

Bu adımda, *Pages/About.cshtml* kaç Öğrenciler her kayıt tarihi için kayıtlı olan görüntülemek üzere güncelleştirilir. Güncelleştirme gruplandırma kullanır ve aşağıdaki adımları içerir:

* Tarafından kullanılan veri için bir görünüm modeli oluşturma **hakkında** sayfası.
* Görünüm modeli kullanılacak hakkında sayfasını güncelleştirin.

### <a name="create-the-view-model"></a>Görünüm modeli oluşturma

Oluşturma bir *SchoolViewModels* klasöründe *modelleri* klasör.

İçinde *SchoolViewModels* klasörüne eklemek bir *EnrollmentDateGroup.cs* aşağıdaki kod ile:

[!code-csharp[](intro/samples/cu21/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-page-model"></a>Güncelleştirme hakkında sayfa modeli

Güncelleştirme *Pages/About.cshtml.cs* aşağıdaki kod ile dosya:

[!code-csharp[](intro/samples/cu21/Pages/About.cshtml.cs)]

LINQ ifadesi Öğrenci varlıklar kayıt tarihe göre gruplar, her grup içindeki varlıkların sayısı hesaplar ve sonuçları bir koleksiyondaki depolar `EnrollmentDateGroup` model nesneleri görüntüleyin.

Not: LINQ `group` komutu EF çekirdek tarafından şu anda desteklenmiyor. Önceki kodda tüm Öğrenci kayıtlarını SQL Server'dan döndürülür. `group` Komutu, SQL Server'da Razor sayfalarının uygulamasında uygulanır. EF çekirdek 2.1 Bu LINQ destekleneceğini `group` işleci ve gruplandırma SQL Server üzerinde meydana gelir. Bkz: [Relational: Destek SQL GroupBy() çevirme](https://github.com/aspnet/EntityFrameworkCore/issues/2341). [EF çekirdek 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/roadmap) .NET Core 2.1 ile birlikte kullanıma sunulacaktır. Daha fazla bilgi için bkz: [.NET Core yol haritası](https://github.com/dotnet/core/blob/master/roadmap.md).

### <a name="modify-the-about-razor-page"></a>Değiştirme Razor sayfa hakkında

Kodla *Pages/About.cshtml* aşağıdaki kod ile dosya:

[!code-html[](intro/samples/cu21/Pages/About.cshtml)]

Uygulamayı çalıştırın ve hakkında sayfasına gidin. Öğrenciler için her kayıt tarihi sayısı bir tabloda görüntülenir.

Olamaz çözmek sorunlarla karşılaşırsanız, indirme [Bu aşama için tamamlanan uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

![Sayfa hakkında](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core 2.x kaynak hata ayıklama](https://github.com/aspnet/Docs/issues/4155)

Sonraki öğreticide uygulama, veri modeli güncelleştirmek için geçişleri kullanır.
::: moniker-end

> [!div class="step-by-step"]
> [Önceki](xref:data/ef-rp/crud)
> [sonraki](xref:data/ef-rp/migrations)
