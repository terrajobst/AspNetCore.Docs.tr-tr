---
title: Mac için ASP.NET Core ve Visual Studio ile Web API oluşturma
author: rick-anderson
description: Mac için ASP.NET Core MVC ve Visual Studio ile Web API oluşturma
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 46050f4bbd6ae821c03d92c8750e839d491328cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="a664b-103">Mac için ASP.NET Core ve Visual Studio ile Web API oluşturma</span><span class="sxs-lookup"><span data-stu-id="a664b-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="a664b-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="a664b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

::: moniker range="= aspnetcore-2.1"
[!INCLUDE[](~/includes/2.1.md)]
::: moniker-end

<span data-ttu-id="a664b-106">Bu öğreticide, "Yapılacaklar" öğeleri listesini yönetmek için bir web API'si oluşturma.</span><span class="sxs-lookup"><span data-stu-id="a664b-106">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="a664b-107">Kullanıcı arabirimini oluşturulan değil.</span><span class="sxs-lookup"><span data-stu-id="a664b-107">The UI isn't constructed.</span></span>

<span data-ttu-id="a664b-108">Bu öğretici için üç sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="a664b-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="a664b-109">macOS: Web API'si Mac (Bu öğretici) için Visual Studio ile</span><span class="sxs-lookup"><span data-stu-id="a664b-109">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="a664b-110">Windows: [Web API'si Visual Studio ile Windows için](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="a664b-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="a664b-111">macOS, Linux, Windows: [Visual Studio Code ile Web API](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="a664b-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

<span data-ttu-id="a664b-112">Bkz: [macOS ASP.NET Core MVC ya da Linux giriş](xref:tutorials/first-mvc-app-xplat/index) kalıcı bir veritabanı kullanan bir örnek.</span><span class="sxs-lookup"><span data-stu-id="a664b-112">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a664b-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="a664b-113">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="a664b-114">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a664b-114">Create the project</span></span>

<span data-ttu-id="a664b-115">Visual Studio'dan seçin **dosya** > **yeni çözüm**.</span><span class="sxs-lookup"><span data-stu-id="a664b-115">From Visual Studio, select **File** > **New Solution**.</span></span>

![macOS yeni çözüm](first-web-api-mac/_static/sln.png)

<span data-ttu-id="a664b-117">Seçin **.NET Core uygulaması** > **ASP.NET Core Web API** > **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="a664b-117">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)

<span data-ttu-id="a664b-119">Girin *TodoApi* için **proje adı**ve ardından **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="a664b-119">Enter *TodoApi* for the **Project Name**, and then click **Create**.</span></span>

