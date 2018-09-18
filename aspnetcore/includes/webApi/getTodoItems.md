::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="e0071-101">Yukarıdaki kod, bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e0071-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="e0071-102">Sonraki bölümde, API'yi uygulamak için yöntemleri eklenir.</span><span class="sxs-lookup"><span data-stu-id="e0071-102">In the next sections, methods are added to implement the API.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="e0071-103">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="e0071-103">The preceding code:</span></span>

* <span data-ttu-id="e0071-104">Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e0071-104">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="e0071-105">Yeni bir Todo oluşturur öğe `TodoItems` boştur.</span><span class="sxs-lookup"><span data-stu-id="e0071-105">Creates a new Todo item when `TodoItems` is empty.</span></span> <span data-ttu-id="e0071-106">Tüm Oluşturucu bir yeni bir IF oluşturduğundan Todo öğelerini silmek mümkün olmayacaktır `TodoItems` boştur.</span><span class="sxs-lookup"><span data-stu-id="e0071-106">You won't be able to delete all the Todo items because the constructor creates a new one if `TodoItems` is empty.</span></span>

<span data-ttu-id="e0071-107">Sonraki bölümde, API'yi uygulamak için yöntemleri eklenir.</span><span class="sxs-lookup"><span data-stu-id="e0071-107">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="e0071-108">Sınıf ile açıklanıyor bir `[ApiController]` kullanışlı bazı özellikleri etkinleştirmek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e0071-108">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="e0071-109">Özniteliği tarafından etkinleştirilen özellikler hakkında daha fazla bilgi için bkz: [ApiControllerAttribute sınıfıyla ek açıklama](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="e0071-109">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>

::: moniker-end

<span data-ttu-id="e0071-110">Denetleyicinin Oluşturucu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) veritabanı bağlamı eklemesine (`TodoContext`) içine denetleyici.</span><span class="sxs-lookup"><span data-stu-id="e0071-110">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="e0071-111">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="e0071-111">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="e0071-112">Yoksa Oluşturucu bir öğe bellek içi veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="e0071-112">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="e0071-113">Yapılacaklar öğelerini alma</span><span class="sxs-lookup"><span data-stu-id="e0071-113">Get to-do items</span></span>

<span data-ttu-id="e0071-114">Yapılacaklar öğelerini almak için aşağıdaki yöntemi ekleyin. `TodoController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="e0071-114">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

<span data-ttu-id="e0071-115">Bu yöntemler, iki GET yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="e0071-115">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="e0071-116">Örnek HTTP yanıtı için `GetAll` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e0071-116">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="e0071-117">Öğreticinin sonraki bölümlerinde miyim ile HTTP yanıtının nasıl görüntülenebilir göstereceğiz [Postman](https://www.getpostman.com/) veya [curl](https://curl.haxx.se/docs/manpage.html).</span><span class="sxs-lookup"><span data-stu-id="e0071-117">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://curl.haxx.se/docs/manpage.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="e0071-118">URL Yönlendirme ve yolları</span><span class="sxs-lookup"><span data-stu-id="e0071-118">Routing and URL paths</span></span>

<span data-ttu-id="e0071-119">`[HttpGet]` Özniteliği bir HTTP GET isteğine yanıt vermeden bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="e0071-119">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="e0071-120">Her yöntem için URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="e0071-120">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e0071-121">Denetleyicinin şablonu dizesi ele `Route` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="e0071-121">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

* <span data-ttu-id="e0071-122">Değiştirin `[controller]` denetleyicinin adı olduğu "Controller" soneki eksi denetleyici sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="e0071-122">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e0071-123">Bu örnek, denetleyici sınıfı adı olan **Todo**denetleyicisi ve kök "todo" adıdır.</span><span class="sxs-lookup"><span data-stu-id="e0071-123">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="e0071-124">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="e0071-124">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="e0071-125">Varsa `[HttpGet]` özniteliğine sahip bir rota şablonu (gibi `[HttpGet("/products")]`, yolunu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e0071-125">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="e0071-126">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="e0071-126">This sample doesn't use a template.</span></span> <span data-ttu-id="e0071-127">Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="e0071-127">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e0071-128">Aşağıdaki `GetById` yöntemi `"{id}"` yapılacak iş öğesi benzersiz tanımlayıcısı için bir yer tutucu değişkendir.</span><span class="sxs-lookup"><span data-stu-id="e0071-128">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="e0071-129">Zaman `GetById` olan çağrılır, değeri atar `"{id}"` yöntemin URL'yi `id` parametresi.</span><span class="sxs-lookup"><span data-stu-id="e0071-129">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

<span data-ttu-id="e0071-130">`Name = "GetTodo"` adlandırılmış bir yol oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e0071-130">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="e0071-131">Adlandırılmış rotalar:</span><span class="sxs-lookup"><span data-stu-id="e0071-131">Named routes:</span></span>

* <span data-ttu-id="e0071-132">Rota adını kullanarak bir HTTP bağlantısı oluşturmak uygulamayı etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="e0071-132">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="e0071-133">Öğreticinin ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e0071-133">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="e0071-134">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="e0071-134">Return values</span></span>

<span data-ttu-id="e0071-135">`GetAll` Yöntem koleksiyonu döndürür `TodoItem` nesneleri.</span><span class="sxs-lookup"><span data-stu-id="e0071-135">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="e0071-136">MVC nesneyi otomatik olarak serileştiren [JSON](https://www.json.org/) ve yanıt iletisinin gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="e0071-136">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e0071-137">Yanıt kodu 200 olarak bu yöntem için işlenmeyen özel durumlar vardır varsayılıyor.</span><span class="sxs-lookup"><span data-stu-id="e0071-137">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e0071-138">İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.</span><span class="sxs-lookup"><span data-stu-id="e0071-138">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="e0071-139">Buna karşılık, `GetById` yöntemi döndürür daha genel [IActionResult türü](xref:web-api/action-return-types#iactionresult-type), dönüş türleri çeşitli temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e0071-139">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="e0071-140">`GetById` iki farklı dönüş türlerine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="e0071-140">`GetById` has two different return types:</span></span>

* <span data-ttu-id="e0071-141">Öğe istenen kimliği eşleşirse, yöntem, 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="e0071-141">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="e0071-142">Döndüren [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) bir HTTP 404 yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="e0071-142">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="e0071-143">Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="e0071-143">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e0071-144">Döndüren [Tamam](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) sonuçları bir HTTP 200 yanıtı.</span><span class="sxs-lookup"><span data-stu-id="e0071-144">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e0071-145">Buna karşılık, `GetById` yöntemi döndürür [actionresult öğesini\<T > türü](xref:web-api/action-return-types#actionresultt-type), dönüş türleri çeşitli temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e0071-145">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="e0071-146">`GetById` iki farklı dönüş türlerine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="e0071-146">`GetById` has two different return types:</span></span>

* <span data-ttu-id="e0071-147">Öğe istenen kimliği eşleşirse, yöntem, 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="e0071-147">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="e0071-148">Döndüren [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) bir HTTP 404 yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="e0071-148">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="e0071-149">Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="e0071-149">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e0071-150">Döndüren `item` sonuçları bir HTTP 200 yanıtı.</span><span class="sxs-lookup"><span data-stu-id="e0071-150">Returning `item` results in an HTTP 200 response.</span></span>

::: moniker-end
