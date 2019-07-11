---
title: ASP.NET Core Razor sayfaları için arama Ekle
author: rick-anderson
description: Arama için ASP.NET Core Razor sayfalar ekleme işlemi açıklanır
ms.author: riande
ms.date: 12/03/2018
uid: tutorials/razor-pages/search
ms.openlocfilehash: 5fc262f92b6a8de68ca8499a4647a9d2fb1450dd
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815634"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a><span data-ttu-id="2e9b4-103">ASP.NET Core Razor sayfaları için arama Ekle</span><span class="sxs-lookup"><span data-stu-id="2e9b4-103">Add search to ASP.NET Core Razor Pages</span></span>

<span data-ttu-id="2e9b4-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2e9b4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="2e9b4-105">Aşağıdaki bölümlerde, arama tarafından filmler *Tarz* veya *adı* eklenir.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-105">In the following sections, searching movies by *genre* or *name* is added.</span></span>

<span data-ttu-id="2e9b4-106">Vurgulanan aşağıdaki özelliği ekleyin *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2e9b4-106">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* <span data-ttu-id="2e9b4-107">`SearchString`: kullanıcıların, arama metin kutusuna girdiğiniz metnin içerir.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-107">`SearchString`: contains the text users enter in the search text box.</span></span> <span data-ttu-id="2e9b4-108">`SearchString` ile donatılmış [ `[BindProperty]` ](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-108">`SearchString` is decorated with the [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute.</span></span> <span data-ttu-id="2e9b4-109">`[BindProperty]` Form değerlerini ve sorgu dizeleriyle özelliğiyle aynı ada bağlar.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-109">`[BindProperty]` binds form values and query strings with the same name as the property.</span></span> <span data-ttu-id="2e9b4-110">`(SupportsGet = true)` GET isteklerinde bağlama için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-110">`(SupportsGet = true)` is required for binding on GET requests.</span></span>
* <span data-ttu-id="2e9b4-111">`Genres`: tür listesini içerir.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-111">`Genres`: contains the list of genres.</span></span> <span data-ttu-id="2e9b4-112">`Genres` kullanıcının listeden bir türe izin verir.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-112">`Genres` allows the user to select a genre from the list.</span></span> <span data-ttu-id="2e9b4-113">`SelectList` gerektirir `using Microsoft.AspNetCore.Mvc.Rendering;`</span><span class="sxs-lookup"><span data-stu-id="2e9b4-113">`SelectList` requires `using Microsoft.AspNetCore.Mvc.Rendering;`</span></span>
* <span data-ttu-id="2e9b4-114">`MovieGenre`: belirli bir türe kullanıcı seçer (örneğin, "Batı") içeriyor.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-114">`MovieGenre`: contains the specific genre the user selects (for example, "Western").</span></span>
* <span data-ttu-id="2e9b4-115">`Genres` ve `MovieGenre` Bu öğreticinin ilerleyen bölümlerinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-115">`Genres` and `MovieGenre` are used later in this tutorial.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="2e9b4-116">Dizin sayfanın güncelleştirme `OnGetAsync` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="2e9b4-116">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="2e9b4-117">İlk satırı `OnGetAsync` yöntemi oluşturur bir [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) filmler seçmek için sorgu:</span><span class="sxs-lookup"><span data-stu-id="2e9b4-117">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="2e9b4-118">Sorgu *yalnızca* bu noktada tanımlanan, sahip **değil** veritabanına karşı çalıştırılmış.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-118">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="2e9b4-119">Varsa `SearchString` özelliği null veya boş değil, filmler sorgu üzerinde arama dizesi filtrelemek için değiştirilir:</span><span class="sxs-lookup"><span data-stu-id="2e9b4-119">If the `SearchString` property is not null or empty, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="2e9b4-120">`s => s.Title.Contains()` Kodu bir [Lambda ifadesi](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="2e9b4-120">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="2e9b4-121">Lambda ifadeleri yöntem tabanlı kullanılan [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) gibi sorgularında standart sorgu işleci yöntemlerinin bağımsız değişkenleri olarak [burada](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) yöntemi veya `Contains` (Önceki kodda kullanılır).</span><span class="sxs-lookup"><span data-stu-id="2e9b4-121">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="2e9b4-122">LINQ sorguları tanımlanan ya da bunlar bir yöntemi çağırarak değiştirildiğinde yürütülmez (gibi `Where`, `Contains` veya `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="2e9b4-122">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="2e9b4-123">Bunun yerine, sorgu yürütme ertelenir.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-123">Rather, query execution is deferred.</span></span> <span data-ttu-id="2e9b4-124">Bir ifade değerlendirmesi üzerinden gerçekleştirilen değerini yinelendiğinde kadar Gecikmeli anlamına veya `ToListAsync` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-124">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="2e9b4-125">Bkz: [sorgu yürütme](/dotnet/framework/data/adonet/ef/language-reference/query-execution) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-125">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="2e9b4-126">**Not:** [İçerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi çalıştırılır veritabanında de C# kod.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-126">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="2e9b4-127">Büyük/küçük harf duyarlılığı sorguda, veritabanı ve harmanlama bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-127">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="2e9b4-128">SQL Server'da `Contains` eşlendiği [SQL gibi](/sql/t-sql/language-elements/like-transact-sql), büyük küçük harfe duyarlı olduğu.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-128">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="2e9b4-129">SQLite içinde varsayılan harmanlama ile büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-129">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="2e9b4-130">Filmler sayfasına gidin ve bir sorgu dizesi gibi ekleme `?searchString=Ghost` URL'sine (örneğin, `https://localhost:5001/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="2e9b4-130">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `https://localhost:5001/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="2e9b4-131">Filtrelenmiş filmler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-131">The filtered movies are displayed.</span></span>

![Dizini görüntüle](search/_static/ghost.png)

<span data-ttu-id="2e9b4-133">Aşağıdaki rota şablonu dizin sayfasına eklenirse, arama dizesi URL kesimi geçirilebilir (örneğin, `https://localhost:5001/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="2e9b4-133">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `https://localhost:5001/Movies/Ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="2e9b4-134">Önceki rota kısıtlaması başlığı olarak bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine arama sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-134">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="2e9b4-135">`?` İçinde `"{searchString?}"` Bu, bir isteğe bağlı bir rota parametresini anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-135">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Dizin görünümünün URL'sini ve döndürülen film listesini Ghostbusters ve Ghostbusters 2 iki filmler eklenen word ghost](search/_static/g2.png)

<span data-ttu-id="2e9b4-137">ASP.NET Core çalışma zamanı kullanan [model bağlama](xref:mvc/models/model-binding) değerini ayarlamak için `SearchString` Sorgu dizesinden özelliği (`?searchString=Ghost`) veya verilerini yönlendirme (`https://localhost:5001/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="2e9b4-137">The ASP.NET Core runtime uses [model binding](xref:mvc/models/model-binding) to set the value of the `SearchString` property from the query string (`?searchString=Ghost`) or route data (`https://localhost:5001/Movies/Ghost`).</span></span> <span data-ttu-id="2e9b4-138">Model bağlama büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-138">Model binding is not case sensitive.</span></span>

<span data-ttu-id="2e9b4-139">Ancak, bir filmi için aranacak URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-139">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="2e9b4-140">Bu adımda, filmler filtrelemek için kullanıcı Arabirimi eklenir.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-140">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="2e9b4-141">Rota kısıtlaması eklediyseniz `"{searchString?}"`, bunu kaldırın.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-141">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="2e9b4-142">Açık *Pages/Movies/Index.cshtml* dosya ve ekleme `<form>` biçimlendirme aşağıdaki kodda vurgulanır:</span><span class="sxs-lookup"><span data-stu-id="2e9b4-142">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="2e9b4-143">HTML `<form>` etiketi kullanan aşağıdaki [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="2e9b4-143">The HTML `<form>` tag uses the following [Tag Helpers](xref:mvc/views/tag-helpers/intro):</span></span>

* <span data-ttu-id="2e9b4-144">[Form etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="2e9b4-144">[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="2e9b4-145">Form gönderildiğinde, filtre dizesi gönderilen *filmler/sayfalar/dizin* sayfası aracılığıyla sorgu dizesi.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-145">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page via query string.</span></span>
* [<span data-ttu-id="2e9b4-146">Giriş Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="2e9b4-146">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms#the-input-tag-helper)

<span data-ttu-id="2e9b4-147">Değişiklikleri kaydetmek ve filtre test edin.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-147">Save the changes and test the filter.</span></span>

![Dizin görünümünün başlık filtre metin kutusuna yazdığınız word ghost](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="2e9b4-149">Türe göre ara</span><span class="sxs-lookup"><span data-stu-id="2e9b4-149">Search by genre</span></span>

<span data-ttu-id="2e9b4-150">Güncelleştirme `OnGetAsync` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="2e9b4-150">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="2e9b4-151">Tüm türleri veritabanından alır bir LINQ Sorgu kodudur.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-151">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="2e9b4-152">`SelectList` Türleri farklı türleri yansıtma tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-152">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a><span data-ttu-id="2e9b4-153">Razor sayfası için türe göre arama Ekle</span><span class="sxs-lookup"><span data-stu-id="2e9b4-153">Add search by genre to the Razor Page</span></span>

<span data-ttu-id="2e9b4-154">Güncelleştirme *Index.cshtml* gibi:</span><span class="sxs-lookup"><span data-stu-id="2e9b4-154">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="2e9b4-155">Uygulama Tarz, film adı ve her ikisi tarafından arama yaparak test edin.</span><span class="sxs-lookup"><span data-stu-id="2e9b4-155">Test the app by searching by genre, by movie title, and by both.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2e9b4-156">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2e9b4-156">Additional resources</span></span>

* [<span data-ttu-id="2e9b4-157">Bu öğreticide YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="2e9b4-157">YouTube version of this tutorial</span></span>](https://youtu.be/4B6pHtdyo08)

> [!div class="step-by-step"]
> <span data-ttu-id="2e9b4-158">[Önceki: Sayfaları güncelleştirme](xref:tutorials/razor-pages/da1)
> [sonraki: Yeni alan ekleme](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="2e9b4-158">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
