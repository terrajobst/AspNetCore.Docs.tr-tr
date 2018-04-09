---
title: Mac için ASP.NET Core ve Visual Studio ile Web API oluşturma
author: rick-anderson
description: Mac için ASP.NET Core MVC ve Visual Studio ile Web API oluşturma
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: ce94091447452a51654f2cd4dad9043b63c737ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="a47fd-103">Mac için ASP.NET Core ve Visual Studio ile Web API oluşturma</span><span class="sxs-lookup"><span data-stu-id="a47fd-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="a47fd-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="a47fd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="a47fd-105">Bu öğreticide, "Yapılacaklar" öğeleri listesini yönetmek için bir web API'si oluşturma.</span><span class="sxs-lookup"><span data-stu-id="a47fd-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="a47fd-106">Kullanıcı arabirimini oluşturulan değil.</span><span class="sxs-lookup"><span data-stu-id="a47fd-106">The UI isn't constructed.</span></span>

<span data-ttu-id="a47fd-107">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="a47fd-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="a47fd-108">macOS: Web API'si Mac (Bu öğretici) için Visual Studio ile</span><span class="sxs-lookup"><span data-stu-id="a47fd-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="a47fd-109">Windows: [Web API'si Visual Studio ile Windows için](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="a47fd-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="a47fd-110">macOS, Linux, Windows: [Visual Studio Code ile Web API](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="a47fd-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE [template files](../includes/webApi/intro.md)]

<span data-ttu-id="a47fd-111">Bkz: [macOS ASP.NET Core MVC ya da Linux giriş](xref:tutorials/first-mvc-app-xplat/index) kalıcı bir veritabanı kullanan bir örnek.</span><span class="sxs-lookup"><span data-stu-id="a47fd-111">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a47fd-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="a47fd-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="a47fd-113">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a47fd-113">Create the project</span></span>

<span data-ttu-id="a47fd-114">Visual Studio'dan seçin **Dosya > Yeni bir çözüm**.</span><span class="sxs-lookup"><span data-stu-id="a47fd-114">From Visual Studio, select **File > New Solution**.</span></span>

![macOS yeni çözüm](first-web-api-mac/_static/sln.png)

<span data-ttu-id="a47fd-116">Seçin **.NET Core uygulaması > ASP.NET Core Web API > sonraki**.</span><span class="sxs-lookup"><span data-stu-id="a47fd-116">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)

<span data-ttu-id="a47fd-118">Girin **TodoApi** için **proje adı**ve Oluştur'u seçin.</span><span class="sxs-lookup"><span data-stu-id="a47fd-118">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="a47fd-120">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="a47fd-120">Launch the app</span></span>

