## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="0f623-101">Diğer CRUD işlemleri uygulama</span><span class="sxs-lookup"><span data-stu-id="0f623-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="0f623-102">Ekleyeceğiz `Create`, `Update`, ve `Delete` denetleyiciye yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="0f623-102">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="0f623-103">I yalnızca kodu gösterir ve temel farklar vurgulayın bunlar bir tema çeşidi olduğundan.</span><span class="sxs-lookup"><span data-stu-id="0f623-103">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="0f623-104">Ekleme veya kod değiştirdikten sonra projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0f623-104">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="0f623-105">Create</span><span class="sxs-lookup"><span data-stu-id="0f623-105">Create</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="0f623-106">Bu belirttiği bir HTTP POST yöntemi olup [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f623-106">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="0f623-107">[ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Özniteliği HTTP isteği gövdesinden Yapılacaklar öğesi değerini almak için MVC söyler.</span><span class="sxs-lookup"><span data-stu-id="0f623-107">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="0f623-108">`CreatedAtRoute` Yöntemi yeni bir kaynak sunucuda oluşturan bir HTTP POST yöntemi için standart yanıt 201 bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="0f623-108">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="0f623-109">`CreatedAtRoute`Ayrıca bir konum üstbilgisi yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="0f623-109">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="0f623-110">Konum üstbilgisi yeni oluşturulan Yapılacaklar öğesi URI'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="0f623-110">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="0f623-111">Bkz: [10.2.2 oluşturulan 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="0f623-111">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="0f623-112">Postman oluşturma isteği göndermek için kullanın</span><span class="sxs-lookup"><span data-stu-id="0f623-112">Use Postman to send a Create request</span></span>

![Postman konsol](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="0f623-114">HTTP yöntemini ayarlayın`POST`</span><span class="sxs-lookup"><span data-stu-id="0f623-114">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="0f623-115">Seçin **gövde** radyo düğmesi</span><span class="sxs-lookup"><span data-stu-id="0f623-115">Select the **Body** radio button</span></span>
* <span data-ttu-id="0f623-116">Seçin **ham** radyo düğmesi</span><span class="sxs-lookup"><span data-stu-id="0f623-116">Select the **raw** radio button</span></span>
* <span data-ttu-id="0f623-117">Türü için JSON ayarlayın</span><span class="sxs-lookup"><span data-stu-id="0f623-117">Set the type to JSON</span></span>
* <span data-ttu-id="0f623-118">Anahtar-değer Düzenleyicisi'nde bir Yapılacaklar öğesi gibi girin</span><span class="sxs-lookup"><span data-stu-id="0f623-118">In the key-value editor, enter a Todo item such as</span></span> 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="0f623-119">Seçin **Gönder**</span><span class="sxs-lookup"><span data-stu-id="0f623-119">Select **Send**</span></span>

* <span data-ttu-id="0f623-120">Alt bölme ve kopyalama Üstbilgileri sekmesini seçin **konumu** üstbilgisi:</span><span class="sxs-lookup"><span data-stu-id="0f623-120">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Postman konsolunun üst bilgiler sekmesi](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="0f623-122">Yeni oluşturduğunuz kaynağa erişmek için konum üstbilgisi URI kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0f623-122">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="0f623-123">Geri çağırma `GetById` oluşturulan yöntemi `"GetTodo"` rota adlı:</span><span class="sxs-lookup"><span data-stu-id="0f623-123">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a><span data-ttu-id="0f623-124">Güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="0f623-124">Update</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="0f623-125">`Update`benzer `Create`, ancak HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="0f623-125">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="0f623-126">Yanıt [204 (No içerik)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="0f623-126">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="0f623-127">HTTP spec göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca farkları göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="0f623-127">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="0f623-128">Kısmi güncelleştirmeler desteklemek için HTTP PATCH kullanın.</span><span class="sxs-lookup"><span data-stu-id="0f623-128">To support partial updates, use HTTP PATCH.</span></span>

![204 (No içerik) yanıt gösteren postman konsol](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="0f623-130">Sil</span><span class="sxs-lookup"><span data-stu-id="0f623-130">Delete</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="0f623-131">Yanıt [204 (No içerik)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="0f623-131">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![204 (No içerik) yanıt gösteren postman konsol](../../tutorials/first-web-api/_static/pmd.png)
