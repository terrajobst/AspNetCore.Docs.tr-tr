---
title: "Mac için ASP.NET Core ve Visual Studio ile Web API oluşturma"
description: "Mac için ASP.NET Core MVC ve Visual Studio ile Web API oluşturma"
author: rick-anderson
ms.author: riande
ms.date: 09/15/2017
ms.topic: get-started-article
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
ms.technology: aspnet
keywords: "ASP.NET Core, Webapı, Web API'si, REST, mac, macOS, HTTP, hizmeti, HTTP hizmeti"
manager: wpickett
ms.openlocfilehash: 6bbd5e332e395928d8f79888ecf190f7f59a4bbc
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="f189d-104">Mac için ASP.NET Core MVC ve Visual Studio ile Web API oluşturma</span><span class="sxs-lookup"><span data-stu-id="f189d-104">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="f189d-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="f189d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f189d-106">Bu öğreticide, "Yapılacaklar" öğeleri listesini yönetmek için bir web API'si oluşturma.</span><span class="sxs-lookup"><span data-stu-id="f189d-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="f189d-107">Bir kullanıcı Arabirimi yapı olmaz.</span><span class="sxs-lookup"><span data-stu-id="f189d-107">You won’t build a UI.</span></span>

<span data-ttu-id="f189d-108">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="f189d-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="f189d-109">macOS: Web API'si Mac (Bu öğretici) için Visual Studio ile</span><span class="sxs-lookup"><span data-stu-id="f189d-109">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="f189d-110">Windows: [Web API'si Visual Studio ile Windows için](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="f189d-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="f189d-111">macOS, Linux, Windows: [Visual Studio Code ile Web API](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="f189d-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="f189d-112">Bkz: [ASP.NET Core MVC Mac veya Linux giriş](xref:tutorials/first-mvc-app-xplat/index) kalıcı bir veritabanı kullanan bir örnek.</span><span class="sxs-lookup"><span data-stu-id="f189d-112">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f189d-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="f189d-113">Prerequisites</span></span>

<span data-ttu-id="f189d-114">Aşağıdaki yükleyin:</span><span class="sxs-lookup"><span data-stu-id="f189d-114">Install the following:</span></span>

- <span data-ttu-id="f189d-115">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya daha yenisi</span><span class="sxs-lookup"><span data-stu-id="f189d-115">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="f189d-116">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f189d-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="f189d-117">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="f189d-117">Create the project</span></span>

<span data-ttu-id="f189d-118">Visual Studio'dan seçin **Dosya > Yeni bir çözüm**.</span><span class="sxs-lookup"><span data-stu-id="f189d-118">From Visual Studio, select **File > New Solution**.</span></span>

![macOS yeni çözüm](first-web-api-mac/_static/sln.png)

<span data-ttu-id="f189d-120">Seçin **.NET Core uygulaması > ASP.NET Core Web API > sonraki**.</span><span class="sxs-lookup"><span data-stu-id="f189d-120">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)

<span data-ttu-id="f189d-122">Girin **TodoApi** için **proje adı**ve Oluştur'u seçin.</span><span class="sxs-lookup"><span data-stu-id="f189d-122">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="f189d-124">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="f189d-124">Launch the app</span></span>