<span data-ttu-id="a47fd-121">Visual Studio'da seçin **Çalıştır > Başlat ile hata ayıklama** uygulamayı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="a47fd-121">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="a47fd-122">Visual Studio bir tarayıcı ile başlatarak `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a47fd-122">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="a47fd-123">Bir HTTP 404 (bulunamadı) hata alıyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="a47fd-123">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="a47fd-124">URL'ye değiştirin `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="a47fd-124">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="a47fd-125">`ValuesController` Veri görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="a47fd-125">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="a47fd-126">Entity Framework Çekirdek desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="a47fd-126">Add support for Entity Framework Core</span></span>

<span data-ttu-id="a47fd-127">Yükleme [Entity Framework Çekirdek Inmemory](https://docs.microsoft.com/ef/core/providers/in-memory/) veritabanı sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="a47fd-127">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="a47fd-128">Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="a47fd-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="a47fd-129">Gelen **proje** menüsünde, select **NuGet paketleri Ekle**.</span><span class="sxs-lookup"><span data-stu-id="a47fd-129">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="a47fd-130">Alternatif olarak, sağ tıklayarak **bağımlılıkları**ve ardından **paketleri Ekle**.</span><span class="sxs-lookup"><span data-stu-id="a47fd-130">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="a47fd-131">Girin `EntityFrameworkCore.InMemory` arama kutusuna.</span><span class="sxs-lookup"><span data-stu-id="a47fd-131">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="a47fd-132">Seçin `Microsoft.EntityFrameworkCore.InMemory`ve ardından **Paketi Ekle**.</span><span class="sxs-lookup"><span data-stu-id="a47fd-132">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="a47fd-133">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="a47fd-133">Add a model class</span></span>

<span data-ttu-id="a47fd-134">Uygulamanızdaki verileri temsil eden bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="a47fd-134">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="a47fd-135">Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.</span><span class="sxs-lookup"><span data-stu-id="a47fd-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="a47fd-136">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="a47fd-136">Add a folder named *Models*.</span></span> <span data-ttu-id="a47fd-137">Çözüm Gezgini'nde projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a47fd-137">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="a47fd-138">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="a47fd-138">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="a47fd-139">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="a47fd-139">Name the folder *Models*.</span></span>

![Yeni klasör](first-web-api-mac/_static/folder.png)

<span data-ttu-id="a47fd-141">Not:, Modeli sınıfları herhangi bir yere, projenizdeki koyabilirsiniz ancak *modelleri* klasörü, kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a47fd-141">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="a47fd-142">Ekleme bir `TodoItem` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a47fd-142">Add a `TodoItem` class.</span></span> <span data-ttu-id="a47fd-143">Sağ *modelleri* klasörü ve select **Ekle > Yeni Dosya > Genel > boş sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="a47fd-143">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="a47fd-144">Sınıf adını `TodoItem`ve ardından **yeni**.</span><span class="sxs-lookup"><span data-stu-id="a47fd-144">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="a47fd-145">Oluşturulan kod ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a47fd-145">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="a47fd-146">Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a47fd-146">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="a47fd-147">Veritabanı bağlamı oluşturma</span><span class="sxs-lookup"><span data-stu-id="a47fd-147">Create the database context</span></span>

<span data-ttu-id="a47fd-148">*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="a47fd-148">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="a47fd-149">Bu sınıf türetme tarafından oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a47fd-149">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="a47fd-150">Ekleme bir `TodoContext` sınıfının *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="a47fd-150">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="a47fd-151">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="a47fd-151">Add a controller</span></span>

<span data-ttu-id="a47fd-152">Çözüm Gezgini'nde, içinde *denetleyicileri* klasörünü sınıfı ekleme `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="a47fd-152">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="a47fd-153">Oluşturulan kod aşağıdakiyle değiştirin (ve kapanış köşeli parantez ekleyin):</span><span class="sxs-lookup"><span data-stu-id="a47fd-153">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE [code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="a47fd-154">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="a47fd-154">Launch the app</span></span>

<span data-ttu-id="a47fd-155">Visual Studio'da seçin **Çalıştır > Başlat ile hata ayıklama** uygulamayı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="a47fd-155">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="a47fd-156">Visual Studio bir tarayıcı ile başlatarak `http://localhost:port`, burada *bağlantı noktası* rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="a47fd-156">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="a47fd-157">Bir HTTP 404 (bulunamadı) hata alıyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="a47fd-157">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="a47fd-158">URL'ye değiştirin `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="a47fd-158">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="a47fd-159">`ValuesController` Veri görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="a47fd-159">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="a47fd-160">Gidin `Todo` denetleyicisinde`http://localhost:port/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="a47fd-160">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="a47fd-161">Diğer CRUD işlemleri uygulama</span><span class="sxs-lookup"><span data-stu-id="a47fd-161">Implement the other CRUD operations</span></span>

<span data-ttu-id="a47fd-162">Ekleyeceğiz `Create`, `Update`, ve `Delete` denetleyiciye yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="a47fd-162">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="a47fd-163">I yalnızca kodu gösterir ve temel farklar vurgulayın bunlar bir tema çeşidi olduğundan.</span><span class="sxs-lookup"><span data-stu-id="a47fd-163">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="a47fd-164">Ekleme veya kod değiştirdikten sonra projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a47fd-164">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="a47fd-165">Create</span><span class="sxs-lookup"><span data-stu-id="a47fd-165">Create</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="a47fd-166">Bu belirttiği bir HTTP POST yöntemi olup [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a47fd-166">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="a47fd-167">[ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Özniteliği HTTP isteği gövdesinden Yapılacaklar öğesi değerini almak için MVC söyler.</span><span class="sxs-lookup"><span data-stu-id="a47fd-167">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="a47fd-168">`CreatedAtRoute` Yöntemi yeni bir kaynak sunucuda oluşturan bir HTTP POST yöntemi için standart yanıt 201 bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="a47fd-168">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="a47fd-169">`CreatedAtRoute` Ayrıca bir konum üstbilgisi yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="a47fd-169">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="a47fd-170">Konum üstbilgisi yeni oluşturulan Yapılacaklar öğesi URI'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a47fd-170">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="a47fd-171">Bkz: [10.2.2 oluşturulan 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="a47fd-171">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="a47fd-172">Postman oluşturma isteği göndermek için kullanın</span><span class="sxs-lookup"><span data-stu-id="a47fd-172">Use Postman to send a Create request</span></span>

* <span data-ttu-id="a47fd-173">Uygulamayı başlatın (**Çalıştır > Başlat hata ayıklamaya**).</span><span class="sxs-lookup"><span data-stu-id="a47fd-173">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="a47fd-174">Postman başlatın.</span><span class="sxs-lookup"><span data-stu-id="a47fd-174">Start Postman.</span></span>

![Postman konsol](first-web-api/_static/pmc.png)

* <span data-ttu-id="a47fd-176">HTTP yöntemini ayarlayın `POST`</span><span class="sxs-lookup"><span data-stu-id="a47fd-176">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="a47fd-177">Seçin **gövde** radyo düğmesi</span><span class="sxs-lookup"><span data-stu-id="a47fd-177">Select the **Body** radio button</span></span>
* <span data-ttu-id="a47fd-178">Seçin **ham** radyo düğmesi</span><span class="sxs-lookup"><span data-stu-id="a47fd-178">Select the **raw** radio button</span></span>
* <span data-ttu-id="a47fd-179">Türü için JSON ayarlayın</span><span class="sxs-lookup"><span data-stu-id="a47fd-179">Set the type to JSON</span></span>
* <span data-ttu-id="a47fd-180">Anahtar-değer Düzenleyicisi'nde bir Yapılacaklar öğesi gibi girin</span><span class="sxs-lookup"><span data-stu-id="a47fd-180">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="a47fd-181">Seçin **Gönder**</span><span class="sxs-lookup"><span data-stu-id="a47fd-181">Select **Send**</span></span>

* <span data-ttu-id="a47fd-182">Alt bölme ve kopyalama Üstbilgileri sekmesini seçin **konumu** üstbilgisi:</span><span class="sxs-lookup"><span data-stu-id="a47fd-182">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmget.png)

<span data-ttu-id="a47fd-184">Yeni oluşturduğunuz kaynağa erişmek için konum üstbilgisi URI kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a47fd-184">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="a47fd-185">Geri çağırma `GetById` oluşturulan yöntemi `"GetTodo"` rota adlı:</span><span class="sxs-lookup"><span data-stu-id="a47fd-185">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="a47fd-186">Güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="a47fd-186">Update</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="a47fd-187">`Update` benzer `Create`, ancak HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="a47fd-187">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="a47fd-188">Yanıt [204 (No içerik)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a47fd-188">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="a47fd-189">HTTP spec göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca farkları göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a47fd-189">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="a47fd-190">Kısmi güncelleştirmeler desteklemek için HTTP PATCH kullanın.</span><span class="sxs-lookup"><span data-stu-id="a47fd-190">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![204 (No içerik) yanıt gösteren postman konsol](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="a47fd-192">Sil</span><span class="sxs-lookup"><span data-stu-id="a47fd-192">Delete</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="a47fd-193">Yanıt [204 (No içerik)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a47fd-193">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![204 (No içerik) yanıt gösteren postman konsol](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="a47fd-195">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="a47fd-195">Next steps</span></span>

* [<span data-ttu-id="a47fd-196">Denetleyici eylemlerine yönlendirme</span><span class="sxs-lookup"><span data-stu-id="a47fd-196">Route to controller actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="a47fd-197">API'nizi dağıtma hakkında daha fazla bilgi için bkz: [konak dağıtıp](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="a47fd-197">For information about deploying your API, see [Host and deploy](xref:host-and-deploy/index).</span></span>
* <span data-ttu-id="a47fd-198">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a47fd-198">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
* [<span data-ttu-id="a47fd-199">Postman</span><span class="sxs-lookup"><span data-stu-id="a47fd-199">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="a47fd-200">Fiddler</span><span class="sxs-lookup"><span data-stu-id="a47fd-200">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
