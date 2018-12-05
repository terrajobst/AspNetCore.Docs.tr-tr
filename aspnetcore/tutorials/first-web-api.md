---
title: "Öğretici: ASP.NET Core MVC ile bir web API'si oluşturma"
author: rick-anderson
description: Bir web API ASP.NET Core MVC ile oluşturma
ms.author: riande
monikerRange: '> aspnetcore-2.1'
ms.custom: mvc
ms.date: 11/19/2018
uid: tutorials/first-web-api
ms.openlocfilehash: 6e71771cd76b83da98bbad3e96469f635909198f
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862401"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core-mvc"></a><span data-ttu-id="a9612-103">Öğretici: ASP.NET Core MVC ile bir web API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="a9612-103">Tutorial: Create a web API with ASP.NET Core MVC</span></span>

<span data-ttu-id="a9612-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="a9612-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="a9612-105">Bu öğretici, bir web API ASP.NET Core ile oluşturmaya ilişkin temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="a9612-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="a9612-106">Bu öğreticide, şunların nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="a9612-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a9612-107">Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a9612-107">Create a web api project.</span></span>
> * <span data-ttu-id="a9612-108">Bir model sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a9612-108">Add a model class.</span></span>
> * <span data-ttu-id="a9612-109">Veritabanı bağlamı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a9612-109">Create the database context.</span></span>
> * <span data-ttu-id="a9612-110">Veritabanı bağlamı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="a9612-110">Register the database context.</span></span>
> * <span data-ttu-id="a9612-111">Bir denetleyici ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="a9612-111">Add a controller.</span></span>
> * <span data-ttu-id="a9612-112">CRUD yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a9612-112">Add CRUD methods.</span></span>
> * <span data-ttu-id="a9612-113">Yönlendirmeyi Yapılandırma ve URL yolu.</span><span class="sxs-lookup"><span data-stu-id="a9612-113">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="a9612-114">Dönüş değerleri belirtin.</span><span class="sxs-lookup"><span data-stu-id="a9612-114">Specify return values.</span></span>
> * <span data-ttu-id="a9612-115">Web API'si Postman ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="a9612-115">Call the web API with Postman.</span></span>
> * <span data-ttu-id="a9612-116">Web arama API'si jQuery ile.</span><span class="sxs-lookup"><span data-stu-id="a9612-116">Call the web api with jQuery.</span></span>

<span data-ttu-id="a9612-117">Sonunda, web API'si "Yapılacaklar" öğelerini ilişkisel bir veritabanında depolanan yönetebileceği sahip.</span><span class="sxs-lookup"><span data-stu-id="a9612-117">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="a9612-118">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="a9612-118">Overview</span></span>

<span data-ttu-id="a9612-119">Bu öğretici yandaki API oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a9612-119">This tutorial creates the following API:</span></span>

