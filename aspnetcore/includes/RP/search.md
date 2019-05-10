# <a name="add-search-to-an-aspnet-core-razor-pages-app"></a><span data-ttu-id="2b670-101">Bir ASP.NET Core Razor sayfalar uygulamaya arama ekleme</span><span class="sxs-lookup"><span data-stu-id="2b670-101">Add search to an ASP.NET Core Razor Pages app</span></span>

<span data-ttu-id="2b670-102">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2b670-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2b670-103">Bu belgede, arama özelliği tarafından arama filmler sağlayan dizin sayfasına eklenen *Tarz* veya *adı*.</span><span class="sxs-lookup"><span data-stu-id="2b670-103">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="2b670-104">Dizin sayfanın güncelleştirme `OnGetAsync` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="2b670-104">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="2b670-105">İlk satırı `OnGetAsync` yöntemi oluşturur bir [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) filmler seçmek için sorgu:</span><span class="sxs-lookup"><span data-stu-id="2b670-105">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="2b670-106">Sorgu *yalnızca* bu noktada tanımlanan, sahip **değil** veritabanına karşı çalıştırılmış.</span><span class="sxs-lookup"><span data-stu-id="2b670-106">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="2b670-107">Varsa `searchString` parametresi içeren bir dize, filmler sorgu üzerinde arama dizesi filtrelemek için değiştirilir:</span><span class="sxs-lookup"><span data-stu-id="2b670-107">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="2b670-108">`s => s.Title.Contains()` Kodu bir [Lambda ifadesi](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="2b670-108">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="2b670-109">Lambda ifadeleri yöntem tabanlı kullanılan [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) gibi sorgularında standart sorgu işleci yöntemlerinin bağımsız değişkenleri olarak [burada](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) yöntemi veya `Contains` (Önceki kodda kullanılır).</span><span class="sxs-lookup"><span data-stu-id="2b670-109">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="2b670-110">LINQ sorguları tanımlanan ya da bunlar bir yöntemi çağırarak değiştirildiğinde yürütülmez (gibi `Where`, `Contains` veya `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="2b670-110">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="2b670-111">Bunun yerine, sorgu yürütme ertelenir.</span><span class="sxs-lookup"><span data-stu-id="2b670-111">Rather, query execution is deferred.</span></span> <span data-ttu-id="2b670-112">Bir ifade değerlendirmesi üzerinden gerçekleştirilen değerini yinelendiğinde kadar Gecikmeli anlamına veya `ToListAsync` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2b670-112">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="2b670-113">Bkz: [sorgu yürütme](/dotnet/framework/data/adonet/ef/language-reference/query-execution) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="2b670-113">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="2b670-114">**Not:** [İçerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi çalıştırılır veritabanında de C# kod.</span><span class="sxs-lookup"><span data-stu-id="2b670-114">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="2b670-115">Büyük/küçük harf duyarlılığı sorguda, veritabanı ve harmanlama bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="2b670-115">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="2b670-116">SQL Server'da `Contains` eşlendiği [SQL gibi](/sql/t-sql/language-elements/like-transact-sql), büyük küçük harfe duyarlı olduğu.</span><span class="sxs-lookup"><span data-stu-id="2b670-116">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="2b670-117">SQLite içinde varsayılan harmanlama ile büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="2b670-117">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="2b670-118">Filmler sayfasına gidin ve bir sorgu dizesi gibi ekleme `?searchString=Ghost` URL'sine (örneğin, `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="2b670-118">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="2b670-119">Filtrelenmiş filmler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2b670-119">The filtered movies are displayed.</span></span>

![Dizini görüntüle](../../tutorials/razor-pages/search/_static/ghost.png)

<span data-ttu-id="2b670-121">Aşağıdaki rota şablonu dizin sayfasına eklenirse, arama dizesi URL kesimi geçirilebilir (örneğin, `http://localhost:5000/Movies/ghost`).</span><span class="sxs-lookup"><span data-stu-id="2b670-121">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="2b670-122">Önceki rota kısıtlaması başlığı olarak bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine arama sağlar.</span><span class="sxs-lookup"><span data-stu-id="2b670-122">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="2b670-123">`?` İçinde `"{searchString?}"` Bu, bir isteğe bağlı bir rota parametresini anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2b670-123">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Dizin görünümünün URL'sini ve döndürülen film listesini Ghostbusters ve Ghostbusters 2 iki filmler eklenen word ghost](../../tutorials/razor-pages/search/_static/g2.png)

<span data-ttu-id="2b670-125">Ancak, bir filmi için aranacak URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz.</span><span class="sxs-lookup"><span data-stu-id="2b670-125">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="2b670-126">Bu adımda, filmler filtrelemek için kullanıcı Arabirimi eklenir.</span><span class="sxs-lookup"><span data-stu-id="2b670-126">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="2b670-127">Rota kısıtlaması eklediyseniz `"{searchString?}"`, bunu kaldırın.</span><span class="sxs-lookup"><span data-stu-id="2b670-127">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="2b670-128">Açık *Pages/Movies/Index.cshtml* dosya ve ekleme `<form>` biçimlendirme aşağıdaki kodda vurgulanır:</span><span class="sxs-lookup"><span data-stu-id="2b670-128">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="2b670-129">HTML `<form>` etiketi kullanan [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="2b670-129">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="2b670-130">Form gönderildiğinde, filtre dizesi gönderilen *filmler/sayfalar/dizin* sayfası.</span><span class="sxs-lookup"><span data-stu-id="2b670-130">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="2b670-131">Değişiklikleri kaydetmek ve filtre test edin.</span><span class="sxs-lookup"><span data-stu-id="2b670-131">Save the changes and test the filter.</span></span>

![Dizin görünümünün başlık filtre metin kutusuna yazdığınız word ghost](../../tutorials/razor-pages/search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="2b670-133">Türe göre ara</span><span class="sxs-lookup"><span data-stu-id="2b670-133">Search by genre</span></span>

<span data-ttu-id="2b670-134">Vurgulanan aşağıdaki özelliği ekleyin *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2b670-134">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

<span data-ttu-id="2b670-135">`SelectList Genres` Türleri listesini içerir.</span><span class="sxs-lookup"><span data-stu-id="2b670-135">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="2b670-136">Bu kullanıcının listeden bir türe izin verir.</span><span class="sxs-lookup"><span data-stu-id="2b670-136">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="2b670-137">`MovieGenre` Özelliği, kullanıcı seçer (örneğin, "Batı") belirli Tarz içerir.</span><span class="sxs-lookup"><span data-stu-id="2b670-137">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="2b670-138">Güncelleştirme `OnGetAsync` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="2b670-138">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="2b670-139">Tüm türleri veritabanından alır bir LINQ Sorgu kodudur.</span><span class="sxs-lookup"><span data-stu-id="2b670-139">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="2b670-140">`SelectList` Türleri farklı türleri yansıtma tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2b670-140">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="2b670-141">Türe göre arama ekleme</span><span class="sxs-lookup"><span data-stu-id="2b670-141">Adding search by genre</span></span>

<span data-ttu-id="2b670-142">Güncelleştirme *Index.cshtml* gibi:</span><span class="sxs-lookup"><span data-stu-id="2b670-142">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="2b670-143">Uygulama Tarz, film adı ve her ikisi tarafından arama yaparak test edin.</span><span class="sxs-lookup"><span data-stu-id="2b670-143">Test the app by searching by genre, by movie title, and by both.</span></span>
