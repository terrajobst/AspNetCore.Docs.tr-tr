## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="4b929-101">Diğer CRUD işlemleri uygulama</span><span class="sxs-lookup"><span data-stu-id="4b929-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="4b929-102">Aşağıdaki bölümlerde `Create`, `Update`, ve `Delete` yöntemleri, denetleyiciye eklenir.</span><span class="sxs-lookup"><span data-stu-id="4b929-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="4b929-103">Create</span><span class="sxs-lookup"><span data-stu-id="4b929-103">Create</span></span>

<span data-ttu-id="4b929-104">Aşağıdakileri ekleyin `Create` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4b929-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="4b929-105">Önceki kod belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4b929-105">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="4b929-106">[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) özniteliği HTTP isteği gövdesinden Yapılacaklar öğesi değerini almak için MVC söyler.</span><span class="sxs-lookup"><span data-stu-id="4b929-106">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="4b929-107">Önceki kod belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4b929-107">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="4b929-108">MVC HTTP isteği gövdesinden Yapılacaklar öğesi değerini alır.</span><span class="sxs-lookup"><span data-stu-id="4b929-108">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="4b929-109">`CreatedAtRoute` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4b929-109">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="4b929-110">201 yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="4b929-110">Returns a 201 response.</span></span> <span data-ttu-id="4b929-111">HTTP 201 yeni bir kaynak sunucuda oluşturan bir HTTP POST yöntemi için standart yanıt'dir.</span><span class="sxs-lookup"><span data-stu-id="4b929-111">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="4b929-112">Bir konum üstbilgisi yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="4b929-112">Adds a Location header to the response.</span></span> <span data-ttu-id="4b929-113">Konum üstbilgisi yeni oluşturulan Yapılacaklar öğesi URI'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="4b929-113">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="4b929-114">Bkz: [10.2.2 oluşturulan 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="4b929-114">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="4b929-115">URL oluşturmak için "adlı rota GetTodo" kullanır.</span><span class="sxs-lookup"><span data-stu-id="4b929-115">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="4b929-116">"Rota adlı GetTodo" tanımlanan `GetById`:</span><span class="sxs-lookup"><span data-stu-id="4b929-116">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="4b929-117">Postman oluşturma isteği göndermek için kullanın</span><span class="sxs-lookup"><span data-stu-id="4b929-117">Use Postman to send a Create request</span></span>

* <span data-ttu-id="4b929-118">Uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="4b929-118">Start the app.</span></span>
* <span data-ttu-id="4b929-119">Postman açın.</span><span class="sxs-lookup"><span data-stu-id="4b929-119">Open Postman.</span></span>

![Postman konsol](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="4b929-121">Localhost URL'sini bağlantı noktası numarasını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="4b929-121">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="4b929-122">HTTP yöntem kümesine *POST*.</span><span class="sxs-lookup"><span data-stu-id="4b929-122">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="4b929-123">Tıklatın **gövde** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="4b929-123">Click the **Body** tab.</span></span>
* <span data-ttu-id="4b929-124">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="4b929-124">Select the **raw** radio button.</span></span>
* <span data-ttu-id="4b929-125">Türü kümesine *JSON (uygulama/json)*.</span><span class="sxs-lookup"><span data-stu-id="4b929-125">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="4b929-126">Bir istek gövdesi aşağıdaki JSON benzeyen bir Yapılacaklar öğesi girin:</span><span class="sxs-lookup"><span data-stu-id="4b929-126">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="4b929-127">Tıklatın **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="4b929-127">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="4b929-128">Yanıt tıkladıktan sonra görüntülerse **Gönder**, devre dışı **SSL sertifika doğrulaması** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="4b929-128">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="4b929-129">Bu altında bulunan **dosya** > **ayarları**.</span><span class="sxs-lookup"><span data-stu-id="4b929-129">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="4b929-130">Tıklatın **Gönder** daha sonra tekrar ayarı devre dışı bırakma düğmesi.</span><span class="sxs-lookup"><span data-stu-id="4b929-130">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="4b929-131">Tıklatın **üstbilgileri** sekmesinde **yanıt** bölmesinde ve kopyalama **konumu** üstbilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="4b929-131">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Postman konsolunun üst bilgiler sekmesi](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="4b929-133">Konum üstbilgisi URI yeni öğe erişmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4b929-133">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="4b929-134">Güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="4b929-134">Update</span></span>

<span data-ttu-id="4b929-135">Aşağıdakileri ekleyin `Update` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4b929-135">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

<span data-ttu-id="4b929-136">`Update` benzer `Create`, HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="4b929-136">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="4b929-137">Yanıt [204 (No içerik)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="4b929-137">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="4b929-138">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca farkları göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4b929-138">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="4b929-139">Kısmi güncelleştirmeler desteklemek için HTTP PATCH kullanın.</span><span class="sxs-lookup"><span data-stu-id="4b929-139">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="4b929-140">Postman "kat yürütmek için" Yapılacaklar öğesi'nin adı güncelleştirmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="4b929-140">Use Postman to update the to-do item's name to "walk cat":</span></span>

![204 (No içerik) yanıt gösteren postman konsol](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="4b929-142">Sil</span><span class="sxs-lookup"><span data-stu-id="4b929-142">Delete</span></span>

<span data-ttu-id="4b929-143">Aşağıdakileri ekleyin `Delete` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4b929-143">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="4b929-144">`Delete` Yanıt [204 (No içerik)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="4b929-144">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="4b929-145">Postman Yapılacaklar öğesi silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="4b929-145">Use Postman to delete the to-do item:</span></span>

![204 (No içerik) yanıt gösteren postman konsol](../../tutorials/first-web-api/_static/pmd.png)