|<span data-ttu-id="a9612-120">API</span><span class="sxs-lookup"><span data-stu-id="a9612-120">API</span></span> | <span data-ttu-id="a9612-121">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a9612-121">Description</span></span> | <span data-ttu-id="a9612-122">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="a9612-122">Request body</span></span> | <span data-ttu-id="a9612-123">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="a9612-123">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="a9612-124">/Api/TODO Al</span><span class="sxs-lookup"><span data-stu-id="a9612-124">GET /api/todo</span></span> | <span data-ttu-id="a9612-125">Tüm yapılacak iş öğeleri al</span><span class="sxs-lookup"><span data-stu-id="a9612-125">Get all to-do items</span></span> | <span data-ttu-id="a9612-126">Yok.</span><span class="sxs-lookup"><span data-stu-id="a9612-126">None</span></span> | <span data-ttu-id="a9612-127">Yapılacaklar öğelerinin bir dizisi</span><span class="sxs-lookup"><span data-stu-id="a9612-127">Array of to-do items</span></span>|
|<span data-ttu-id="a9612-128">Alma/API'si/todo / {id}</span><span class="sxs-lookup"><span data-stu-id="a9612-128">GET /api/todo/{id}</span></span> | <span data-ttu-id="a9612-129">Bir öğeyi Kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="a9612-129">Get an item by ID</span></span> | <span data-ttu-id="a9612-130">Yok.</span><span class="sxs-lookup"><span data-stu-id="a9612-130">None</span></span> | <span data-ttu-id="a9612-131">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="a9612-131">To-do item</span></span>|
|<span data-ttu-id="a9612-132">Todo/api/gönderin</span><span class="sxs-lookup"><span data-stu-id="a9612-132">POST /api/todo</span></span> | <span data-ttu-id="a9612-133">Yeni Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="a9612-133">Add a new item</span></span> | <span data-ttu-id="a9612-134">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="a9612-134">To-do item</span></span> | <span data-ttu-id="a9612-135">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="a9612-135">To-do item</span></span> |
|<span data-ttu-id="a9612-136">PUT/API'si/todo / {id}</span><span class="sxs-lookup"><span data-stu-id="a9612-136">PUT /api/todo/{id}</span></span> | <span data-ttu-id="a9612-137">Mevcut öğeyi güncelleştirin &nbsp;</span><span class="sxs-lookup"><span data-stu-id="a9612-137">Update an existing item &nbsp;</span></span> | <span data-ttu-id="a9612-138">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="a9612-138">To-do item</span></span> | <span data-ttu-id="a9612-139">Yok.</span><span class="sxs-lookup"><span data-stu-id="a9612-139">None</span></span> |
|<span data-ttu-id="a9612-140">/ API'si/todo / {id} Sil &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="a9612-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="a9612-141">Öğeyi Sil &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="a9612-141">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="a9612-142">Yok.</span><span class="sxs-lookup"><span data-stu-id="a9612-142">None</span></span> | <span data-ttu-id="a9612-143">Yok.</span><span class="sxs-lookup"><span data-stu-id="a9612-143">None</span></span>|

<span data-ttu-id="a9612-144">Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a9612-144">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki kutuyu temsil edilir ve bir istek gönderdiğini ve sağ tarafta çizilmiş bir kutu uygulamasından bir yanıt alır.](first-web-api/_static/architecture.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="a9612-149">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a9612-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9612-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9612-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a9612-151">Gelen **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="a9612-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a9612-152">Seçin **ASP.NET Core Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="a9612-152">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="a9612-153">Projeyi adlandırın *TodoApi* tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a9612-153">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="a9612-154">İçinde **yeni ASP.NET Core Web uygulaması - TodoApi** iletişim kutusunda, ASP.NET Core sürümünü seçin.</span><span class="sxs-lookup"><span data-stu-id="a9612-154">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="a9612-155">Seçin **API** şablonu ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a9612-155">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="a9612-156">Yapmak **değil** seçin **Docker desteğini etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="a9612-156">Do **not** select **Enable Docker Support**.</span></span>