![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="a664b-121">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="a664b-121">Launch the app</span></span>

<span data-ttu-id="a664b-122">Visual Studio'da seçin **çalıştırmak** > **Başlat ile hata ayıklama** uygulamayı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="a664b-122">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="a664b-123">Visual Studio bir tarayıcı ile başlatarak `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a664b-123">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="a664b-124">Bir HTTP 404 (bulunamadı) hata alıyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="a664b-124">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="a664b-125">URL'ye değiştirin `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="a664b-125">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="a664b-126">`ValuesController` Veri görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="a664b-126">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="a664b-127">Entity Framework Çekirdek desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="a664b-127">Add support for Entity Framework Core</span></span>

<span data-ttu-id="a664b-128">Yükleme [Entity Framework Çekirdek Inmemory](/ef/core/providers/in-memory/) veritabanı sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="a664b-128">Install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="a664b-129">Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="a664b-129">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="a664b-130">Gelen **proje** menüsünde, select **NuGet paketleri Ekle**.</span><span class="sxs-lookup"><span data-stu-id="a664b-130">From the **Project** menu, select **Add NuGet Packages**.</span></span>

  * <span data-ttu-id="a664b-131">Alternatif olarak, sağ tıklayarak **bağımlılıkları**ve ardından **paketleri Ekle**.</span><span class="sxs-lookup"><span data-stu-id="a664b-131">Alternatively, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="a664b-132">Girin `EntityFrameworkCore.InMemory` arama kutusuna.</span><span class="sxs-lookup"><span data-stu-id="a664b-132">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="a664b-133">Seçin `Microsoft.EntityFrameworkCore.InMemory`ve ardından **Paketi Ekle**.</span><span class="sxs-lookup"><span data-stu-id="a664b-133">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="a664b-134">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="a664b-134">Add a model class</span></span>

<span data-ttu-id="a664b-135">Uygulamanızdaki verileri temsil eden bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="a664b-135">A model is an object representing the data in your app.</span></span> <span data-ttu-id="a664b-136">Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.</span><span class="sxs-lookup"><span data-stu-id="a664b-136">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="a664b-137">Çözüm Gezgini'nde projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a664b-137">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="a664b-138">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="a664b-138">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="a664b-139">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="a664b-139">Name the folder *Models*.</span></span>

![Yeni klasör](first-web-api-mac/_static/folder.png)

> [!NOTE]
> <span data-ttu-id="a664b-141">Model sınıfları projenizde, herhangi bir yere koyabilirsiniz ancak *modelleri* klasörü, kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a664b-141">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="a664b-142">Sağ *modelleri* klasörü ve select **Ekle** > **yeni dosya** > **genel**  >  **Boş sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="a664b-142">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span> <span data-ttu-id="a664b-143">Sınıf adını *Todoıtem*ve ardından **yeni**.</span><span class="sxs-lookup"><span data-stu-id="a664b-143">Name the class *TodoItem*, and then click **New**.</span></span>

<span data-ttu-id="a664b-144">Oluşturulan kod ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a664b-144">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="a664b-145">Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a664b-145">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="a664b-146">Veritabanı bağlamı oluşturma</span><span class="sxs-lookup"><span data-stu-id="a664b-146">Create the database context</span></span>

<span data-ttu-id="a664b-147">*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="a664b-147">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="a664b-148">Bu sınıf türetme tarafından oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a664b-148">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="a664b-149">Ekleme bir `TodoContext` sınıfının *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="a664b-149">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="a664b-150">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="a664b-150">Add a controller</span></span>

<span data-ttu-id="a664b-151">Çözüm Gezgini'nde, içinde *denetleyicileri* klasörünü sınıfı ekleme `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="a664b-151">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="a664b-152">Oluşturulan kod aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a664b-152">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="a664b-153">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="a664b-153">Launch the app</span></span>

<span data-ttu-id="a664b-154">Visual Studio'da seçin **çalıştırmak** > **Başlat ile hata ayıklama** uygulamayı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="a664b-154">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="a664b-155">Visual Studio bir tarayıcı ile başlatarak `http://localhost:<port>`, burada `<port>` rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="a664b-155">Visual Studio launches a browser and navigates to `http://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="a664b-156">Bir HTTP 404 (bulunamadı) hata alıyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="a664b-156">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="a664b-157">URL'ye değiştirin `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="a664b-157">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="a664b-158">`ValuesController` Veri görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="a664b-158">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="a664b-159">Gidin `Todo` denetleyicisinde `http://localhost:<port>/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="a664b-159">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`:</span></span>

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="a664b-160">Diğer CRUD işlemleri uygulama</span><span class="sxs-lookup"><span data-stu-id="a664b-160">Implement the other CRUD operations</span></span>

<span data-ttu-id="a664b-161">Ekleyeceğiz `Create`, `Update`, ve `Delete` denetleyiciye yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="a664b-161">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="a664b-162">Bu yöntemler ı yalnızca kodu gösterir ve temel farklar vurgulayın bir tema çeşidi olduğundan.</span><span class="sxs-lookup"><span data-stu-id="a664b-162">These methods are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="a664b-163">Ekleme veya kod değiştirdikten sonra projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a664b-163">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="a664b-164">Create</span><span class="sxs-lookup"><span data-stu-id="a664b-164">Create</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="a664b-165">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="a664b-165">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="a664b-166">Yukarıdaki yöntem tarafından belirtildiği gibi bir HTTP POST yanıt [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a664b-166">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="a664b-167">[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) özniteliği HTTP isteği gövdesinden Yapılacaklar öğesi değerini almak için MVC söyler.</span><span class="sxs-lookup"><span data-stu-id="a664b-167">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="a664b-168">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="a664b-168">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="a664b-169">Yukarıdaki yöntem tarafından belirtildiği gibi bir HTTP POST yanıt [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a664b-169">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="a664b-170">MVC HTTP isteği gövdesinden Yapılacaklar öğesi değerini alır.</span><span class="sxs-lookup"><span data-stu-id="a664b-170">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="a664b-171">`CreatedAtRoute` Yöntemi 201 bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="a664b-171">The `CreatedAtRoute` method returns a 201 response.</span></span> <span data-ttu-id="a664b-172">Yeni bir kaynak sunucuda oluşturan bir HTTP POST yöntemi için standart yanıt değil.</span><span class="sxs-lookup"><span data-stu-id="a664b-172">It's the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="a664b-173">`CreatedAtRoute` Ayrıca bir konum üstbilgisi yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="a664b-173">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="a664b-174">Konum üstbilgisi yeni oluşturulan Yapılacaklar öğesi URI'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a664b-174">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="a664b-175">Bkz: [10.2.2 oluşturulan 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="a664b-175">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="a664b-176">Postman oluşturma isteği göndermek için kullanın</span><span class="sxs-lookup"><span data-stu-id="a664b-176">Use Postman to send a Create request</span></span>

* <span data-ttu-id="a664b-177">Uygulamayı başlatın (**çalıştırmak** > **Başlat hata ayıklamaya**).</span><span class="sxs-lookup"><span data-stu-id="a664b-177">Start the app (**Run** > **Start With Debugging**).</span></span>
* <span data-ttu-id="a664b-178">Postman açın.</span><span class="sxs-lookup"><span data-stu-id="a664b-178">Open Postman.</span></span>

![Postman konsol](first-web-api/_static/pmc.png)

* <span data-ttu-id="a664b-180">Localhost URL'sini bağlantı noktası numarasını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="a664b-180">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="a664b-181">HTTP yöntem kümesine *POST*.</span><span class="sxs-lookup"><span data-stu-id="a664b-181">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="a664b-182">Tıklatın **gövde** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="a664b-182">Click the **Body** tab.</span></span>
* <span data-ttu-id="a664b-183">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="a664b-183">Select the **raw** radio button.</span></span>
* <span data-ttu-id="a664b-184">Türü kümesine *JSON (uygulama/json)*.</span><span class="sxs-lookup"><span data-stu-id="a664b-184">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="a664b-185">Bir istek gövdesi aşağıdaki JSON benzeyen bir Yapılacaklar öğesi girin:</span><span class="sxs-lookup"><span data-stu-id="a664b-185">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="a664b-186">Tıklatın **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="a664b-186">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="a664b-187">Yanıt tıkladıktan sonra görüntülerse **Gönder**, devre dışı **SSL sertifika doğrulaması** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="a664b-187">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="a664b-188">Bu altında bulunan **dosya** > **ayarları**.</span><span class="sxs-lookup"><span data-stu-id="a664b-188">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="a664b-189">Tıklatın **Gönder** daha sonra tekrar ayarı devre dışı bırakma düğmesi.</span><span class="sxs-lookup"><span data-stu-id="a664b-189">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="a664b-190">Tıklatın **üstbilgileri** sekmesinde **yanıt** bölmesinde ve kopyalama **konumu** üstbilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="a664b-190">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmc2.png)

<span data-ttu-id="a664b-192">Konum üstbilgisi URI oluşturduğunuz kaynağa erişmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a664b-192">You can use the Location header URI to access the resource you created.</span></span> <span data-ttu-id="a664b-193">`Create` Yöntemi döndürür [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span><span class="sxs-lookup"><span data-stu-id="a664b-193">The `Create` method returns [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span></span> <span data-ttu-id="a664b-194">Geçirilen ilk parametre `CreatedAtRoute` URL'yi oluşturmak için kullanılacak adlı rotayı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="a664b-194">The first parameter passed to `CreatedAtRoute` represents the named route to use for generating the URL.</span></span> <span data-ttu-id="a664b-195">Sözcüğünün `GetById` oluşturulan yöntemi `"GetTodo"` rota adlı:</span><span class="sxs-lookup"><span data-stu-id="a664b-195">Recall that the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a><span data-ttu-id="a664b-196">Güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="a664b-196">Update</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="a664b-197">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="a664b-197">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="a664b-198">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="a664b-198">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="a664b-199">`Update` benzer `Create`, ancak HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="a664b-199">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="a664b-200">Yanıt [204 (No içerik)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a664b-200">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="a664b-201">HTTP spec göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca farkları göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a664b-201">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="a664b-202">Kısmi güncelleştirmeler desteklemek için HTTP PATCH kullanın.</span><span class="sxs-lookup"><span data-stu-id="a664b-202">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![204 (No içerik) yanıt gösteren postman konsol](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="a664b-204">Sil</span><span class="sxs-lookup"><span data-stu-id="a664b-204">Delete</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="a664b-205">Yanıt [204 (No içerik)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a664b-205">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![204 (No içerik) yanıt gösteren postman konsol](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
