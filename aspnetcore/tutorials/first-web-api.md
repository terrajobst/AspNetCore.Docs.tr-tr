---
title: "Öğretici: ASP.NET Core web API'si oluşturma"
author: rick-anderson
description: Bir web API ASP.NET Core ile oluşturmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 06/23/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 1c3d911593a288aa897373dc01616498706e7069
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815141"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="0cadb-103">Öğretici: ASP.NET Core web API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="0cadb-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="0cadb-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="0cadb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="0cadb-105">Bu öğretici, bir web API ASP.NET Core ile oluşturmaya ilişkin temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="0cadb-106">Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="0cadb-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0cadb-107">Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0cadb-107">Create a web API project.</span></span>
> * <span data-ttu-id="0cadb-108">Bir model sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-108">Add a model class.</span></span>
> * <span data-ttu-id="0cadb-109">Veritabanı bağlamı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0cadb-109">Create the database context.</span></span>
> * <span data-ttu-id="0cadb-110">Veritabanı bağlamı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-110">Register the database context.</span></span>
> * <span data-ttu-id="0cadb-111">Bir denetleyici ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="0cadb-111">Add a controller.</span></span>
> * <span data-ttu-id="0cadb-112">CRUD yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-112">Add CRUD methods.</span></span>
> * <span data-ttu-id="0cadb-113">Yönlendirmeyi Yapılandırma ve URL yolu.</span><span class="sxs-lookup"><span data-stu-id="0cadb-113">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="0cadb-114">Dönüş değerleri belirtin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-114">Specify return values.</span></span>
> * <span data-ttu-id="0cadb-115">Web API'si Postman ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="0cadb-115">Call the web API with Postman.</span></span>
> * <span data-ttu-id="0cadb-116">JQuery ile web API'sini çağırın.</span><span class="sxs-lookup"><span data-stu-id="0cadb-116">Call the web API with jQuery.</span></span>

<span data-ttu-id="0cadb-117">Sonunda, web API'si "Yapılacaklar" öğelerini ilişkisel bir veritabanında depolanan yönetebileceği sahip.</span><span class="sxs-lookup"><span data-stu-id="0cadb-117">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="0cadb-118">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="0cadb-118">Overview</span></span>

<span data-ttu-id="0cadb-119">Bu öğretici yandaki API oluşturur:</span><span class="sxs-lookup"><span data-stu-id="0cadb-119">This tutorial creates the following API:</span></span>

|<span data-ttu-id="0cadb-120">API</span><span class="sxs-lookup"><span data-stu-id="0cadb-120">API</span></span> | <span data-ttu-id="0cadb-121">Açıklama</span><span class="sxs-lookup"><span data-stu-id="0cadb-121">Description</span></span> | <span data-ttu-id="0cadb-122">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="0cadb-122">Request body</span></span> | <span data-ttu-id="0cadb-123">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="0cadb-123">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="0cadb-124">/Api/TODO Al</span><span class="sxs-lookup"><span data-stu-id="0cadb-124">GET /api/todo</span></span> | <span data-ttu-id="0cadb-125">Tüm yapılacak iş öğeleri al</span><span class="sxs-lookup"><span data-stu-id="0cadb-125">Get all to-do items</span></span> | <span data-ttu-id="0cadb-126">Yok.</span><span class="sxs-lookup"><span data-stu-id="0cadb-126">None</span></span> | <span data-ttu-id="0cadb-127">Yapılacaklar öğelerinin bir dizisi</span><span class="sxs-lookup"><span data-stu-id="0cadb-127">Array of to-do items</span></span>|
|<span data-ttu-id="0cadb-128">Alma/API'si/todo / {id}</span><span class="sxs-lookup"><span data-stu-id="0cadb-128">GET /api/todo/{id}</span></span> | <span data-ttu-id="0cadb-129">Bir öğeyi Kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="0cadb-129">Get an item by ID</span></span> | <span data-ttu-id="0cadb-130">Yok.</span><span class="sxs-lookup"><span data-stu-id="0cadb-130">None</span></span> | <span data-ttu-id="0cadb-131">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="0cadb-131">To-do item</span></span>|
|<span data-ttu-id="0cadb-132">Todo/api/gönderin</span><span class="sxs-lookup"><span data-stu-id="0cadb-132">POST /api/todo</span></span> | <span data-ttu-id="0cadb-133">Yeni Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="0cadb-133">Add a new item</span></span> | <span data-ttu-id="0cadb-134">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="0cadb-134">To-do item</span></span> | <span data-ttu-id="0cadb-135">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="0cadb-135">To-do item</span></span> |
|<span data-ttu-id="0cadb-136">PUT/API'si/todo / {id}</span><span class="sxs-lookup"><span data-stu-id="0cadb-136">PUT /api/todo/{id}</span></span> | <span data-ttu-id="0cadb-137">Mevcut öğeyi güncelleştirin &nbsp;</span><span class="sxs-lookup"><span data-stu-id="0cadb-137">Update an existing item &nbsp;</span></span> | <span data-ttu-id="0cadb-138">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="0cadb-138">To-do item</span></span> | <span data-ttu-id="0cadb-139">Hiçbiri</span><span class="sxs-lookup"><span data-stu-id="0cadb-139">None</span></span> |
|<span data-ttu-id="0cadb-140">/ API'si/todo / {id} Sil &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="0cadb-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="0cadb-141">Öğeyi Sil &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="0cadb-141">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="0cadb-142">Hiçbiri</span><span class="sxs-lookup"><span data-stu-id="0cadb-142">None</span></span> | <span data-ttu-id="0cadb-143">Hiçbiri</span><span class="sxs-lookup"><span data-stu-id="0cadb-143">None</span></span>|

