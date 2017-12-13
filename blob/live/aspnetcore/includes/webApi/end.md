## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="f94ad-101">Diğer CRUD işlemleri uygulama</span><span class="sxs-lookup"><span data-stu-id="f94ad-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="f94ad-102">Aşağıdaki bölümlerde `Create`, `Update`, ve `Delete` yöntemleri, denetleyiciye eklenir.</span><span class="sxs-lookup"><span data-stu-id="f94ad-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="f94ad-103">Create</span><span class="sxs-lookup"><span data-stu-id="f94ad-103">Create</span></span>

<span data-ttu-id="f94ad-104">Aşağıdakileri ekleyin `Create` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f94ad-104">Add the following `Create` method.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="f94ad-105">Önceki kod tarafından gösterilen bir HTTP POST yöntemi olup [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f94ad-105">The preceding code is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="f94ad-106">[ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Özniteliği HTTP isteği gövdesinden Yapılacaklar öğesi değerini almak için MVC söyler.</span><span class="sxs-lookup"><span data-stu-id="f94ad-106">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="f94ad-107">`CreatedAtRoute` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f94ad-107">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="f94ad-108">201 yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="f94ad-108">Returns a 201 response.</span></span> <span data-ttu-id="f94ad-109">HTTP 201 yeni bir kaynak sunucuda oluşturan bir HTTP POST yöntemi için standart yanıt'dir.</span><span class="sxs-lookup"><span data-stu-id="f94ad-109">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="f94ad-110">Bir konum üstbilgisi yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="f94ad-110">Adds a Location header to the response.</span></span> <span data-ttu-id="f94ad-111">Konum üstbilgisi yeni oluşturulan Yapılacaklar öğesi URI'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="f94ad-111">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="f94ad-112">Bkz: [10.2.2 oluşturulan 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="f94ad-112">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="f94ad-113">URL oluşturmak için "adlı rota GetTodo" kullanır.</span><span class="sxs-lookup"><span data-stu-id="f94ad-113">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="f94ad-114">"Rota adlı GetTodo" tanımlanan `GetById`:</span><span class="sxs-lookup"><span data-stu-id="f94ad-114">The "GetTodo" named route is defined in `GetById`:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="f94ad-115">Postman oluşturma isteği göndermek için kullanın</span><span class="sxs-lookup"><span data-stu-id="f94ad-115">Use Postman to send a Create request</span></span>

![Postman konsol](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="f94ad-117">HTTP yöntemini ayarlayın`POST`</span><span class="sxs-lookup"><span data-stu-id="f94ad-117">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="f94ad-118">Seçin **gövde** radyo düğmesi</span><span class="sxs-lookup"><span data-stu-id="f94ad-118">Select the **Body** radio button</span></span>
* <span data-ttu-id="f94ad-119">Seçin **ham** radyo düğmesi</span><span class="sxs-lookup"><span data-stu-id="f94ad-119">Select the **raw** radio button</span></span>
* <span data-ttu-id="f94ad-120">Türü için JSON ayarlayın</span><span class="sxs-lookup"><span data-stu-id="f94ad-120">Set the type to JSON</span></span>
* <span data-ttu-id="f94ad-121">Anahtar-değer Düzenleyicisi'nde bir Yapılacaklar öğesi gibi girin</span><span class="sxs-lookup"><span data-stu-id="f94ad-121">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="f94ad-122">Seçin **Gönder**</span><span class="sxs-lookup"><span data-stu-id="f94ad-122">Select **Send**</span></span>
* <span data-ttu-id="f94ad-123">Alt bölme ve kopyalama Üstbilgileri sekmesini seçin **konumu** üstbilgisi:</span><span class="sxs-lookup"><span data-stu-id="f94ad-123">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Postman konsolunun üst bilgiler sekmesi](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="f94ad-125">Konum üstbilgisi URI yeni öğe erişmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f94ad-125">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="f94ad-126">Güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="f94ad-126">Update</span></span>

<span data-ttu-id="f94ad-127">Aşağıdakileri ekleyin `Update` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f94ad-127">Add the following `Update` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="f94ad-128">`Update`benzer `Create`, ancak HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="f94ad-128">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="f94ad-129">Yanıt [204 (No içerik)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f94ad-129">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f94ad-130">HTTP spec göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca farkları göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f94ad-130">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="f94ad-131">Kısmi güncelleştirmeler desteklemek için HTTP PATCH kullanın.</span><span class="sxs-lookup"><span data-stu-id="f94ad-131">To support partial updates, use HTTP PATCH.</span></span>

![204 (No içerik) yanıt gösteren postman konsol](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="f94ad-133">Sil</span><span class="sxs-lookup"><span data-stu-id="f94ad-133">Delete</span></span>

<span data-ttu-id="f94ad-134">Aşağıdakileri ekleyin `Delete` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f94ad-134">Add the following `Delete` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="f94ad-135">`Delete` Yanıt [204 (No içerik)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f94ad-135">The `Delete` response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="f94ad-136">Test `Delete`:</span><span class="sxs-lookup"><span data-stu-id="f94ad-136">Test `Delete`:</span></span> 

![204 (No içerik) yanıt gösteren postman konsol](../../tutorials/first-web-api/_static/pmd.png)
