---
title: ASP.NET Core Razor Pages arama Ekle
author: rick-anderson
description: ASP.NET Core Razor Pages nasıl arama ekleneceğini gösterir
ms.author: riande
ms.date: 7/23/2019
uid: tutorials/razor-pages/search
ms.openlocfilehash: 1eeb3aa86f2a6928b6d0b368c90e4760a66a6c6e
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334059"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a>ASP.NET Core Razor Pages arama Ekle

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

Aşağıdaki bölümlerde, film *tarzya* veya *ada* göre arama eklenir.

Aşağıdaki Vurgulanan özellikleri *sayfalara/filmlere/Index. cshtml. cs*öğesine ekleyin:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* `SearchString`: kullanıcıların arama metin kutusuna girebileceği metni içerir. `SearchString`, [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) özniteliğiyle donatılmalıdır. `[BindProperty]` form değerlerini ve Sorgu dizelerini özelliğiyle aynı ada bağlar. GET isteklerinde bağlama için `(SupportsGet = true)` gereklidir.
* `Genres`: tarzlar listesini içerir. `Genres`, kullanıcının listeden bir tarz seçmesine izin verir. `SelectList` `using Microsoft.AspNetCore.Mvc.Rendering;` gerektiriyor
* `MovieGenre`: kullanıcının seçtiği belirli tarzı içerir (örneğin, "Batı").
* `Genres` ve `MovieGenre` daha sonra bu öğreticide kullanılır.

[!INCLUDE[](~/includes/bind-get.md)]

Dizin sayfasının `OnGetAsync` yöntemini aşağıdaki kodla güncelleştirin:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

`OnGetAsync` yönteminin ilk satırı, filmleri seçmek için bir [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) sorgusu oluşturur:

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

Sorgu *yalnızca* bu noktada tanımlanmış, veritabanında çalıştırılmadı.

`SearchString` özelliği null veya boş değilse, filmler sorgusu arama dizesinde filtrelenecek şekilde değiştirilir:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