<span data-ttu-id="f189d-125">Visual Studio'da seçin **Çalıştır > Başlat ile hata ayıklama** uygulamayı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="f189d-125">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="f189d-126">Visual Studio bir tarayıcı ile başlatarak `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f189d-126">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="f189d-127">Bir HTTP 404 (bulunamadı) hata alıyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="f189d-127">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="f189d-128">URL'ye değiştirin `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="f189d-128">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="f189d-129">`ValuesController` Veri görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="f189d-129">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="f189d-130">Entity Framework Çekirdek desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="f189d-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="f189d-131">Yükleme [Entity Framework Çekirdek Inmemory](https://docs.microsoft.com/ef/core/providers/in-memory/) veritabanı sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="f189d-131">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="f189d-132">Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="f189d-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="f189d-133">Gelen **proje** menüsünde, select **NuGet paketleri Ekle**.</span><span class="sxs-lookup"><span data-stu-id="f189d-133">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="f189d-134">Alternatif olarak, sağ tıklayarak **bağımlılıkları**ve ardından **paketleri Ekle**.</span><span class="sxs-lookup"><span data-stu-id="f189d-134">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="f189d-135">Girin `EntityFrameworkCore.InMemory` arama kutusuna.</span><span class="sxs-lookup"><span data-stu-id="f189d-135">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="f189d-136">Seçin `Microsoft.EntityFrameworkCore.InMemory`ve ardından **Paketi Ekle**.</span><span class="sxs-lookup"><span data-stu-id="f189d-136">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="f189d-137">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="f189d-137">Add a model class</span></span>

<span data-ttu-id="f189d-138">Uygulamanızdaki verileri temsil eden bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="f189d-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="f189d-139">Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.</span><span class="sxs-lookup"><span data-stu-id="f189d-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="f189d-140">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="f189d-140">Add a folder named *Models*.</span></span> <span data-ttu-id="f189d-141">Çözüm Gezgini'nde projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f189d-141">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="f189d-142">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="f189d-142">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f189d-143">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="f189d-143">Name the folder *Models*.</span></span>

![Yeni klasör](first-web-api-mac/_static/folder.png)

<span data-ttu-id="f189d-145">Not:, Modeli sınıfları herhangi bir yere, projenizdeki koyabilirsiniz ancak *modelleri* klasörü, kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f189d-145">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="f189d-146">Ekleme bir `TodoItem` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="f189d-146">Add a `TodoItem` class.</span></span> <span data-ttu-id="f189d-147">Sağ *modelleri* klasörü ve select **Ekle > Yeni Dosya > Genel > boş sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="f189d-147">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="f189d-148">Sınıf adını `TodoItem`ve ardından **yeni**.</span><span class="sxs-lookup"><span data-stu-id="f189d-148">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="f189d-149">Oluşturulan kod ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="f189d-149">Replace the generated code with:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f189d-150">Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f189d-150">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="f189d-151">Veritabanı bağlamı oluşturma</span><span class="sxs-lookup"><span data-stu-id="f189d-151">Create the database context</span></span>

<span data-ttu-id="f189d-152">*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="f189d-152">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="f189d-153">Bu sınıf türetme tarafından oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="f189d-153">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="f189d-154">Ekleme bir `TodoContext` sınıfının *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="f189d-154">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="f189d-155">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="f189d-155">Add a controller</span></span>

<span data-ttu-id="f189d-156">Çözüm Gezgini'nde, içinde *denetleyicileri* klasörünü sınıfı ekleme `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="f189d-156">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="f189d-157">Oluşturulan kod aşağıdakiyle değiştirin (ve kapanış köşeli parantez ekleyin):</span><span class="sxs-lookup"><span data-stu-id="f189d-157">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="f189d-158">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="f189d-158">Launch the app</span></span>

<span data-ttu-id="f189d-159">Visual Studio'da seçin **Çalıştır > Başlat ile hata ayıklama** uygulamayı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="f189d-159">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="f189d-160">Visual Studio bir tarayıcı ile başlatarak `http://localhost:port`, burada *bağlantı noktası* rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="f189d-160">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="f189d-161">Bir HTTP 404 (bulunamadı) hata alıyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="f189d-161">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="f189d-162">URL'ye değiştirin `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="f189d-162">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="f189d-163">`ValuesController` Veri görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="f189d-163">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="f189d-164">Gidin `Todo` denetleyicisinde`http://localhost:port/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="f189d-164">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="f189d-165">Diğer CRUD işlemleri uygulama</span><span class="sxs-lookup"><span data-stu-id="f189d-165">Implement the other CRUD operations</span></span>