<span data-ttu-id="0cadb-144">Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-144">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki kutuyu temsil edilir.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="0cadb-150">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="0cadb-150">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0cadb-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0cadb-151">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0cadb-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0cadb-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0cadb-153">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0cadb-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="0cadb-154">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="0cadb-154">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0cadb-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0cadb-155">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0cadb-156">Gelen **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-156">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0cadb-157">Seçin **ASP.NET Core Web uygulaması** şablonu ve tıklatın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-157">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="0cadb-158">Projeyi adlandırın *TodoApi* tıklatıp **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-158">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="0cadb-159">İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda onaylayın **.NET Core** ve **ASP.NET Core 2.2** seçilir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-159">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="0cadb-160">Seçin **API** şablonu ve tıklatın **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-160">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="0cadb-161">**Yoksa** seçin **Docker desteğini etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-161">**Don't** select **Enable Docker Support**.</span></span>

![VS yeni proje iletişim kutusu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0cadb-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0cadb-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0cadb-164">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="0cadb-164">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="0cadb-165">Dizinleri (`cd`) proje klasörünü içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="0cadb-165">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="0cadb-166">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0cadb-166">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="0cadb-167">Bu komutlar, yeni bir web API projesi oluşturun ve yeni proje klasöründe Visual Studio Code yeni bir örneğini açın.</span><span class="sxs-lookup"><span data-stu-id="0cadb-167">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="0cadb-168">Bir iletişim kutusu gerekli varlıklar projeye eklemek isteyip istemediğinizi seçin sorduğunda **Evet**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-168">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0cadb-169">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0cadb-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0cadb-170">Seçin **dosya** > **yeni çözüm**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-170">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="0cadb-172">Seçin **.NET Core** > **uygulama** > **API** > **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-172">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="0cadb-174">İçinde **, yeni ASP.NET Core Web API'sini yapılandırma** iletişim kutusunda varsayılan değerleri kabul **hedef Framework'ü** , \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="0cadb-174">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="0cadb-175">Girin *TodoApi* için **proje adı** seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-175">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="0cadb-177">API'yi test etme</span><span class="sxs-lookup"><span data-stu-id="0cadb-177">Test the API</span></span>

<span data-ttu-id="0cadb-178">Proje şablonu oluşturur bir `values` API.</span><span class="sxs-lookup"><span data-stu-id="0cadb-178">The project template creates a `values` API.</span></span> <span data-ttu-id="0cadb-179">Çağrı `Get` uygulamayı test etmek için bir tarayıcıdan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0cadb-179">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0cadb-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0cadb-180">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0cadb-181">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="0cadb-181">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="0cadb-182">Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>/api/values`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-182">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="0cadb-183">IIS Express sertifika güven varsa soran bir iletişim kutusu alırsanız seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-183">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="0cadb-184">İçinde **Güvenlik Uyarısı** ardından, görüntülenen iletişim seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-184">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0cadb-185">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0cadb-185">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0cadb-186">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="0cadb-186">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="0cadb-187">Bir tarayıcıda aşağıdaki URL'ye gidin: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="0cadb-187">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0cadb-188">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0cadb-188">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0cadb-189">Seçin **çalıştırma** > **hata ayıklamayı Başlat** uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="0cadb-189">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="0cadb-190">Mac için Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-190">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="0cadb-191">HTTP 404 (bulunamadı) hatası döndürülür.</span><span class="sxs-lookup"><span data-stu-id="0cadb-191">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="0cadb-192">Append `/api/values` URL'sine (URL'yi `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="0cadb-192">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="0cadb-193">Aşağıdaki JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="0cadb-193">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="0cadb-194">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="0cadb-194">Add a model class</span></span>

<span data-ttu-id="0cadb-195">A *modeli* uygulamayı yöneten verilerini temsil eden sınıflar kümesidir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-195">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="0cadb-196">Tek bir modeldir bu uygulama için `TodoItem` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="0cadb-196">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0cadb-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0cadb-197">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0cadb-198">İçinde **Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0cadb-198">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="0cadb-199">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-199">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="0cadb-200">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="0cadb-200">Name the folder *Models*.</span></span>

* <span data-ttu-id="0cadb-201">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-201">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="0cadb-202">Sınıf adı *Todoıtem* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-202">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="0cadb-203">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="0cadb-203">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0cadb-204">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0cadb-204">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0cadb-205">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="0cadb-205">Add a folder named *Models*.</span></span>

* <span data-ttu-id="0cadb-206">Ekleme bir `TodoItem` sınıfının *modelleri* aşağıdaki kodla klasörü:</span><span class="sxs-lookup"><span data-stu-id="0cadb-206">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0cadb-207">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0cadb-207">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0cadb-208">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0cadb-208">Right-click the project.</span></span> <span data-ttu-id="0cadb-209">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-209">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="0cadb-210">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="0cadb-210">Name the folder *Models*.</span></span>

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="0cadb-212">Sağ *modelleri* klasörü ve select **Ekle** > **yeni dosya** > **genel**  >  **Boş sınıf**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-212">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="0cadb-213">Sınıf adı *Todoıtem*ve ardından **yeni**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-213">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="0cadb-214">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="0cadb-214">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="0cadb-215">`Id` Özelliği işlevlerinin bir ilişkisel veritabanında benzersiz anahtar.</span><span class="sxs-lookup"><span data-stu-id="0cadb-215">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="0cadb-216">Model sınıfları herhangi bir projede gidip ancak *modelleri* klasörü, kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-216">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="0cadb-217">Veritabanı bağlamı Ekle</span><span class="sxs-lookup"><span data-stu-id="0cadb-217">Add a database context</span></span>

<span data-ttu-id="0cadb-218">*Veritabanı bağlamı* koordine eden bir veri modeli için Entity Framework işlevsellik ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-218">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="0cadb-219">Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="0cadb-219">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0cadb-220">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0cadb-220">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0cadb-221">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-221">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="0cadb-222">Sınıf adı *TodoContext* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-222">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0cadb-223">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="0cadb-223">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="0cadb-224">Ekleme bir `TodoContext` sınıfının *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="0cadb-224">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="0cadb-225">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="0cadb-225">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="0cadb-226">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="0cadb-226">Register the database context</span></span>

<span data-ttu-id="0cadb-227">ASP.NET Core DB bağlamı gibi hizmetler ile kaydedilmelidir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="0cadb-227">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="0cadb-228">Kapsayıcı hizmeti denetleyicilerine sağlar.</span><span class="sxs-lookup"><span data-stu-id="0cadb-228">The container provides the service to controllers.</span></span>

<span data-ttu-id="0cadb-229">Güncelleştirme *Startup.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="0cadb-229">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="0cadb-230">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="0cadb-230">The preceding code:</span></span>

* <span data-ttu-id="0cadb-231">Kullanılmayan kaldırır `using` bildirimleri.</span><span class="sxs-lookup"><span data-stu-id="0cadb-231">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="0cadb-232">Veritabanı bağlamı DI kapsayıcıya ekler.</span><span class="sxs-lookup"><span data-stu-id="0cadb-232">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="0cadb-233">Veritabanı bağlamı bir bellek içi veritabanına kullanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-233">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="0cadb-234">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="0cadb-234">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0cadb-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0cadb-235">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0cadb-236">Sağ *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="0cadb-236">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="0cadb-237">Seçin **ekleme** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-237">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="0cadb-238">İçinde **Yeni Öğe Ekle** iletişim kutusunda **API denetleyici sınıfı** şablonu.</span><span class="sxs-lookup"><span data-stu-id="0cadb-238">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="0cadb-239">Sınıf adı *TodoController*seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-239">Name the class *TodoController*, and select **Add**.</span></span>

  ![Yeni öğe iletişim denetleyicisiyle seçilen arama kutusu ve web API denetleyicisi Ekle](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0cadb-241">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="0cadb-241">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="0cadb-242">İçinde *denetleyicileri* klasör adında bir sınıf oluşturma `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="0cadb-242">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="0cadb-243">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="0cadb-243">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="0cadb-244">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="0cadb-244">The preceding code:</span></span>

* <span data-ttu-id="0cadb-245">Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.</span><span class="sxs-lookup"><span data-stu-id="0cadb-245">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="0cadb-246">Sınıf ile düzenler [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0cadb-246">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="0cadb-247">Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-247">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="0cadb-248">Özniteliği sağlayan belirli davranışları hakkında daha fazla bilgi için bkz: <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="0cadb-248">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="0cadb-249">Veritabanı bağlamı eklemesine DI kullanır (`TodoContext`) içine denetleyici.</span><span class="sxs-lookup"><span data-stu-id="0cadb-249">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="0cadb-250">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="0cadb-250">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="0cadb-251">Adlı bir öğe ekler `Item1` veritabanı boşsa veritabanı.</span><span class="sxs-lookup"><span data-stu-id="0cadb-251">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="0cadb-252">Her çalıştığında bu kod oluşturucusunun içinde yeni bir HTTP isteği olduğundan.</span><span class="sxs-lookup"><span data-stu-id="0cadb-252">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="0cadb-253">Tüm öğeleri silerseniz, oluşturucu oluşturur `Item1` API yöntemi çağrıldığında tekrar başlattığınızda.</span><span class="sxs-lookup"><span data-stu-id="0cadb-253">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="0cadb-254">Bu nedenle, gerçekten işe yaradı silme işlemi işe yaramadı gibi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-254">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="0cadb-255">Get yöntemleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="0cadb-255">Add Get methods</span></span>

<span data-ttu-id="0cadb-256">Yapılacak iş öğeleri alır bir API sağlamak için aşağıdaki yöntemi ekleyin. `TodoController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="0cadb-256">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="0cadb-257">İki GET uç noktası bu yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="0cadb-257">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="0cadb-258">Uygulamayı hala çalışıyorsa durdurun.</span><span class="sxs-lookup"><span data-stu-id="0cadb-258">Stop the app if it's still running.</span></span> <span data-ttu-id="0cadb-259">Daha sonra yeniden en son değişiklikleri içerecek şekilde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0cadb-259">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="0cadb-260">Bir tarayıcıdan iki uç nokta çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-260">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="0cadb-261">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0cadb-261">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="0cadb-262">Şu HTTP yanıtı çağrısı tarafından üretilen `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="0cadb-262">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="0cadb-263">URL Yönlendirme ve yolları</span><span class="sxs-lookup"><span data-stu-id="0cadb-263">Routing and URL paths</span></span>

<span data-ttu-id="0cadb-264">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Özniteliği bir HTTP GET isteğine yanıt vermeden bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-264">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="0cadb-265">Her yöntem için URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="0cadb-265">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="0cadb-266">Denetleyicinin şablonu dizesi ile başlayıp `Route` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="0cadb-266">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="0cadb-267">Değiştirin `[controller]` denetleyicinin adı ile kural tarafından olduğu "Controller" soneki eksi denetleyici sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="0cadb-267">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="0cadb-268">Bu örnek, denetleyici sınıfı adı olan **Todo**Denetleyici adı "todo" Bu nedenle denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="0cadb-268">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="0cadb-269">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-269">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="0cadb-270">Varsa `[HttpGet]` özniteliğine sahip bir rota şablonu (örneğin, `[HttpGet("products")]`), yolunu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-270">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="0cadb-271">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="0cadb-271">This sample doesn't use a template.</span></span> <span data-ttu-id="0cadb-272">Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="0cadb-272">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="0cadb-273">Aşağıdaki `GetTodoItem` yöntemi `"{id}"` yapılacak iş öğesi benzersiz tanımlayıcısı için bir yer tutucu değişkendir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-273">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="0cadb-274">Zaman `GetTodoItem` çağrılır, değerini `"{id}"` yöntemine URL'de sağlanan kendi`id` parametresi.</span><span class="sxs-lookup"><span data-stu-id="0cadb-274">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="0cadb-275">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="0cadb-275">Return values</span></span>

<span data-ttu-id="0cadb-276">Dönüş türünü `GetTodoItems` ve `GetTodoItem` yöntemler [actionresult öğesini\<T > türü](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="0cadb-276">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="0cadb-277">ASP.NET Core, nesneyi otomatik olarak serileştiren [JSON](https://www.json.org/) ve yanıt iletisinin gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="0cadb-277">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="0cadb-278">Yanıt kodu 200 bu dönüş türü için olduğu varsayılırsa işlenmeyen özel durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-278">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="0cadb-279">İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-279">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="0cadb-280">`ActionResult` dönüş türleri, geniş HTTP durum kodları temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-280">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="0cadb-281">Örneğin, `GetTodoItem` iki farklı durum değerleri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="0cadb-281">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="0cadb-282">Öğe istenen kimliği eşleşirse, yöntem bir 404 döndürür [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu.</span><span class="sxs-lookup"><span data-stu-id="0cadb-282">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="0cadb-283">Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="0cadb-283">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="0cadb-284">Döndüren `item` sonuçları bir HTTP 200 yanıtı.</span><span class="sxs-lookup"><span data-stu-id="0cadb-284">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="0cadb-285">Test GetTodoItems yöntemi</span><span class="sxs-lookup"><span data-stu-id="0cadb-285">Test the GetTodoItems method</span></span>

<span data-ttu-id="0cadb-286">Bu öğreticide Postman web API'si test etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-286">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="0cadb-287">Yükleme [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="0cadb-287">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="0cadb-288">Web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="0cadb-288">Start the web app.</span></span>
* <span data-ttu-id="0cadb-289">Postman'i başlatın.</span><span class="sxs-lookup"><span data-stu-id="0cadb-289">Start Postman.</span></span>
* <span data-ttu-id="0cadb-290">Devre dışı **SSL sertifika doğrulama**</span><span class="sxs-lookup"><span data-stu-id="0cadb-290">Disable **SSL certificate verification**</span></span>
  
  * <span data-ttu-id="0cadb-291">Gelen **Dosya > Ayarlar** (\**genel* sekmesinde), devre dışı **SSL sertifika doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-291">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="0cadb-292">Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-292">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="0cadb-293">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0cadb-293">Create a new request.</span></span>
  * <span data-ttu-id="0cadb-294">HTTP yöntemi kümesine **alma**.</span><span class="sxs-lookup"><span data-stu-id="0cadb-294">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="0cadb-295">İstek URL'si kümesine `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="0cadb-295">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="0cadb-296">Örneğin: `https://localhost:5001/api/todo`</span><span class="sxs-lookup"><span data-stu-id="0cadb-296">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="0cadb-297">Ayarlama **iki bölme görünümü** postman'deki.</span><span class="sxs-lookup"><span data-stu-id="0cadb-297">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="0cadb-298">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-298">Select **Send**.</span></span>

![Get isteğiyle postman](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="0cadb-300">Create yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="0cadb-300">Add a Create method</span></span>

<span data-ttu-id="0cadb-301">Aşağıdaki `PostTodoItem` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="0cadb-301">Add the following `PostTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="0cadb-302">Yukarıdaki kod tarafından belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0cadb-302">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="0cadb-303">Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-303">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="0cadb-304">`CreatedAtAction` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="0cadb-304">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="0cadb-305">Başarılı olursa, bir HTTP 201 durum kodunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="0cadb-305">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="0cadb-306">HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-306">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="0cadb-307">Ekler bir `Location` yanıt üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="0cadb-307">Adds a `Location` header to the response.</span></span> <span data-ttu-id="0cadb-308">`Location` Üst bilgisi, yeni oluşturulan yapılacak iş öğesi URI'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-308">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="0cadb-309">Daha fazla bilgi için [10.2.2 201 oluşturuldu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="0cadb-309">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="0cadb-310">Başvuruları `GetTodoItem` oluşturmak için eylem `Location` başlığının URI.</span><span class="sxs-lookup"><span data-stu-id="0cadb-310">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="0cadb-311">C# `nameof` Eylem adı, sabit kodlama önlemek için kullanılan anahtar sözcüğü `CreatedAtAction` çağırın.</span><span class="sxs-lookup"><span data-stu-id="0cadb-311">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="0cadb-312">Test PostTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="0cadb-312">Test the PostTodoItem method</span></span>

* <span data-ttu-id="0cadb-313">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0cadb-313">Build the project.</span></span>
* <span data-ttu-id="0cadb-314">Postman HTTP yöntemi kümesine `POST`.</span><span class="sxs-lookup"><span data-stu-id="0cadb-314">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="0cadb-315">Seçin **gövdesi** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="0cadb-315">Select the **Body** tab.</span></span>
* <span data-ttu-id="0cadb-316">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0cadb-316">Select the **raw** radio button.</span></span>
* <span data-ttu-id="0cadb-317">Tür kümesine **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="0cadb-317">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="0cadb-318">İstek gövdesinde bir yapılacak iş öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="0cadb-318">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="0cadb-319">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-319">Select **Send**.</span></span>

  ![Postman ile isteği oluştur](first-web-api/_static/create.png)

  <span data-ttu-id="0cadb-321">405 bir yönteme izin verilmiyor hata alırsanız, büyük olasılıkla projeye ekledikten sonra derleme değil sonucudur `PostTodoItem` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0cadb-321">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="0cadb-322">Konum üst bilgisi URI test</span><span class="sxs-lookup"><span data-stu-id="0cadb-322">Test the location header URI</span></span>

* <span data-ttu-id="0cadb-323">Seçin **üstbilgileri** sekmesinde **yanıt** bölmesi.</span><span class="sxs-lookup"><span data-stu-id="0cadb-323">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="0cadb-324">Kopyalama **konumu** üst bilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="0cadb-324">Copy the **Location** header value:</span></span>

  ![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmc2.png)

* <span data-ttu-id="0cadb-326">Yöntemini GET öğesine Ayarla.</span><span class="sxs-lookup"><span data-stu-id="0cadb-326">Set the method to GET.</span></span>
* <span data-ttu-id="0cadb-327">URİ'sini yapıştırın (örneğin, `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="0cadb-327">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="0cadb-328">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-328">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="0cadb-329">PutTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="0cadb-329">Add a PutTodoItem method</span></span>

<span data-ttu-id="0cadb-330">Aşağıdaki `PutTodoItem` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="0cadb-330">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="0cadb-331">`PutTodoItem` benzer `PostTodoItem`, HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-331">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="0cadb-332">Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="0cadb-332">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="0cadb-333">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca değişiklikler değil göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-333">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="0cadb-334">Kısmi güncelleştirmeleri desteklemek için kullanma [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="0cadb-334">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="0cadb-335">Hata arama alırsanız `PutTodoItem`, çağrı `GET` veritabanında bir öğe var. olmak.</span><span class="sxs-lookup"><span data-stu-id="0cadb-335">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="0cadb-336">Test PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="0cadb-336">Test the PutTodoItem method</span></span>

<span data-ttu-id="0cadb-337">Bu örnek, uygulama her başlatıldığında initialed bir bellek içi veritabanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-337">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="0cadb-338">Olmalıdır bir öğe veritabanında bir PUT çağrısı yapmadan önce.</span><span class="sxs-lookup"><span data-stu-id="0cadb-338">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="0cadb-339">Bir öğe var. veritabanında bir PUT çağrısı yapmadan önce sağlamak üzere GET çağırın.</span><span class="sxs-lookup"><span data-stu-id="0cadb-339">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="0cadb-340">Kimliğine sahip bir yapılacak iş öğesi güncelleştirme = 1 ve "balık akış için" adını girin:</span><span class="sxs-lookup"><span data-stu-id="0cadb-340">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="0cadb-341">Aşağıdaki görüntüde, Postman güncelleştirme gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0cadb-341">The following image shows the Postman update:</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="0cadb-343">DeleteTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="0cadb-343">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="0cadb-344">Aşağıdaki `DeleteTodoItem` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="0cadb-344">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="0cadb-345">`DeleteTodoItem` Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="0cadb-345">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="0cadb-346">Test DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="0cadb-346">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="0cadb-347">Postman bir yapılacak iş öğesini silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="0cadb-347">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="0cadb-348">Yöntem kümesine `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="0cadb-348">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="0cadb-349">Silmek, örneğin nesnenin URI ayarlayın `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="0cadb-349">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="0cadb-350">Seçin **Gönder**</span><span class="sxs-lookup"><span data-stu-id="0cadb-350">Select **Send**</span></span>

<span data-ttu-id="0cadb-351">Örnek uygulamayı tüm öğeleri silmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="0cadb-351">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="0cadb-352">Bununla birlikte, son öğe silindiğinde, yeni bir API bir sonraki zamana model sınıfı Oluşturucu tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0cadb-352">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="0cadb-353">JQuery ile API çağırma</span><span class="sxs-lookup"><span data-stu-id="0cadb-353">Call the API with jQuery</span></span>

<span data-ttu-id="0cadb-354">Bu bölümde, Web'i çağırmaya jQuery kullanan bir HTML sayfasına eklenen API.</span><span class="sxs-lookup"><span data-stu-id="0cadb-354">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="0cadb-355">jQuery isteği başlatır ve API'nin yanıt Ayrıntıları sayfası güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-355">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="0cadb-356">İçin uygulamayı yapılandırma [statik dosyaları işleme](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [varsayılan dosya eşlemesini etkinleştir](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) güncelleştirerek *Startup.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="0cadb-356">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
<span data-ttu-id="0cadb-357">Oluşturma bir *wwwroot* proje dizininde klasör.</span><span class="sxs-lookup"><span data-stu-id="0cadb-357">Create a *wwwroot* folder in the project directory.</span></span>
::: moniker-end

<span data-ttu-id="0cadb-358">Adlı bir HTML dosyası ekleyin *index.html* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-358">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="0cadb-359">Dosyanın içeriğini aşağıdaki biçimlendirme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="0cadb-359">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="0cadb-360">Adlı bir JavaScript dosyası ekleyin *site.js* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-360">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="0cadb-361">Dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="0cadb-361">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="0cadb-362">ASP.NET Core proje başlatma ayarlarında bir değişiklik HTML sayfasını yerel olarak test etmek için gerekli:</span><span class="sxs-lookup"><span data-stu-id="0cadb-362">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="0cadb-363">Açık *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0cadb-363">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="0cadb-364">Kaldırma `launchUrl` , açmak için uygulamayı zorlamak için özellik *index.html*&mdash;projenin varsayılan dosya.</span><span class="sxs-lookup"><span data-stu-id="0cadb-364">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="0cadb-365">JQuery almak için birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-365">There are several ways to get jQuery.</span></span> <span data-ttu-id="0cadb-366">Yukarıdaki kod parçacığında, bir CDN kitaplığı yüklenir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-366">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="0cadb-367">Bu örnek, tüm API CRUD yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-367">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="0cadb-368">API çağrıları açıklamaları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-368">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="0cadb-369">Yapılacaklar öğelerinin bir listesini alın</span><span class="sxs-lookup"><span data-stu-id="0cadb-369">Get a list of to-do items</span></span>

<span data-ttu-id="0cadb-370">JQuery [ajax](https://api.jquery.com/jquery.ajax/) işlev gönderen bir `GET` Yapılacaklar öğelerini dizisini temsil eden JSON döndüren API isteği.</span><span class="sxs-lookup"><span data-stu-id="0cadb-370">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="0cadb-371">`success` İstek başarılı olursa geri çağırma işlevi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-371">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="0cadb-372">Geri çağırma içinde DOM Yapılacaklar bilgilerle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-372">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="0cadb-373">Yapılacak İş Öğesi Ekle</span><span class="sxs-lookup"><span data-stu-id="0cadb-373">Add a to-do item</span></span>

<span data-ttu-id="0cadb-374">[Ajax](https://api.jquery.com/jquery.ajax/) işlev gönderen bir `POST` istek ile istek gövdesindeki yapılacak iş öğesi.</span><span class="sxs-lookup"><span data-stu-id="0cadb-374">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="0cadb-375">`accepts` Ve `contentType` seçeneklerini ayarlamak `application/json` gönderilen ve alınan medya türü belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="0cadb-375">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="0cadb-376">Yapılacak iş öğesi kullanarak JSON'a dönüştürülür [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="0cadb-376">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="0cadb-377">API'yi bir başarılı durum kodu döndürdüğünde `getData` işlevi HTML tablosu güncelleştirmek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="0cadb-377">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="0cadb-378">Yapılacak iş öğesini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="0cadb-378">Update a to-do item</span></span>

<span data-ttu-id="0cadb-379">Yapılacak iş öğesi güncelleştirilirken bir eklemeye benzerdir.</span><span class="sxs-lookup"><span data-stu-id="0cadb-379">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="0cadb-380">`url` Öğenin benzersiz tanıtıcısı eklemek için değişiklikleri ve `type` olduğu `PUT`.</span><span class="sxs-lookup"><span data-stu-id="0cadb-380">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="0cadb-381">Yapılacak iş öğesi silme</span><span class="sxs-lookup"><span data-stu-id="0cadb-381">Delete a to-do item</span></span>

<span data-ttu-id="0cadb-382">Yapılacak iş öğesi silme gerçekleştirilir ayarlayarak `type` AJAX çağrısı hedefi üzerinde `DELETE` ve öğenin benzersiz tanıtıcısı URL'yi belirterek.</span><span class="sxs-lookup"><span data-stu-id="0cadb-382">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a><span data-ttu-id="0cadb-383">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0cadb-383">Additional resources</span></span>

<span data-ttu-id="0cadb-384">[Görüntülemek veya Bu öğretici için örnek kodu indirdikten](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="0cadb-384">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="0cadb-385">Bkz: [nasıl indirileceğini](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="0cadb-385">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="0cadb-386">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="0cadb-386">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="0cadb-387">Bu öğreticide YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="0cadb-387">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)

## <a name="next-steps"></a><span data-ttu-id="0cadb-388">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="0cadb-388">Next steps</span></span>

<span data-ttu-id="0cadb-389">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="0cadb-389">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0cadb-390">Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0cadb-390">Create a web api project.</span></span>
> * <span data-ttu-id="0cadb-391">Bir model sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-391">Add a model class.</span></span>
> * <span data-ttu-id="0cadb-392">Veritabanı bağlamı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0cadb-392">Create the database context.</span></span>
> * <span data-ttu-id="0cadb-393">Veritabanı bağlamı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-393">Register the database context.</span></span>
> * <span data-ttu-id="0cadb-394">Bir denetleyici ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="0cadb-394">Add a controller.</span></span>
> * <span data-ttu-id="0cadb-395">CRUD yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-395">Add CRUD methods.</span></span>
> * <span data-ttu-id="0cadb-396">Yönlendirmeyi Yapılandırma ve URL yolu.</span><span class="sxs-lookup"><span data-stu-id="0cadb-396">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="0cadb-397">Dönüş değerleri belirtin.</span><span class="sxs-lookup"><span data-stu-id="0cadb-397">Specify return values.</span></span>
> * <span data-ttu-id="0cadb-398">Web API'si Postman ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="0cadb-398">Call the web API with Postman.</span></span>
> * <span data-ttu-id="0cadb-399">Web arama API'si jQuery ile.</span><span class="sxs-lookup"><span data-stu-id="0cadb-399">Call the web api with jQuery.</span></span>

<span data-ttu-id="0cadb-400">API Yardım sayfaları oluşturma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="0cadb-400">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
