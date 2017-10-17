<span data-ttu-id="dcf78-101">[!code-csharp[Ana](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="dcf78-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="dcf78-102">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="dcf78-102">The preceding code:</span></span>

* <span data-ttu-id="dcf78-103">Bir boş denetleyicisi sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="dcf78-103">Defines an empty controller class.</span></span> <span data-ttu-id="dcf78-104">Sonraki bölümlerde, API uygulamaya yönelik yöntemleri ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="dcf78-104">In the next sections, we'll add methods to implement the API.</span></span>
* <span data-ttu-id="dcf78-105">Oluşturucusu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) veritabanı bağlamı eklemesine (`TodoContext `) denetleyicisi içine.</span><span class="sxs-lookup"><span data-stu-id="dcf78-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="dcf78-106">Veritabanı bağlamı her biri kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyicisi yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="dcf78-106">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="dcf78-107">Yoksa Oluşturucu bir öğe bellek içi veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="dcf78-107">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="dcf78-108">Yapılacaklar öğelerini alma</span><span class="sxs-lookup"><span data-stu-id="dcf78-108">Getting to-do items</span></span>

<span data-ttu-id="dcf78-109">Yapılacaklar öğelerini almak için aşağıdaki yöntemi ekleyin `TodoController` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="dcf78-109">To get to-do items, add the following methods to the `TodoController` class.</span></span>

<span data-ttu-id="dcf78-110">[!code-csharp[Ana](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="dcf78-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>

<span data-ttu-id="dcf78-111">Bu yöntemler iki GET yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="dcf78-111">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="dcf78-112">Bir örnek HTTP yanıtı için işte `GetAll` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="dcf78-112">Here is an example HTTP response for the `GetAll` method:</span></span>

```
HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8
   Server: Microsoft-IIS/10.0
   Date: Thu, 18 Jun 2015 20:51:10 GMT
   Content-Length: 82

   [{"Key":"1", "Name":"Item1","IsComplete":false}]
   ```

<span data-ttu-id="dcf78-113">Öğreticide daha sonra ı HTTP yanıt kullanarak nasıl görüntüleyebileceğiniz göstereceğiz [Postman](https://www.getpostman.com/) veya [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="dcf78-113">Later in the tutorial I'll show how you can view the HTTP response using [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="dcf78-114">Yönlendirme ve URL yolları</span><span class="sxs-lookup"><span data-stu-id="dcf78-114">Routing and URL paths</span></span>

<span data-ttu-id="dcf78-115">`[HttpGet]` Özniteliği, bir HTTP GET yöntemi belirtir.</span><span class="sxs-lookup"><span data-stu-id="dcf78-115">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="dcf78-116">Her bir yöntemin URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="dcf78-116">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="dcf78-117">Şablon dizesini denetleyicinin rota özniteliğinde alın:</span><span class="sxs-lookup"><span data-stu-id="dcf78-117">Take the template string in the controller’s route attribute:</span></span>

<span data-ttu-id="dcf78-118">[!code-csharp[Ana](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="dcf78-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>

* <span data-ttu-id="dcf78-119">"[Denetleyici]" denetleyici sınıfı adı "Controller" soneki eksi denetleyicinin adı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="dcf78-119">Replace "[Controller]" with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="dcf78-120">Bu örnek, denetleyici sınıfı adı olan **Yapılacaklar**denetleyicisi ve kök "todo" adıdır.</span><span class="sxs-lookup"><span data-stu-id="dcf78-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="dcf78-121">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="dcf78-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is not case sensitive.</span></span>
* <span data-ttu-id="dcf78-122">Varsa `[HttpGet]` özniteliğine sahip bir rota şablonu (gibi `[HttpGet("/products")]`, yolunu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="dcf78-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="dcf78-123">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="dcf78-123">This sample doesn't use a template.</span></span> <span data-ttu-id="dcf78-124">Bkz: [özniteliği Http [eylem] özniteliklerle yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="dcf78-124">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="dcf78-125">İçinde `GetById` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="dcf78-125">In the `GetById` method:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

<span data-ttu-id="dcf78-126">`"{id}"`Kimliği için bir yer tutucu değişkendir `todo` öğesi.</span><span class="sxs-lookup"><span data-stu-id="dcf78-126">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="dcf78-127">Zaman `GetById` olan çağrılır, "{id}" değerini yöntemin URL'yi atar `id` parametresi.</span><span class="sxs-lookup"><span data-stu-id="dcf78-127">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="dcf78-128">`Name = "GetTodo"`adlandırılmış bir yol oluşturur ve bu yolun bir HTTP yanıt bağlantı olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="dcf78-128">`Name = "GetTodo"` creates a named route and allows you to link to this route in an HTTP Response.</span></span> <span data-ttu-id="dcf78-129">I bunu bir örnekle daha sonra açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="dcf78-129">I'll explain it with an example later.</span></span> <span data-ttu-id="dcf78-130">Bkz: [denetleyici eylemleri için yönlendirme](xref:mvc/controllers/routing) ayrıntılı bilgi için.</span><span class="sxs-lookup"><span data-stu-id="dcf78-130">See [Routing to Controller Actions](xref:mvc/controllers/routing) for detailed information.</span></span>

### <a name="return-values"></a><span data-ttu-id="dcf78-131">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="dcf78-131">Return values</span></span>

<span data-ttu-id="dcf78-132">`GetAll` Yöntemi döndürür bir `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="dcf78-132">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="dcf78-133">MVC otomatik olarak serileştiren nesnesine [JSON](http://www.json.org/) ve JSON yanıt iletisi gövdesine yazar.</span><span class="sxs-lookup"><span data-stu-id="dcf78-133">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="dcf78-134">Yanıt kodu 200 bu yöntem olduğu için hiçbir işlenmeyen özel durumları varsayılarak.</span><span class="sxs-lookup"><span data-stu-id="dcf78-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="dcf78-135">(İşlenmeyen özel durumlar 5xx hatalarla karşılaşırsanız çevrilir.)</span><span class="sxs-lookup"><span data-stu-id="dcf78-135">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="dcf78-136">Buna karşılık, `GetById` yöntemi döndürür daha genel `IActionResult` çok çeşitli dönüş türleri temsil eden tür.</span><span class="sxs-lookup"><span data-stu-id="dcf78-136">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="dcf78-137">`GetById`iki farklı dönüş türü vardır:</span><span class="sxs-lookup"><span data-stu-id="dcf78-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="dcf78-138">Öğe istenen kimliği eşleşirse, yöntem 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="dcf78-138">If no item matches the requested ID, the method returns a 404 error.</span></span>  <span data-ttu-id="dcf78-139">Bu döndürerek yapılır `NotFound`.</span><span class="sxs-lookup"><span data-stu-id="dcf78-139">This is done by returning `NotFound`.</span></span>

* <span data-ttu-id="dcf78-140">Aksi takdirde, yöntemi bir JSON yanıt gövdesine ile 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="dcf78-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="dcf78-141">Bu döndürerek yapılır bir`ObjectResult`</span><span class="sxs-lookup"><span data-stu-id="dcf78-141">This is done by returning an `ObjectResult`</span></span>
