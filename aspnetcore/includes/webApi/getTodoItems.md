::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="749c4-101">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="749c4-101">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="749c4-102">Önceki kod yöntemleri olmadan bir API denetleyicisi sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="749c4-102">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="749c4-103">Sonraki bölümlerde, API uygulamak için yöntemler eklenir.</span><span class="sxs-lookup"><span data-stu-id="749c4-103">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="749c4-104">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="749c4-104">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="749c4-105">Önceki kod yöntemleri olmadan bir API denetleyicisi sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="749c4-105">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="749c4-106">Sonraki bölümlerde, API uygulamak için yöntemler eklenir.</span><span class="sxs-lookup"><span data-stu-id="749c4-106">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="749c4-107">Sınıf ile Açıklama bir `[ApiController]` uygun bazı özellikleri etkinleştirmek için öznitelik.</span><span class="sxs-lookup"><span data-stu-id="749c4-107">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="749c4-108">Özniteliği tarafından etkin özellikler hakkında daha fazla bilgi için bkz: [ApiControllerAttribute sınıfıyla açıklama](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="749c4-108">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="749c4-109">Denetleyicinin Oluşturucusu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) veritabanı bağlamı eklemesine (`TodoContext`) denetleyicisi içine.</span><span class="sxs-lookup"><span data-stu-id="749c4-109">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="749c4-110">Veritabanı bağlamı her biri kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyicisi yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="749c4-110">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="749c4-111">Yoksa Oluşturucu bir öğe bellek içi veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="749c4-111">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="749c4-112">Yapılacaklar öğelerini alma</span><span class="sxs-lookup"><span data-stu-id="749c4-112">Get to-do items</span></span>

<span data-ttu-id="749c4-113">Yapılacaklar öğelerini almak için aşağıdaki yöntemi ekleyin `TodoController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="749c4-113">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="749c4-114">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="749c4-114">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="749c4-115">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="749c4-115">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>
::: moniker-end

<span data-ttu-id="749c4-116">Bu yöntemler iki GET yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="749c4-116">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="749c4-117">Bir örnek HTTP yanıtı için işte `GetAll` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="749c4-117">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="749c4-118">Öğreticide daha sonra t ile HTTP yanıtı nasıl görüntülenebilir göstereceğiz [Postman](https://www.getpostman.com/) veya [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="749c4-118">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="749c4-119">Yönlendirme ve URL yolları</span><span class="sxs-lookup"><span data-stu-id="749c4-119">Routing and URL paths</span></span>

<span data-ttu-id="749c4-120">`[HttpGet]` Öznitelik bir HTTP GET isteğine yanıt bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="749c4-120">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="749c4-121">Her bir yöntemin URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="749c4-121">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="749c4-122">Denetleyicinin şablonu dizesi ele `Route` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="749c4-122">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="749c4-123">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="749c4-123">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="749c4-124">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="749c4-124">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>
::: moniker-end

* <span data-ttu-id="749c4-125">Değiştir `[controller]` denetleyicinin adı ile olduğu "Controller" soneki eksi denetleyici sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="749c4-125">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="749c4-126">Bu örnek, denetleyici sınıfı adı olan **Yapılacaklar**denetleyicisi ve kök "todo" adıdır.</span><span class="sxs-lookup"><span data-stu-id="749c4-126">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="749c4-127">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="749c4-127">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="749c4-128">Varsa `[HttpGet]` özniteliğine sahip bir rota şablonu (gibi `[HttpGet("/products")]`, yolunu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="749c4-128">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="749c4-129">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="749c4-129">This sample doesn't use a template.</span></span> <span data-ttu-id="749c4-130">Daha fazla bilgi için bkz: [özniteliği Http [eylem] özniteliklerle yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="749c4-130">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="749c4-131">Aşağıdaki `GetById` yöntemi, `"{id}"` yapılacak iş öğesinin benzersiz tanımlayıcısı için yer tutucu değişkenidir.</span><span class="sxs-lookup"><span data-stu-id="749c4-131">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="749c4-132">Zaman `GetById` olan çağrılan değeri atar `"{id}"` yöntemin URL'yi `id` parametresi.</span><span class="sxs-lookup"><span data-stu-id="749c4-132">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="749c4-133">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="749c4-133">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="749c4-134">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="749c4-134">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

<span data-ttu-id="749c4-135">`Name = "GetTodo"` adlandırılmış bir yol oluşturur.</span><span class="sxs-lookup"><span data-stu-id="749c4-135">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="749c4-136">Adlandırılmış yollar:</span><span class="sxs-lookup"><span data-stu-id="749c4-136">Named routes:</span></span>

* <span data-ttu-id="749c4-137">Rota adını kullanarak bir HTTP bağlantısı oluşturmak için uygulama etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="749c4-137">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="749c4-138">Daha sonra öğreticide açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="749c4-138">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="749c4-139">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="749c4-139">Return values</span></span>

<span data-ttu-id="749c4-140">`GetAll` Yöntem koleksiyonu döndürür `TodoItem` nesneleri.</span><span class="sxs-lookup"><span data-stu-id="749c4-140">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="749c4-141">MVC otomatik olarak serileştiren nesnesine [JSON](https://www.json.org/) ve JSON yanıt iletisi gövdesine yazar.</span><span class="sxs-lookup"><span data-stu-id="749c4-141">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="749c4-142">Yanıt kodu 200 bu yöntem olduğu için hiçbir işlenmeyen özel durumları varsayılarak.</span><span class="sxs-lookup"><span data-stu-id="749c4-142">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="749c4-143">İşlenmeyen özel durumlar 5xx hatalarla karşılaşırsanız çevrilir.</span><span class="sxs-lookup"><span data-stu-id="749c4-143">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="749c4-144">Buna karşılık, `GetById` yöntemi döndürür daha genel [IActionResult türü](xref:web-api/action-return-types#iactionresult-type), çok çeşitli dönüş türleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="749c4-144">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="749c4-145">`GetById` iki farklı dönüş türü vardır:</span><span class="sxs-lookup"><span data-stu-id="749c4-145">`GetById` has two different return types:</span></span>

* <span data-ttu-id="749c4-146">Öğe istenen kimliği eşleşirse, yöntem 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="749c4-146">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="749c4-147">Döndürme [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) bir HTTP 404 yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="749c4-147">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="749c4-148">Aksi takdirde, yöntemi bir JSON yanıt gövdesine ile 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="749c4-148">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="749c4-149">Döndürme [Tamam](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) sonuçları bir HTTP 200 yanıtı.</span><span class="sxs-lookup"><span data-stu-id="749c4-149">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="749c4-150">Buna karşılık, `GetById` yöntemi döndürür [ActionResult\<T > türü](xref:web-api/action-return-types#actionresultt-type), çok çeşitli dönüş türleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="749c4-150">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="749c4-151">`GetById` iki farklı dönüş türü vardır:</span><span class="sxs-lookup"><span data-stu-id="749c4-151">`GetById` has two different return types:</span></span>

* <span data-ttu-id="749c4-152">Öğe istenen kimliği eşleşirse, yöntem 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="749c4-152">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="749c4-153">Döndürme [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) bir HTTP 404 yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="749c4-153">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="749c4-154">Aksi takdirde, yöntemi bir JSON yanıt gövdesine ile 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="749c4-154">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="749c4-155">Döndürme `item` sonuçları bir HTTP 200 yanıtı.</span><span class="sxs-lookup"><span data-stu-id="749c4-155">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end