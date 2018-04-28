## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="23453-101">Diğer CRUD işlemleri uygulama</span><span class="sxs-lookup"><span data-stu-id="23453-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="23453-102">Aşağıdaki bölümlerde `Create`, `Update`, ve `Delete` yöntemleri, denetleyiciye eklenir.</span><span class="sxs-lookup"><span data-stu-id="23453-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="23453-103">Create</span><span class="sxs-lookup"><span data-stu-id="23453-103">Create</span></span>

<span data-ttu-id="23453-104">Aşağıdakileri ekleyin `Create` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="23453-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="23453-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="23453-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="23453-106">Önceki kod belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="23453-106">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="23453-107">[[FromBody]](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) özniteliği HTTP isteği gövdesinden Yapılacaklar öğesi değerini almak için MVC söyler.</span><span class="sxs-lookup"><span data-stu-id="23453-107">The [[FromBody]](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="23453-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="23453-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="23453-109">Önceki kod belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="23453-109">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="23453-110">MVC HTTP isteği gövdesinden Yapılacaklar öğesi değerini alır.</span><span class="sxs-lookup"><span data-stu-id="23453-110">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="23453-111">`CreatedAtRoute` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="23453-111">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="23453-112">201 yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="23453-112">Returns a 201 response.</span></span> <span data-ttu-id="23453-113">HTTP 201 yeni bir kaynak sunucuda oluşturan bir HTTP POST yöntemi için standart yanıt'dir.</span><span class="sxs-lookup"><span data-stu-id="23453-113">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="23453-114">Bir konum üstbilgisi yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="23453-114">Adds a Location header to the response.</span></span> <span data-ttu-id="23453-115">Konum üstbilgisi yeni oluşturulan Yapılacaklar öğesi URI'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="23453-115">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="23453-116">Bkz: [10.2.2 oluşturulan 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="23453-116">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="23453-117">URL oluşturmak için "adlı rota GetTodo" kullanır.</span><span class="sxs-lookup"><span data-stu-id="23453-117">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="23453-118">"Rota adlı GetTodo" tanımlanan `GetById`:</span><span class="sxs-lookup"><span data-stu-id="23453-118">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="23453-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="23453-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="23453-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="23453-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="23453-121">Postman oluşturma isteği göndermek için kullanın</span><span class="sxs-lookup"><span data-stu-id="23453-121">Use Postman to send a Create request</span></span>

* <span data-ttu-id="23453-122">Uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="23453-122">Start the app.</span></span>
* <span data-ttu-id="23453-123">Postman açın.</span><span class="sxs-lookup"><span data-stu-id="23453-123">Open Postman.</span></span>

![Postman konsol](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="23453-125">Localhost URL'sini bağlantı noktası numarasını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="23453-125">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="23453-126">HTTP yöntem kümesine *POST*.</span><span class="sxs-lookup"><span data-stu-id="23453-126">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="23453-127">Tıklatın **gövde** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="23453-127">Click the **Body** tab.</span></span>
* <span data-ttu-id="23453-128">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="23453-128">Select the **raw** radio button.</span></span>
* <span data-ttu-id="23453-129">Türü kümesine *JSON (uygulama/json)*.</span><span class="sxs-lookup"><span data-stu-id="23453-129">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="23453-130">Bir istek gövdesi aşağıdaki JSON benzeyen bir Yapılacaklar öğesi girin:</span><span class="sxs-lookup"><span data-stu-id="23453-130">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="23453-131">Tıklatın **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="23453-131">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="23453-132">Yanıt tıkladıktan sonra görüntülerse **Gönder**, devre dışı **SSL sertifika doğrulaması** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="23453-132">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="23453-133">Bu altında bulunan **dosya** > **ayarları**.</span><span class="sxs-lookup"><span data-stu-id="23453-133">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="23453-134">Tıklatın **Gönder** daha sonra tekrar ayarı devre dışı bırakma düğmesi.</span><span class="sxs-lookup"><span data-stu-id="23453-134">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="23453-135">Tıklatın **üstbilgileri** sekmesinde **yanıt** bölmesinde ve kopyalama **konumu** üstbilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="23453-135">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Postman konsolunun üst bilgiler sekmesi](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="23453-137">Konum üstbilgisi URI yeni öğe erişmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="23453-137">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="23453-138">Güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="23453-138">Update</span></span>

<span data-ttu-id="23453-139">Aşağıdakileri ekleyin `Update` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="23453-139">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="23453-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="23453-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="23453-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="23453-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="23453-142">`Update` benzer `Create`, HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="23453-142">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="23453-143">Yanıt [204 (No içerik)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="23453-143">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="23453-144">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca farkları göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="23453-144">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="23453-145">Kısmi güncelleştirmeler desteklemek için HTTP PATCH kullanın.</span><span class="sxs-lookup"><span data-stu-id="23453-145">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="23453-146">Postman "kat yürütmek için" Yapılacaklar öğesi'nin adı güncelleştirmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="23453-146">Use Postman to update the to-do item's name to "walk cat":</span></span>

![204 (No içerik) yanıt gösteren postman konsol](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="23453-148">Sil</span><span class="sxs-lookup"><span data-stu-id="23453-148">Delete</span></span>

<span data-ttu-id="23453-149">Aşağıdakileri ekleyin `Delete` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="23453-149">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="23453-150">`Delete` Yanıt [204 (No içerik)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="23453-150">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="23453-151">Postman Yapılacaklar öğesi silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="23453-151">Use Postman to delete the to-do item:</span></span>

![204 (No içerik) yanıt gösteren postman konsol](../../tutorials/first-web-api/_static/pmd.png)
