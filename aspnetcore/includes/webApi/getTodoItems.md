[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="081d4-101">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="081d4-101">The preceding code:</span></span>

* <span data-ttu-id="081d4-102">Bir boş denetleyicisi sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="081d4-102">Defines an empty controller class.</span></span> <span data-ttu-id="081d4-103">Sonraki bölümlerde, API uygulamak için yöntemler eklenir.</span><span class="sxs-lookup"><span data-stu-id="081d4-103">In the next sections, methods are added to implement the API.</span></span>
* <span data-ttu-id="081d4-104">Oluşturucusu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) veritabanı bağlamı eklemesine (`TodoContext `) denetleyicisi içine.</span><span class="sxs-lookup"><span data-stu-id="081d4-104">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="081d4-105">Veritabanı bağlamı her biri kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyicisi yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="081d4-105">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="081d4-106">Yoksa Oluşturucu bir öğe bellek içi veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="081d4-106">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="081d4-107">Yapılacaklar öğelerini alma</span><span class="sxs-lookup"><span data-stu-id="081d4-107">Getting to-do items</span></span>

<span data-ttu-id="081d4-108">Yapılacaklar öğelerini almak için aşağıdaki yöntemi ekleyin `TodoController` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="081d4-108">To get to-do items, add the following methods to the `TodoController` class.</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="081d4-109">Bu yöntemler iki GET yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="081d4-109">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="081d4-110">Bir örnek HTTP yanıtı için işte `GetAll` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="081d4-110">Here is an example HTTP response for the `GetAll` method:</span></span>

```
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
   ```

<span data-ttu-id="081d4-111">Öğreticide daha sonra t ile HTTP yanıtı nasıl görüntülenebilir göstereceğiz [Postman](https://www.getpostman.com/) veya [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="081d4-111">Later in the tutorial I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="081d4-112">Yönlendirme ve URL yolları</span><span class="sxs-lookup"><span data-stu-id="081d4-112">Routing and URL paths</span></span>

<span data-ttu-id="081d4-113">`[HttpGet]` Özniteliği, bir HTTP GET yöntemi belirtir.</span><span class="sxs-lookup"><span data-stu-id="081d4-113">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="081d4-114">Her bir yöntemin URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="081d4-114">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="081d4-115">Denetleyicinin şablonu dizesi ele `Route` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="081d4-115">Take the template string in the controller's `Route` attribute:</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="081d4-116">Değiştirir `[controller]` denetleyicinin adı ile olduğu "Controller" soneki eksi denetleyici sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="081d4-116">Replaces `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="081d4-117">Bu örnek, denetleyici sınıfı adı olan **Yapılacaklar**denetleyicisi ve kök "todo" adıdır.</span><span class="sxs-lookup"><span data-stu-id="081d4-117">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="081d4-118">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="081d4-118">ASP.NET Core [routing](xref:mvc/controllers/routing) isn't case sensitive.</span></span>
* <span data-ttu-id="081d4-119">Varsa `[HttpGet]` özniteliğine sahip bir rota şablonu (gibi `[HttpGet("/products")]`, yolunu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="081d4-119">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="081d4-120">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="081d4-120">This sample doesn't use a template.</span></span> <span data-ttu-id="081d4-121">Bkz: [özniteliği Http [eylem] özniteliklerle yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="081d4-121">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="081d4-122">İçinde `GetById` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="081d4-122">In the `GetById` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="081d4-123">`"{id}"` Kimliği için bir yer tutucu değişkendir `todo` öğesi.</span><span class="sxs-lookup"><span data-stu-id="081d4-123">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="081d4-124">Zaman `GetById` olan çağrılır, "{id}" değerini yöntemin URL'yi atar `id` parametresi.</span><span class="sxs-lookup"><span data-stu-id="081d4-124">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="081d4-125">`Name = "GetTodo"` adlandırılmış bir yol oluşturur.</span><span class="sxs-lookup"><span data-stu-id="081d4-125">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="081d4-126">Adlandırılmış yollar:</span><span class="sxs-lookup"><span data-stu-id="081d4-126">Named routes:</span></span>

* <span data-ttu-id="081d4-127">Rota adını kullanarak bir HTTP bağlantısı oluşturmak için uygulama etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="081d4-127">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="081d4-128">Daha sonra öğreticide açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="081d4-128">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="081d4-129">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="081d4-129">Return values</span></span>

<span data-ttu-id="081d4-130">`GetAll` Yöntemi döndürür bir `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="081d4-130">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="081d4-131">MVC otomatik olarak serileştiren nesnesine [JSON](http://www.json.org/) ve JSON yanıt iletisi gövdesine yazar.</span><span class="sxs-lookup"><span data-stu-id="081d4-131">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="081d4-132">Yanıt kodu 200 bu yöntem olduğu için hiçbir işlenmeyen özel durumları varsayılarak.</span><span class="sxs-lookup"><span data-stu-id="081d4-132">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="081d4-133">(İşlenmeyen özel durumlar 5xx hatalarla karşılaşırsanız çevrilir.)</span><span class="sxs-lookup"><span data-stu-id="081d4-133">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="081d4-134">Buna karşılık, `GetById` yöntemi döndürür daha genel `IActionResult` çok çeşitli dönüş türleri temsil eden tür.</span><span class="sxs-lookup"><span data-stu-id="081d4-134">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="081d4-135">`GetById` iki farklı dönüş türü vardır:</span><span class="sxs-lookup"><span data-stu-id="081d4-135">`GetById` has two different return types:</span></span>

* <span data-ttu-id="081d4-136">Öğe istenen kimliği eşleşirse, yöntem 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="081d4-136">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="081d4-137">Döndürme `NotFound` bir HTTP 404 yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="081d4-137">Returning `NotFound` returns an HTTP 404 response.</span></span>

* <span data-ttu-id="081d4-138">Aksi takdirde, yöntemi bir JSON yanıt gövdesine ile 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="081d4-138">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="081d4-139">Döndürme `ObjectResult` bir HTTP 200 yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="081d4-139">Returning `ObjectResult` returns an HTTP 200 response.</span></span>
