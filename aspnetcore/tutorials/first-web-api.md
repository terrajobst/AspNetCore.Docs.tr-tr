---
title: "Öğretici: ASP.NET Core ile Web API 'SI oluşturma"
author: rick-anderson
description: ASP.NET Core ile Web API 'SI oluşturmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 08/05/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 855d05fa2b9c1a7572212c40adbe61bb396f4bac
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819842"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="cd878-103">Öğretici: ASP.NET Core ile Web API 'SI oluşturma</span><span class="sxs-lookup"><span data-stu-id="cd878-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="cd878-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="cd878-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="cd878-105">Bu öğretici, bir web API ASP.NET Core ile oluşturmaya ilişkin temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="cd878-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cd878-106">Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="cd878-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cd878-107">Bir Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cd878-107">Create a web API project.</span></span>
> * <span data-ttu-id="cd878-108">Bir model sınıfı ve bir veritabanı bağlamı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cd878-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="cd878-109">CRUD yöntemleriyle bir denetleyiciyi dolandırın.</span><span class="sxs-lookup"><span data-stu-id="cd878-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="cd878-110">Yönlendirmeyi, URL yollarını ve dönüş değerlerini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="cd878-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="cd878-111">Web API'si Postman ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="cd878-111">Call the web API with Postman.</span></span>

<span data-ttu-id="cd878-112">Sonunda, bir veritabanında depolanan "yapılacaklar" öğelerini yönetebilmek için bir Web API 'SI vardır.</span><span class="sxs-lookup"><span data-stu-id="cd878-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="cd878-113">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="cd878-113">Overview</span></span>

<span data-ttu-id="cd878-114">Bu öğretici yandaki API oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cd878-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="cd878-115">API</span><span class="sxs-lookup"><span data-stu-id="cd878-115">API</span></span> | <span data-ttu-id="cd878-116">Açıklama</span><span class="sxs-lookup"><span data-stu-id="cd878-116">Description</span></span> | <span data-ttu-id="cd878-117">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="cd878-117">Request body</span></span> | <span data-ttu-id="cd878-118">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="cd878-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="cd878-119">/Api/TodoItems al</span><span class="sxs-lookup"><span data-stu-id="cd878-119">GET /api/TodoItems</span></span> | <span data-ttu-id="cd878-120">Tüm yapılacak iş öğeleri al</span><span class="sxs-lookup"><span data-stu-id="cd878-120">Get all to-do items</span></span> | <span data-ttu-id="cd878-121">Yok.</span><span class="sxs-lookup"><span data-stu-id="cd878-121">None</span></span> | <span data-ttu-id="cd878-122">Yapılacaklar öğelerinin bir dizisi</span><span class="sxs-lookup"><span data-stu-id="cd878-122">Array of to-do items</span></span>|
|<span data-ttu-id="cd878-123">/Api/TodoItems/{id} al</span><span class="sxs-lookup"><span data-stu-id="cd878-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="cd878-124">Bir öğeyi Kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="cd878-124">Get an item by ID</span></span> | <span data-ttu-id="cd878-125">Yok.</span><span class="sxs-lookup"><span data-stu-id="cd878-125">None</span></span> | <span data-ttu-id="cd878-126">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="cd878-126">To-do item</span></span>|
|<span data-ttu-id="cd878-127">POST/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="cd878-127">POST /api/TodoItems</span></span> | <span data-ttu-id="cd878-128">Yeni Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="cd878-128">Add a new item</span></span> | <span data-ttu-id="cd878-129">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="cd878-129">To-do item</span></span> | <span data-ttu-id="cd878-130">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="cd878-130">To-do item</span></span> |
|<span data-ttu-id="cd878-131">/Api/TodoItems/{id} koy</span><span class="sxs-lookup"><span data-stu-id="cd878-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="cd878-132">Mevcut öğeyi güncelleştirin &nbsp;</span><span class="sxs-lookup"><span data-stu-id="cd878-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="cd878-133">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="cd878-133">To-do item</span></span> | <span data-ttu-id="cd878-134">Yok.</span><span class="sxs-lookup"><span data-stu-id="cd878-134">None</span></span> |
|<span data-ttu-id="cd878-135">/Api/TodoItems/{id} &nbsp; Sil&nbsp;</span><span class="sxs-lookup"><span data-stu-id="cd878-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="cd878-136">Öğeyi Sil &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="cd878-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="cd878-137">Hiçbiri</span><span class="sxs-lookup"><span data-stu-id="cd878-137">None</span></span> | <span data-ttu-id="cd878-138">Hiçbiri</span><span class="sxs-lookup"><span data-stu-id="cd878-138">None</span></span>|

<span data-ttu-id="cd878-139">Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="cd878-139">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki bir kutu ile temsil edilir.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="cd878-145">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="cd878-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd878-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cd878-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cd878-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cd878-148">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="cd878-149">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="cd878-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd878-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cd878-151">**Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="cd878-152">**ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="cd878-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="cd878-153">Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="cd878-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="cd878-154">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,0** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="cd878-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="cd878-155">**API** şablonunu seçin ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="cd878-155">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="cd878-156">**Docker desteğini etkinleştir**' i seçmeyin.</span><span class="sxs-lookup"><span data-stu-id="cd878-156">**Don't** select **Enable Docker Support**.</span></span>