`s => s.Title.Contains()` kodu bir [lambda ifadesidir](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Lambdalar, Yöntem tabanlı [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) sorgularında, [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) yöntemi veya `Contains` (önceki kodda kullanılan) gibi standart sorgu işleci yöntemlerine bağımsız değişkenler olarak kullanılır. LINQ sorguları tanımlandıklarında veya bir Yöntem (örneğin, `Where`, `Contains` veya `OrderBy`) çağırarak değiştirildiklerinde yürütülmez. Bunun yerine sorgu yürütmesi ertelenir. Diğer bir deyişle, bir ifadenin değerlendirmesi, gerçekleştirilmiş değeri yinelenene veya `ToListAsync` yöntemi çağrılana kadar gecikir. Daha fazla bilgi için bkz. [sorgu yürütme](/dotnet/framework/data/adonet/ef/language-reference/query-execution) .

> [!NOTE]
> [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi C# kodda değil, veritabanında çalıştırılır. Sorgudaki büyük/küçük harf duyarlılığı veritabanına ve harmanlamaya bağlıdır. SQL Server, [SQL Ile benzer](/sql/t-sql/language-elements/like-transact-sql), büyük/küçük harfe duyarsız `Contains` eşlenir. SQLite ' da, varsayılan harmanlama ile büyük/küçük harfe duyarlıdır.

Filmler sayfasına gidin ve URL 'ye `?searchString=Ghost` gibi bir sorgu dizesi ekleyin (örneğin, `https://localhost:5001/Movies?searchString=Ghost`). Filtrelenmiş filmler görüntülenir.

![Dizin görünümü](search/_static/ghost.png)

Aşağıdaki yol şablonu dizin sayfasına eklendiyse, arama dizesi bir URL segmenti olarak geçirilebilir (örneğin, `https://localhost:5001/Movies/Ghost`).

```cshtml
@page "{searchString?}"
```

Önceki yol kısıtlaması, başlığın sorgu dizesi değeri yerine rota verileri (bir URL segmenti) olarak aranmasına olanak tanır.  `"{searchString?}"` `?`, bu isteğe bağlı bir yol parametresi anlamına gelir.

![URL 'ye hayalet sözcük eklenmiş olan dizin görünümü, Ghostbusters ve Ghostbusters ters ve 2 adet film listesi](search/_static/g2.png)

ASP.NET Core çalışma zamanı, `SearchString` özelliğinin değerini sorgu dizesinden (`?searchString=Ghost`) veya rota verilerinden (`https://localhost:5001/Movies/Ghost`) ayarlamak için [model bağlamayı](xref:mvc/models/model-binding) kullanır. Model bağlama büyük/küçük harfe duyarlı değildir.

Ancak, kullanıcıların bir filmi aramak için URL 'YI değiştirmesini beklemeniz gerekmez. Bu adımda, filmleri filtrelemek için Kullanıcı arabirimi eklenir. `"{searchString?}"`yol kısıtlaması eklediyseniz, kaldırın.

*Pages/filmler/Index. cshtml* dosyasını açın ve aşağıdaki kodda vurgulanan `<form>` işaretlemesini ekleyin:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/Index2.cshtml?highlight=14-19&range=1-22)]

HTML `<form>` etiketi aşağıdaki [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)kullanır:

* [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-form-tag-helper). Form gönderildiğinde, filtre dizesi, sorgu dizesi aracılığıyla *Sayfalar/filmler/Dizin* sayfasına gönderilir.
* [Giriş Etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-input-tag-helper)

Değişiklikleri kaydedin ve filtreyi test edin.

![Başlık filtresi metin kutusuna hayalet sözcük türü ile dizin görünümü](search/_static/filter.png)

## <a name="search-by-genre"></a>Tarza göre ara

`OnGetAsync` yöntemini aşağıdaki kodla güncelleştirin:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

Aşağıdaki kod, veritabanından tüm tarzları alan bir LINQ sorgusudur.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

Tarzın `SelectList`, farklı tarzlar yansıtılayarak oluşturulur.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a>Türe göre, Razor sayfasına arama ekleme

*Index. cshtml* 'yi aşağıdaki şekilde güncelleştirin:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

Türe göre, film başlığına göre ve her ikisine birden arayarak uygulamayı test edin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticinin YouTube sürümü](https://youtu.be/4B6pHtdyo08)

> [!div class="step-by-step"]
> [Önceki: sayfaları güncelleştirme](xref:tutorials/razor-pages/da1)
> [İleri: yeni bir alan ekleme](xref:tutorials/razor-pages/new-field)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

Aşağıdaki bölümlerde, film *tarzya* veya *ada* göre arama eklenir.

Aşağıdaki Vurgulanan özellikleri *sayfalara/filmlere/Index. cshtml. cs*öğesine ekleyin:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* `SearchString`: kullanıcıların arama metin kutusuna girebileceği metni içerir. `SearchString`, [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) özniteliğiyle donatılmalıdır. `[BindProperty]` form değerlerini ve Sorgu dizelerini özelliğiyle aynı ada bağlar. GET isteklerinde bağlama için `(SupportsGet = true)` gereklidir.
* `Genres`: tarzlar listesini içerir. `Genres`, kullanıcının listeden bir tarz seçmesine izin verir. `SelectList` `using Microsoft.AspNetCore.Mvc.Rendering;` gerektiriyor
* `MovieGenre`: kullanıcının seçtiği belirli tarzı içerir (örneğin, "Batı").
* `Genres` ve `MovieGenre` daha sonra bu öğreticide kullanılır.

[!INCLUDE[](~/includes/bind-get.md)]

Dizin sayfasının `OnGetAsync` yöntemini aşağıdaki kodla güncelleştirin:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

`OnGetAsync` yönteminin ilk satırı, filmleri seçmek için bir [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) sorgusu oluşturur:

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

Sorgu *yalnızca* bu noktada tanımlanmış, veritabanında çalıştırılmadı.

`SearchString` özelliği null veya boş değilse, filmler sorgusu arama dizesinde filtrelenecek şekilde değiştirilir:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

`s => s.Title.Contains()` kodu bir [lambda ifadesidir](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Lambdalar, Yöntem tabanlı [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) sorgularında, [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) yöntemi veya `Contains` (önceki kodda kullanılan) gibi standart sorgu işleci yöntemlerine bağımsız değişkenler olarak kullanılır. LINQ sorguları tanımlandıklarında veya bir Yöntem (örneğin, `Where`, `Contains` veya `OrderBy`) çağırarak değiştirildiklerinde yürütülmez. Bunun yerine sorgu yürütmesi ertelenir. Diğer bir deyişle, bir ifadenin değerlendirmesi, gerçekleştirilmiş değeri yinelenene veya `ToListAsync` yöntemi çağrılana kadar gecikir. Daha fazla bilgi için bkz. [sorgu yürütme](/dotnet/framework/data/adonet/ef/language-reference/query-execution) .

**Note:** [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi C# kodda değil, veritabanında çalıştırılır. Sorgudaki büyük/küçük harf duyarlılığı veritabanına ve harmanlamaya bağlıdır. SQL Server, [SQL Ile benzer](/sql/t-sql/language-elements/like-transact-sql), büyük/küçük harfe duyarsız `Contains` eşlenir. SQLite ' da, varsayılan harmanlama ile büyük/küçük harfe duyarlıdır.

Filmler sayfasına gidin ve URL 'ye `?searchString=Ghost` gibi bir sorgu dizesi ekleyin (örneğin, `https://localhost:5001/Movies?searchString=Ghost`). Filtrelenmiş filmler görüntülenir.

![Dizin görünümü](search/_static/ghost.png)

Aşağıdaki yol şablonu dizin sayfasına eklendiyse, arama dizesi bir URL segmenti olarak geçirilebilir (örneğin, `https://localhost:5001/Movies/Ghost`).

```cshtml
@page "{searchString?}"
```

Önceki yol kısıtlaması, başlığın sorgu dizesi değeri yerine rota verileri (bir URL segmenti) olarak aranmasına olanak tanır.  `"{searchString?}"` `?`, bu isteğe bağlı bir yol parametresi anlamına gelir.

![URL 'ye hayalet sözcük eklenmiş olan dizin görünümü, Ghostbusters ve Ghostbusters ters ve 2 adet film listesi](search/_static/g2.png)

ASP.NET Core çalışma zamanı, `SearchString` özelliğinin değerini sorgu dizesinden (`?searchString=Ghost`) veya rota verilerinden (`https://localhost:5001/Movies/Ghost`) ayarlamak için [model bağlamayı](xref:mvc/models/model-binding) kullanır. Model bağlama büyük/küçük harfe duyarlı değildir.

Ancak, kullanıcıların bir filmi aramak için URL 'YI değiştirmesini beklemeniz gerekmez. Bu adımda, filmleri filtrelemek için Kullanıcı arabirimi eklenir. `"{searchString?}"`yol kısıtlaması eklediyseniz, kaldırın.

*Pages/filmler/Index. cshtml* dosyasını açın ve aşağıdaki kodda vurgulanan `<form>` işaretlemesini ekleyin:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

HTML `<form>` etiketi aşağıdaki [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)kullanır:

* [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-form-tag-helper). Form gönderildiğinde, filtre dizesi, sorgu dizesi aracılığıyla *Sayfalar/filmler/Dizin* sayfasına gönderilir.
* [Giriş Etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-input-tag-helper)

Değişiklikleri kaydedin ve filtreyi test edin.

![Başlık filtresi metin kutusuna hayalet sözcük türü ile dizin görünümü](search/_static/filter.png)

## <a name="search-by-genre"></a>Tarza göre ara

`OnGetAsync` yöntemini aşağıdaki kodla güncelleştirin:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

Aşağıdaki kod, veritabanından tüm tarzları alan bir LINQ sorgusudur.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

Tarzın `SelectList`, farklı tarzlar yansıtılayarak oluşturulur.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a>Türe göre, Razor sayfasına arama ekleme

*Index. cshtml* 'yi aşağıdaki şekilde güncelleştirin:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

Türe göre, film başlığına göre ve her ikisine birden arayarak uygulamayı test edin.
Önceki kod, [Select etiketi yardımcısını](xref:mvc/views/working-with-forms#the-select-tag-helper) ve seçenek etiketi yardımcısını kullanır.

## <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticinin YouTube sürümü](https://youtu.be/4B6pHtdyo08)

> [!div class="step-by-step"]
> [Önceki: sayfaları güncelleştirme](xref:tutorials/razor-pages/da1)
> [İleri: yeni bir alan ekleme](xref:tutorials/razor-pages/new-field)

::: moniker-end
