---
title: "Arama ASP.NET Core Razor sayfalarına ekleme"
author: rick-anderson
description: "Arama ASP.NET Core Razor sayfalara eklemek nasıl gösterir"
keywords: "ASP.NET Core, arama, Razor sayfaları"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/search
ms.openlocfilehash: 3729f351ba7d1e5f566046a619c17e9e1e6614cb
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
---
# <a name="adding-search-to-a-razor-pages-app"></a><span data-ttu-id="a80e6-104">Arama bir Razor sayfalarının uygulamasına ekleme</span><span class="sxs-lookup"><span data-stu-id="a80e6-104">Adding search to a Razor Pages app</span></span>

<span data-ttu-id="a80e6-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a80e6-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a80e6-106">Bu belgede, arama özelliği tarafından arama filmler etkinleştirir dizin sayfası eklenir *Tarz* veya *adı*.</span><span class="sxs-lookup"><span data-stu-id="a80e6-106">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="a80e6-107">Dizin sayfasının güncelleştirme `OnGetAsync` aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a80e6-107">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="a80e6-108">İlk satırı `OnGetAsync` yöntemi oluşturur bir [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) filmler seçmek için sorgu:</span><span class="sxs-lookup"><span data-stu-id="a80e6-108">The first line of the `OnGetAsync` method creates a [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="a80e6-109">Sorgu *yalnızca* bu noktada tanımlı olan **değil** veritabanına karşı çalışırlar bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="a80e6-109">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="a80e6-110">Varsa `searchString` parametre içeren bir dize, filmler sorgu filtre arama dizesi şekilde değiştirilir:</span><span class="sxs-lookup"><span data-stu-id="a80e6-110">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="a80e6-111">`s => s.Title.Contains()` Kodu bir [Lambda ifadesi](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="a80e6-111">The `s => s.Title.Contains()` code is a [Lambda Expression](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="a80e6-112">Lambda'lar yöntemi tabanlı içinde kullanılan [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) standart sorgu işleci yöntemlerinden bağımsız değişken olarak gibi sorgular [nerede](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) yöntemi veya `Contains` (Yukarıdaki kod içinde kullanılan).</span><span class="sxs-lookup"><span data-stu-id="a80e6-112">Lambdas are used in method-based [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="a80e6-113">Bunlar tanımlandığında veya bunların bir yöntemini çağırarak değiştirildiğinde, LINQ sorgularını yürütülen değil (gibi `Where`, `Contains` veya `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="a80e6-113">LINQ queries are not executed when they are defined or when they are modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="a80e6-114">Bunun yerine, sorgu yürütme ertelenir.</span><span class="sxs-lookup"><span data-stu-id="a80e6-114">Rather, query execution is deferred.</span></span> <span data-ttu-id="a80e6-115">Bir ifadenin değerlendirmesine üzerinden gerçekleşen değerini yinelendiğinde kadar Gecikmeli anlamına veya `ToListAsync` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a80e6-115">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="a80e6-116">Bkz: [sorgu yürütme](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="a80e6-116">See [Query Execution](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="a80e6-117">**Not:** [içerir](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi, C# kodu değil, veritabanında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="a80e6-117">**Note:** The [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="a80e6-118">Büyük küçük harfe duyarlılığın sorgusu, veritabanı ve harmanlama bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="a80e6-118">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="a80e6-119">SQL Server `Contains` eşlendiği [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), büyük küçük harfe duyarlı olduğu.</span><span class="sxs-lookup"><span data-stu-id="a80e6-119">On SQL Server, `Contains` maps to [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="a80e6-120">SQLite içinde varsayılan harmanlaması ile büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="a80e6-120">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="a80e6-121">Film sayfasına gidin ve bir sorgu dizesi gibi ilave `?searchString=Ghost` URL (örneğin, `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="a80e6-121">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="a80e6-122">Filtrelenmiş filmler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="a80e6-122">The filtered movies are displayed.</span></span>

![Dizin görünümü](search/_static/ghost.png)

<span data-ttu-id="a80e6-124">Aşağıdaki rota şablonu dizin sayfasına eklediyseniz, arama dizesini bir URL kesimi geçirilebilir (örneğin, `http://localhost:5000/Movies/ghost`).</span><span class="sxs-lookup"><span data-stu-id="a80e6-124">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="a80e6-125">Önceki rota kısıtlaması, bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine unvanını arama sağlar.</span><span class="sxs-lookup"><span data-stu-id="a80e6-125">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="a80e6-126">`?` İçinde `"{searchString?}"` bu bir isteğe bağlı bir rota parametresini anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="a80e6-126">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Url ve iki filmler, Ghostbusters ve Ghostbusters 2 döndürülen film listesi eklenen word hayalet dizin görünümünün](search/_static/g2.png)

<span data-ttu-id="a80e6-128">Ancak, bir filmi için aranacak URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz.</span><span class="sxs-lookup"><span data-stu-id="a80e6-128">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="a80e6-129">Bu adımda, filmler filtrelemek için kullanıcı Arabirimi eklenir.</span><span class="sxs-lookup"><span data-stu-id="a80e6-129">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="a80e6-130">Rota kısıtlaması eklediyseniz `"{searchString?}"`, kaldırın.</span><span class="sxs-lookup"><span data-stu-id="a80e6-130">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="a80e6-131">Açık *Pages/Movies/Index.cshtml* dosya ve ekleme `<form>` aşağıdaki kodda vurgulanan biçimlendirme:</span><span class="sxs-lookup"><span data-stu-id="a80e6-131">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="a80e6-132">HTML `<form>` etiketi kullanır [Form etiketi yardımcı](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="a80e6-132">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="a80e6-133">Form gönderildiğinde, filtre dizesi gönderilen *filmler/sayfalar/dizin* sayfası.</span><span class="sxs-lookup"><span data-stu-id="a80e6-133">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="a80e6-134">Değişiklikleri kaydetmek ve filtre sınayın.</span><span class="sxs-lookup"><span data-stu-id="a80e6-134">Save the changes and test the filter.</span></span>

![Word hayalet başlığı filtre metin kutusuna yazdığınız dizin görünümünün](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="a80e6-136">Türe göre ara</span><span class="sxs-lookup"><span data-stu-id="a80e6-136">Search by genre</span></span>

<span data-ttu-id="a80e6-137">Ekleme aşağıdaki vurgulanmış özelliklerine *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a80e6-137">Add the the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]

<span data-ttu-id="a80e6-138">`SelectList Genres` Türler listesini içerir.</span><span class="sxs-lookup"><span data-stu-id="a80e6-138">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="a80e6-139">Bu kullanıcının listeden bir tarzını seçmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="a80e6-139">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="a80e6-140">`MovieGenre` Özelliği, kullanıcı seçer (örneğin, "Batı") belirli bir tarzını içerir.</span><span class="sxs-lookup"><span data-stu-id="a80e6-140">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="a80e6-141">Güncelleştirme `OnGetAsync` aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a80e6-141">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="a80e6-142">Tüm türler veritabanından alır bir LINQ Sorgu kodudur.</span><span class="sxs-lookup"><span data-stu-id="a80e6-142">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="a80e6-143">`SelectList` Türler farklı türler yansıtma tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a80e6-143">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There is no start line.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="a80e6-144">Türe göre arama ekleme</span><span class="sxs-lookup"><span data-stu-id="a80e6-144">Adding search by genre</span></span>

<span data-ttu-id="a80e6-145">Güncelleştirme *Index.cshtml* gibi:</span><span class="sxs-lookup"><span data-stu-id="a80e6-145">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="a80e6-146">Genre, filmi ve her ikisi tarafından arayarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="a80e6-146">Test the app by searching by genre, by movie title, and by both.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a80e6-147">[Önceki: sayfalarını güncelleştirme](xref:tutorials/razor-pages/da1)
[sonraki: yeni bir alan ekleme](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="a80e6-147">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
