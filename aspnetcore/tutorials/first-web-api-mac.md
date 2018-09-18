---
title: Mac için Visual Studio ile ASP.NET Core ile Web API'si oluşturma
author: rick-anderson
description: Mac için ASP.NET Core MVC ve Visual Studio ile Web API'si oluşturma
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 40f9bd9c57b97826edfddeb00cb4fb38a026d46e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011631"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="ee0fc-103">Mac için Visual Studio ile ASP.NET Core ile Web API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="ee0fc-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="ee0fc-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="ee0fc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="ee0fc-105">Bu öğreticide, bir "Yapılacaklar" öğelerinin listesi yönetmek için bir web API'si oluşturma.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="ee0fc-106">Kullanıcı Arabirimi oluşturulur değil.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-106">The UI isn't constructed.</span></span>

<span data-ttu-id="ee0fc-107">Bu öğretici üç sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="ee0fc-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="ee0fc-108">macOS: (Bu öğreticide) Mac için Visual Studio ile Web API'si</span><span class="sxs-lookup"><span data-stu-id="ee0fc-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="ee0fc-109">Windows: [Windows için Visual Studio ile API Web](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="ee0fc-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="ee0fc-110">macOS, Linux, Windows: [Visual Studio Code ile Web API](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="ee0fc-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

<span data-ttu-id="ee0fc-111">Bkz: [ASP.NET Core MVC MacOS veya Linux giriş](xref:tutorials/first-mvc-app-xplat/index) kalıcı bir veritabanı kullanan bir örnek.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-111">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee0fc-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="ee0fc-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="ee0fc-113">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="ee0fc-113">Create the project</span></span>

<span data-ttu-id="ee0fc-114">Visual Studio'dan seçin **dosya** > **yeni çözüm**.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-114">From Visual Studio, select **File** > **New Solution**.</span></span>

![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

<span data-ttu-id="ee0fc-116">Seçin **.NET Core uygulaması** > **ASP.NET Core Web API'sini** > **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-116">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)

<span data-ttu-id="ee0fc-118">Girin *TodoApi* için **proje adı**ve ardından **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-118">Enter *TodoApi* for the **Project Name**, and then click **Create**.</span></span>

![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="ee0fc-120">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="ee0fc-120">Launch the app</span></span>

<span data-ttu-id="ee0fc-121">Visual Studio'da **çalıştırmak** > **ile Start Debugging** uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-121">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="ee0fc-122">Visual Studio bir tarayıcı ile başlatarak `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-122">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="ee0fc-123">HTTP 404 (bulunamadı) hatası alırsınız.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-123">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="ee0fc-124">URL'yi `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-124">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="ee0fc-125">`ValuesController` Veri görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="ee0fc-125">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="ee0fc-126">Entity Framework Core desteği eklendi</span><span class="sxs-lookup"><span data-stu-id="ee0fc-126">Add support for Entity Framework Core</span></span>

<span data-ttu-id="ee0fc-127">Yükleme [Entity Framework Core Inmemory](/ef/core/providers/in-memory/) veritabanı sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-127">Install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="ee0fc-128">Bu veritabanı sağlayıcısı, bir bellek içi veritabanı ile kullanılacak Entity Framework Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="ee0fc-129">Gelen **proje** menüsünde **NuGet paketleri Ekle**.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-129">From the **Project** menu, select **Add NuGet Packages**.</span></span>

  * <span data-ttu-id="ee0fc-130">Alternatif olarak, sağ **bağımlılıkları**ve ardından **paketleri Ekle**.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-130">Alternatively, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="ee0fc-131">Girin `EntityFrameworkCore.InMemory` arama kutusuna.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-131">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="ee0fc-132">Seçin `Microsoft.EntityFrameworkCore.InMemory`ve ardından **Paketi Ekle**.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-132">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="ee0fc-133">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="ee0fc-133">Add a model class</span></span>

<span data-ttu-id="ee0fc-134">Uygulamanızda veri temsil eden bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="ee0fc-135">Bu durumda, bir yapılacak iş öğesi tek model budur.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="ee0fc-136">Çözüm Gezgini'nde projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-136">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="ee0fc-137">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-137">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="ee0fc-138">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-138">Name the folder *Models*.</span></span>

![Yeni klasör](first-web-api-mac/_static/folder.png)

> [!NOTE]
> <span data-ttu-id="ee0fc-140">Model sınıfları, projenizdeki herhangi bir yere koyabilirsiniz ancak *modelleri* klasörü, kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="ee0fc-141">Sağ *modelleri* klasörü ve select **Ekle** > **yeni dosya** > **genel**  >  **Boş sınıf**.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-141">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span> <span data-ttu-id="ee0fc-142">Sınıf adı *Todoıtem*ve ardından **yeni**.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-142">Name the class *TodoItem*, and then click **New**.</span></span>

<span data-ttu-id="ee0fc-143">Oluşturulan kod ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="ee0fc-143">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="ee0fc-144">Veritabanı oluşturur `Id` olduğunda bir `TodoItem` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="ee0fc-145">Veritabanı bağlamı oluşturur</span><span class="sxs-lookup"><span data-stu-id="ee0fc-145">Create the database context</span></span>

<span data-ttu-id="ee0fc-146">*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="ee0fc-147">Türeterek Bu sınıf oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="ee0fc-148">Ekleme bir `TodoContext` sınıfının *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-148">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="ee0fc-149">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="ee0fc-149">Add a controller</span></span>

<span data-ttu-id="ee0fc-150">Çözüm Gezgini içinde *denetleyicileri* klasöründe sınıfı Ekle `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-150">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="ee0fc-151">Oluşturulan kodu aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="ee0fc-151">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="ee0fc-152">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="ee0fc-152">Launch the app</span></span>

<span data-ttu-id="ee0fc-153">Visual Studio'da **çalıştırmak** > **ile Start Debugging** uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-153">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="ee0fc-154">Visual Studio bir tarayıcı ile başlatarak `http://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-154">Visual Studio launches a browser and navigates to `http://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="ee0fc-155">HTTP 404 (bulunamadı) hatası alırsınız.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-155">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="ee0fc-156">URL'yi `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-156">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="ee0fc-157">`ValuesController` Veri görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="ee0fc-157">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="ee0fc-158">Gidin `Todo` denetleyicisinde `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-158">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="ee0fc-159">Aşağıdaki JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="ee0fc-159">The following JSON is returned:</span></span>

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="ee0fc-160">Bir CRUD işlemleri uygulama</span><span class="sxs-lookup"><span data-stu-id="ee0fc-160">Implement the other CRUD operations</span></span>

<span data-ttu-id="ee0fc-161">Ekleyeceğiz `Create`, `Update`, ve `Delete` denetleyiciye yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-161">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="ee0fc-162">Ben yalnızca kodu gösterir ve temel farklar vurgulayın bu yöntemler bir tema çeşidi olduğundan.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-162">These methods are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="ee0fc-163">Ekleme veya değiştirme kod sonra projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-163">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="ee0fc-164">Create</span><span class="sxs-lookup"><span data-stu-id="ee0fc-164">Create</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="ee0fc-165">Önceki yöntem tarafından belirtildiği gibi bir HTTP POST için yanıt [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-165">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="ee0fc-166">[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) özniteliği HTTP isteği gövdesinden yapılacak iş öğesi değeri almak için MVC söyler.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-166">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="ee0fc-167">Önceki yöntem tarafından belirtildiği gibi bir HTTP POST için yanıt [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-167">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="ee0fc-168">MVC, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-168">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

<span data-ttu-id="ee0fc-169">`CreatedAtRoute` 201 yanıtında yöntemi döndürür.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-169">The `CreatedAtRoute` method returns a 201 response.</span></span> <span data-ttu-id="ee0fc-170">Yeni bir kaynak sunucuda oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-170">It's the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="ee0fc-171">`CreatedAtRoute` Ayrıca bir konum üst bilgisi yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-171">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="ee0fc-172">Location üst bilgisini, yeni oluşturulan yapılacak iş öğesi URI'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-172">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="ee0fc-173">Bkz: [10.2.2 oluşturulan 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="ee0fc-173">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="ee0fc-174">Bir oluşturma isteği göndermek için Postman'ı kullanma</span><span class="sxs-lookup"><span data-stu-id="ee0fc-174">Use Postman to send a Create request</span></span>

* <span data-ttu-id="ee0fc-175">Uygulamayı başlatın (**çalıştırma** > **Başlat hatalarını ayıklamaya**).</span><span class="sxs-lookup"><span data-stu-id="ee0fc-175">Start the app (**Run** > **Start With Debugging**).</span></span>
* <span data-ttu-id="ee0fc-176">Postman'i açın.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-176">Open Postman.</span></span>

![Postman Konsolu](first-web-api/_static/pmc.png)

* <span data-ttu-id="ee0fc-178">Localhost URL'sini kullanılan bağlantı noktası numarasını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-178">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="ee0fc-179">HTTP yöntemi kümesine *POST*.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-179">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="ee0fc-180">Tıklayın **gövdesi** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-180">Click the **Body** tab.</span></span>
* <span data-ttu-id="ee0fc-181">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-181">Select the **raw** radio button.</span></span>
* <span data-ttu-id="ee0fc-182">Tür kümesine *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-182">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="ee0fc-183">İstek gövdesi aşağıdaki JSON benzeyen bir yapılacak iş öğesi ile girin:</span><span class="sxs-lookup"><span data-stu-id="ee0fc-183">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="ee0fc-184">Tıklayın **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-184">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> <span data-ttu-id="ee0fc-185">Yanıt tıklandıktan sonra görüntülerse **Gönder**, devre dışı **SSL sertifika doğrulama** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-185">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="ee0fc-186">Bunun altında bulunan **dosya** > **ayarları**.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-186">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="ee0fc-187">Tıklayın **Gönder** ayarı devre dışı bırakma sonra yeniden düğmesi.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-187">Click the **Send** button again after disabling the setting.</span></span>

::: moniker-end

<span data-ttu-id="ee0fc-188">Tıklayın **üstbilgileri** sekmesinde **yanıt** bölmesi ve kopyalama **konumu** üst bilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="ee0fc-188">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmc2.png)

<span data-ttu-id="ee0fc-190">URI konum üst bilgisi, oluşturduğunuz kaynağa erişmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-190">You can use the Location header URI to access the resource you created.</span></span> <span data-ttu-id="ee0fc-191">`Create` Yöntemi döndürür [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span><span class="sxs-lookup"><span data-stu-id="ee0fc-191">The `Create` method returns [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span></span> <span data-ttu-id="ee0fc-192">Geçirilen ilk parametre `CreatedAtRoute` URL'yi oluşturmak için adlandırılmış bir rotayı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-192">The first parameter passed to `CreatedAtRoute` represents the named route to use for generating the URL.</span></span> <span data-ttu-id="ee0fc-193">Bu geri çağırma `GetById` oluşturulan yöntemi `"GetTodo"` rota adı:</span><span class="sxs-lookup"><span data-stu-id="ee0fc-193">Recall that the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a><span data-ttu-id="ee0fc-194">Güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="ee0fc-194">Update</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

<span data-ttu-id="ee0fc-195">`Update` benzer `Create`, ancak HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-195">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="ee0fc-196">Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="ee0fc-196">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="ee0fc-197">HTTP spec göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca deltaları göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-197">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="ee0fc-198">Kısmi güncelleştirmeleri desteklemek için HTTP PATCH kullanın.</span><span class="sxs-lookup"><span data-stu-id="ee0fc-198">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="ee0fc-200">Sil</span><span class="sxs-lookup"><span data-stu-id="ee0fc-200">Delete</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="ee0fc-201">Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="ee0fc-201">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