![VS yeni proje iletişim kutusu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9612-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9612-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a9612-159">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a9612-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a9612-160">Dizinleri (`cd`) proje klasörünü içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="a9612-160">Change directories (`cd`) to the folder which will contain the project folder.</span></span>
* <span data-ttu-id="a9612-161">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a9612-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="a9612-162">Bu komutlar, yeni bir web API projesi oluşturun ve Visual Studio Code yeni bir örneğini kendisi içinde yeni proje klasörü açın.</span><span class="sxs-lookup"><span data-stu-id="a9612-162">These commands create a new web API project and open a new instance of Visual Studio Code in he new project folder.</span></span>

* <span data-ttu-id="a9612-163">Bir iletişim kutusu gerekli varlıklar projeye eklemek isteyip istemediğinizi seçin sorduğunda **Evet**</span><span class="sxs-lookup"><span data-stu-id="a9612-163">When a dialog box asks if you want to add required assets to the project, select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a9612-164">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9612-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a9612-165">Seçin **dosya** > **yeni çözüm**.</span><span class="sxs-lookup"><span data-stu-id="a9612-165">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="a9612-167">Seçin **.NET Core uygulaması** > **ASP.NET Core Web API'sini** > **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="a9612-167">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)

* <span data-ttu-id="a9612-169">Girin *TodoApi* için **proje adı** seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="a9612-169">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="a9612-171">API'yi test etme</span><span class="sxs-lookup"><span data-stu-id="a9612-171">Test the API</span></span>

<span data-ttu-id="a9612-172">Proje şablonu oluşturur bir `values` API.</span><span class="sxs-lookup"><span data-stu-id="a9612-172">The project template creates a `values` API.</span></span> <span data-ttu-id="a9612-173">Çağrı `Get` uygulamayı test etmek için bir tarayıcıdan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a9612-173">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9612-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9612-174">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a9612-175">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="a9612-175">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="a9612-176">Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>/api/values`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="a9612-176">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="a9612-177">IIS Express sertifika güven varsa soran bir iletişim kutusu alırsanız seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="a9612-177">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="a9612-178">İçinde **Güvenlik Uyarısı** ardından, görüntülenen iletişim seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="a9612-178">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9612-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9612-179">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a9612-180">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="a9612-180">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="a9612-181">Bir tarayıcıda aşağıdaki URL'ye gidin: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="a9612-181">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a9612-182">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9612-182">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a9612-183">Seçin **çalıştırma** > **ile Start Debugging** uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="a9612-183">Select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="a9612-184">Mac için Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="a9612-184">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="a9612-185">HTTP 404 (bulunamadı) hatası döndürülür.</span><span class="sxs-lookup"><span data-stu-id="a9612-185">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="a9612-186">Append `/api/values` URL'sine (URL'yi `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="a9612-186">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="a9612-187">Aşağıdaki JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="a9612-187">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="a9612-188">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="a9612-188">Add a model class</span></span>

<span data-ttu-id="a9612-189">A *modeli* uygulamayı yöneten verilerini temsil eden sınıflar kümesidir.</span><span class="sxs-lookup"><span data-stu-id="a9612-189">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="a9612-190">Tek bir modeldir bu uygulama için `TodoItem` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a9612-190">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9612-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9612-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a9612-192">İçinde **Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a9612-192">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="a9612-193">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="a9612-193">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="a9612-194">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="a9612-194">Name the folder *Models*.</span></span>

* <span data-ttu-id="a9612-195">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="a9612-195">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="a9612-196">Sınıf adı *Todoıtem* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="a9612-196">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="a9612-197">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a9612-197">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9612-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9612-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a9612-199">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="a9612-199">Add a folder named *Models*.</span></span>

* <span data-ttu-id="a9612-200">Ekleme bir `TodoItem` sınıfının *modelleri* aşağıdaki kodla klasörü:</span><span class="sxs-lookup"><span data-stu-id="a9612-200">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a9612-201">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9612-201">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a9612-202">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a9612-202">Right-click the project.</span></span> <span data-ttu-id="a9612-203">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="a9612-203">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="a9612-204">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="a9612-204">Name the folder *Models*.</span></span>

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="a9612-206">Sağ *modelleri* klasörü ve select **Ekle** > **yeni dosya** > **genel**  >  **Boş sınıf**.</span><span class="sxs-lookup"><span data-stu-id="a9612-206">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="a9612-207">Sınıf adı *Todoıtem*ve ardından **yeni**.</span><span class="sxs-lookup"><span data-stu-id="a9612-207">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="a9612-208">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a9612-208">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="a9612-209">`Id` Özelliği işlevlerinin bir ilişkisel veritabanında benzersiz anahtar.</span><span class="sxs-lookup"><span data-stu-id="a9612-209">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="a9612-210">Model sınıfları herhangi bir projede gidip ancak *modelleri* klasörü, kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a9612-210">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="a9612-211">Veritabanı bağlamı Ekle</span><span class="sxs-lookup"><span data-stu-id="a9612-211">Add a database context</span></span>

<span data-ttu-id="a9612-212">*Veritabanı bağlamı* koordine eden bir veri modeli için Entity Framework işlevsellik ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="a9612-212">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="a9612-213">Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a9612-213">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9612-214">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9612-214">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a9612-215">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="a9612-215">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="a9612-216">Sınıf adı *TodoContext* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="a9612-216">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9612-217">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9612-217">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a9612-218">Ekleme bir `TodoContext` sınıfının *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="a9612-218">Add a `TodoContext` class to the *Models* folder.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a9612-219">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9612-219">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a9612-220">Ekleme bir `TodoContext` sınıfını *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="a9612-220">Add a `TodoContext` class in the *Models* folder:</span></span>

---

* <span data-ttu-id="a9612-221">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a9612-221">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="a9612-222">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="a9612-222">Register the database context</span></span>

<span data-ttu-id="a9612-223">ASP.NET Core DB bağlamı gibi hizmetler ile kaydedilmelidir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="a9612-223">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="a9612-224">Kapsayıcı hizmeti denetleyicilerine sağlar.</span><span class="sxs-lookup"><span data-stu-id="a9612-224">The container provides the service to controllers.</span></span>

<span data-ttu-id="a9612-225">Güncelleştirme *Startup.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="a9612-225">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="a9612-226">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="a9612-226">The preceding code:</span></span>

* <span data-ttu-id="a9612-227">Kullanılmayan kaldırır `using` bildirimleri.</span><span class="sxs-lookup"><span data-stu-id="a9612-227">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="a9612-228">Veritabanı bağlamı DI kapsayıcıya ekler.</span><span class="sxs-lookup"><span data-stu-id="a9612-228">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="a9612-229">Veritabanı bağlamı bir bellek içi veritabanına kullanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="a9612-229">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="a9612-230">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="a9612-230">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9612-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9612-231">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a9612-232">Sağ *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="a9612-232">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="a9612-233">Seçin **ekleme** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="a9612-233">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="a9612-234">İçinde **Yeni Öğe Ekle** iletişim kutusunda **API denetleyici sınıfı** şablonu.</span><span class="sxs-lookup"><span data-stu-id="a9612-234">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="a9612-235">Sınıf adı *TodoController*seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="a9612-235">Name the class *TodoController*, and select **Add**.</span></span>

  ![Yeni öğe iletişim denetleyicisiyle seçilen arama kutusu ve web API denetleyicisi Ekle](first-web-api/_static/new_controller.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9612-237">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9612-237">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a9612-238">İçinde *denetleyicileri* klasör adında bir sınıf oluşturma `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="a9612-238">In the *Controllers* folder, create a class named `TodoController`.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a9612-239">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9612-239">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a9612-240">İçinde *denetleyicileri* klasörü, sınıf ekleme `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="a9612-240">In the *Controllers* folder, add the class `TodoController`.</span></span>

---

* <span data-ttu-id="a9612-241">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a9612-241">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="a9612-242">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="a9612-242">The preceding code:</span></span>

* <span data-ttu-id="a9612-243">Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a9612-243">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="a9612-244">Sınıf ile düzenler [ `[ApiController]` ](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a9612-244">Decorates the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="a9612-245">Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="a9612-245">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="a9612-246">Özniteliği sağlayan belirli davranışları hakkında daha fazla bilgi için bkz: [ApiController özniteliğiyle ek açıklama](xref:web-api/index#annotation-with-apicontroller-attribute).</span><span class="sxs-lookup"><span data-stu-id="a9612-246">For information about specific behaviors that the attribute enables, see [Annotation with ApiController attribute](xref:web-api/index#annotation-with-apicontroller-attribute).</span></span>
* <span data-ttu-id="a9612-247">Veritabanı bağlamı eklemesine DI kullanır (`TodoContext`) içine denetleyici.</span><span class="sxs-lookup"><span data-stu-id="a9612-247">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="a9612-248">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="a9612-248">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="a9612-249">Adlı bir öğe ekler `Item1` veritabanı boşsa veritabanı.</span><span class="sxs-lookup"><span data-stu-id="a9612-249">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="a9612-250">Her çalıştığında bu kod oluşturucusunun içinde yeni bir HTTP isteği olduğundan.</span><span class="sxs-lookup"><span data-stu-id="a9612-250">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="a9612-251">Tüm öğeleri silerseniz, oluşturucu oluşturur `Item1` API yöntemi çağrıldığında tekrar başlattığınızda.</span><span class="sxs-lookup"><span data-stu-id="a9612-251">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="a9612-252">Bu nedenle, gerçekten işe yaradı silme işlemi işe yaramadı gibi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="a9612-252">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="a9612-253">Get yöntemleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="a9612-253">Add Get methods</span></span>

<span data-ttu-id="a9612-254">Yapılacak iş öğeleri alır bir API sağlamak için aşağıdaki yöntemi ekleyin. `TodoController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="a9612-254">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="a9612-255">İki GET uç noktası bu yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="a9612-255">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="a9612-256">Bir tarayıcıdan iki uç nokta çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="a9612-256">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="a9612-257">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a9612-257">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="a9612-258">Şu HTTP yanıtı çağrısı tarafından üretilen `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="a9612-258">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="a9612-259">URL Yönlendirme ve yolları</span><span class="sxs-lookup"><span data-stu-id="a9612-259">Routing and URL paths</span></span>

<span data-ttu-id="a9612-260">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Özniteliği bir HTTP GET isteğine yanıt vermeden bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="a9612-260">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="a9612-261">Her yöntem için URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="a9612-261">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="a9612-262">Denetleyicinin şablonu dizesi ile başlayıp `Route` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="a9612-262">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="a9612-263">Değiştirin `[controller]` denetleyicinin adı ile kural tarafından olduğu "Controller" soneki eksi denetleyici sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="a9612-263">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="a9612-264">Bu örnek, denetleyici sınıfı adı olan **Todo**Denetleyici adı "todo" Bu nedenle denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="a9612-264">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="a9612-265">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="a9612-265">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="a9612-266">Varsa `[HttpGet]` özniteliğine sahip bir rota şablonu (örneğin, `[HttpGet("/products")]`, yolunu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a9612-266">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="a9612-267">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="a9612-267">This sample doesn't use a template.</span></span> <span data-ttu-id="a9612-268">Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="a9612-268">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="a9612-269">Aşağıdaki `GetTodoItem` yöntemi `"{id}"` yapılacak iş öğesi benzersiz tanımlayıcısı için bir yer tutucu değişkendir.</span><span class="sxs-lookup"><span data-stu-id="a9612-269">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="a9612-270">Zaman `GetTodoItem` çağrılır, değerini `"{id}"` yöntemine URL'de sağlanan kendi`id` parametresi.</span><span class="sxs-lookup"><span data-stu-id="a9612-270">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetTodoItem&highlight=1-2)]

<span data-ttu-id="a9612-271">`Name = "GetTodo"` Adlandırılan bir rota parametresi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a9612-271">The `Name = "GetTodo"` parameter creates a named route.</span></span> <span data-ttu-id="a9612-272">Uygulama için rota adlarını kullanarak bir HTTP bağlantısı oluşturmak için adı daha sonra nasıl kullanabileceğinizi görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="a9612-272">You'll see later how the app can use the name to create an HTTP link using the route name.</span></span>

## <a name="return-values"></a><span data-ttu-id="a9612-273">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="a9612-273">Return values</span></span>

<span data-ttu-id="a9612-274">Dönüş türünü `GetTodoItems` ve `GetTodoItem` yöntemler [actionresult öğesini\<T > türü](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="a9612-274">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="a9612-275">ASP.NET Core, nesneyi otomatik olarak serileştiren [JSON](https://www.json.org/) ve yanıt iletisinin gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="a9612-275">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="a9612-276">Yanıt kodu 200 bu dönüş türü için olduğu varsayılırsa işlenmeyen özel durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="a9612-276">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="a9612-277">İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.</span><span class="sxs-lookup"><span data-stu-id="a9612-277">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="a9612-278">`ActionResult` dönüş türleri, geniş HTTP durum kodları temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="a9612-278">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="a9612-279">Örneğin, `GetTodoItem` iki farklı durum değerleri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="a9612-279">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="a9612-280">Öğe istenen kimliği eşleşirse, yöntem bir 404 döndürür [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu.</span><span class="sxs-lookup"><span data-stu-id="a9612-280">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="a9612-281">Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="a9612-281">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="a9612-282">Döndüren `item` sonuçları bir HTTP 200 yanıtı.</span><span class="sxs-lookup"><span data-stu-id="a9612-282">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="a9612-283">Test GetTodoItems yöntemi</span><span class="sxs-lookup"><span data-stu-id="a9612-283">Test the GetTodoItems method</span></span>

<span data-ttu-id="a9612-284">Bu öğreticide Postman web API'si test etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a9612-284">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="a9612-285">Yükleme [Postman](https://www.getpostman.com/apps)</span><span class="sxs-lookup"><span data-stu-id="a9612-285">Install [Postman](https://www.getpostman.com/apps)</span></span>
* <span data-ttu-id="a9612-286">Web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="a9612-286">Start the web app.</span></span>
* <span data-ttu-id="a9612-287">Postman'i başlatın.</span><span class="sxs-lookup"><span data-stu-id="a9612-287">Start Postman.</span></span>
* <span data-ttu-id="a9612-288">Devre dışı **SSL sertifika doğrulama**</span><span class="sxs-lookup"><span data-stu-id="a9612-288">Disable **SSL certificate verification**</span></span>
  
  * <span data-ttu-id="a9612-289">Gelen **Dosya > Ayarlar** (\**genel* sekmesinde), devre dışı **SSL sertifika doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="a9612-289">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="a9612-290">Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="a9612-290">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="a9612-291">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a9612-291">Create a new request.</span></span>
  * <span data-ttu-id="a9612-292">HTTP yöntemi kümesine **alma**.</span><span class="sxs-lookup"><span data-stu-id="a9612-292">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="a9612-293">İstek URL'si kümesine `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="a9612-293">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="a9612-294">Örneğin: `https://localhost:5001/api/todo`</span><span class="sxs-lookup"><span data-stu-id="a9612-294">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="a9612-295">Ayarlama **iki bölme görünümü** postman'deki.</span><span class="sxs-lookup"><span data-stu-id="a9612-295">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="a9612-296">Seçin **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="a9612-296">Select **Send**.</span></span>

![Get isteğiyle postman](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="a9612-298">Create yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="a9612-298">Add a Create method</span></span>

<span data-ttu-id="a9612-299">Aşağıdaki `PostTodoItem` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a9612-299">Add the following `PostTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="a9612-300">Yukarıdaki kod tarafından belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a9612-300">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="a9612-301">Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="a9612-301">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="a9612-302">`CreatedAtRoute` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a9612-302">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="a9612-303">201 yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="a9612-303">Returns a 201 response.</span></span> <span data-ttu-id="a9612-304">HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="a9612-304">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="a9612-305">Bir konum üst bilgisi yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="a9612-305">Adds a Location header to the response.</span></span> <span data-ttu-id="a9612-306">Location üst bilgisini, yeni oluşturulan yapılacak iş öğesi URI'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a9612-306">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="a9612-307">Daha fazla bilgi için [10.2.2 201 oluşturuldu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="a9612-307">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="a9612-308">URL oluşturmak için "adlı rota GetTodo" kullanır.</span><span class="sxs-lookup"><span data-stu-id="a9612-308">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="a9612-309">"Adlı rota GetTodo" içinde tanımlanan `GetTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="a9612-309">The "GetTodo" named route is defined in `GetTodoItem`:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="a9612-310">Test PostTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="a9612-310">Test the PostTodoItem method</span></span>

* <span data-ttu-id="a9612-311">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a9612-311">Build the project.</span></span>
* <span data-ttu-id="a9612-312">Postman HTTP yöntemi kümesine `POST`.</span><span class="sxs-lookup"><span data-stu-id="a9612-312">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="a9612-313">Seçin **gövdesi** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="a9612-313">Select the **Body** tab.</span></span>
* <span data-ttu-id="a9612-314">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="a9612-314">Select the **raw** radio button.</span></span>
* <span data-ttu-id="a9612-315">Tür kümesine **JSON (application/json)**.</span><span class="sxs-lookup"><span data-stu-id="a9612-315">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="a9612-316">İstek gövdesinde bir yapılacak iş öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="a9612-316">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="a9612-317">Seçin **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="a9612-317">Select **Send**.</span></span>

  ![Postman ile isteği oluştur](first-web-api/_static/create.png)

  <span data-ttu-id="a9612-319">405 bir yönteme izin verilmiyor hata alırsanız, büyük olasılıkla proje sonra ekleme ekledikten sonra derleme değil sonucudur `PostTodoItem` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a9612-319">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="a9612-320">Konum üst bilgisi URI test</span><span class="sxs-lookup"><span data-stu-id="a9612-320">Test the location header URI</span></span>

* <span data-ttu-id="a9612-321">Seçin **üstbilgileri** sekmesinde **yanıt** bölmesi.</span><span class="sxs-lookup"><span data-stu-id="a9612-321">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="a9612-322">Kopyalama **konumu** üst bilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="a9612-322">Copy the **Location** header value:</span></span>

  ![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmc2.png)

* <span data-ttu-id="a9612-324">Yöntemini GET öğesine Ayarla.</span><span class="sxs-lookup"><span data-stu-id="a9612-324">Set the method to GET.</span></span>
* <span data-ttu-id="a9612-325">URİ'sini yapıştırın (örneğin, `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="a9612-325">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="a9612-326">Seçin **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="a9612-326">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="a9612-327">PutTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="a9612-327">Add a PutTodoItem method</span></span>

<span data-ttu-id="a9612-328">Aşağıdaki `PutTodoItem` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a9612-328">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="a9612-329">`PutTodoItem` benzer `PostTodoItem`, HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="a9612-329">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="a9612-330">Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a9612-330">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="a9612-331">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca değişiklikler değil göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a9612-331">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="a9612-332">Kısmi güncelleştirmeleri desteklemek için kullanma [HTTP PATCH](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="a9612-332">To support partial updates, use [HTTP PATCH](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="a9612-333">Test PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="a9612-333">Test the PutTodoItem method</span></span>

<span data-ttu-id="a9612-334">Kimliğine sahip bir yapılacak iş öğesi güncelleştirme = 1 ve "balık akış için" adını girin:</span><span class="sxs-lookup"><span data-stu-id="a9612-334">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="a9612-335">Aşağıdaki görüntüde, Postman güncelleştirme gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="a9612-335">The following image shows the Postman update:</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="a9612-337">DeleteTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="a9612-337">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="a9612-338">Aşağıdaki `DeleteTodoItem` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a9612-338">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="a9612-339">`DeleteTodoItem` Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a9612-339">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="a9612-340">Test DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="a9612-340">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="a9612-341">Postman bir yapılacak iş öğesini silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="a9612-341">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="a9612-342">Yöntem kümesine `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="a9612-342">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="a9612-343">Silmek, örneğin nesnenin URI ayarlayın `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="a9612-343">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="a9612-344">Seçin **Gönder**</span><span class="sxs-lookup"><span data-stu-id="a9612-344">Select **Send**</span></span>

<span data-ttu-id="a9612-345">Örnek uygulamayı tüm öğeleri silmenize olanak sağlar, ancak son öğe silindiğinde, yeni bir model sınıfı Oluşturucu tarafından API'sı çağrılan başlatıldığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a9612-345">The sample app allows you to delete all the items, but when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="a9612-346">JQuery ile API çağırma</span><span class="sxs-lookup"><span data-stu-id="a9612-346">Call the API with jQuery</span></span>

<span data-ttu-id="a9612-347">Bu bölümde, Web'i çağırmaya jQuery kullanan bir HTML sayfasına eklenen API.</span><span class="sxs-lookup"><span data-stu-id="a9612-347">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="a9612-348">jQuery isteği başlatır ve API'nin yanıt Ayrıntıları sayfası güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="a9612-348">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="a9612-349">İçin uygulamayı yapılandırma [statik dosyaları işleme](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [varsayılan dosya eşlemesini etkinleştir](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):</span><span class="sxs-lookup"><span data-stu-id="a9612-349">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
<span data-ttu-id="a9612-350">Oluşturma bir *wwwroot* proje dizininde klasör.</span><span class="sxs-lookup"><span data-stu-id="a9612-350">Create a *wwwroot* folder in the project directory.</span></span>
::: moniker-end

<span data-ttu-id="a9612-351">Adlı bir HTML dosyası ekleyin *index.html* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="a9612-351">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="a9612-352">Dosyanın içeriğini aşağıdaki biçimlendirme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a9612-352">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="a9612-353">Adlı bir JavaScript dosyası ekleyin *site.js* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="a9612-353">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="a9612-354">Dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a9612-354">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="a9612-355">ASP.NET Core proje başlatma ayarlarında bir değişiklik HTML sayfasını yerel olarak test etmek için gerekli:</span><span class="sxs-lookup"><span data-stu-id="a9612-355">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="a9612-356">Açık *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a9612-356">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="a9612-357">Kaldırma `launchUrl` , açmak için uygulamayı zorlamak için özellik *index.html*&mdash;projenin varsayılan dosya.</span><span class="sxs-lookup"><span data-stu-id="a9612-357">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="a9612-358">JQuery almak için birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="a9612-358">There are several ways to get jQuery.</span></span> <span data-ttu-id="a9612-359">Yukarıdaki kod parçacığında, bir CDN kitaplığı yüklenir.</span><span class="sxs-lookup"><span data-stu-id="a9612-359">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="a9612-360">Bu örnek, tüm API CRUD yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="a9612-360">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="a9612-361">API çağrıları açıklamaları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a9612-361">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="a9612-362">Yapılacaklar öğelerinin bir listesini alın</span><span class="sxs-lookup"><span data-stu-id="a9612-362">Get a list of to-do items</span></span>

<span data-ttu-id="a9612-363">JQuery [ajax](https://api.jquery.com/jquery.ajax/) işlev gönderen bir `GET` Yapılacaklar öğelerini dizisini temsil eden JSON döndüren API isteği.</span><span class="sxs-lookup"><span data-stu-id="a9612-363">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="a9612-364">`success` İstek başarılı olursa geri çağırma işlevi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a9612-364">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="a9612-365">Geri çağırma içinde DOM Yapılacaklar bilgilerle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a9612-365">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="a9612-366">Yapılacak İş Öğesi Ekle</span><span class="sxs-lookup"><span data-stu-id="a9612-366">Add a to-do item</span></span>

<span data-ttu-id="a9612-367">[Ajax](https://api.jquery.com/jquery.ajax/) işlev gönderen bir `POST` istek ile istek gövdesindeki yapılacak iş öğesi.</span><span class="sxs-lookup"><span data-stu-id="a9612-367">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="a9612-368">`accepts` Ve `contentType` seçeneklerini ayarlamak `application/json` gönderilen ve alınan medya türü belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="a9612-368">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="a9612-369">Yapılacak iş öğesi kullanarak JSON'a dönüştürülür [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="a9612-369">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="a9612-370">API'yi bir başarılı durum kodu döndürdüğünde `getData` işlevi HTML tablosu güncelleştirmek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a9612-370">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="a9612-371">Yapılacak iş öğesini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="a9612-371">Update a to-do item</span></span>

<span data-ttu-id="a9612-372">Yapılacak iş öğesi güncelleştirilirken bir eklemeye benzerdir.</span><span class="sxs-lookup"><span data-stu-id="a9612-372">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="a9612-373">`url` Öğenin benzersiz tanıtıcısı eklemek için değişiklikleri ve `type` olduğu `PUT`.</span><span class="sxs-lookup"><span data-stu-id="a9612-373">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="a9612-374">Yapılacak iş öğesi silme</span><span class="sxs-lookup"><span data-stu-id="a9612-374">Delete a to-do item</span></span>

<span data-ttu-id="a9612-375">Yapılacak iş öğesi silme gerçekleştirilir ayarlayarak `type` AJAX çağrısı hedefi üzerinde `DELETE` ve öğenin benzersiz tanıtıcısı URL'yi belirterek.</span><span class="sxs-lookup"><span data-stu-id="a9612-375">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a><span data-ttu-id="a9612-376">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a9612-376">Additional resources</span></span>

<span data-ttu-id="a9612-377">[Görüntülemek veya Bu öğretici için örnek kodu indirdikten](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="a9612-377">[View or download sample code for this tutorial](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="a9612-378">Bkz: [nasıl indirileceğini](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="a9612-378">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="a9612-379">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="a9612-379">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>

## <a name="next-steps"></a><span data-ttu-id="a9612-380">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="a9612-380">Next steps</span></span>

<span data-ttu-id="a9612-381">Bu öğreticide şunları öğrendiniz: nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="a9612-381">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a9612-382">Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a9612-382">Create a web api project.</span></span>
> * <span data-ttu-id="a9612-383">Bir model sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a9612-383">Add a model class.</span></span>
> * <span data-ttu-id="a9612-384">Veritabanı bağlamı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a9612-384">Create the database context.</span></span>
> * <span data-ttu-id="a9612-385">Veritabanı bağlamı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="a9612-385">Register the database context.</span></span>
> * <span data-ttu-id="a9612-386">Bir denetleyici ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="a9612-386">Add a controller.</span></span>
> * <span data-ttu-id="a9612-387">CRUD yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a9612-387">Add CRUD methods.</span></span>
> * <span data-ttu-id="a9612-388">Yönlendirmeyi Yapılandırma ve URL yolu.</span><span class="sxs-lookup"><span data-stu-id="a9612-388">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="a9612-389">Dönüş değerleri belirtin.</span><span class="sxs-lookup"><span data-stu-id="a9612-389">Specify return values.</span></span>
> * <span data-ttu-id="a9612-390">Web API'si Postman ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="a9612-390">Call the web API with Postman.</span></span>
> * <span data-ttu-id="a9612-391">Web arama API'si jQuery ile.</span><span class="sxs-lookup"><span data-stu-id="a9612-391">Call the web api with jQuery.</span></span>

<span data-ttu-id="a9612-392">API Yardım sayfaları oluşturma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="a9612-392">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
