<span data-ttu-id="ac7aa-101">Gösterildiği eklenmesini film veritabanı bağlamı vurgulanan kod [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı (içinde *Startup.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="ac7aa-101">The highlighted code above shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container (In the *Startup.cs* file).</span></span> <span data-ttu-id="ac7aa-102">`services.AddDbContext<MvcMovieContext>(options =>` Veritabanı ve bağlantı dizesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-102">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span> <span data-ttu-id="ac7aa-103">`=>` olan bir [lambda işleci](/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span><span class="sxs-lookup"><span data-stu-id="ac7aa-103">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span></span>

<span data-ttu-id="ac7aa-104">Açık *Controllers/MoviesController.cs* dosya ve oluşturucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="ac7aa-104">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

<span data-ttu-id="ac7aa-105">Oluşturucu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) veritabanı bağlamı eklemesine (`MvcMovieContext `) içine denetleyici.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext `) into the controller.</span></span> <span data-ttu-id="ac7aa-106">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-106">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="ac7aa-107">Kesin olarak modelleri ve @model anahtar sözcüğü</span><span class="sxs-lookup"><span data-stu-id="ac7aa-107">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="ac7aa-108">Bu öğreticide daha önce nasıl bir denetleyici veri veya nesneleri kullanarak bir görünüm geçirebilirsiniz gördüğünüz `ViewData` sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-108">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="ac7aa-109">`ViewData` Sözlüktür bilgi bir görünüme iletmek için kullanışlı bir geç bağlanan yol sağlayan bir dinamik Nesne.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-109">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="ac7aa-110">MVC, model nesneleri bir görünüme kesin geçirme özelliği yazılan de sağlar.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-110">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="ac7aa-111">Bu yaklaşım etkinleştirir daha iyi derleme, kodu denetleme zamanı kesin.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-111">This strongly typed approach enables better compile-time checking of your code.</span></span> <span data-ttu-id="ac7aa-112">(Bu, türü kesin belirlenmiş bir model geçirdiğini) Bu yaklaşımı kullanılan yapı iskelesi mekanizması `MoviesController` sınıfı ve metotları ve görünümleri oluştururken görünümleri.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-112">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="ac7aa-113">Oluşturulan inceleyin `Details` yönteminde *Controllers/MoviesController.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ac7aa-113">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end


<span data-ttu-id="ac7aa-114">`id` Parametre rota verileri genel olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-114">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="ac7aa-115">Örneğin `http://localhost:5000/movies/details/1` ayarlar:</span><span class="sxs-lookup"><span data-stu-id="ac7aa-115">For example `http://localhost:5000/movies/details/1` sets:</span></span>

* <span data-ttu-id="ac7aa-116">Denetleyiciye `movies` denetleyici (ilk URL kesimini).</span><span class="sxs-lookup"><span data-stu-id="ac7aa-116">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="ac7aa-117">Eyleme `details` (ikinci URL kesimini).</span><span class="sxs-lookup"><span data-stu-id="ac7aa-117">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="ac7aa-118">1 (son URL kesimini) kimliği.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-118">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="ac7aa-119">Ayrıca, geçirebilirsiniz `id` ile bir sorgu dizesi şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="ac7aa-119">You can also pass in the `id` with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="ac7aa-120">`id` Parametresi olarak tanımlanmış olan bir [boş değer atanabilir tür](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) durumda bir kimlik değeri sağlanmadı.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-120">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>



::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ac7aa-121">A [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) için geçirilen `FirstOrDefaultAsync` rota verileri veya sorgu dizesi değeriyle eşleşen film varlıkları seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-121">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.ID == id);
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="ac7aa-122">A [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) için geçirilen `SingleOrDefaultAsync` rota verileri veya sorgu dizesi değeriyle eşleşen film varlıkları seçin.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-122">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `SingleOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```

::: moniker-end



<span data-ttu-id="ac7aa-123">Bir filmi bulunursa örneği `Movie` modeline geçirilir `Details` görüntüle:</span><span class="sxs-lookup"><span data-stu-id="ac7aa-123">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="ac7aa-124">İçeriğini incelemek *Views/Movies/Details.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ac7aa-124">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="ac7aa-125">Ekleyerek bir `@model` deyimi görünüm dosyası üst kısmındaki görünümü bekliyor nesne türünü belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-125">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="ac7aa-126">Film denetleyicisi oluşturduğunuzda, Visual Studio otomatik olarak aşağıdaki dahil `@model` en üstündeki deyimi *Details.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ac7aa-126">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="ac7aa-127">Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen film erişmenize olanak sağlayan bir `Model` türü kesin belirlenmiş bir nesne.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-127">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="ac7aa-128">Örneğin, *Details.cshtml* görünümü, kod, her filmin alanına geçirir `DisplayNameFor` ve `DisplayFor` HTML Yardımcıları ile kesin olarak belirlenmiş `Model` nesne.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-128">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="ac7aa-129">`Create` Ve `Edit` metotları ve görünümleri de başarılı bir `Movie` model nesnesi.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-129">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="ac7aa-130">İnceleme *Index.cshtml* görünümü ve `Index` denetleyici filmler yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-130">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="ac7aa-131">Kodun nasıl oluşturduğunu fark bir `List` nesne çağırdığında `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-131">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="ac7aa-132">Bu kod geçirir `Movies` gelen listesinde `Index` eylem yöntemine görünümü:</span><span class="sxs-lookup"><span data-stu-id="ac7aa-132">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="ac7aa-133">Denetleyici filmler oluşturduğunuzda otomatik olarak iskele kurma özelliği aşağıdaki dahil `@model` en üstündeki deyimi *Index.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ac7aa-133">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="ac7aa-134">`@model` Yönergesi kullanarak görünüm tarafından geçirilen denetleyici filmler listesini erişmenize olanak sağlayan bir `Model` türü kesin belirlenmiş bir nesne.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-134">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="ac7aa-135">Örneğin, *Index.cshtml* görüntülemek, filmlerle kodunu döner bir `foreach` deyimi kesin olarak belirlenmiş üzerinden `Model` nesnesi:</span><span class="sxs-lookup"><span data-stu-id="ac7aa-135">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="ac7aa-136">Çünkü `Model` nesne türü kesin belirlenmiş (olarak bir `IEnumerable<Movie>` nesne), Döngüdeki her bir öğe olarak yazılan `Movie`.</span><span class="sxs-lookup"><span data-stu-id="ac7aa-136">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="ac7aa-137">Diğer avantajlar arasında bu derleme zamanı elde anlamına gelir kodunu denetleniyor:</span><span class="sxs-lookup"><span data-stu-id="ac7aa-137">Among other benefits, this means that you get compile-time checking of the code:</span></span>
