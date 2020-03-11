---
title: ASP.NET Core sıralama, filtreleme, sayfalama-3/8 ' de EF Core Razor Pages
author: rick-anderson
description: Bu öğreticide, ASP.NET Core ve Entity Framework Core kullanarak bir Razor sayfasına sıralama, filtreleme ve sayfalama işlevselliği ekleyeceksiniz.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 9563f3ef52ce429eb0a58b468acb8e9cd7b276e2
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656466"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---sort-filter-paging---3-of-8"></a>ASP.NET Core sıralama, filtreleme, sayfalama-3/8 ' de EF Core Razor Pages

By [Tom Dykstra](https://github.com/tdykstra), [Rick Anderson](https://twitter.com/RickAndMSFT)ve [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

Bu öğretici, öğrenciler sayfalarına sıralama, filtreleme ve sayfalama işlevselliği ekler.

Aşağıdaki çizimde tamamlanmış bir sayfa gösterilmektedir. Sütun başlıkları sütunu sıralamak için tıklatılabilir bağlantılardır. Artan ve azalan sıralama düzeni arasında geçiş yapmak için bir sütun başlığına tekrar tekrar tıklayın.

![Öğrenciler Dizin sayfası](sort-filter-page/_static/paging30.png)

## <a name="add-sorting"></a>Sıralama Ekle

*Pages/öğrenciler/Index. cshtml. cs* içindeki kodu, sıralama eklemek için aşağıdaki kodla değiştirin.

[!code-csharp[Main](intro/samples/cu30snapshots/3-sorting/Pages/Students/Index1.cshtml.cs?name=snippet_All&highlight=21-24,26,28-52)]

Yukarıdaki kod:

* Sıralama parametrelerini içeren özellikleri ekler.
* `Student` özelliğinin adını `Students`olarak değiştirir.
* `OnGetAsync` yöntemindeki kodu değiştirir.

`OnGetAsync` yöntemi, URL 'deki sorgu dizesinden bir `sortOrder` parametresi alır. URL (sorgu dizesi dahil), [tutturucu etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)tarafından oluşturulur.

`sortOrder` parametresi "ad" veya "Date" dir. `sortOrder` parametresi, isteğe bağlı olarak azalan sıra belirtmek için "_desc" tarafından izlenir. Varsayılan sıralama düzeni artan.

**Öğrenciler** bağlantısından Dizin sayfası istendiğinde sorgu dizesi yoktur. Öğrenciler, son ada göre artan sırada görüntülenir. Son ada göre artan sıralama, `switch` deyimindeki varsayılan (gelen durumdur). Kullanıcı bir sütun başlığı bağlantısına tıkladığında, uygun `sortOrder` değeri sorgu dizesi değerinde sağlanır.

`NameSort` ve `DateSort`, Razor sayfası tarafından, sütun başlığı köprülerini uygun sorgu dizesi değerleriyle yapılandırmak için kullanılır:

[!code-csharp[Main](intro/samples/cu30snapshots/3-sorting/Pages/Students/Index1.cshtml.cs?name=snippet_Ternary)]

Kod, C# koşullu işleci kullanıyor [?:](/dotnet/csharp/language-reference/operators/conditional-operator). `?:` işleci üçlü bir işleçtir (üç işlenen alır). İlk satır `sortOrder` null ya da boş olduğunda `NameSort` "name_desc" olarak ayarlandığını belirtir. `sortOrder` null veya boş **değilse** , `NameSort` boş bir dizeye ayarlanır.

Bu iki deyim, sayfanın sütun başlığı köprülerini şu şekilde ayarlamanızı sağlar:

| Geçerli sıralama düzeni   | Son ad Köprüsü | Tarih Köprüsü |
|:--------------------:|:-------------------:|:--------------:|
| Artan son ad  | descending          | ascending      |
| Azalan son ad | ascending           | ascending      |
| Artan Tarih       | ascending           | descending     |
| Azalan Tarih      | ascending           | ascending      |

Yöntemi, sıralama yapılacak sütunu belirtmek için LINQ to Entities kullanır. Kod, Switch ifadesinden önce bir `IQueryable<Student>` başlatır ve Switch ifadesinde onu değiştirir:

[!code-csharp[Main](intro/samples/cu30snapshots/3-sorting/Pages/Students/Index1.cshtml.cs?name=snippet_IQueryable)]

Bir`IQueryable` oluşturulduğunda veya değiştirildiğinde veritabanına hiçbir sorgu gönderilmez. `IQueryable` nesnesi bir koleksiyona dönüştürülene kadar sorgu yürütülmez. `IQueryable`, `ToListAsync`gibi bir yöntemi çağırarak bir koleksiyona dönüştürülür. Bu nedenle `IQueryable` kod, aşağıdaki deyime kadar yürütülemeyen tek bir sorgu ile sonuçlanır:

[!code-csharp[Main](intro/samples/cu30snapshots/3-sorting/Pages/Students/Index1.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync`, çok sayıda sıralanabilir sütunla ayrıntılı alabilir. Bu işlevi kodun alternatif bir yolu hakkında daha fazla bilgi için, bu öğretici serisinin MVC sürümünde [kodu basitleştirmek için dınamık LINQ kullanma](xref:data/ef-mvc/advanced#dynamic-linq) konusuna bakın.

### <a name="add-column-heading-hyperlinks-to-the-student-index-page"></a>Öğrenci dizini sayfasına sütun başlığı köprüleri ekleme

*Öğrenciler/Index. cshtml*içindeki kodu aşağıdaki kodla değiştirin. Değişiklikler vurgulanır.

[!code-cshtml[Main](intro/samples/cu30snapshots/3-sorting/Pages/Students/Index1.cshtml?highlight=5,8,17-19,22,25-27,33)]

Yukarıdaki kod:

* `LastName` ve `EnrollmentDate` sütun başlıklarına köprüler ekler.
* , Geçerli sıralama düzeni değerleriyle köprüler ayarlamak için `NameSort` ve `DateSort` içindeki bilgileri kullanır.
* Sayfa başlığını dizinden öğrencilerle değiştirir.
* `Model.Students``Model.Student` değişiklikler.

Sıralamanın çalıştığını doğrulamak için:

* Uygulamayı çalıştırın ve **öğrenciler** sekmesini seçin.
* Sütun başlıklarına tıklayın.

## <a name="add-filtering"></a>Filtre ekleme

Öğrenciler dizin sayfasına filtre eklemek için:

* Razor sayfasına bir metin kutusu ve bir Gönder düğmesi eklenir. Metin kutusu, ad veya soyadı üzerinde bir arama dizesi sağlar.
* Sayfa modeli metin kutusu değerini kullanacak şekilde güncelleştirilir.

### <a name="update-the-ongetasync-method"></a>OnGetAsync yöntemini güncelleştirme

*Öğrenciler/Index. cshtml. cs* dosyasındaki kodu, filtreleme eklemek için aşağıdaki kodla değiştirin:

[!code-csharp[Main](intro/samples/cu30snapshots/3-sorting/Pages/Students/Index2.cshtml.cs?name=snippet_All&highlight=28,33,37-41)]

Yukarıdaki kod:

* `OnGetAsync` yöntemine `searchString` parametresini ekler ve parametre değerini `CurrentFilter` özelliğinde kaydeder. Arama dizesi değeri bir sonraki bölüme eklenen bir metin kutusundan alınır.
* `Where` yan tümcesine LINQ deyimi ekler. `Where` yan tümcesi, yalnızca adı veya soyadı arama dizesini içeren öğrencileri seçer. LINQ deyimleri yalnızca aranacak bir değer varsa yürütülür.

### <a name="iqueryable-vs-ienumerable"></a>IQueryable vs. IEnumerable

Kod, bir `IQueryable` nesnesi üzerinde `Where` yöntemini çağırır ve filtre sunucuda işlenir. Bazı senaryolarda, uygulama bellek içi bir koleksiyonda `Where` yöntemi genişletme yöntemi olarak çağırıyor olabilir. Örneğin, EF Core `DbSet` `_context.Students` değişikliklerin `IEnumerable` koleksiyonu döndüren bir depo yöntemine göre olduğunu varsayalım. Sonuç normalde aynı olur, ancak bazı durumlarda farklı olabilir.

Örneğin, `Contains` .NET Framework uygulanması varsayılan olarak büyük/küçük harfe duyarlı bir karşılaştırma gerçekleştirir. SQL Server, büyük/küçük harfe duyarlılık `Contains` SQL Server örneğinin harmanlama ayarı tarafından belirlenir. SQL Server varsayılan olarak büyük/küçük harfe duyarlı değildir. SQLite, büyük/küçük harfe duyarlı olur. `ToUpper`, testi açık büyük/küçük harfe duyarsız hale getirmek için çağrılabilir:

```csharp
Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`
```

Yukarıdaki kod, `Where` yöntemi bir `IEnumerable` çağrılıp veya SQLite üzerinde çalıştırılsa bile, filtrenin büyük/küçük harf duyarsız olmasını güvence altına alır.

`IEnumerable` bir koleksiyonda `Contains` çağrıldığında .NET Core uygulama kullanılır. Bir `IQueryable` nesnesi üzerinde `Contains` çağrıldığında, veritabanı uygulamasını kullanır.

Bir `IQueryable` `Contains` çağırmak genellikle performans nedenleriyle tercih edilir. `IQueryable`, filtreleme veritabanı sunucusu tarafından yapılır. Önce bir `IEnumerable` oluşturulursa, tüm satırların veritabanı sunucusundan döndürülmesi gerekir.

`ToUpper`çağırmak için bir performans cezası vardır. `ToUpper` kodu, TSQL SELECT ifadesinin WHERE yan tümcesine bir işlev ekler. Eklenen işlev, iyileştiricinin bir dizin kullanmasını önler. SQL, büyük/küçük harfe duyarsız olarak yüklendiği için, gerek duyulmadığında `ToUpper` çağrısından kaçınmak en iyisidir.

Daha fazla bilgi için bkz. [SQLite sağlayıcı ile büyük/küçük harfe duyarsız sorgu kullanma](https://github.com/aspnet/EntityFrameworkCore/issues/11414).

### <a name="update-the-razor-page"></a>Razor sayfasını güncelleştirme

*Sayfalar/öğrenciler/Index. cshtml* içindeki kodu, bir **arama** düğmesi ve assıralanan Chrome oluşturmak için değiştirin.

[!code-cshtml[Main](intro/samples/cu30snapshots/3-sorting/Pages/Students/Index2.cshtml?highlight=14-23)]

Yukarıdaki kod, arama metin kutusu ve düğmesini eklemek için `<form>` [Tag yardımcısını](xref:mvc/views/tag-helpers/intro) kullanır. Varsayılan olarak, `<form>` etiket Yardımcısı form verilerini bir GÖNDERIYLE gönderir. POST ile parametreler, URL 'de değil HTTP ileti gövdesine geçirilir. HTTP GET kullanıldığında, form verileri URL 'ye sorgu dizeleri olarak geçirilir. Verilerin sorgu dizelerine geçirilmesi, kullanıcıların URL 'YI yer işaretine eklemesini sağlar. [W3C yönergeleri](https://www.w3.org/2001/tag/doc/whenToUseGet.html) , eylem bir güncelleştirme ile SONUÇLANMAZSA, Get 'in kullanılması önerilir.

Uygulamayı test edin:

* **Öğrenciler** sekmesini seçin ve bir arama dizesi girin. SQLite kullanıyorsanız, filtre yalnızca daha önce gösterilen isteğe bağlı `ToUpper` kodu uyguladıysanız, büyük/küçük harfe duyarlıdır.

* **Ara**' yı seçin.

URL 'nin arama dizesini içerdiğine dikkat edin. Örnek:

```
https://localhost:<port>/Students?SearchString=an
```

Sayfa yer işaretiyle, yer işareti sayfanın URL 'sini ve `SearchString` sorgu dizesini içerir. `form` etiketindeki `method="get"`, sorgu dizesinin oluşturulmasına neden olur.

Şu anda, bir sütun başlığı sıralama bağlantısı seçildiğinde, **arama** kutusundaki filtre değeri kaybedilir. Kayıp filtre değeri bir sonraki bölümde düzeltilir.

## <a name="add-paging"></a>Sayfalama Ekle

Bu bölümde, sayfalama desteklemek için bir `PaginatedList` sınıfı oluşturulur. `PaginatedList` sınıfı, tablodaki tüm satırları almak yerine sunucu üzerindeki verileri filtrelemek için `Skip` ve `Take` deyimlerini kullanır. Aşağıdaki çizimde sayfalama düğmeleri gösterilmektedir.

![Sayfa bağlantılarıyla öğrenciler Dizin sayfası](sort-filter-page/_static/paging30.png)

### <a name="create-the-paginatedlist-class"></a>Sayfalı liste sınıfını oluşturma

Proje klasöründe aşağıdaki kodla `PaginatedList.cs` oluşturun:

[!code-csharp[Main](intro/samples/cu30/PaginatedList.cs)]

Yukarıdaki koddaki `CreateAsync` yöntemi sayfa boyutunu ve sayfa numarasını alır ve uygun `Skip` ve `Take` deyimlerini `IQueryable`uygular. `IQueryable``ToListAsync` çağrıldığında, yalnızca istenen sayfayı içeren bir liste döndürür. `HasPreviousPage` ve `HasNextPage` özellikleri, **önceki** ve **sonraki** sayfalama düğmelerini etkinleştirmek veya devre dışı bırakmak için kullanılır.

`CreateAsync` yöntemi `PaginatedList<T>`oluşturmak için kullanılır. Bir Oluşturucu `PaginatedList<T>` nesnesi oluşturamaz; oluşturucular zaman uyumsuz kod çalıştıramıyor.

### <a name="add-paging-to-the-pagemodel-class"></a>PageModel sınıfına sayfalama ekleme

*Öğrenciler/Index. cshtml. cs* ' deki kodu, sayfalama eklemek için değiştirin.

[!code-csharp[Main](intro/samples/cu30/Pages/Students/Index.cshtml.cs?name=snippet_All&highlight=26,28-29,31,34-41,68-70)]

Yukarıdaki kod:

* `Students` özelliğinin türünü `IList<Student>` `PaginatedList<Student>`olarak değiştirir.
* Sayfa dizinini, geçerli `sortOrder`ve `currentFilter` `OnGetAsync` Yöntem imzasına ekler.
* Sıralama düzenini CurrentSort özelliğine kaydeder.
* Yeni bir arama dizesi olduğunda sayfa dizinini 1 olarak sıfırlar.
* Öğrenci varlıklarını almak için `PaginatedList` sınıfını kullanır.

`OnGetAsync` aldığı tüm parametreler şu durumlarda null:

* Sayfa **öğrenciler** bağlantısından çağrılır.
* Kullanıcı bir sayfalama veya sıralama bağlantısına tıklamadı.

Bir sayfalama bağlantısına tıklandığında, sayfa dizin değişkeni görüntülenecek sayfa numarasını içerir.

`CurrentSort` özelliği, geçerli sıralama düzeni ile Razor sayfasını sağlar. Disk belleği sırasında sıralama düzenini korumak için geçerli sıralama düzeni, sayfalama bağlantılarına eklenmelidir.

`CurrentFilter` özelliği, Razor sayfasını geçerli filtre dizesiyle sağlar. `CurrentFilter` değeri:

* Disk belleği sırasında filtre ayarlarını korumak için disk belleği bağlantılarına eklenmelidir.
* Sayfa yeniden görüntülendiğinde metin kutusuna geri yüklenmelidir.

Sayfalama sırasında arama dizesi değiştirilirse sayfa 1 ' e sıfırlanır. Yeni filtre farklı verilerin görüntülenmesini sağladığından sayfanın 1 olarak sıfırlanması. Bir arama değeri girildiğinde ve **Gönder** seçildiğinde:

  * Arama dizesi değiştirildi.
  * `searchString` parametresi null değil.

  `PaginatedList.CreateAsync` yöntemi, öğrenci sorgusunu, sayfalama destekleyen bir koleksiyon türündeki tek bir öğrenci sayfasına dönüştürür. Bu tek öğrenci sayfası Razor sayfasına geçirilir.

  `PaginatedList.CreateAsync` çağrısındaki `pageIndex` sonraki iki soru işareti, [null birleşim işlecini](/dotnet/csharp/language-reference/operators/null-conditional-operator)temsil eder. Null birleşim işleci, null yapılabilir bir tür için varsayılan değeri tanımlar. İfade `(pageIndex ?? 1)`, bir değeri varsa `pageIndex` değerini döndürür anlamına gelir. `pageIndex` bir değere sahip değilse, 1 döndürün.

### <a name="add-paging-links-to-the-razor-page"></a>Razor sayfasına sayfalama bağlantıları ekleme

*Öğrenciler/Index. cshtml* içindeki kodu aşağıdaki kodla değiştirin. Değişiklikler vurgulanır:

[!code-cshtml[Main](intro/samples/cu30/Pages/Students/Index.cshtml?highlight=29-32,38-41,69-87)]

Sütun üst bilgisi bağlantıları, geçerli arama dizesini `OnGetAsync` yönteme geçirmek için sorgu dizesini kullanır:

[!code-cshtml[Main](intro/samples/cu30/Pages/Students/Index.cshtml?range=29-32)]

Sayfalama düğmeleri etiket yardımcıları tarafından görüntülenir:

[!code-cshtml[Main](intro/samples/cu30/Pages/Students/Index.cshtml?range=73-87)]

Uygulamayı çalıştırın ve öğrenciler sayfasına gidin.

* Sayfalama 'nin çalıştığından emin olmak için, farklı sıralama emirlerindeki disk belleği bağlantılarına tıklayın.
* Disk belleğinin sıralama ve filtreleme ile düzgün çalıştığını doğrulamak için bir arama dizesi girin ve sayfalama yapmayı deneyin.

![Sayfa bağlantılarıyla öğrenciler Dizin sayfası](sort-filter-page/_static/paging30.png)

## <a name="add-grouping"></a>Gruplandırma Ekle

Bu bölüm, her kayıt tarihi için kaç öğrenciye kaydolduğunu görüntüleyen bir hakkında sayfası oluşturur. Güncelleştirme gruplamayı kullanır ve aşağıdaki adımları içerir:

* **Hakkında** sayfasında kullanılan veriler için bir görünüm modeli oluşturun.
* Görünüm modelini kullanmak için hakkında sayfasını güncelleştirin.

### <a name="create-the-view-model"></a>Görünüm modeli oluşturma

*Modeller/SchoolViewModels* klasörü oluşturun.

Aşağıdaki kodla *SchoolViewModels/kayıtlarını Mentdategroup. cs* oluşturun:

[!code-csharp[Main](intro/samples/cu30/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="create-the-razor-page"></a>Razor sayfasını oluşturma

Aşağıdaki kodla bir *Pages/about. cshtml* dosyası oluşturun:

[!code-cshtml[Main](intro/samples/cu30/Pages/About.cshtml)]

### <a name="create-the-page-model"></a>Sayfa modelini oluşturma

Aşağıdaki kodla bir *Pages/about. cshtml. cs* dosyası oluşturun:

[!code-csharp[Main](intro/samples/cu30/Pages/About.cshtml.cs)]

LINQ deyimleri, öğrenci varlıklarını kayıt tarihine göre gruplandırır, her bir gruptaki varlıkların sayısını hesaplar ve sonuçları bir `EnrollmentDateGroup` View model nesneleri koleksiyonunda depolar.

Uygulamayı çalıştırın ve hakkında sayfasına gidin. Her kayıt tarihi için öğrenci sayısı bir tabloda görüntülenir.

![Sayfa hakkında](sort-filter-page/_static/about30.png)

## <a name="next-steps"></a>Sonraki adımlar

Sonraki öğreticide, uygulama, veri modelini güncelleştirmek için geçişleri kullanır.

> [!div class="step-by-step"]
> [Önceki öğretici](xref:data/ef-rp/crud)
> [sonraki öğretici](xref:data/ef-rp/migrations)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu öğreticide sıralama, filtreleme, gruplama ve sayfalama işlevleri eklenmiştir.

Aşağıdaki çizimde tamamlanmış bir sayfa gösterilmektedir. Sütun başlıkları sütunu sıralamak için tıklatılabilir bağlantılardır. Sütun başlığına tıklanması, artan ve azalan sıralama düzeni arasında sürekli olarak geçiş yapar.

![Öğrenciler Dizin sayfası](sort-filter-page/_static/paging.png)

Çözemediğiniz sorunlarla karşılaşırsanız, [Tamamlanmış uygulamayı](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)indirin.

## <a name="add-sorting-to-the-index-page"></a>Dizin sayfasına sıralama Ekle

Sıralama parametrelerini içerecek şekilde *öğrenciler/Index. cshtml. cs* `PageModel` dizeler ekleyin:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]

*Öğrenciler/Index. cshtml. cs* `OnGetAsync` aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

Yukarıdaki kod, URL 'deki sorgu dizesinden bir `sortOrder` parametresi alır. URL (sorgu dizesi dahil), [tutturucu etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
) tarafından oluşturulur

`sortOrder` parametresi "ad" veya "Date" dir. `sortOrder` parametresi, isteğe bağlı olarak azalan sıra belirtmek için "_desc" tarafından izlenir. Varsayılan sıralama düzeni artan.

**Öğrenciler** bağlantısından Dizin sayfası istendiğinde sorgu dizesi yoktur. Öğrenciler, son ada göre artan sırada görüntülenir. Son ada göre artan sıralama, `switch` deyimindeki varsayılan (gelen durumdur). Kullanıcı bir sütun başlığı bağlantısına tıkladığında, uygun `sortOrder` değeri sorgu dizesi değerinde sağlanır.

`NameSort` ve `DateSort`, Razor sayfası tarafından, sütun başlığı köprülerini uygun sorgu dizesi değerleriyle yapılandırmak için kullanılır:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

Aşağıdaki kod, C# koşullu [?: işlecini](/dotnet/csharp/language-reference/operators/conditional-operator)içerir:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

İlk satır `sortOrder` null ya da boş olduğunda `NameSort` "name_desc" olarak ayarlandığını belirtir. `sortOrder` null veya boş **değilse** , `NameSort` boş bir dizeye ayarlanır.

`?: operator` Üçlü işleç olarak da bilinir.

Bu iki deyim, sayfanın sütun başlığı köprülerini şu şekilde ayarlamanızı sağlar:

| Geçerli sıralama düzeni | Son ad Köprüsü | Tarih Köprüsü |
|:--------------------:|:-------------------:|:--------------:|
| Artan son ad | descending        | ascending      |
| Azalan son ad | ascending           | ascending      |
| Artan Tarih       | ascending           | descending     |
| Azalan Tarih      | ascending           | ascending      |

Yöntemi, sıralama yapılacak sütunu belirtmek için LINQ to Entities kullanır. Kod, Switch ifadesinden önce bir `IQueryable<Student>` başlatır ve Switch ifadesinde onu değiştirir:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-999)]

 Bir`IQueryable` oluşturulduğunda veya değiştirildiğinde veritabanına hiçbir sorgu gönderilmez. `IQueryable` nesnesi bir koleksiyona dönüştürülene kadar sorgu yürütülmez. `IQueryable`, `ToListAsync`gibi bir yöntemi çağırarak bir koleksiyona dönüştürülür. Bu nedenle `IQueryable` kod, aşağıdaki deyime kadar yürütülemeyen tek bir sorgu ile sonuçlanır:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync`, çok sayıda sıralanabilir sütunla ayrıntılı alabilir.

### <a name="add-column-heading-hyperlinks-to-the-student-index-page"></a>Öğrenci dizini sayfasına sütun başlığı köprüleri ekleme

*Öğrenciler/Index. cshtml*içindeki kodu aşağıdaki vurgulanmış kodla değiştirin:

[!code-html[](intro/samples/cu21/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

Yukarıdaki kod:

* `LastName` ve `EnrollmentDate` sütun başlıklarına köprüler ekler.
* , Geçerli sıralama düzeni değerleriyle köprüler ayarlamak için `NameSort` ve `DateSort` içindeki bilgileri kullanır.

Sıralamanın çalıştığını doğrulamak için:

* Uygulamayı çalıştırın ve **öğrenciler** sekmesini seçin.
* **Son ad**' a tıklayın.
* **Kayıt tarihi**' ne tıklayın.

Kodu daha iyi anlamak için:

* *Öğrenciler/Index. cshtml. cs*dosyasında `switch (sortOrder)`bir kesme noktası ayarlayın.
* `NameSort` ve `DateSort`için bir izleme ekleyin.
* *Öğrenciler/Index. cshtml*'de `@Html.DisplayNameFor(model => model.Student[0].LastName)`bir kesme noktası ayarlayın.

Hata ayıklayıcıda adım adım.

## <a name="add-a-search-box-to-the-students-index-page"></a>Öğrenciler dizin sayfasına bir arama kutusu ekleyin

Öğrenciler dizin sayfasına filtre eklemek için:

* Razor sayfasına bir metin kutusu ve bir Gönder düğmesi eklenir. Metin kutusu, ad veya soyadı üzerinde bir arama dizesi sağlar.
* Sayfa modeli metin kutusu değerini kullanacak şekilde güncelleştirilir.

### <a name="add-filtering-functionality-to-the-index-method"></a>Dizin yöntemine filtreleme işlevi ekleme

*Öğrenciler/Index. cshtml. cs* `OnGetAsync` aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Yukarıdaki kod:

* `OnGetAsync` yöntemine `searchString` parametresini ekler. Arama dizesi değeri bir sonraki bölüme eklenen bir metin kutusundan alınır.
* LINQ ifadesine bir `Where` yan tümcesine eklenmiştir. `Where` yan tümcesi, yalnızca adı veya soyadı arama dizesini içeren öğrencileri seçer. LINQ deyimleri yalnızca aranacak bir değer varsa yürütülür.

Note: Yukarıdaki kod, bir `IQueryable` nesnesi üzerinde `Where` yöntemini çağırır ve filtre sunucuda işlenir. Bazı senaryolarda, uygulama bellek içi bir koleksiyonda `Where` yöntemi genişletme yöntemi olarak çağırıyor olabilir. Örneğin, EF Core `DbSet` `_context.Students` değişikliklerin `IEnumerable` koleksiyonu döndüren bir depo yöntemine göre olduğunu varsayalım. Sonuç normalde aynı olur, ancak bazı durumlarda farklı olabilir.

Örneğin, `Contains` .NET Framework uygulanması varsayılan olarak büyük/küçük harfe duyarlı bir karşılaştırma gerçekleştirir. SQL Server, büyük/küçük harfe duyarlılık `Contains` SQL Server örneğinin harmanlama ayarı tarafından belirlenir. SQL Server varsayılan olarak büyük/küçük harfe duyarlı değildir. `ToUpper`, testi açık büyük/küçük harfe duyarsız hale getirmek için çağrılabilir:

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

Yukarıdaki kod, kod `IEnumerable`kullanım değişirse sonuçların büyük/küçük harf duyarsız olmasını sağlar. `IEnumerable` bir koleksiyonda `Contains` çağrıldığında .NET Core uygulama kullanılır. Bir `IQueryable` nesnesi üzerinde `Contains` çağrıldığında, veritabanı uygulamasını kullanır. Bir depodan bir `IEnumerable` döndürmek, önemli bir performans cezasına sahip olabilir:

1. Tüm satırlar DB sunucusundan döndürülür.
1. Filtre, uygulamadaki tüm döndürülen satırlara uygulanır.

`ToUpper`çağırmak için bir performans cezası vardır. `ToUpper` kodu, TSQL SELECT ifadesinin WHERE yan tümcesine bir işlev ekler. Eklenen işlev, iyileştiricinin bir dizin kullanmasını önler. SQL, büyük/küçük harfe duyarsız olarak yüklendiği için, gerek duyulmadığında `ToUpper` çağrısından kaçınmak en iyisidir.

### <a name="add-a-search-box-to-the-student-index-page"></a>Öğrenci dizin sayfasına bir arama kutusu ekleyin

*Sayfalar/öğrenciler/Index. cshtml*' de, bir **arama** düğmesi ve asi grafik Chrome oluşturmak için aşağıdaki vurgulanmış kodu ekleyin.

[!code-html[](intro/samples/cu21/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

Yukarıdaki kod, arama metin kutusu ve düğmesini eklemek için `<form>` [Tag yardımcısını](xref:mvc/views/tag-helpers/intro) kullanır. Varsayılan olarak, `<form>` etiket Yardımcısı form verilerini bir GÖNDERIYLE gönderir. POST ile parametreler, URL 'de değil HTTP ileti gövdesine geçirilir. HTTP GET kullanıldığında, form verileri URL 'ye sorgu dizeleri olarak geçirilir. Verilerin sorgu dizelerine geçirilmesi, kullanıcıların URL 'YI yer işaretine eklemesini sağlar. [W3C yönergeleri](https://www.w3.org/2001/tag/doc/whenToUseGet.html) , eylem bir güncelleştirme ile SONUÇLANMAZSA, Get 'in kullanılması önerilir.

Uygulamayı test edin:

* **Öğrenciler** sekmesini seçin ve bir arama dizesi girin.
* **Ara**' yı seçin.

URL 'nin arama dizesini içerdiğine dikkat edin.

```html
http://localhost:5000/Students?SearchString=an
```

Sayfa yer işaretiyle, yer işareti sayfanın URL 'sini ve `SearchString` sorgu dizesini içerir. `form` etiketindeki `method="get"`, sorgu dizesinin oluşturulmasına neden olur.

Şu anda, bir sütun başlığı sıralama bağlantısı seçildiğinde, **arama** kutusundaki filtre değeri kaybedilir. Kayıp filtre değeri bir sonraki bölümde düzeltilir.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Öğrenciler dizin sayfasına sayfalama işlevselliği ekleme

Bu bölümde, sayfalama desteklemek için bir `PaginatedList` sınıfı oluşturulur. `PaginatedList` sınıfı, tablodaki tüm satırları almak yerine sunucu üzerindeki verileri filtrelemek için `Skip` ve `Take` deyimlerini kullanır. Aşağıdaki çizimde sayfalama düğmeleri gösterilmektedir.

![Sayfa bağlantılarıyla öğrenciler Dizin sayfası](sort-filter-page/_static/paging.png)

Proje klasöründe aşağıdaki kodla `PaginatedList.cs` oluşturun:

[!code-csharp[](intro/samples/cu21/PaginatedList.cs)]

Yukarıdaki koddaki `CreateAsync` yöntemi sayfa boyutunu ve sayfa numarasını alır ve uygun `Skip` ve `Take` deyimlerini `IQueryable`uygular. `IQueryable``ToListAsync` çağrıldığında, yalnızca istenen sayfayı içeren bir liste döndürür. `HasPreviousPage` ve `HasNextPage` özellikleri, **önceki** ve **sonraki** sayfalama düğmelerini etkinleştirmek veya devre dışı bırakmak için kullanılır.

`CreateAsync` yöntemi `PaginatedList<T>`oluşturmak için kullanılır. Oluşturucu `PaginatedList<T>` nesnesi oluşturamaz, oluşturucular zaman uyumsuz kod çalıştıramıyor.

## <a name="add-paging-functionality-to-the-index-method"></a>Dizin yöntemine sayfalama işlevselliği ekleme

*Öğrenciler/Index. cshtml. cs*dosyasında `IList<Student>` `Student` türünü `PaginatedList<Student>`olarak güncelleştirin:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

*Öğrenciler/Index. cshtml. cs* `OnGetAsync` aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-999)]

Yukarıdaki kod, sayfa dizinini, geçerli `sortOrder`ve `currentFilter` Yöntem imzasına ekler.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

Şu durumlarda tüm parametreler null:

* Sayfa **öğrenciler** bağlantısından çağrılır.
* Kullanıcı bir sayfalama veya sıralama bağlantısına tıklamadı.

Bir sayfalama bağlantısına tıklandığında, sayfa dizin değişkeni görüntülenecek sayfa numarasını içerir.

`CurrentSort`, geçerli sıralama düzeni ile Razor sayfasını sağlar. Disk belleği sırasında sıralama düzenini korumak için geçerli sıralama düzeni, sayfalama bağlantılarına eklenmelidir.

`CurrentFilter`, Razor sayfasını geçerli filtre dizesiyle sağlar. `CurrentFilter` değeri:

* Disk belleği sırasında filtre ayarlarını korumak için disk belleği bağlantılarına eklenmelidir.
* Sayfa yeniden görüntülendiğinde metin kutusuna geri yüklenmelidir.

Sayfalama sırasında arama dizesi değiştirilirse sayfa 1 ' e sıfırlanır. Yeni filtre farklı verilerin görüntülenmesini sağladığından sayfanın 1 olarak sıfırlanması. Bir arama değeri girildiğinde ve **Gönder** seçildiğinde:

* Arama dizesi değiştirildi.
* `searchString` parametresi null değil.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

`PaginatedList.CreateAsync` yöntemi, öğrenci sorgusunu, sayfalama destekleyen bir koleksiyon türündeki tek bir öğrenci sayfasına dönüştürür. Bu tek öğrenci sayfası Razor sayfasına geçirilir.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

`PaginatedList.CreateAsync` iki soru işareti, [null birleşim işlecini](/dotnet/csharp/language-reference/operators/null-conditional-operator)temsil eder. Null birleşim işleci, null yapılabilir bir tür için varsayılan değeri tanımlar. İfade `(pageIndex ?? 1)`, bir değeri varsa `pageIndex` değerini döndürür anlamına gelir. `pageIndex` bir değere sahip değilse, 1 döndürün.

## <a name="add-paging-links-to-the-student-razor-page"></a>Öğrenci Razor sayfasına sayfalama bağlantıları ekleme

*Öğrenciler/Index. cshtml*'de biçimlendirmeyi güncelleştirin. Değişiklikler vurgulanır:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-999)]

Sütun üst bilgisi bağlantıları, kullanıcının filtre sonuçları içinde sıralama yapabilmesi için geçerli arama dizesini `OnGetAsync` yöntemine geçirmek üzere sorgu dizesini kullanır:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=28-31)]

Sayfalama düğmeleri etiket yardımcıları tarafından görüntülenir:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=72-)]

Uygulamayı çalıştırın ve öğrenciler sayfasına gidin.

* Sayfalama 'nin çalıştığından emin olmak için, farklı sıralama emirlerindeki disk belleği bağlantılarına tıklayın.
* Disk belleğinin sıralama ve filtreleme ile düzgün çalıştığını doğrulamak için bir arama dizesi girin ve sayfalama yapmayı deneyin.

![Sayfa bağlantılarıyla öğrenciler Dizin sayfası](sort-filter-page/_static/paging.png)

Kodu daha iyi anlamak için:

* *Öğrenciler/Index. cshtml. cs*dosyasında `switch (sortOrder)`bir kesme noktası ayarlayın.
* `NameSort`, `DateSort`, `CurrentSort`ve `Model.Student.PageIndex`için bir izleme ekleyin.
* *Öğrenciler/Index. cshtml*'de `@Html.DisplayNameFor(model => model.Student[0].LastName)`bir kesme noktası ayarlayın.

Hata ayıklayıcıda adım adım.

## <a name="update-the-about-page-to-show-student-statistics"></a>Öğrenci istatistiklerini göstermek için hakkında sayfasını güncelleştirin

Bu adımda, *Sayfalar/about. cshtml* , her bir kayıt tarihi için kaç öğrenciye kaydolduğunu görüntüleyecek şekilde güncelleştirilir. Güncelleştirme gruplamayı kullanır ve aşağıdaki adımları içerir:

* **Hakkında** sayfasında kullanılan veriler için bir görünüm modeli oluşturun.
* Görünüm modelini kullanmak için hakkında sayfasını güncelleştirin.

### <a name="create-the-view-model"></a>Görünüm modeli oluşturma

*Modeller* klasöründe bir *SchoolViewModels* klasörü oluşturun.

*SchoolViewModels* klasöründe aşağıdaki kodla bir *EnrollmentDateGroup.cs* ekleyin:

[!code-csharp[](intro/samples/cu21/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-page-model"></a>Hakkında sayfa modelini Güncelleştir

ASP.NET Core 2,2 ' deki Web şablonları hakkında sayfasını içermez. ASP.NET Core 2,2 kullanıyorsanız, Razor hakkında sayfasını oluşturun.

*Pages/about. cshtml. cs* dosyasını aşağıdaki kodla güncelleştirin:

[!code-csharp[](intro/samples/cu21/Pages/About.cshtml.cs)]

LINQ deyimleri, öğrenci varlıklarını kayıt tarihine göre gruplandırır, her bir gruptaki varlıkların sayısını hesaplar ve sonuçları bir `EnrollmentDateGroup` View model nesneleri koleksiyonunda depolar.

### <a name="modify-the-about-razor-page"></a>Razor hakkında sayfasında değişiklik yapma

*Pages/about. cshtml* dosyasındaki kodu aşağıdaki kodla değiştirin:

[!code-html[](intro/samples/cu21/Pages/About.cshtml)]

Uygulamayı çalıştırın ve hakkında sayfasına gidin. Her kayıt tarihi için öğrenci sayısı bir tabloda görüntülenir.

Çözemediğiniz sorunlarla karşılaşırsanız, [Bu aşama için tamamlanmış uygulamayı](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting)indirin.

![Sayfa hakkında](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>Ek kaynaklar

* [2. x kaynağı ASP.NET Core hata ayıklaması](https://github.com/dotnet/AspNetCore.Docs/issues/4155)
* [Bu öğreticinin YouTube sürümü](https://www.youtube.com/watch?v=MDs7PFpoMqI)

Sonraki öğreticide, uygulama, veri modelini güncelleştirmek için geçişleri kullanır.

> [!div class="step-by-step"]
> [Önceki](xref:data/ef-rp/crud)
> [İleri](xref:data/ef-rp/migrations)

::: moniker-end