<span data-ttu-id="f189d-166">Ekleyeceğiz `Create`, `Update`, ve `Delete` denetleyiciye yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="f189d-166">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="f189d-167">I yalnızca kodu gösterir ve temel farklar vurgulayın bunlar bir tema çeşidi olduğundan.</span><span class="sxs-lookup"><span data-stu-id="f189d-167">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="f189d-168">Ekleme veya kod değiştirdikten sonra projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f189d-168">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="f189d-169">Create</span><span class="sxs-lookup"><span data-stu-id="f189d-169">Create</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="f189d-170">Bu belirttiği bir HTTP POST yöntemi olup [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f189d-170">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="f189d-171">[ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Özniteliği HTTP isteği gövdesinden Yapılacaklar öğesi değerini almak için MVC söyler.</span><span class="sxs-lookup"><span data-stu-id="f189d-171">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="f189d-172">`CreatedAtRoute` Yöntemi yeni bir kaynak sunucuda oluşturan bir HTTP POST yöntemi için standart yanıt 201 bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="f189d-172">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="f189d-173">`CreatedAtRoute`Ayrıca bir konum üstbilgisi yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="f189d-173">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="f189d-174">Konum üstbilgisi yeni oluşturulan Yapılacaklar öğesi URI'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="f189d-174">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="f189d-175">Bkz: [10.2.2 oluşturulan 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="f189d-175">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="f189d-176">Postman oluşturma isteği göndermek için kullanın</span><span class="sxs-lookup"><span data-stu-id="f189d-176">Use Postman to send a Create request</span></span>

* <span data-ttu-id="f189d-177">Uygulamayı başlatın (**Çalıştır > Başlat hata ayıklamaya**).</span><span class="sxs-lookup"><span data-stu-id="f189d-177">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="f189d-178">Postman başlatın.</span><span class="sxs-lookup"><span data-stu-id="f189d-178">Start Postman.</span></span>

![Postman konsol](first-web-api/_static/pmc.png)

* <span data-ttu-id="f189d-180">HTTP yöntemini ayarlayın`POST`</span><span class="sxs-lookup"><span data-stu-id="f189d-180">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="f189d-181">Seçin **gövde** radyo düğmesi</span><span class="sxs-lookup"><span data-stu-id="f189d-181">Select the **Body** radio button</span></span>
* <span data-ttu-id="f189d-182">Seçin **ham** radyo düğmesi</span><span class="sxs-lookup"><span data-stu-id="f189d-182">Select the **raw** radio button</span></span>
* <span data-ttu-id="f189d-183">Türü için JSON ayarlayın</span><span class="sxs-lookup"><span data-stu-id="f189d-183">Set the type to JSON</span></span>
* <span data-ttu-id="f189d-184">Anahtar-değer Düzenleyicisi'nde bir Yapılacaklar öğesi gibi girin</span><span class="sxs-lookup"><span data-stu-id="f189d-184">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="f189d-185">Seçin **Gönder**</span><span class="sxs-lookup"><span data-stu-id="f189d-185">Select **Send**</span></span>

* <span data-ttu-id="f189d-186">Alt bölme ve kopyalama Üstbilgileri sekmesini seçin **konumu** üstbilgisi:</span><span class="sxs-lookup"><span data-stu-id="f189d-186">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmget.png)

<span data-ttu-id="f189d-188">Yeni oluşturduğunuz kaynağa erişmek için konum üstbilgisi URI kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f189d-188">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="f189d-189">Geri çağırma `GetById` oluşturulan yöntemi `"GetTodo"` rota adlı:</span><span class="sxs-lookup"><span data-stu-id="f189d-189">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="f189d-190">Güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="f189d-190">Update</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="f189d-191">`Update`benzer `Create`, ancak HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="f189d-191">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="f189d-192">Yanıt [204 (No içerik)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f189d-192">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f189d-193">HTTP spec göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca farkları göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f189d-193">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="f189d-194">Kısmi güncelleştirmeler desteklemek için HTTP PATCH kullanın.</span><span class="sxs-lookup"><span data-stu-id="f189d-194">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![204 (No içerik) yanıt gösteren postman konsol](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="f189d-196">Sil</span><span class="sxs-lookup"><span data-stu-id="f189d-196">Delete</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="f189d-197">Yanıt [204 (No içerik)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f189d-197">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![204 (No içerik) yanıt gösteren postman konsol](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="f189d-199">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="f189d-199">Next steps</span></span>

* [<span data-ttu-id="f189d-200">Denetleyici eylemleri için yönlendirme</span><span class="sxs-lookup"><span data-stu-id="f189d-200">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="f189d-201">API'nizi dağıtma hakkında daha fazla bilgi için bkz: [yayımlama ve dağıtım](../publishing/index.md).</span><span class="sxs-lookup"><span data-stu-id="f189d-201">For information about deploying your API, see [Publishing and Deployment](../publishing/index.md).</span></span>
* <span data-ttu-id="f189d-202">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f189d-202">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
* [<span data-ttu-id="f189d-203">Postman</span><span class="sxs-lookup"><span data-stu-id="f189d-203">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="f189d-204">Fiddler</span><span class="sxs-lookup"><span data-stu-id="f189d-204">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
