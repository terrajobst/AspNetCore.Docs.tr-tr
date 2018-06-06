<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="9d490-101">Şimdi bir arama gönderdiğinizde, URL arama sorgu dizesi içeriyor.</span><span class="sxs-lookup"><span data-stu-id="9d490-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="9d490-102">Arama da gider için `HttpGet Index` eylem yöntemi, varsa bile bir `HttpPost Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9d490-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![Aramasını gösteren bir tarayıcı penceresi hayalet = Url ve döndürülen, filmler Ghostbusters ve Ghostbusters 2, word hayalet içerir](~/tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="9d490-104">Aşağıdaki biçimlendirmede değişiklik gösterir `form` etiketi:</span><span class="sxs-lookup"><span data-stu-id="9d490-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="9d490-105">Türe göre arama ekleme</span><span class="sxs-lookup"><span data-stu-id="9d490-105">Adding Search by genre</span></span>

<span data-ttu-id="9d490-106">Aşağıdakileri ekleyin `MovieGenreViewModel` sınıfının *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="9d490-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="9d490-107">Film Tarz görünüm modeli içerir:</span><span class="sxs-lookup"><span data-stu-id="9d490-107">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="9d490-108">Film listesi.</span><span class="sxs-lookup"><span data-stu-id="9d490-108">A list of movies.</span></span>
   * <span data-ttu-id="9d490-109">A `SelectList` türler listesini içeren.</span><span class="sxs-lookup"><span data-stu-id="9d490-109">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="9d490-110">Bu kullanıcının listeden bir tarzını seçmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="9d490-110">This will allow the user to select a genre from the list.</span></span>
   * <span data-ttu-id="9d490-111">`movieGenre`, seçilen Tarz içerir.</span><span class="sxs-lookup"><span data-stu-id="9d490-111">`movieGenre`, which contains the selected genre.</span></span>

<span data-ttu-id="9d490-112">Değiştir `Index` yönteminde `MoviesController.cs` aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="9d490-112">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="9d490-113">Aşağıdaki kod bir `LINQ` tüm türler veritabanından alır sorgu.</span><span class="sxs-lookup"><span data-stu-id="9d490-113">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="9d490-114">`SelectList` Türler (sizi istemiyorsanız yinelenen türler için bizim seçim listesi) farklı türler yansıtma tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9d490-114">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="9d490-115">Arama türe göre dizini görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="9d490-115">Adding search by genre to the Index view</span></span>

<span data-ttu-id="9d490-116">Güncelleştirme `Index.cshtml` gibi:</span><span class="sxs-lookup"><span data-stu-id="9d490-116">Update `Index.cshtml` as follows:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

<span data-ttu-id="9d490-117">Aşağıdaki HTML Yardımcısı kullanılan lambda ifadesi inceleyin:</span><span class="sxs-lookup"><span data-stu-id="9d490-117">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
<span data-ttu-id="9d490-118">Önceki kod `DisplayNameFor` HTML Yardımcısı inceler `Title` görünen adı belirlemek için lambda ifadesinde başvurulan özelliği.</span><span class="sxs-lookup"><span data-stu-id="9d490-118">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="9d490-119">Lambda ifadesi Denetlenmekte değerlendirilmesi yerine olduğundan, bir erişim ihlali almadığınız zaman `model`, `model.movies`, veya `model.movies[0]` olan `null` veya boş.</span><span class="sxs-lookup"><span data-stu-id="9d490-119">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.movies`, or `model.movies[0]` are `null` or empty.</span></span> <span data-ttu-id="9d490-120">Lambda ifadesi ne zaman değerlendirildiği (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="9d490-120">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="9d490-121">Genre, filmi ve her ikisi tarafından arayarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="9d490-121">Test the app by searching by genre, by movie title, and by both.</span></span>
