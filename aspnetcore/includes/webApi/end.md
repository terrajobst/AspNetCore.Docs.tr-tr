## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="bfae6-101">Bir CRUD işlemleri uygulama</span><span class="sxs-lookup"><span data-stu-id="bfae6-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="bfae6-102">Aşağıdaki bölümlerde `Create`, `Update`, ve `Delete` yöntemleri, denetleyiciye eklenir.</span><span class="sxs-lookup"><span data-stu-id="bfae6-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="bfae6-103">Create</span><span class="sxs-lookup"><span data-stu-id="bfae6-103">Create</span></span>

<span data-ttu-id="bfae6-104">Aşağıdaki `Create` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="bfae6-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="bfae6-105">Yukarıdaki kod tarafından belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="bfae6-105">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="bfae6-106">[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) özniteliği HTTP isteği gövdesinden yapılacak iş öğesi değeri almak için MVC söyler.</span><span class="sxs-lookup"><span data-stu-id="bfae6-106">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="bfae6-107">Yukarıdaki kod tarafından belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="bfae6-107">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="bfae6-108">MVC, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="bfae6-108">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

<span data-ttu-id="bfae6-109">`CreatedAtRoute` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="bfae6-109">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="bfae6-110">201 yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="bfae6-110">Returns a 201 response.</span></span> <span data-ttu-id="bfae6-111">HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="bfae6-111">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="bfae6-112">Bir konum üst bilgisi yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="bfae6-112">Adds a Location header to the response.</span></span> <span data-ttu-id="bfae6-113">Location üst bilgisini, yeni oluşturulan yapılacak iş öğesi URI'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="bfae6-113">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="bfae6-114">Bkz: [10.2.2 oluşturulan 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="bfae6-114">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="bfae6-115">URL oluşturmak için "adlı rota GetTodo" kullanır.</span><span class="sxs-lookup"><span data-stu-id="bfae6-115">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="bfae6-116">"Adlı rota GetTodo" içinde tanımlanan `GetById`:</span><span class="sxs-lookup"><span data-stu-id="bfae6-116">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="bfae6-117">Bir oluşturma isteği göndermek için Postman'ı kullanma</span><span class="sxs-lookup"><span data-stu-id="bfae6-117">Use Postman to send a Create request</span></span>

* <span data-ttu-id="bfae6-118">Uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="bfae6-118">Start the app.</span></span>
* <span data-ttu-id="bfae6-119">Postman'i açın.</span><span class="sxs-lookup"><span data-stu-id="bfae6-119">Open Postman.</span></span>

![Postman Konsolu](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="bfae6-121">Localhost URL'sini kullanılan bağlantı noktası numarasını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="bfae6-121">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="bfae6-122">HTTP yöntemi kümesine *POST*.</span><span class="sxs-lookup"><span data-stu-id="bfae6-122">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="bfae6-123">Tıklayın **gövdesi** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="bfae6-123">Click the **Body** tab.</span></span>
* <span data-ttu-id="bfae6-124">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="bfae6-124">Select the **raw** radio button.</span></span>
* <span data-ttu-id="bfae6-125">Tür kümesine *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="bfae6-125">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="bfae6-126">İstek gövdesi aşağıdaki JSON benzeyen bir yapılacak iş öğesi ile girin:</span><span class="sxs-lookup"><span data-stu-id="bfae6-126">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="bfae6-127">Tıklayın **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="bfae6-127">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> <span data-ttu-id="bfae6-128">Yanıt tıklandıktan sonra görüntülerse **Gönder**, devre dışı **SSL sertifika doğrulama** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="bfae6-128">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="bfae6-129">Bunun altında bulunan **dosya** > **ayarları**.</span><span class="sxs-lookup"><span data-stu-id="bfae6-129">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="bfae6-130">Tıklayın **Gönder** ayarı devre dışı bırakma sonra yeniden düğmesi.</span><span class="sxs-lookup"><span data-stu-id="bfae6-130">Click the **Send** button again after disabling the setting.</span></span>

::: moniker-end

<span data-ttu-id="bfae6-131">Tıklayın **üstbilgileri** sekmesinde **yanıt** bölmesi ve kopyalama **konumu** üst bilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="bfae6-131">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Postman konsolunun üst bilgiler sekmesi](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="bfae6-133">Konum üst bilgisi URI, yeni öğeye erişmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bfae6-133">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="bfae6-134">Güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="bfae6-134">Update</span></span>

<span data-ttu-id="bfae6-135">Aşağıdaki `Update` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="bfae6-135">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

<span data-ttu-id="bfae6-136">`Update` benzer `Create`, HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="bfae6-136">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="bfae6-137">Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="bfae6-137">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="bfae6-138">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca deltaları göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="bfae6-138">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="bfae6-139">Kısmi güncelleştirmeleri desteklemek için HTTP PATCH kullanın.</span><span class="sxs-lookup"><span data-stu-id="bfae6-139">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="bfae6-140">"Cat yürütmek için" Yapılacaklar öğenin adını güncelleştirmek için Postman'ı kullanın:</span><span class="sxs-lookup"><span data-stu-id="bfae6-140">Use Postman to update the to-do item's name to "walk cat":</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="bfae6-142">Sil</span><span class="sxs-lookup"><span data-stu-id="bfae6-142">Delete</span></span>

<span data-ttu-id="bfae6-143">Aşağıdaki `Delete` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="bfae6-143">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="bfae6-144">`Delete` Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="bfae6-144">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="bfae6-145">Postman yapılacak iş öğesini silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="bfae6-145">Use Postman to delete the to-do item:</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](../../tutorials/first-web-api/_static/pmd.png)
