<span data-ttu-id="bd58a-101">Vurgulanan gösterildiği eklenmesini film veritabanı bağlamı kod [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı (içinde *haline* dosyası).</span><span class="sxs-lookup"><span data-stu-id="bd58a-101">The highlighted code above shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container (In the *Startup.cs* file).</span></span> <span data-ttu-id="bd58a-102">`services.AddDbContext<MvcMovieContext>(options =>` Kullanım ve bağlantı dizesi veritabanına belirtir.</span><span class="sxs-lookup"><span data-stu-id="bd58a-102">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span> <span data-ttu-id="bd58a-103">`=>` olan bir [lambda işleci](/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span><span class="sxs-lookup"><span data-stu-id="bd58a-103">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span></span>

<span data-ttu-id="bd58a-104">Açık *Controllers/MoviesController.cs* dosya ve Oluşturucusu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="bd58a-104">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

<span data-ttu-id="bd58a-105">Oluşturucusu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) veritabanı bağlamı eklemesine (`MvcMovieContext `) denetleyicisi içine.</span><span class="sxs-lookup"><span data-stu-id="bd58a-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext `) into the controller.</span></span> <span data-ttu-id="bd58a-106">Veritabanı bağlamı her biri kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyicisi yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="bd58a-106">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="bd58a-107">Modelleri'kesin türü belirtilmiş ve @model anahtar sözcüğü</span><span class="sxs-lookup"><span data-stu-id="bd58a-107">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="bd58a-108">Bu öğreticide daha önce nasıl bir denetleyici veri veya nesneler kullanarak bir görünüm geçirebilirsiniz gördüğünüzü `ViewData` sözlük.</span><span class="sxs-lookup"><span data-stu-id="bd58a-108">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="bd58a-109">`ViewData` Sözlük olan bir görünüme bilgi geçirmek için kullanışlı bir geç bağlama yol sağlayan dinamik bir nesne.</span><span class="sxs-lookup"><span data-stu-id="bd58a-109">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="bd58a-110">MVC, model nesneleri bir görünüme kesin geçirme özelliği yazılan de sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd58a-110">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="bd58a-111">Bu yaklaşım etkinleştirir daha iyi kodunuzu denetleme zamanı kesin türü belirtilmiş.</span><span class="sxs-lookup"><span data-stu-id="bd58a-111">This strongly typed approach enables better compile-time checking of your code.</span></span> <span data-ttu-id="bd58a-112">Yapı iskelesi mekanizması (kesin türü belirtilmiş bir model geçen) Bu yaklaşım kullanılan `MoviesController` sınıfı ve yöntemleri ve görünümler oluşturduğunuzda görünümleri.</span><span class="sxs-lookup"><span data-stu-id="bd58a-112">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="bd58a-113">Oluşturulan inceleyin `Details` yönteminde *Controllers/MoviesController.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="bd58a-113">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="bd58a-114">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="bd58a-114">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="bd58a-115">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="bd58a-115">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span></span>
::: moniker-end


<span data-ttu-id="bd58a-116">`id` Parametre genellikle rota verileri olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="bd58a-116">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="bd58a-117">Örneğin `http://localhost:5000/movies/details/1` ayarlar:</span><span class="sxs-lookup"><span data-stu-id="bd58a-117">For example `http://localhost:5000/movies/details/1` sets:</span></span>

* <span data-ttu-id="bd58a-118">Denetleyiciye `movies` denetleyici (ilk URL kesimi).</span><span class="sxs-lookup"><span data-stu-id="bd58a-118">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="bd58a-119">Eyleme `details` (ikinci URL kesimi).</span><span class="sxs-lookup"><span data-stu-id="bd58a-119">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="bd58a-120">1 (son URL kesimi) kimliği.</span><span class="sxs-lookup"><span data-stu-id="bd58a-120">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="bd58a-121">İçinde geçebilen `id` içeren bir sorgu dizesi şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="bd58a-121">You can also pass in the `id` with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="bd58a-122">`id` Parametre olarak tanımlanmış bir [boş değer atanabilir tür](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) durumda bir kimlik değeri sağlanmadı.</span><span class="sxs-lookup"><span data-stu-id="bd58a-122">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>



::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bd58a-123">A [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) için geçirilen `FirstOrDefaultAsync` rota verileri veya sorgu dize değerinin eşleşmesi film varlıkları seçin.</span><span class="sxs-lookup"><span data-stu-id="bd58a-123">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.ID == id);
```
::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="bd58a-124">A [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) için geçirilen `SingleOrDefaultAsync` rota verileri veya sorgu dize değerinin eşleşmesi film varlıkları seçin.</span><span class="sxs-lookup"><span data-stu-id="bd58a-124">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `SingleOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```
::: moniker-end



<span data-ttu-id="bd58a-125">Bir filmi bulunursa örneği `Movie` modeli iletilir `Details` görünümü:</span><span class="sxs-lookup"><span data-stu-id="bd58a-125">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="bd58a-126">İçeriğini incelemek *Views/Movies/Details.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="bd58a-126">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="bd58a-127">Ekleyerek bir `@model` deyimini dosyanın üst kısmındaki görünümü Görünüm bekliyor nesne türünü belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bd58a-127">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="bd58a-128">Film denetleyicisini oluşturduğunuzda, Visual Studio otomatik olarak aşağıdaki dahil `@model` deyimi en üstündeki *Details.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="bd58a-128">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="bd58a-129">Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen film erişmenize olanak sağlayan bir `Model` kesin türü belirtilmiş nesnesi.</span><span class="sxs-lookup"><span data-stu-id="bd58a-129">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="bd58a-130">Örneğin, *Details.cshtml* görünümü, kodu her film alanına geçirir `DisplayNameFor` ve `DisplayFor` HTML Yardımcıları ile güçlü şekilde yazılan `Model` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="bd58a-130">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="bd58a-131">`Create` Ve `Edit` yöntemlere ve görünümleri de geçirmek bir `Movie` model nesnesi.</span><span class="sxs-lookup"><span data-stu-id="bd58a-131">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="bd58a-132">İncelemek *Index.cshtml* Görünüm ve `Index` filmler denetleyicisi yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bd58a-132">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="bd58a-133">Kodu nasıl oluşturduğunu fark bir `List` nesne çağırdığında `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bd58a-133">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="bd58a-134">Bu kod iletir `Movies` dan listesinde `Index` eylem yöntemi görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="bd58a-134">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="bd58a-135">Film denetleyicisini oluşturduğunuzda, otomatik olarak iskele aşağıdaki dahil `@model` deyimi en üstündeki *Index.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="bd58a-135">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="bd58a-136">`@model` Yönergesi kullanarak denetleyici görünüm tarafından geçirilen filmler listesi erişmenize olanak sağlayan bir `Model` kesin türü belirtilmiş nesnesi.</span><span class="sxs-lookup"><span data-stu-id="bd58a-136">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="bd58a-137">Örneğin, *Index.cshtml* görüntülemek, kod döngüler ile filmler aracılığıyla bir `foreach` kesin türü belirtilmiş üzerinden deyimi `Model` nesnesi:</span><span class="sxs-lookup"><span data-stu-id="bd58a-137">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="bd58a-138">Çünkü `Model` nesne kesin türü belirtilmiş (olarak bir `IEnumerable<Movie>` nesnesi), döngünün her öğe olarak yazılan `Movie`.</span><span class="sxs-lookup"><span data-stu-id="bd58a-138">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="bd58a-139">Diğer avantajlar arasında bu derleme zamanı elde anlamına gelir kodunu denetleme:</span><span class="sxs-lookup"><span data-stu-id="bd58a-139">Among other benefits, this means that you get compile-time checking of the code:</span></span>