![VS yeni proje iletişim kutusu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cd878-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cd878-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="cd878-159">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="cd878-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="cd878-160">Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="cd878-160">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="cd878-161">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cd878-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   code -r ../TodoApi
   ```

* <span data-ttu-id="cd878-162">Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-162">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="cd878-163">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="cd878-163">The preceding commands:</span></span>

  * <span data-ttu-id="cd878-164">Yeni bir Web API projesi oluşturur ve Visual Studio Code açar.</span><span class="sxs-lookup"><span data-stu-id="cd878-164">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="cd878-165">Sonraki bölümde gerekli olan NuGet paketlerini ekler.</span><span class="sxs-lookup"><span data-stu-id="cd878-165">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cd878-166">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="cd878-167">**Dosya** > **yeni çözüm**' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-167">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="cd878-169">**.NET Core** > **uygulama** **API 'si** ileri ' **yi**seçin. > ></span><span class="sxs-lookup"><span data-stu-id="cd878-169">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="cd878-171">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **hedef Framework** \* *.NET Core 3,0*' i seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-171">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="cd878-172">Girin *TodoApi* için **proje adı** seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="cd878-172">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="cd878-174">API'yi test etme</span><span class="sxs-lookup"><span data-stu-id="cd878-174">Test the API</span></span>

<span data-ttu-id="cd878-175">Proje şablonu oluşturur bir `WeatherForecast` API.</span><span class="sxs-lookup"><span data-stu-id="cd878-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="cd878-176">Çağrı `Get` uygulamayı test etmek için bir tarayıcıdan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cd878-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd878-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cd878-178">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="cd878-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="cd878-179">Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>/WeatherForecast`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="cd878-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="cd878-180">IIS Express sertifika güven varsa soran bir iletişim kutusu alırsanız seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="cd878-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="cd878-181">İçinde **Güvenlik Uyarısı** ardından, görüntülenen iletişim seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="cd878-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cd878-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cd878-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="cd878-183">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="cd878-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="cd878-184">Bir tarayıcıda aşağıdaki URL'ye gidin: [ https://localhost:5001/WeatherForecast ](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="cd878-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cd878-185">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="cd878-186">Uygulamayı başlatmak için**hata ayıklamayı Başlat** ' ı seçin. > </span><span class="sxs-lookup"><span data-stu-id="cd878-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="cd878-187">Mac için Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="cd878-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="cd878-188">HTTP 404 (bulunamadı) hatası döndürülür.</span><span class="sxs-lookup"><span data-stu-id="cd878-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="cd878-189">Append `/WeatherForecast` URL'sine (URL'yi `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="cd878-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="cd878-190">Aşağıdakine benzer bir JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="cd878-190">JSON similar to the following is returned:</span></span>

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a><span data-ttu-id="cd878-191">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="cd878-191">Add a model class</span></span>

<span data-ttu-id="cd878-192">A *modeli* uygulamayı yöneten verilerini temsil eden sınıflar kümesidir.</span><span class="sxs-lookup"><span data-stu-id="cd878-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="cd878-193">Tek bir modeldir bu uygulama için `TodoItem` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="cd878-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd878-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cd878-195">İçinde **Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="cd878-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="cd878-196">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="cd878-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="cd878-197">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="cd878-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="cd878-198">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="cd878-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="cd878-199">Sınıf adı *Todoıtem* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="cd878-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="cd878-200">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="cd878-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cd878-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cd878-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="cd878-202">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="cd878-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="cd878-203">Ekleme bir `TodoItem` sınıfının *modelleri* aşağıdaki kodla klasörü:</span><span class="sxs-lookup"><span data-stu-id="cd878-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cd878-204">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="cd878-205">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="cd878-205">Right-click the project.</span></span> <span data-ttu-id="cd878-206">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="cd878-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="cd878-207">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="cd878-207">Name the folder *Models*.</span></span>

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="cd878-209">*Modeller* klasörüne sağ tıklayın ve **yeni dosya** > **Ekle** > **genel** > **boş sınıfı**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="cd878-210">Sınıf adı *Todoıtem*ve ardından **yeni**.</span><span class="sxs-lookup"><span data-stu-id="cd878-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="cd878-211">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="cd878-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="cd878-212">`Id` Özelliği işlevlerinin bir ilişkisel veritabanında benzersiz anahtar.</span><span class="sxs-lookup"><span data-stu-id="cd878-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="cd878-213">Model sınıfları herhangi bir projede gidip ancak *modelleri* klasörü, kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cd878-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="cd878-214">Veritabanı bağlamı Ekle</span><span class="sxs-lookup"><span data-stu-id="cd878-214">Add a database context</span></span>

<span data-ttu-id="cd878-215">*Veritabanı bağlamı* koordine eden bir veri modeli için Entity Framework işlevsellik ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="cd878-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="cd878-216">Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="cd878-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd878-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="cd878-218">Microsoft. EntityFrameworkCore. SqlServer ekleyin</span><span class="sxs-lookup"><span data-stu-id="cd878-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="cd878-219">**Araçlar** menüsünde **nuget Paket Yöneticisi > çözüm Için NuGet Paketlerini Yönet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="cd878-220">**Ön sürümü dahil et** onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-220">Select the **Include prerelease** checkbox.</span></span>
* <span data-ttu-id="cd878-221">**Araştır** sekmesini seçin ve arama kutusuna **Microsoft. Entityframeworkcore. SqlServer** yazın.</span><span class="sxs-lookup"><span data-stu-id="cd878-221">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="cd878-222">Sol bölmedeki **Microsoft. EntityFrameworkCore. SqlServer v 3.0.0-Preview** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-222">Select  **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** in the left pane.</span></span>
* <span data-ttu-id="cd878-223">Sağ bölmedeki **Proje** onay kutusunu seçin ve ardından **Install**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-223">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="cd878-224">Önceki yönergeleri kullanarak `Microsoft.EntityFrameworkCore.InMemory` NuGet paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cd878-224">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet Paket Yöneticisi](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="cd878-226">TodoContext veritabanı bağlamını ekleme</span><span class="sxs-lookup"><span data-stu-id="cd878-226">Add the TodoContext database context</span></span>

* <span data-ttu-id="cd878-227">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="cd878-227">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="cd878-228">Sınıf adı *TodoContext* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="cd878-228">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="cd878-229">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-229">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="cd878-230">Ekleme bir `TodoContext` sınıfının *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="cd878-230">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="cd878-231">Aşağıdaki kodu girin:</span><span class="sxs-lookup"><span data-stu-id="cd878-231">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="cd878-232">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="cd878-232">Register the database context</span></span>

<span data-ttu-id="cd878-233">ASP.NET Core DB bağlamı gibi hizmetler ile kaydedilmelidir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="cd878-233">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="cd878-234">Kapsayıcı hizmeti denetleyicilerine sağlar.</span><span class="sxs-lookup"><span data-stu-id="cd878-234">The container provides the service to controllers.</span></span>

<span data-ttu-id="cd878-235">Güncelleştirme *Startup.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="cd878-235">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="cd878-236">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="cd878-236">The preceding code:</span></span>

* <span data-ttu-id="cd878-237">Kullanılmayan kaldırır `using` bildirimleri.</span><span class="sxs-lookup"><span data-stu-id="cd878-237">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="cd878-238">Veritabanı bağlamı DI kapsayıcıya ekler.</span><span class="sxs-lookup"><span data-stu-id="cd878-238">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="cd878-239">Veritabanı bağlamı bir bellek içi veritabanına kullanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="cd878-239">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="cd878-240">Denetleyiciyi bir denetleyiciye katlama</span><span class="sxs-lookup"><span data-stu-id="cd878-240">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd878-241">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-241">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cd878-242">Sağ *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="cd878-242">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="cd878-243">**Yeni yapı iskelesi öğesi** **Ekle** > öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-243">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="cd878-244">**Entity Framework kullanarak ve eylemler Içeren API denetleyicisi**' ni seçin ve ardından **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-244">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="cd878-245">**API denetleyiciyi eylemler Ile Ekle ' de Entity Framework** iletişim kutusunu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="cd878-245">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="cd878-246">**Model sınıfında** **TodoItem (TodoAPI. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-246">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="cd878-247">**Veri bağlamı sınıfında** **TodoContext (TodoAPI. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-247">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="cd878-248">**Ekle** 'yi seçin</span><span class="sxs-lookup"><span data-stu-id="cd878-248">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="cd878-249">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-249">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="cd878-250">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cd878-250">Run the following commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="cd878-251">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="cd878-251">The preceding commands:</span></span>

* <span data-ttu-id="cd878-252">Yapı iskelesi için gereken NuGet paketlerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cd878-252">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="cd878-253">Scafkatlama altyapısını (`dotnet-aspnet-codegenerator`) kurar.</span><span class="sxs-lookup"><span data-stu-id="cd878-253">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="cd878-254">Yapı iskelesi `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="cd878-254">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="cd878-255">Oluşturulan kod:</span><span class="sxs-lookup"><span data-stu-id="cd878-255">The generated code:</span></span>

* <span data-ttu-id="cd878-256">Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.</span><span class="sxs-lookup"><span data-stu-id="cd878-256">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="cd878-257">Sınıfı [[Apicontroller]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle süsler.</span><span class="sxs-lookup"><span data-stu-id="cd878-257">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="cd878-258">Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="cd878-258">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="cd878-259">Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="cd878-259">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="cd878-260">Veritabanı bağlamı eklemesine DI kullanır (`TodoContext`) içine denetleyici.</span><span class="sxs-lookup"><span data-stu-id="cd878-260">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="cd878-261">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="cd878-261">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="cd878-262">PostTodoItem Create metodunu inceleyin</span><span class="sxs-lookup"><span data-stu-id="cd878-262">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="cd878-263">İçindeki `PostTodoItem` return ifadesini, [NameOf](/dotnet/csharp/language-reference/operators/nameof) işlecini kullanmak için değiştirin:</span><span class="sxs-lookup"><span data-stu-id="cd878-263">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="cd878-264">Yukarıdaki kod tarafından belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="cd878-264">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="cd878-265">Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="cd878-265">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="cd878-266"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="cd878-266">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="cd878-267">Başarılı olursa bir HTTP 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="cd878-267">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="cd878-268">HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="cd878-268">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="cd878-269">Yanıta bir [konum](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) üst bilgisi ekler.</span><span class="sxs-lookup"><span data-stu-id="cd878-269">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="cd878-270">Üst bilgi, yeni oluşturulan Yapılacaklar öğesinin URI 'sini belirtir. [](https://developer.mozilla.org/docs/Glossary/URI) `Location`</span><span class="sxs-lookup"><span data-stu-id="cd878-270">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="cd878-271">Daha fazla bilgi için [10.2.2 201 oluşturuldu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="cd878-271">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="cd878-272">`Location` Üstbilginin URI 'sini oluşturma eylemine başvurur. `GetTodoItem`</span><span class="sxs-lookup"><span data-stu-id="cd878-272">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="cd878-273">Anahtar sözcüğü, `CreatedAtAction` çağrıda eylem adının sabit kodlanmasını önlemek için kullanılır. C# `nameof`</span><span class="sxs-lookup"><span data-stu-id="cd878-273">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="cd878-274">Postman yükleme</span><span class="sxs-lookup"><span data-stu-id="cd878-274">Install Postman</span></span>

<span data-ttu-id="cd878-275">Bu öğreticide Postman web API'si test etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cd878-275">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="cd878-276">Yükleme [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="cd878-276">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="cd878-277">Web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="cd878-277">Start the web app.</span></span>
* <span data-ttu-id="cd878-278">Postman'i başlatın.</span><span class="sxs-lookup"><span data-stu-id="cd878-278">Start Postman.</span></span>
* <span data-ttu-id="cd878-279">Devre dışı **SSL sertifika doğrulama**</span><span class="sxs-lookup"><span data-stu-id="cd878-279">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="cd878-280">Gelen **Dosya > Ayarlar** (\**genel* sekmesinde), devre dışı **SSL sertifika doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="cd878-280">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="cd878-281">Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="cd878-281">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="cd878-282">Postman ile test PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="cd878-282">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="cd878-283">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cd878-283">Create a new request.</span></span>
* <span data-ttu-id="cd878-284">HTTP yöntemini olarak `POST`ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="cd878-284">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="cd878-285">Seçin **gövdesi** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="cd878-285">Select the **Body** tab.</span></span>
* <span data-ttu-id="cd878-286">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="cd878-286">Select the **raw** radio button.</span></span>
* <span data-ttu-id="cd878-287">Tür kümesine **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="cd878-287">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="cd878-288">İstek gövdesinde bir yapılacak iş öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="cd878-288">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="cd878-289">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-289">Select **Send**.</span></span>

  ![Postman ile isteği oluştur](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="cd878-291">Konum üst bilgisi URI test</span><span class="sxs-lookup"><span data-stu-id="cd878-291">Test the location header URI</span></span>

* <span data-ttu-id="cd878-292">Seçin **üstbilgileri** sekmesinde **yanıt** bölmesi.</span><span class="sxs-lookup"><span data-stu-id="cd878-292">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="cd878-293">Kopyalama **konumu** üst bilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="cd878-293">Copy the **Location** header value:</span></span>

  ![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/3/create.png)

* <span data-ttu-id="cd878-295">Yöntemini GET öğesine Ayarla.</span><span class="sxs-lookup"><span data-stu-id="cd878-295">Set the method to GET.</span></span>
* <span data-ttu-id="cd878-296">URİ'sini yapıştırın (örneğin, `https://localhost:5001/api/TodoItems/1`)</span><span class="sxs-lookup"><span data-stu-id="cd878-296">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="cd878-297">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-297">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="cd878-298">GET yöntemlerini inceleyin</span><span class="sxs-lookup"><span data-stu-id="cd878-298">Examine the GET methods</span></span>

<span data-ttu-id="cd878-299">İki GET uç noktası bu yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="cd878-299">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="cd878-300">Tarayıcıdan veya Postman 'dan iki uç noktayı çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="cd878-300">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="cd878-301">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cd878-301">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="cd878-302">Şuna benzer bir yanıt, şu çağrı `GetTodoItems`tarafından üretilir:</span><span class="sxs-lookup"><span data-stu-id="cd878-302">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="cd878-303">Postman ile test al</span><span class="sxs-lookup"><span data-stu-id="cd878-303">Test Get with Postman</span></span>

* <span data-ttu-id="cd878-304">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cd878-304">Create a new request.</span></span>
* <span data-ttu-id="cd878-305">HTTP yöntemi kümesine **alma**.</span><span class="sxs-lookup"><span data-stu-id="cd878-305">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="cd878-306">İstek URL'si kümesine `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="cd878-306">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="cd878-307">Örneğin: `https://localhost:5001/api/TodoItems`</span><span class="sxs-lookup"><span data-stu-id="cd878-307">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="cd878-308">Ayarlama **iki bölme görünümü** postman'deki.</span><span class="sxs-lookup"><span data-stu-id="cd878-308">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="cd878-309">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-309">Select **Send**.</span></span>

<span data-ttu-id="cd878-310">Bu uygulama, bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="cd878-310">This app uses an in-memory database.</span></span> <span data-ttu-id="cd878-311">Uygulama durdurulup başlatılırsa, önceki GET isteği herhangi bir veri döndürmez.</span><span class="sxs-lookup"><span data-stu-id="cd878-311">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="cd878-312">Hiçbir veri döndürülmezse, verileri uygulamaya [gönderin](#post) .</span><span class="sxs-lookup"><span data-stu-id="cd878-312">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="cd878-313">URL Yönlendirme ve yolları</span><span class="sxs-lookup"><span data-stu-id="cd878-313">Routing and URL paths</span></span>

<span data-ttu-id="cd878-314">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Özniteliği bir HTTP GET isteğine yanıt vermeden bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="cd878-314">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="cd878-315">Her yöntem için URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="cd878-315">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="cd878-316">Denetleyicinin şablonu dizesi ile başlayıp `Route` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="cd878-316">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="cd878-317">Değiştirin `[controller]` denetleyicinin adı ile kural tarafından olduğu "Controller" soneki eksi denetleyici sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="cd878-317">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="cd878-318">Bu örnek için denetleyici sınıfı adı **todoıtems**denetleyicisidir, bu nedenle denetleyicinin adı "todoıtems" olur.</span><span class="sxs-lookup"><span data-stu-id="cd878-318">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="cd878-319">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="cd878-319">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="cd878-320">Özniteliğin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin. `[HttpGet]`</span><span class="sxs-lookup"><span data-stu-id="cd878-320">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="cd878-321">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="cd878-321">This sample doesn't use a template.</span></span> <span data-ttu-id="cd878-322">Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="cd878-322">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="cd878-323">Aşağıdaki `GetTodoItem` yöntemi `"{id}"` yapılacak iş öğesi benzersiz tanımlayıcısı için bir yer tutucu değişkendir.</span><span class="sxs-lookup"><span data-stu-id="cd878-323">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="cd878-324">Zaman `GetTodoItem` çağrılır, değerini `"{id}"` yöntemine URL'de sağlanan kendi`id` parametresi.</span><span class="sxs-lookup"><span data-stu-id="cd878-324">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="cd878-325">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="cd878-325">Return values</span></span>

<span data-ttu-id="cd878-326">Dönüş türünü `GetTodoItems` ve `GetTodoItem` yöntemler [actionresult öğesini\<T > türü](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="cd878-326">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="cd878-327">ASP.NET Core, nesneyi otomatik olarak serileştiren [JSON](https://www.json.org/) ve yanıt iletisinin gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="cd878-327">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="cd878-328">Yanıt kodu 200 bu dönüş türü için olduğu varsayılırsa işlenmeyen özel durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="cd878-328">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="cd878-329">İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.</span><span class="sxs-lookup"><span data-stu-id="cd878-329">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="cd878-330">`ActionResult` dönüş türleri, geniş HTTP durum kodları temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="cd878-330">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="cd878-331">Örneğin, `GetTodoItem` iki farklı durum değerleri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="cd878-331">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="cd878-332">Öğe istenen kimliği eşleşirse, yöntem bir 404 döndürür [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu.</span><span class="sxs-lookup"><span data-stu-id="cd878-332">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="cd878-333">Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="cd878-333">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="cd878-334">Döndüren `item` sonuçları bir HTTP 200 yanıtı.</span><span class="sxs-lookup"><span data-stu-id="cd878-334">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="cd878-335">PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="cd878-335">The PutTodoItem method</span></span>

<span data-ttu-id="cd878-336">`PutTodoItem` Yöntemi inceleyin:</span><span class="sxs-lookup"><span data-stu-id="cd878-336">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="cd878-337">`PutTodoItem` benzer `PostTodoItem`, HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="cd878-337">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="cd878-338">Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="cd878-338">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="cd878-339">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca değişiklikler değil göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="cd878-339">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="cd878-340">Kısmi güncelleştirmeleri desteklemek için kullanma [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="cd878-340">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="cd878-341">Çağrılırken `PutTodoItem`bir hata alırsanız, veritabanında bir öğe `GET` olduğundan emin olmak için çağırın.</span><span class="sxs-lookup"><span data-stu-id="cd878-341">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="cd878-342">Test PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="cd878-342">Test the PutTodoItem method</span></span>

<span data-ttu-id="cd878-343">Bu örnek, uygulama her başlatıldığında yeniden başlatılması gereken bellek içi veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="cd878-343">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="cd878-344">Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="cd878-344">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="cd878-345">PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="cd878-345">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="cd878-346">ID = 1 olan Yapılacaklar öğesini güncelleştirin ve adını "Feed balık" olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="cd878-346">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="cd878-347">Aşağıdaki görüntüde, Postman güncelleştirme gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="cd878-347">The following image shows the Postman update:</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="cd878-349">DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="cd878-349">The DeleteTodoItem method</span></span>

<span data-ttu-id="cd878-350">`DeleteTodoItem` Yöntemi inceleyin:</span><span class="sxs-lookup"><span data-stu-id="cd878-350">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="cd878-351">`DeleteTodoItem` Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="cd878-351">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="cd878-352">Test DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="cd878-352">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="cd878-353">Postman bir yapılacak iş öğesini silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="cd878-353">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="cd878-354">Yöntem kümesine `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="cd878-354">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="cd878-355">Silmek, örneğin nesnenin URI ayarlayın `https://localhost:5001/api/TodoItems/1`</span><span class="sxs-lookup"><span data-stu-id="cd878-355">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="cd878-356">Seçin **Gönder**</span><span class="sxs-lookup"><span data-stu-id="cd878-356">Select **Send**</span></span>

## <a name="call-the-api-from-jquery"></a><span data-ttu-id="cd878-357">JQuery 'ten API çağırma</span><span class="sxs-lookup"><span data-stu-id="cd878-357">Call the API from jQuery</span></span>

<span data-ttu-id="cd878-358">Öğreticiye bakın [: JQuery](xref:tutorials/web-api-jquery)ile ASP.NET Core Web API 'si çağırma.</span><span class="sxs-lookup"><span data-stu-id="cd878-358">See [Tutorial: Call an ASP.NET Core web API with jQuery](xref:tutorials/web-api-jquery).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="cd878-359">Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="cd878-359">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cd878-360">Bir Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cd878-360">Create a web API project.</span></span>
> * <span data-ttu-id="cd878-361">Bir model sınıfı ve bir veritabanı bağlamı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cd878-361">Add a model class and a database context.</span></span>
> * <span data-ttu-id="cd878-362">Bir denetleyici ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="cd878-362">Add a controller.</span></span>
> * <span data-ttu-id="cd878-363">CRUD yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cd878-363">Add CRUD methods.</span></span>
> * <span data-ttu-id="cd878-364">Yönlendirmeyi Yapılandırma ve URL yolu.</span><span class="sxs-lookup"><span data-stu-id="cd878-364">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="cd878-365">Dönüş değerleri belirtin.</span><span class="sxs-lookup"><span data-stu-id="cd878-365">Specify return values.</span></span>
> * <span data-ttu-id="cd878-366">Web API'si Postman ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="cd878-366">Call the web API with Postman.</span></span>
> * <span data-ttu-id="cd878-367">JQuery ile Web API 'sini çağırın.</span><span class="sxs-lookup"><span data-stu-id="cd878-367">Call the web API with jQuery.</span></span>

<span data-ttu-id="cd878-368">Sonunda, web API'si "Yapılacaklar" öğelerini ilişkisel bir veritabanında depolanan yönetebileceği sahip.</span><span class="sxs-lookup"><span data-stu-id="cd878-368">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>
## <a name="overview"></a><span data-ttu-id="cd878-369">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="cd878-369">Overview</span></span>

<span data-ttu-id="cd878-370">Bu öğretici yandaki API oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cd878-370">This tutorial creates the following API:</span></span>

|<span data-ttu-id="cd878-371">API</span><span class="sxs-lookup"><span data-stu-id="cd878-371">API</span></span> | <span data-ttu-id="cd878-372">Açıklama</span><span class="sxs-lookup"><span data-stu-id="cd878-372">Description</span></span> | <span data-ttu-id="cd878-373">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="cd878-373">Request body</span></span> | <span data-ttu-id="cd878-374">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="cd878-374">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="cd878-375">/Api/TodoItems al</span><span class="sxs-lookup"><span data-stu-id="cd878-375">GET /api/TodoItems</span></span> | <span data-ttu-id="cd878-376">Tüm yapılacak iş öğeleri al</span><span class="sxs-lookup"><span data-stu-id="cd878-376">Get all to-do items</span></span> | <span data-ttu-id="cd878-377">Yok.</span><span class="sxs-lookup"><span data-stu-id="cd878-377">None</span></span> | <span data-ttu-id="cd878-378">Yapılacaklar öğelerinin bir dizisi</span><span class="sxs-lookup"><span data-stu-id="cd878-378">Array of to-do items</span></span>|
|<span data-ttu-id="cd878-379">/Api/TodoItems/{id} al</span><span class="sxs-lookup"><span data-stu-id="cd878-379">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="cd878-380">Bir öğeyi Kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="cd878-380">Get an item by ID</span></span> | <span data-ttu-id="cd878-381">Yok.</span><span class="sxs-lookup"><span data-stu-id="cd878-381">None</span></span> | <span data-ttu-id="cd878-382">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="cd878-382">To-do item</span></span>|
|<span data-ttu-id="cd878-383">POST/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="cd878-383">POST /api/TodoItems</span></span> | <span data-ttu-id="cd878-384">Yeni Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="cd878-384">Add a new item</span></span> | <span data-ttu-id="cd878-385">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="cd878-385">To-do item</span></span> | <span data-ttu-id="cd878-386">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="cd878-386">To-do item</span></span> |
|<span data-ttu-id="cd878-387">/Api/TodoItems/{id} koy</span><span class="sxs-lookup"><span data-stu-id="cd878-387">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="cd878-388">Mevcut öğeyi güncelleştirin &nbsp;</span><span class="sxs-lookup"><span data-stu-id="cd878-388">Update an existing item &nbsp;</span></span> | <span data-ttu-id="cd878-389">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="cd878-389">To-do item</span></span> | <span data-ttu-id="cd878-390">Yok.</span><span class="sxs-lookup"><span data-stu-id="cd878-390">None</span></span> |
|<span data-ttu-id="cd878-391">/Api/TodoItems/{id} &nbsp; Sil&nbsp;</span><span class="sxs-lookup"><span data-stu-id="cd878-391">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="cd878-392">Öğeyi Sil &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="cd878-392">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="cd878-393">Hiçbiri</span><span class="sxs-lookup"><span data-stu-id="cd878-393">None</span></span> | <span data-ttu-id="cd878-394">Hiçbiri</span><span class="sxs-lookup"><span data-stu-id="cd878-394">None</span></span>|

<span data-ttu-id="cd878-395">Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="cd878-395">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki bir kutu ile temsil edilir.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="cd878-401">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="cd878-401">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd878-402">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-402">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cd878-403">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cd878-403">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cd878-404">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-404">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="cd878-405">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="cd878-405">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd878-406">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-406">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cd878-407">**Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-407">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="cd878-408">**ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="cd878-408">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="cd878-409">Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="cd878-409">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="cd878-410">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 2,2** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="cd878-410">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="cd878-411">**API** şablonunu seçin ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="cd878-411">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="cd878-412">**Docker desteğini etkinleştir**' i seçmeyin.</span><span class="sxs-lookup"><span data-stu-id="cd878-412">**Don't** select **Enable Docker Support**.</span></span>

![VS yeni proje iletişim kutusu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cd878-414">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cd878-414">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="cd878-415">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="cd878-415">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="cd878-416">Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="cd878-416">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="cd878-417">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cd878-417">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="cd878-418">Bu komutlar yeni bir Web API projesi oluşturur ve yeni proje klasöründe Visual Studio Code yeni bir örneğini açar.</span><span class="sxs-lookup"><span data-stu-id="cd878-418">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="cd878-419">Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-419">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cd878-420">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-420">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="cd878-421">**Dosya** > **yeni çözüm**' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-421">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="cd878-423">**.NET Core** > **uygulama** **API 'si** ileri ' **yi**seçin. > ></span><span class="sxs-lookup"><span data-stu-id="cd878-423">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="cd878-425">İçinde **, yeni ASP.NET Core Web API'sini yapılandırma** iletişim kutusunda varsayılan değerleri kabul **hedef Framework'ü** , \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="cd878-425">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="cd878-426">Girin *TodoApi* için **proje adı** seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="cd878-426">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="cd878-428">API'yi test etme</span><span class="sxs-lookup"><span data-stu-id="cd878-428">Test the API</span></span>

<span data-ttu-id="cd878-429">Proje şablonu oluşturur bir `values` API.</span><span class="sxs-lookup"><span data-stu-id="cd878-429">The project template creates a `values` API.</span></span> <span data-ttu-id="cd878-430">Çağrı `Get` uygulamayı test etmek için bir tarayıcıdan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cd878-430">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd878-431">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-431">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cd878-432">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="cd878-432">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="cd878-433">Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>/api/values`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="cd878-433">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="cd878-434">IIS Express sertifika güven varsa soran bir iletişim kutusu alırsanız seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="cd878-434">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="cd878-435">İçinde **Güvenlik Uyarısı** ardından, görüntülenen iletişim seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="cd878-435">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cd878-436">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cd878-436">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="cd878-437">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="cd878-437">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="cd878-438">Bir tarayıcıda aşağıdaki URL'ye gidin: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="cd878-438">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cd878-439">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-439">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="cd878-440">Uygulamayı başlatmak için**hata ayıklamayı Başlat** ' ı seçin. > </span><span class="sxs-lookup"><span data-stu-id="cd878-440">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="cd878-441">Mac için Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="cd878-441">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="cd878-442">HTTP 404 (bulunamadı) hatası döndürülür.</span><span class="sxs-lookup"><span data-stu-id="cd878-442">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="cd878-443">Append `/api/values` URL'sine (URL'yi `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="cd878-443">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="cd878-444">Aşağıdaki JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="cd878-444">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="cd878-445">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="cd878-445">Add a model class</span></span>

<span data-ttu-id="cd878-446">A *modeli* uygulamayı yöneten verilerini temsil eden sınıflar kümesidir.</span><span class="sxs-lookup"><span data-stu-id="cd878-446">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="cd878-447">Tek bir modeldir bu uygulama için `TodoItem` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="cd878-447">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd878-448">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-448">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cd878-449">İçinde **Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="cd878-449">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="cd878-450">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="cd878-450">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="cd878-451">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="cd878-451">Name the folder *Models*.</span></span>

* <span data-ttu-id="cd878-452">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="cd878-452">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="cd878-453">Sınıf adı *Todoıtem* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="cd878-453">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="cd878-454">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="cd878-454">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cd878-455">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cd878-455">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="cd878-456">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="cd878-456">Add a folder named *Models*.</span></span>

* <span data-ttu-id="cd878-457">Ekleme bir `TodoItem` sınıfının *modelleri* aşağıdaki kodla klasörü:</span><span class="sxs-lookup"><span data-stu-id="cd878-457">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cd878-458">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-458">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="cd878-459">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="cd878-459">Right-click the project.</span></span> <span data-ttu-id="cd878-460">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="cd878-460">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="cd878-461">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="cd878-461">Name the folder *Models*.</span></span>

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="cd878-463">*Modeller* klasörüne sağ tıklayın ve **yeni dosya** > **Ekle** > **genel** > **boş sınıfı**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-463">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="cd878-464">Sınıf adı *Todoıtem*ve ardından **yeni**.</span><span class="sxs-lookup"><span data-stu-id="cd878-464">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="cd878-465">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="cd878-465">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="cd878-466">`Id` Özelliği işlevlerinin bir ilişkisel veritabanında benzersiz anahtar.</span><span class="sxs-lookup"><span data-stu-id="cd878-466">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="cd878-467">Model sınıfları herhangi bir projede gidip ancak *modelleri* klasörü, kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cd878-467">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="cd878-468">Veritabanı bağlamı Ekle</span><span class="sxs-lookup"><span data-stu-id="cd878-468">Add a database context</span></span>

<span data-ttu-id="cd878-469">*Veritabanı bağlamı* koordine eden bir veri modeli için Entity Framework işlevsellik ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="cd878-469">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="cd878-470">Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="cd878-470">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd878-471">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-471">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cd878-472">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="cd878-472">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="cd878-473">Sınıf adı *TodoContext* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="cd878-473">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="cd878-474">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-474">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="cd878-475">Ekleme bir `TodoContext` sınıfının *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="cd878-475">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="cd878-476">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="cd878-476">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="cd878-477">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="cd878-477">Register the database context</span></span>

<span data-ttu-id="cd878-478">ASP.NET Core DB bağlamı gibi hizmetler ile kaydedilmelidir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="cd878-478">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="cd878-479">Kapsayıcı hizmeti denetleyicilerine sağlar.</span><span class="sxs-lookup"><span data-stu-id="cd878-479">The container provides the service to controllers.</span></span>

<span data-ttu-id="cd878-480">Güncelleştirme *Startup.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="cd878-480">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="cd878-481">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="cd878-481">The preceding code:</span></span>

* <span data-ttu-id="cd878-482">Kullanılmayan kaldırır `using` bildirimleri.</span><span class="sxs-lookup"><span data-stu-id="cd878-482">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="cd878-483">Veritabanı bağlamı DI kapsayıcıya ekler.</span><span class="sxs-lookup"><span data-stu-id="cd878-483">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="cd878-484">Veritabanı bağlamı bir bellek içi veritabanına kullanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="cd878-484">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="cd878-485">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="cd878-485">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd878-486">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-486">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cd878-487">Sağ *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="cd878-487">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="cd878-488">**Yeni öğe** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-488">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="cd878-489">İçinde **Yeni Öğe Ekle** iletişim kutusunda **API denetleyici sınıfı** şablonu.</span><span class="sxs-lookup"><span data-stu-id="cd878-489">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="cd878-490">Sınıf adı *TodoController*seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="cd878-490">Name the class *TodoController*, and select **Add**.</span></span>

  ![Yeni öğe iletişim denetleyicisiyle seçilen arama kutusu ve web API denetleyicisi Ekle](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="cd878-492">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-492">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="cd878-493">İçinde *denetleyicileri* klasör adında bir sınıf oluşturma `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="cd878-493">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="cd878-494">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="cd878-494">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="cd878-495">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="cd878-495">The preceding code:</span></span>

* <span data-ttu-id="cd878-496">Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.</span><span class="sxs-lookup"><span data-stu-id="cd878-496">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="cd878-497">Sınıfı [[Apicontroller]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle süsler.</span><span class="sxs-lookup"><span data-stu-id="cd878-497">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="cd878-498">Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="cd878-498">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="cd878-499">Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="cd878-499">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="cd878-500">Veritabanı bağlamı eklemesine DI kullanır (`TodoContext`) içine denetleyici.</span><span class="sxs-lookup"><span data-stu-id="cd878-500">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="cd878-501">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="cd878-501">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="cd878-502">Adlı bir öğe ekler `Item1` veritabanı boşsa veritabanı.</span><span class="sxs-lookup"><span data-stu-id="cd878-502">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="cd878-503">Her çalıştığında bu kod oluşturucusunun içinde yeni bir HTTP isteği olduğundan.</span><span class="sxs-lookup"><span data-stu-id="cd878-503">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="cd878-504">Tüm öğeleri silerseniz, oluşturucu oluşturur `Item1` API yöntemi çağrıldığında tekrar başlattığınızda.</span><span class="sxs-lookup"><span data-stu-id="cd878-504">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="cd878-505">Bu nedenle, gerçekten işe yaradı silme işlemi işe yaramadı gibi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="cd878-505">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="cd878-506">Get yöntemleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="cd878-506">Add Get methods</span></span>

<span data-ttu-id="cd878-507">Yapılacak iş öğeleri alır bir API sağlamak için aşağıdaki yöntemi ekleyin. `TodoController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="cd878-507">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="cd878-508">İki GET uç noktası bu yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="cd878-508">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="cd878-509">Hala çalışıyorsa uygulamayı durdurun.</span><span class="sxs-lookup"><span data-stu-id="cd878-509">Stop the app if it's still running.</span></span> <span data-ttu-id="cd878-510">Ardından, en son değişiklikleri dahil etmek için yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cd878-510">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="cd878-511">Bir tarayıcıdan iki uç nokta çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="cd878-511">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="cd878-512">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cd878-512">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="cd878-513">Şu HTTP yanıtı çağrısı tarafından üretilen `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="cd878-513">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="cd878-514">URL Yönlendirme ve yolları</span><span class="sxs-lookup"><span data-stu-id="cd878-514">Routing and URL paths</span></span>

<span data-ttu-id="cd878-515">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Özniteliği bir HTTP GET isteğine yanıt vermeden bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="cd878-515">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="cd878-516">Her yöntem için URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="cd878-516">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="cd878-517">Denetleyicinin şablonu dizesi ile başlayıp `Route` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="cd878-517">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="cd878-518">Değiştirin `[controller]` denetleyicinin adı ile kural tarafından olduğu "Controller" soneki eksi denetleyici sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="cd878-518">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="cd878-519">Bu örnek, denetleyici sınıfı adı olan **Todo**Denetleyici adı "todo" Bu nedenle denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="cd878-519">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="cd878-520">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="cd878-520">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="cd878-521">Özniteliğin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin. `[HttpGet]`</span><span class="sxs-lookup"><span data-stu-id="cd878-521">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="cd878-522">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="cd878-522">This sample doesn't use a template.</span></span> <span data-ttu-id="cd878-523">Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="cd878-523">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="cd878-524">Aşağıdaki `GetTodoItem` yöntemi `"{id}"` yapılacak iş öğesi benzersiz tanımlayıcısı için bir yer tutucu değişkendir.</span><span class="sxs-lookup"><span data-stu-id="cd878-524">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="cd878-525">Zaman `GetTodoItem` çağrılır, değerini `"{id}"` yöntemine URL'de sağlanan kendi`id` parametresi.</span><span class="sxs-lookup"><span data-stu-id="cd878-525">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="cd878-526">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="cd878-526">Return values</span></span>

<span data-ttu-id="cd878-527">Dönüş türünü `GetTodoItems` ve `GetTodoItem` yöntemler [actionresult öğesini\<T > türü](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="cd878-527">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="cd878-528">ASP.NET Core, nesneyi otomatik olarak serileştiren [JSON](https://www.json.org/) ve yanıt iletisinin gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="cd878-528">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="cd878-529">Yanıt kodu 200 bu dönüş türü için olduğu varsayılırsa işlenmeyen özel durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="cd878-529">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="cd878-530">İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.</span><span class="sxs-lookup"><span data-stu-id="cd878-530">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="cd878-531">`ActionResult` dönüş türleri, geniş HTTP durum kodları temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="cd878-531">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="cd878-532">Örneğin, `GetTodoItem` iki farklı durum değerleri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="cd878-532">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="cd878-533">Öğe istenen kimliği eşleşirse, yöntem bir 404 döndürür [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu.</span><span class="sxs-lookup"><span data-stu-id="cd878-533">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="cd878-534">Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="cd878-534">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="cd878-535">Döndüren `item` sonuçları bir HTTP 200 yanıtı.</span><span class="sxs-lookup"><span data-stu-id="cd878-535">Returning `item` results in an HTTP 200 response.</span></span>


## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="cd878-536">Test GetTodoItems yöntemi</span><span class="sxs-lookup"><span data-stu-id="cd878-536">Test the GetTodoItems method</span></span>

<span data-ttu-id="cd878-537">Bu öğreticide Postman web API'si test etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cd878-537">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="cd878-538">Yükleme [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="cd878-538">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="cd878-539">Web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="cd878-539">Start the web app.</span></span>
* <span data-ttu-id="cd878-540">Postman'i başlatın.</span><span class="sxs-lookup"><span data-stu-id="cd878-540">Start Postman.</span></span>
* <span data-ttu-id="cd878-541">Devre dışı **SSL sertifika doğrulama**</span><span class="sxs-lookup"><span data-stu-id="cd878-541">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd878-542">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-542">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cd878-543">**Dosya** ayarlarından (Genel sekmesinden) SSL sertifikası doğrulamasını devre dışı bırakın. ></span><span class="sxs-lookup"><span data-stu-id="cd878-543">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="cd878-544">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd878-544">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="cd878-545">**Postman** > **tercihleri** ' nden (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="cd878-545">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="cd878-546">Alternatif olarak, wranı seçin ve **Ayarlar**' ı seçip SSL sertifikası doğrulamasını devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="cd878-546">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="cd878-547">Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="cd878-547">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="cd878-548">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cd878-548">Create a new request.</span></span>
  * <span data-ttu-id="cd878-549">HTTP yöntemi kümesine **alma**.</span><span class="sxs-lookup"><span data-stu-id="cd878-549">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="cd878-550">İstek URL'si kümesine `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="cd878-550">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="cd878-551">Örneğin: `https://localhost:5001/api/todo`</span><span class="sxs-lookup"><span data-stu-id="cd878-551">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="cd878-552">Ayarlama **iki bölme görünümü** postman'deki.</span><span class="sxs-lookup"><span data-stu-id="cd878-552">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="cd878-553">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-553">Select **Send**.</span></span>

![Get isteğiyle postman](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="cd878-555">Create yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="cd878-555">Add a Create method</span></span>

<span data-ttu-id="cd878-556">`PostTodoItem` *Controllers/TodoController. cs*içindeki şu yöntemi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="cd878-556">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="cd878-557">Yukarıdaki kod tarafından belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="cd878-557">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="cd878-558">Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="cd878-558">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="cd878-559">`CreatedAtAction` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="cd878-559">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="cd878-560">Başarılı olursa bir HTTP 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="cd878-560">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="cd878-561">HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="cd878-561">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="cd878-562">Yanıta bir `Location` üst bilgi ekler.</span><span class="sxs-lookup"><span data-stu-id="cd878-562">Adds a `Location` header to the response.</span></span> <span data-ttu-id="cd878-563">`Location` Üst bilgi, yeni oluşturulan Yapılacaklar öğesinin URI 'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="cd878-563">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="cd878-564">Daha fazla bilgi için [10.2.2 201 oluşturuldu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="cd878-564">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="cd878-565">`Location` Üstbilginin URI 'sini oluşturma eylemine başvurur. `GetTodoItem`</span><span class="sxs-lookup"><span data-stu-id="cd878-565">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="cd878-566">Anahtar sözcüğü, `CreatedAtAction` çağrıda eylem adının sabit kodlanmasını önlemek için kullanılır. C# `nameof`</span><span class="sxs-lookup"><span data-stu-id="cd878-566">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="cd878-567">Test PostTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="cd878-567">Test the PostTodoItem method</span></span>

* <span data-ttu-id="cd878-568">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cd878-568">Build the project.</span></span>
* <span data-ttu-id="cd878-569">Postman HTTP yöntemi kümesine `POST`.</span><span class="sxs-lookup"><span data-stu-id="cd878-569">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="cd878-570">Seçin **gövdesi** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="cd878-570">Select the **Body** tab.</span></span>
* <span data-ttu-id="cd878-571">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="cd878-571">Select the **raw** radio button.</span></span>
* <span data-ttu-id="cd878-572">Tür kümesine **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="cd878-572">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="cd878-573">İstek gövdesinde bir yapılacak iş öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="cd878-573">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="cd878-574">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-574">Select **Send**.</span></span>

  ![Postman ile isteği oluştur](first-web-api/_static/create.png)

  <span data-ttu-id="cd878-576">Bir 405 yöntemine izin verilmiyor hatası alırsanız, `PostTodoItem` Yöntem eklendikten sonra projeyi derlememe sonucu büyük olasılıkla oluşur.</span><span class="sxs-lookup"><span data-stu-id="cd878-576">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="cd878-577">Konum üst bilgisi URI test</span><span class="sxs-lookup"><span data-stu-id="cd878-577">Test the location header URI</span></span>

* <span data-ttu-id="cd878-578">Seçin **üstbilgileri** sekmesinde **yanıt** bölmesi.</span><span class="sxs-lookup"><span data-stu-id="cd878-578">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="cd878-579">Kopyalama **konumu** üst bilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="cd878-579">Copy the **Location** header value:</span></span>

  ![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmc2.png)

* <span data-ttu-id="cd878-581">Yöntemini GET öğesine Ayarla.</span><span class="sxs-lookup"><span data-stu-id="cd878-581">Set the method to GET.</span></span>
* <span data-ttu-id="cd878-582">URİ'sini yapıştırın (örneğin, `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="cd878-582">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="cd878-583">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="cd878-583">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="cd878-584">PutTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="cd878-584">Add a PutTodoItem method</span></span>

<span data-ttu-id="cd878-585">Aşağıdaki `PutTodoItem` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="cd878-585">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="cd878-586">`PutTodoItem` benzer `PostTodoItem`, HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="cd878-586">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="cd878-587">Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="cd878-587">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="cd878-588">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca değişiklikler değil göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="cd878-588">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="cd878-589">Kısmi güncelleştirmeleri desteklemek için kullanma [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="cd878-589">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="cd878-590">Çağrılırken `PutTodoItem`bir hata alırsanız, veritabanında bir öğe `GET` olduğundan emin olmak için çağırın.</span><span class="sxs-lookup"><span data-stu-id="cd878-590">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="cd878-591">Test PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="cd878-591">Test the PutTodoItem method</span></span>

<span data-ttu-id="cd878-592">Bu örnek, uygulama her başlatıldığında yeniden başlatılması gereken bellek içi veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="cd878-592">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="cd878-593">Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="cd878-593">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="cd878-594">PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="cd878-594">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="cd878-595">Kimliğine sahip bir yapılacak iş öğesi güncelleştirme = 1 ve "balık akış için" adını girin:</span><span class="sxs-lookup"><span data-stu-id="cd878-595">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="cd878-596">Aşağıdaki görüntüde, Postman güncelleştirme gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="cd878-596">The following image shows the Postman update:</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="cd878-598">DeleteTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="cd878-598">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="cd878-599">Aşağıdaki `DeleteTodoItem` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="cd878-599">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="cd878-600">`DeleteTodoItem` Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="cd878-600">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="cd878-601">Test DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="cd878-601">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="cd878-602">Postman bir yapılacak iş öğesini silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="cd878-602">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="cd878-603">Yöntem kümesine `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="cd878-603">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="cd878-604">Silmek, örneğin nesnenin URI ayarlayın `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="cd878-604">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="cd878-605">Seçin **Gönder**</span><span class="sxs-lookup"><span data-stu-id="cd878-605">Select **Send**</span></span>

<span data-ttu-id="cd878-606">Örnek uygulama, tüm öğeleri silmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="cd878-606">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="cd878-607">Ancak, son öğe silindiğinde, API 'nin bir sonraki çağrılışında model sınıfı Oluşturucu tarafından yeni bir tane oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cd878-607">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="cd878-608">JQuery ile API çağırma</span><span class="sxs-lookup"><span data-stu-id="cd878-608">Call the API with jQuery</span></span>

<span data-ttu-id="cd878-609">Bu bölümde, Web'i çağırmaya jQuery kullanan bir HTML sayfasına eklenen API.</span><span class="sxs-lookup"><span data-stu-id="cd878-609">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="cd878-610">jQuery isteği başlatır ve API'nin yanıt Ayrıntıları sayfası güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="cd878-610">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="cd878-611">Uygulamayı [statik dosyalara sunacak](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) şekilde yapılandırın ve aşağıdaki vurgulanmış kodla *Startup.cs* güncelleştirerek [varsayılan dosya eşlemesini etkinleştirin](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) :</span><span class="sxs-lookup"><span data-stu-id="cd878-611">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="cd878-612">Oluşturma bir *wwwroot* proje dizininde klasör.</span><span class="sxs-lookup"><span data-stu-id="cd878-612">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="cd878-613">Adlı bir HTML dosyası ekleyin *index.html* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="cd878-613">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="cd878-614">Dosyanın içeriğini aşağıdaki biçimlendirme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="cd878-614">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="cd878-615">Adlı bir JavaScript dosyası ekleyin *site.js* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="cd878-615">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="cd878-616">Dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="cd878-616">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="cd878-617">ASP.NET Core proje başlatma ayarlarında bir değişiklik HTML sayfasını yerel olarak test etmek için gerekli:</span><span class="sxs-lookup"><span data-stu-id="cd878-617">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="cd878-618">Açık *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="cd878-618">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="cd878-619">Kaldırma `launchUrl` , açmak için uygulamayı zorlamak için özellik *index.html*&mdash;projenin varsayılan dosya.</span><span class="sxs-lookup"><span data-stu-id="cd878-619">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="cd878-620">JQuery almak için birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="cd878-620">There are several ways to get jQuery.</span></span> <span data-ttu-id="cd878-621">Yukarıdaki kod parçacığında, bir CDN kitaplığı yüklenir.</span><span class="sxs-lookup"><span data-stu-id="cd878-621">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="cd878-622">Bu örnek, tüm API CRUD yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="cd878-622">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="cd878-623">API çağrıları açıklamaları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="cd878-623">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="cd878-624">Yapılacaklar öğelerinin bir listesini alın</span><span class="sxs-lookup"><span data-stu-id="cd878-624">Get a list of to-do items</span></span>

<span data-ttu-id="cd878-625">JQuery [ajax](https://api.jquery.com/jquery.ajax/) işlev gönderen bir `GET` Yapılacaklar öğelerini dizisini temsil eden JSON döndüren API isteği.</span><span class="sxs-lookup"><span data-stu-id="cd878-625">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="cd878-626">`success` İstek başarılı olursa geri çağırma işlevi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="cd878-626">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="cd878-627">Geri çağırma içinde DOM Yapılacaklar bilgilerle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="cd878-627">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="cd878-628">Yapılacak İş Öğesi Ekle</span><span class="sxs-lookup"><span data-stu-id="cd878-628">Add a to-do item</span></span>

<span data-ttu-id="cd878-629">[Ajax](https://api.jquery.com/jquery.ajax/) işlev gönderen bir `POST` istek ile istek gövdesindeki yapılacak iş öğesi.</span><span class="sxs-lookup"><span data-stu-id="cd878-629">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="cd878-630">`accepts` Ve `contentType` seçeneklerini ayarlamak `application/json` gönderilen ve alınan medya türü belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="cd878-630">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="cd878-631">Yapılacak iş öğesi kullanarak JSON'a dönüştürülür [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="cd878-631">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="cd878-632">API'yi bir başarılı durum kodu döndürdüğünde `getData` işlevi HTML tablosu güncelleştirmek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="cd878-632">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="cd878-633">Yapılacak iş öğesini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="cd878-633">Update a to-do item</span></span>

<span data-ttu-id="cd878-634">Yapılacak iş öğesi güncelleştirilirken bir eklemeye benzerdir.</span><span class="sxs-lookup"><span data-stu-id="cd878-634">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="cd878-635">`url` Öğenin benzersiz tanıtıcısı eklemek için değişiklikleri ve `type` olduğu `PUT`.</span><span class="sxs-lookup"><span data-stu-id="cd878-635">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="cd878-636">Yapılacak iş öğesi silme</span><span class="sxs-lookup"><span data-stu-id="cd878-636">Delete a to-do item</span></span>

<span data-ttu-id="cd878-637">Yapılacak iş öğesi silme gerçekleştirilir ayarlayarak `type` AJAX çağrısı hedefi üzerinde `DELETE` ve öğenin benzersiz tanıtıcısı URL'yi belirterek.</span><span class="sxs-lookup"><span data-stu-id="cd878-637">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="cd878-638">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="cd878-638">Additional resources</span></span>

<span data-ttu-id="cd878-639">[Görüntülemek veya Bu öğretici için örnek kodu indirdikten](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="cd878-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="cd878-640">Bkz: [nasıl indirileceğini](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="cd878-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="cd878-641">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="cd878-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="cd878-642">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="cd878-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
