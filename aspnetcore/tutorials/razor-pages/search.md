---
title: ASP.NET Core Razor sayfaları için arama Ekle
author: rick-anderson
description: Arama için ASP.NET Core Razor sayfalar ekleme işlemi açıklanır
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/search
ms.openlocfilehash: c88441b39d8c96ec817c58fc56ebd51a0887b077
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045568"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a><span data-ttu-id="d8302-103">ASP.NET Core Razor sayfaları için arama Ekle</span><span class="sxs-lookup"><span data-stu-id="d8302-103">Add search to ASP.NET Core Razor Pages</span></span>

<span data-ttu-id="d8302-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d8302-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d8302-105">Bu belgede, arama özelliği tarafından arama filmler sağlayan dizin sayfasına eklenen *Tarz* veya *adı*.</span><span class="sxs-lookup"><span data-stu-id="d8302-105">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="d8302-106">Dizin sayfanın güncelleştirme `OnGetAsync` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="d8302-106">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="d8302-107">İlk satırı `OnGetAsync` yöntemi oluşturur bir [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) filmler seçmek için sorgu:</span><span class="sxs-lookup"><span data-stu-id="d8302-107">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="d8302-108">Sorgu *yalnızca* bu noktada tanımlanan, sahip **değil** veritabanına karşı çalıştırılmış.</span><span class="sxs-lookup"><span data-stu-id="d8302-108">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="d8302-109">Varsa `searchString` parametresi içeren bir dize, filmler sorgu üzerinde arama dizesi filtrelemek için değiştirilir:</span><span class="sxs-lookup"><span data-stu-id="d8302-109">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="d8302-110">`s => s.Title.Contains()` Kodu bir [Lambda ifadesi](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="d8302-110">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="d8302-111">Lambda ifadeleri yöntem tabanlı kullanılan [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) gibi sorgularında standart sorgu işleci yöntemlerinin bağımsız değişkenleri olarak [burada](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) yöntemi veya `Contains` (Önceki kodda kullanılır).</span><span class="sxs-lookup"><span data-stu-id="d8302-111">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="d8302-112">LINQ sorguları tanımlanan ya da bunlar bir yöntemi çağırarak değiştirildiğinde yürütülmez (gibi `Where`, `Contains` veya `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="d8302-112">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="d8302-113">Bunun yerine, sorgu yürütme ertelenir.</span><span class="sxs-lookup"><span data-stu-id="d8302-113">Rather, query execution is deferred.</span></span> <span data-ttu-id="d8302-114">Bir ifade değerlendirmesi üzerinden gerçekleştirilen değerini yinelendiğinde kadar Gecikmeli anlamına veya `ToListAsync` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d8302-114">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="d8302-115">Bkz: [sorgu yürütme](/dotnet/framework/data/adonet/ef/language-reference/query-execution) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="d8302-115">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="d8302-116">**Not:** [içerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi, C# kodu değil, veritabanı üzerinde çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="d8302-116">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="d8302-117">Büyük/küçük harf duyarlılığı sorguda, veritabanı ve harmanlama bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="d8302-117">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="d8302-118">SQL Server'da `Contains` eşlendiği [SQL gibi](/sql/t-sql/language-elements/like-transact-sql), büyük küçük harfe duyarlı olduğu.</span><span class="sxs-lookup"><span data-stu-id="d8302-118">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="d8302-119">SQLite içinde varsayılan harmanlama ile büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="d8302-119">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="d8302-120">Filmler sayfasına gidin ve bir sorgu dizesi gibi ekleme `?searchString=Ghost` URL'sine (örneğin, `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="d8302-120">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="d8302-121">Filtrelenmiş filmler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d8302-121">The filtered movies are displayed.</span></span>

![Dizini görüntüle](search/_static/ghost.png)

<span data-ttu-id="d8302-123">Aşağıdaki rota şablonu dizin sayfasına eklenirse, arama dizesi URL kesimi geçirilebilir (örneğin, `http://localhost:5000/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="d8302-123">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/Ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="d8302-124">Önceki rota kısıtlaması başlığı olarak bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine arama sağlar.</span><span class="sxs-lookup"><span data-stu-id="d8302-124">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="d8302-125">`?` İçinde `"{searchString?}"` Bu, bir isteğe bağlı bir rota parametresini anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="d8302-125">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Dizin görünümünün URL'sini ve döndürülen film listesini Ghostbusters ve Ghostbusters 2 iki filmler eklenen word ghost](search/_static/g2.png)

<span data-ttu-id="d8302-127">Ancak, bir filmi için aranacak URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz.</span><span class="sxs-lookup"><span data-stu-id="d8302-127">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="d8302-128">Bu adımda, filmler filtrelemek için kullanıcı Arabirimi eklenir.</span><span class="sxs-lookup"><span data-stu-id="d8302-128">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="d8302-129">Rota kısıtlaması eklediyseniz `"{searchString?}"`, bunu kaldırın.</span><span class="sxs-lookup"><span data-stu-id="d8302-129">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="d8302-130">Açık *Pages/Movies/Index.cshtml* dosya ve ekleme `<form>` biçimlendirme aşağıdaki kodda vurgulanır:</span><span class="sxs-lookup"><span data-stu-id="d8302-130">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="d8302-131">HTML `<form>` etiketi kullanan [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="d8302-131">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="d8302-132">Form gönderildiğinde, filtre dizesi gönderilen *filmler/sayfalar/dizin* sayfası.</span><span class="sxs-lookup"><span data-stu-id="d8302-132">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="d8302-133">Değişiklikleri kaydetmek ve filtre test edin.</span><span class="sxs-lookup"><span data-stu-id="d8302-133">Save the changes and test the filter.</span></span>

![Dizin görünümünün başlık filtre metin kutusuna yazdığınız word ghost](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="d8302-135">Türe göre ara</span><span class="sxs-lookup"><span data-stu-id="d8302-135">Search by genre</span></span>

<span data-ttu-id="d8302-136">Vurgulanan aşağıdaki özelliği ekleyin *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d8302-136">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

::: moniker-end


<span data-ttu-id="d8302-137">`Genres` Özellik türleri listesini içerir.</span><span class="sxs-lookup"><span data-stu-id="d8302-137">The `Genres` property contains the list of genres.</span></span> <span data-ttu-id="d8302-138">Bu kullanıcının listeden bir türe izin verir.</span><span class="sxs-lookup"><span data-stu-id="d8302-138">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="d8302-139">`MovieGenre` Özelliği, kullanıcı seçer (örneğin, "Batı") belirli Tarz içerir.</span><span class="sxs-lookup"><span data-stu-id="d8302-139">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="d8302-140">Güncelleştirme `OnGetAsync` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="d8302-140">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="d8302-141">Tüm türleri veritabanından alır bir LINQ Sorgu kodudur.</span><span class="sxs-lookup"><span data-stu-id="d8302-141">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="d8302-142">`SelectList` Türleri farklı türleri yansıtma tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d8302-142">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="d8302-143">Türe göre arama ekleme</span><span class="sxs-lookup"><span data-stu-id="d8302-143">Adding search by genre</span></span>

<span data-ttu-id="d8302-144">Güncelleştirme *Index.cshtml* gibi:</span><span class="sxs-lookup"><span data-stu-id="d8302-144">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="d8302-145">Uygulama Tarz, film adı ve her ikisi tarafından arama yaparak test edin.</span><span class="sxs-lookup"><span data-stu-id="d8302-145">Test the app by searching by genre, by movie title, and by both.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d8302-146">[Önceki: sayfaları güncelleştirme](xref:tutorials/razor-pages/da1)
> [sonraki: yeni alan ekleme](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="d8302-146">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
