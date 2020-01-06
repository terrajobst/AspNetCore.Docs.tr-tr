---
title: "Öğretici: ASP.NET Core bir Web API 'SI oluşturma"
author: rick-anderson
description: ASP.NET Core ile Web API 'SI oluşturmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 3bf930d19684e84365f0ff0255fccd2939fb3f39
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354917"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="e8638-103">Öğretici: ASP.NET Core bir Web API 'SI oluşturma</span><span class="sxs-lookup"><span data-stu-id="e8638-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="e8638-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="e8638-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="e8638-105">Bu öğretici, bir web API ASP.NET Core ile oluşturmaya ilişkin temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="e8638-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e8638-106">Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="e8638-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e8638-107">Bir Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e8638-107">Create a web API project.</span></span>
> * <span data-ttu-id="e8638-108">Bir model sınıfı ve bir veritabanı bağlamı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e8638-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="e8638-109">CRUD yöntemleriyle bir denetleyiciyi dolandırın.</span><span class="sxs-lookup"><span data-stu-id="e8638-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="e8638-110">Yönlendirmeyi, URL yollarını ve dönüş değerlerini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e8638-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="e8638-111">Web API'si Postman ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="e8638-111">Call the web API with Postman.</span></span>

<span data-ttu-id="e8638-112">Sonunda, bir veritabanında depolanan "yapılacaklar" öğelerini yönetebilmek için bir Web API 'SI vardır.</span><span class="sxs-lookup"><span data-stu-id="e8638-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="e8638-113">Genel bakış</span><span class="sxs-lookup"><span data-stu-id="e8638-113">Overview</span></span>

<span data-ttu-id="e8638-114">Bu öğretici yandaki API oluşturur:</span><span class="sxs-lookup"><span data-stu-id="e8638-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="e8638-115">API</span><span class="sxs-lookup"><span data-stu-id="e8638-115">API</span></span> | <span data-ttu-id="e8638-116">Açıklama</span><span class="sxs-lookup"><span data-stu-id="e8638-116">Description</span></span> | <span data-ttu-id="e8638-117">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="e8638-117">Request body</span></span> | <span data-ttu-id="e8638-118">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="e8638-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="e8638-119">/Api/TodoItems al</span><span class="sxs-lookup"><span data-stu-id="e8638-119">GET /api/TodoItems</span></span> | <span data-ttu-id="e8638-120">Tüm yapılacak iş öğeleri al</span><span class="sxs-lookup"><span data-stu-id="e8638-120">Get all to-do items</span></span> | <span data-ttu-id="e8638-121">Yok.</span><span class="sxs-lookup"><span data-stu-id="e8638-121">None</span></span> | <span data-ttu-id="e8638-122">Yapılacaklar öğelerinin bir dizisi</span><span class="sxs-lookup"><span data-stu-id="e8638-122">Array of to-do items</span></span>|
|<span data-ttu-id="e8638-123">/Api/TodoItems/{id} al</span><span class="sxs-lookup"><span data-stu-id="e8638-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="e8638-124">Bir öğeyi Kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="e8638-124">Get an item by ID</span></span> | <span data-ttu-id="e8638-125">Yok.</span><span class="sxs-lookup"><span data-stu-id="e8638-125">None</span></span> | <span data-ttu-id="e8638-126">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="e8638-126">To-do item</span></span>|
|<span data-ttu-id="e8638-127">POST/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="e8638-127">POST /api/TodoItems</span></span> | <span data-ttu-id="e8638-128">Yeni Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="e8638-128">Add a new item</span></span> | <span data-ttu-id="e8638-129">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="e8638-129">To-do item</span></span> | <span data-ttu-id="e8638-130">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="e8638-130">To-do item</span></span> |
|<span data-ttu-id="e8638-131">/Api/TodoItems/{id} koy</span><span class="sxs-lookup"><span data-stu-id="e8638-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="e8638-132">Mevcut öğeyi güncelleştirin &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e8638-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="e8638-133">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="e8638-133">To-do item</span></span> | <span data-ttu-id="e8638-134">Yok.</span><span class="sxs-lookup"><span data-stu-id="e8638-134">None</span></span> |
|<span data-ttu-id="e8638-135">/Api/TodoItems/{id} &nbsp; SIL &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e8638-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="e8638-136">Öğeyi Sil &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e8638-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="e8638-137">Yok.</span><span class="sxs-lookup"><span data-stu-id="e8638-137">None</span></span> | <span data-ttu-id="e8638-138">Yok.</span><span class="sxs-lookup"><span data-stu-id="e8638-138">None</span></span>|

<span data-ttu-id="e8638-139">Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="e8638-139">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki bir kutu ile temsil edilir.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="e8638-145">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="e8638-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8638-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e8638-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e8638-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e8638-148">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="e8638-149">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e8638-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8638-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e8638-151">**Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e8638-152">**ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e8638-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="e8638-153">Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e8638-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="e8638-154">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,1** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="e8638-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="e8638-155">**API** şablonunu seçin ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e8638-155">Select the **API** template and click **Create**.</span></span>

![VS yeni proje iletişim kutusu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e8638-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e8638-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e8638-158">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e8638-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e8638-159">Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="e8638-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="e8638-160">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e8638-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="e8638-161">Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="e8638-162">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="e8638-162">The preceding commands:</span></span>

  * <span data-ttu-id="e8638-163">Yeni bir Web API projesi oluşturur ve Visual Studio Code açar.</span><span class="sxs-lookup"><span data-stu-id="e8638-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="e8638-164">Sonraki bölümde gerekli olan NuGet paketlerini ekler.</span><span class="sxs-lookup"><span data-stu-id="e8638-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e8638-165">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e8638-166">**Yeni çözüm**> **Dosya** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-166">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="e8638-168">**.NET Core** > **App** > **API** > ' **yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="e8638-170">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **hedef Framework** \* *.NET Core 3,1*' i seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.1*.</span></span>

* <span data-ttu-id="e8638-171">Girin *TodoApi* için **proje adı** seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="e8638-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="e8638-173">Proje klasöründe bir komut terminali açın ve aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e8638-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="e8638-174">API'yi test etme</span><span class="sxs-lookup"><span data-stu-id="e8638-174">Test the API</span></span>

<span data-ttu-id="e8638-175">Proje şablonu oluşturur bir `WeatherForecast` API.</span><span class="sxs-lookup"><span data-stu-id="e8638-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="e8638-176">Çağrı `Get` uygulamayı test etmek için bir tarayıcıdan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e8638-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8638-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e8638-178">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="e8638-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e8638-179">Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>/WeatherForecast`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="e8638-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="e8638-180">IIS Express sertifika güven varsa soran bir iletişim kutusu alırsanız seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="e8638-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="e8638-181">İçinde **Güvenlik Uyarısı** ardından, görüntülenen iletişim seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="e8638-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e8638-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e8638-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e8638-183">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="e8638-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e8638-184">Bir tarayıcıda aşağıdaki URL'ye gidin: [ https://localhost:5001/WeatherForecast ](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="e8638-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e8638-185">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e8638-186">Uygulamayı başlatmak için **hata ayıklamayı başlat** > **Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="e8638-187">Mac için Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="e8638-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="e8638-188">HTTP 404 (bulunamadı) hatası döndürülür.</span><span class="sxs-lookup"><span data-stu-id="e8638-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="e8638-189">Append `/WeatherForecast` URL'sine (URL'yi `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="e8638-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="e8638-190">Aşağıdakine benzer bir JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="e8638-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="e8638-191">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="e8638-191">Add a model class</span></span>

<span data-ttu-id="e8638-192">A *modeli* uygulamayı yöneten verilerini temsil eden sınıflar kümesidir.</span><span class="sxs-lookup"><span data-stu-id="e8638-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="e8638-193">Tek bir modeldir bu uygulama için `TodoItem` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e8638-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8638-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e8638-195">İçinde **Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e8638-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="e8638-196">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="e8638-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e8638-197">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="e8638-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="e8638-198">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="e8638-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e8638-199">Sınıf adı *Todoıtem* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="e8638-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="e8638-200">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e8638-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e8638-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e8638-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e8638-202">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="e8638-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="e8638-203">Ekleme bir `TodoItem` sınıfının *modelleri* aşağıdaki kodla klasörü:</span><span class="sxs-lookup"><span data-stu-id="e8638-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e8638-204">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e8638-205">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e8638-205">Right-click the project.</span></span> <span data-ttu-id="e8638-206">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="e8638-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e8638-207">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="e8638-207">Name the folder *Models*.</span></span>

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="e8638-209">*Modeller* klasörüne sağ tıklayın ve > **yeni dosya** **ekle** ' yi > **genel** > **boş sınıf**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="e8638-210">Sınıf adı *Todoıtem*ve ardından **yeni**.</span><span class="sxs-lookup"><span data-stu-id="e8638-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="e8638-211">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e8638-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="e8638-212">`Id` Özelliği işlevlerinin bir ilişkisel veritabanında benzersiz anahtar.</span><span class="sxs-lookup"><span data-stu-id="e8638-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="e8638-213">Model sınıfları herhangi bir projede gidip ancak *modelleri* klasörü, kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e8638-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="e8638-214">Veritabanı bağlamı Ekle</span><span class="sxs-lookup"><span data-stu-id="e8638-214">Add a database context</span></span>

<span data-ttu-id="e8638-215">*Veritabanı bağlamı* koordine eden bir veri modeli için Entity Framework işlevsellik ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="e8638-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="e8638-216">Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e8638-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8638-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="e8638-218">Microsoft. EntityFrameworkCore. SqlServer ekleyin</span><span class="sxs-lookup"><span data-stu-id="e8638-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="e8638-219">**Araçlar** menüsünde **nuget Paket Yöneticisi > çözüm Için NuGet Paketlerini Yönet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="e8638-220">**Araştır** sekmesini seçin ve arama kutusuna **Microsoft. Entityframeworkcore. SqlServer** yazın.</span><span class="sxs-lookup"><span data-stu-id="e8638-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="e8638-221">Sol bölmedeki **Microsoft. EntityFrameworkCore. SqlServer** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="e8638-222">Sağ bölmedeki **Proje** onay kutusunu seçin ve ardından **Install**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="e8638-223">`Microsoft.EntityFrameworkCore.InMemory` NuGet paketini eklemek için yukarıdaki yönergeleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="e8638-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet Paket Yöneticisi](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="e8638-225">TodoContext veritabanı bağlamını ekleme</span><span class="sxs-lookup"><span data-stu-id="e8638-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="e8638-226">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="e8638-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e8638-227">Sınıf adı *TodoContext* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="e8638-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e8638-228">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e8638-229">Ekleme bir `TodoContext` sınıfının *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="e8638-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="e8638-230">Aşağıdaki kodu girin:</span><span class="sxs-lookup"><span data-stu-id="e8638-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="e8638-231">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="e8638-231">Register the database context</span></span>

<span data-ttu-id="e8638-232">ASP.NET Core DB bağlamı gibi hizmetler ile kaydedilmelidir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="e8638-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e8638-233">Kapsayıcı hizmeti denetleyicilerine sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8638-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="e8638-234">Güncelleştirme *Startup.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="e8638-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="e8638-235">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="e8638-235">The preceding code:</span></span>

* <span data-ttu-id="e8638-236">Kullanılmayan kaldırır `using` bildirimleri.</span><span class="sxs-lookup"><span data-stu-id="e8638-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="e8638-237">Veritabanı bağlamı DI kapsayıcıya ekler.</span><span class="sxs-lookup"><span data-stu-id="e8638-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="e8638-238">Veritabanı bağlamı bir bellek içi veritabanına kullanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="e8638-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="e8638-239">Denetleyiciyi bir denetleyiciye katlama</span><span class="sxs-lookup"><span data-stu-id="e8638-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8638-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e8638-241">Sağ *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="e8638-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="e8638-242">> **yeni yapı Iskelesi öğesi** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="e8638-243">**Entity Framework kullanarak ve eylemler Içeren API denetleyicisi**' ni seçin ve ardından **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="e8638-244">**API denetleyiciyi eylemler Ile Ekle ' de Entity Framework** iletişim kutusunu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="e8638-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="e8638-245">**Model sınıfında** **TodoItem (TodoApi. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="e8638-246">**Veri bağlamı sınıfında** **TodoContext (TodoApi. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="e8638-247">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="e8638-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e8638-248">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e8638-249">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e8638-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="e8638-250">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="e8638-250">The preceding commands:</span></span>

* <span data-ttu-id="e8638-251">Yapı iskelesi için gereken NuGet paketlerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e8638-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="e8638-252">Scafkatlama altyapısını (`dotnet-aspnet-codegenerator`) kurar.</span><span class="sxs-lookup"><span data-stu-id="e8638-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="e8638-253">`TodoItemsController`yapı iskelesi.</span><span class="sxs-lookup"><span data-stu-id="e8638-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="e8638-254">Oluşturulan kod:</span><span class="sxs-lookup"><span data-stu-id="e8638-254">The generated code:</span></span>

* <span data-ttu-id="e8638-255">Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e8638-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="e8638-256">Sınıfı [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle işaretler.</span><span class="sxs-lookup"><span data-stu-id="e8638-256">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="e8638-257">Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="e8638-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="e8638-258">Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz. <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="e8638-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="e8638-259">Veritabanı bağlamı eklemesine DI kullanır (`TodoContext`) içine denetleyici.</span><span class="sxs-lookup"><span data-stu-id="e8638-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="e8638-260">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="e8638-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="e8638-261">PostTodoItem Create metodunu inceleyin</span><span class="sxs-lookup"><span data-stu-id="e8638-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="e8638-262">`PostTodoItem` ' deki return ifadesini, [NameOf](/dotnet/csharp/language-reference/operators/nameof) işlecini kullanacak şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e8638-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="e8638-263">Yukarıdaki kod, [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliğiyle gösterildiği gıbı bır http post yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="e8638-263">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="e8638-264">Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="e8638-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="e8638-265"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e8638-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="e8638-266">Başarılı olursa bir HTTP 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="e8638-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="e8638-267">HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="e8638-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="e8638-268">Yanıta bir [konum](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) üst bilgisi ekler.</span><span class="sxs-lookup"><span data-stu-id="e8638-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="e8638-269">`Location` üstbilgisi, yeni oluşturulan Yapılacaklar öğesinin [URI](https://developer.mozilla.org/docs/Glossary/URI) 'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="e8638-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="e8638-270">Daha fazla bilgi için [10.2.2 201 oluşturuldu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="e8638-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="e8638-271">`Location` üst bilgisinin URI 'sini oluşturmak için `GetTodoItem` eyleme başvurur.</span><span class="sxs-lookup"><span data-stu-id="e8638-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="e8638-272">C# `nameof` anahtar sözcüğü, `CreatedAtAction` çağrısında eylem adının sabit kodlanmasını önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e8638-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="e8638-273">Postman yükleme</span><span class="sxs-lookup"><span data-stu-id="e8638-273">Install Postman</span></span>

<span data-ttu-id="e8638-274">Bu öğreticide Postman web API'si test etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e8638-274">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="e8638-275">Yükleme [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="e8638-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="e8638-276">Web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="e8638-276">Start the web app.</span></span>
* <span data-ttu-id="e8638-277">Postman'i başlatın.</span><span class="sxs-lookup"><span data-stu-id="e8638-277">Start Postman.</span></span>
* <span data-ttu-id="e8638-278">Devre dışı **SSL sertifika doğrulama**</span><span class="sxs-lookup"><span data-stu-id="e8638-278">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="e8638-279">**Dosya** > **ayarları** ' ndan (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="e8638-279">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="e8638-280">Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="e8638-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="e8638-281">Postman ile test PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="e8638-281">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="e8638-282">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e8638-282">Create a new request.</span></span>
* <span data-ttu-id="e8638-283">HTTP yöntemini `POST`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="e8638-283">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="e8638-284">Seçin **gövdesi** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="e8638-284">Select the **Body** tab.</span></span>
* <span data-ttu-id="e8638-285">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="e8638-285">Select the **raw** radio button.</span></span>
* <span data-ttu-id="e8638-286">Tür kümesine **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="e8638-286">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="e8638-287">İstek gövdesinde bir yapılacak iş öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="e8638-287">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="e8638-288">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-288">Select **Send**.</span></span>

  ![Postman ile isteği oluştur](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="e8638-290">Konum üst bilgisi URI test</span><span class="sxs-lookup"><span data-stu-id="e8638-290">Test the location header URI</span></span>

* <span data-ttu-id="e8638-291">Seçin **üstbilgileri** sekmesinde **yanıt** bölmesi.</span><span class="sxs-lookup"><span data-stu-id="e8638-291">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="e8638-292">Kopyalama **konumu** üst bilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="e8638-292">Copy the **Location** header value:</span></span>

  ![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/3/create.png)

* <span data-ttu-id="e8638-294">Yöntemini GET öğesine Ayarla.</span><span class="sxs-lookup"><span data-stu-id="e8638-294">Set the method to GET.</span></span>
* <span data-ttu-id="e8638-295">URI 'yi yapıştırın (örneğin, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="e8638-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="e8638-296">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="e8638-297">GET yöntemlerini inceleyin</span><span class="sxs-lookup"><span data-stu-id="e8638-297">Examine the GET methods</span></span>

<span data-ttu-id="e8638-298">İki GET uç noktası bu yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="e8638-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="e8638-299">Tarayıcıdan veya Postman 'dan iki uç noktayı çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="e8638-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="e8638-300">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e8638-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="e8638-301">Aşağıdakine benzer bir yanıt, `GetTodoItems`çağrısı tarafından üretilir:</span><span class="sxs-lookup"><span data-stu-id="e8638-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="e8638-302">Postman ile test al</span><span class="sxs-lookup"><span data-stu-id="e8638-302">Test Get with Postman</span></span>

* <span data-ttu-id="e8638-303">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e8638-303">Create a new request.</span></span>
* <span data-ttu-id="e8638-304">HTTP yöntemi kümesine **alma**.</span><span class="sxs-lookup"><span data-stu-id="e8638-304">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="e8638-305">İstek URL'si kümesine `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="e8638-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="e8638-306">Örneğin: `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="e8638-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="e8638-307">Ayarlama **iki bölme görünümü** postman'deki.</span><span class="sxs-lookup"><span data-stu-id="e8638-307">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="e8638-308">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-308">Select **Send**.</span></span>

<span data-ttu-id="e8638-309">Bu uygulama, bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="e8638-309">This app uses an in-memory database.</span></span> <span data-ttu-id="e8638-310">Uygulama durdurulup başlatılırsa, önceki GET isteği herhangi bir veri döndürmez.</span><span class="sxs-lookup"><span data-stu-id="e8638-310">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="e8638-311">Hiçbir veri döndürülmezse, verileri uygulamaya [gönderin](#post) .</span><span class="sxs-lookup"><span data-stu-id="e8638-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="e8638-312">URL Yönlendirme ve yolları</span><span class="sxs-lookup"><span data-stu-id="e8638-312">Routing and URL paths</span></span>

<span data-ttu-id="e8638-313">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Özniteliği bir HTTP GET isteğine yanıt vermeden bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="e8638-313">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="e8638-314">Her yöntem için URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="e8638-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e8638-315">Denetleyicinin şablonu dizesi ile başlayıp `Route` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="e8638-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="e8638-316">Değiştirin `[controller]` denetleyicinin adı ile kural tarafından olduğu "Controller" soneki eksi denetleyici sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="e8638-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e8638-317">Bu örnek için denetleyici sınıfı adı **todoıtems**denetleyicisidir, bu nedenle denetleyicinin adı "todoıtems" olur.</span><span class="sxs-lookup"><span data-stu-id="e8638-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="e8638-318">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="e8638-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="e8638-319">`[HttpGet]` özniteliğinin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e8638-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="e8638-320">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="e8638-320">This sample doesn't use a template.</span></span> <span data-ttu-id="e8638-321">Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="e8638-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e8638-322">Aşağıdaki `GetTodoItem` yöntemi `"{id}"` yapılacak iş öğesi benzersiz tanımlayıcısı için bir yer tutucu değişkendir.</span><span class="sxs-lookup"><span data-stu-id="e8638-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="e8638-323">`GetTodoItem` çağrıldığında, URL 'deki `"{id}"` değeri `id` parametresindeki yöntemine sağlanır.</span><span class="sxs-lookup"><span data-stu-id="e8638-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="e8638-324">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="e8638-324">Return values</span></span>

<span data-ttu-id="e8638-325">Dönüş türünü `GetTodoItems` ve `GetTodoItem` yöntemler [actionresult öğesini\<T > türü](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="e8638-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="e8638-326">ASP.NET Core, nesneyi otomatik olarak serileştiren [JSON](https://www.json.org/) ve yanıt iletisinin gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="e8638-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e8638-327">Yanıt kodu 200 bu dönüş türü için olduğu varsayılırsa işlenmeyen özel durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="e8638-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e8638-328">İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.</span><span class="sxs-lookup"><span data-stu-id="e8638-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="e8638-329">`ActionResult` dönüş türleri, geniş HTTP durum kodları temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="e8638-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="e8638-330">Örneğin, `GetTodoItem` iki farklı durum değerleri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="e8638-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="e8638-331">Öğe istenen kimliği eşleşirse, yöntem bir 404 döndürür [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu.</span><span class="sxs-lookup"><span data-stu-id="e8638-331">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="e8638-332">Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="e8638-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e8638-333">Döndüren `item` sonuçları bir HTTP 200 yanıtı.</span><span class="sxs-lookup"><span data-stu-id="e8638-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="e8638-334">PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="e8638-334">The PutTodoItem method</span></span>

<span data-ttu-id="e8638-335">`PutTodoItem` yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="e8638-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="e8638-336">`PutTodoItem` benzer `PostTodoItem`, HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="e8638-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="e8638-337">Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e8638-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e8638-338">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca değişiklikler değil göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e8638-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="e8638-339">Kısmi güncelleştirmeleri desteklemek için kullanma [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="e8638-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="e8638-340">`PutTodoItem`çağırırken bir hata alırsanız, veritabanında bir öğe olduğundan emin olmak için `GET` çağırın.</span><span class="sxs-lookup"><span data-stu-id="e8638-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="e8638-341">Test PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="e8638-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="e8638-342">Bu örnek, uygulama her başlatıldığında başlatılmış olması gereken bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="e8638-342">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="e8638-343">Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e8638-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="e8638-344">PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="e8638-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="e8638-345">ID = 1 olan Yapılacaklar öğesini güncelleştirin ve adını "Feed balık" olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="e8638-345">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="e8638-346">Aşağıdaki görüntüde, Postman güncelleştirme gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="e8638-346">The following image shows the Postman update:</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="e8638-348">DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="e8638-348">The DeleteTodoItem method</span></span>

<span data-ttu-id="e8638-349">`DeleteTodoItem` yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="e8638-349">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="e8638-350">Test DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="e8638-350">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="e8638-351">Postman bir yapılacak iş öğesini silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="e8638-351">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="e8638-352">Yöntem kümesine `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="e8638-352">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="e8638-353">Silinecek nesnenin URI 'sini ayarlayın (örneğin `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="e8638-353">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="e8638-354">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-354">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="e8638-355">JavaScript ile Web API 'sini çağırma</span><span class="sxs-lookup"><span data-stu-id="e8638-355">Call the web API with JavaScript</span></span>

<span data-ttu-id="e8638-356">Bkz. [öğretici: JavaScript ile ASP.NET Core Web API 'Si çağırma](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="e8638-356">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e8638-357">Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="e8638-357">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e8638-358">Bir Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e8638-358">Create a web API project.</span></span>
> * <span data-ttu-id="e8638-359">Bir model sınıfı ve bir veritabanı bağlamı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e8638-359">Add a model class and a database context.</span></span>
> * <span data-ttu-id="e8638-360">Bir denetleyici ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="e8638-360">Add a controller.</span></span>
> * <span data-ttu-id="e8638-361">CRUD yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e8638-361">Add CRUD methods.</span></span>
> * <span data-ttu-id="e8638-362">Yönlendirmeyi Yapılandırma ve URL yolu.</span><span class="sxs-lookup"><span data-stu-id="e8638-362">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="e8638-363">Dönüş değerleri belirtin.</span><span class="sxs-lookup"><span data-stu-id="e8638-363">Specify return values.</span></span>
> * <span data-ttu-id="e8638-364">Web API'si Postman ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="e8638-364">Call the web API with Postman.</span></span>
> * <span data-ttu-id="e8638-365">JavaScript ile Web API 'sini çağırın.</span><span class="sxs-lookup"><span data-stu-id="e8638-365">Call the web API with JavaScript.</span></span>

<span data-ttu-id="e8638-366">Sonunda, web API'si "Yapılacaklar" öğelerini ilişkisel bir veritabanında depolanan yönetebileceği sahip.</span><span class="sxs-lookup"><span data-stu-id="e8638-366">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="e8638-367">Genel bakış</span><span class="sxs-lookup"><span data-stu-id="e8638-367">Overview</span></span>

<span data-ttu-id="e8638-368">Bu öğretici yandaki API oluşturur:</span><span class="sxs-lookup"><span data-stu-id="e8638-368">This tutorial creates the following API:</span></span>

|<span data-ttu-id="e8638-369">API</span><span class="sxs-lookup"><span data-stu-id="e8638-369">API</span></span> | <span data-ttu-id="e8638-370">Açıklama</span><span class="sxs-lookup"><span data-stu-id="e8638-370">Description</span></span> | <span data-ttu-id="e8638-371">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="e8638-371">Request body</span></span> | <span data-ttu-id="e8638-372">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="e8638-372">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="e8638-373">/Api/TodoItems al</span><span class="sxs-lookup"><span data-stu-id="e8638-373">GET /api/TodoItems</span></span> | <span data-ttu-id="e8638-374">Tüm yapılacak iş öğeleri al</span><span class="sxs-lookup"><span data-stu-id="e8638-374">Get all to-do items</span></span> | <span data-ttu-id="e8638-375">Yok.</span><span class="sxs-lookup"><span data-stu-id="e8638-375">None</span></span> | <span data-ttu-id="e8638-376">Yapılacaklar öğelerinin bir dizisi</span><span class="sxs-lookup"><span data-stu-id="e8638-376">Array of to-do items</span></span>|
|<span data-ttu-id="e8638-377">/Api/TodoItems/{id} al</span><span class="sxs-lookup"><span data-stu-id="e8638-377">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="e8638-378">Bir öğeyi Kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="e8638-378">Get an item by ID</span></span> | <span data-ttu-id="e8638-379">Yok.</span><span class="sxs-lookup"><span data-stu-id="e8638-379">None</span></span> | <span data-ttu-id="e8638-380">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="e8638-380">To-do item</span></span>|
|<span data-ttu-id="e8638-381">POST/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="e8638-381">POST /api/TodoItems</span></span> | <span data-ttu-id="e8638-382">Yeni Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="e8638-382">Add a new item</span></span> | <span data-ttu-id="e8638-383">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="e8638-383">To-do item</span></span> | <span data-ttu-id="e8638-384">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="e8638-384">To-do item</span></span> |
|<span data-ttu-id="e8638-385">/Api/TodoItems/{id} koy</span><span class="sxs-lookup"><span data-stu-id="e8638-385">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="e8638-386">Mevcut öğeyi güncelleştirin &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e8638-386">Update an existing item &nbsp;</span></span> | <span data-ttu-id="e8638-387">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="e8638-387">To-do item</span></span> | <span data-ttu-id="e8638-388">Yok.</span><span class="sxs-lookup"><span data-stu-id="e8638-388">None</span></span> |
|<span data-ttu-id="e8638-389">/Api/TodoItems/{id} &nbsp; SIL &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e8638-389">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="e8638-390">Öğeyi Sil &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e8638-390">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="e8638-391">Yok.</span><span class="sxs-lookup"><span data-stu-id="e8638-391">None</span></span> | <span data-ttu-id="e8638-392">Yok.</span><span class="sxs-lookup"><span data-stu-id="e8638-392">None</span></span>|

<span data-ttu-id="e8638-393">Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="e8638-393">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki bir kutu ile temsil edilir.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="e8638-399">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="e8638-399">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8638-400">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-400">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e8638-401">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e8638-401">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e8638-402">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-402">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="e8638-403">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e8638-403">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8638-404">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-404">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e8638-405">**Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-405">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e8638-406">**ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e8638-406">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="e8638-407">Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e8638-407">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="e8638-408">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 2,2** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="e8638-408">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="e8638-409">**API** şablonunu seçin ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e8638-409">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="e8638-410">**Docker desteğini etkinleştir** **' i seçmeyin** .</span><span class="sxs-lookup"><span data-stu-id="e8638-410">**Don't** select **Enable Docker Support**.</span></span>

![VS yeni proje iletişim kutusu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e8638-412">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e8638-412">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e8638-413">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e8638-413">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e8638-414">Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="e8638-414">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="e8638-415">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e8638-415">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="e8638-416">Bu komutlar yeni bir Web API projesi oluşturur ve yeni proje klasöründe Visual Studio Code yeni bir örneğini açar.</span><span class="sxs-lookup"><span data-stu-id="e8638-416">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="e8638-417">Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-417">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e8638-418">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-418">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e8638-419">**Yeni çözüm**> **Dosya** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-419">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="e8638-421">**.NET Core** > **App** > **API** > ' **yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-421">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="e8638-423">İçinde **, yeni ASP.NET Core Web API'sini yapılandırma** iletişim kutusunda varsayılan değerleri kabul **hedef Framework'ü** , \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="e8638-423">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="e8638-424">Girin *TodoApi* için **proje adı** seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="e8638-424">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="e8638-426">API'yi test etme</span><span class="sxs-lookup"><span data-stu-id="e8638-426">Test the API</span></span>

<span data-ttu-id="e8638-427">Proje şablonu oluşturur bir `values` API.</span><span class="sxs-lookup"><span data-stu-id="e8638-427">The project template creates a `values` API.</span></span> <span data-ttu-id="e8638-428">Çağrı `Get` uygulamayı test etmek için bir tarayıcıdan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e8638-428">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8638-429">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-429">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e8638-430">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="e8638-430">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e8638-431">Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>/api/values`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="e8638-431">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="e8638-432">IIS Express sertifika güven varsa soran bir iletişim kutusu alırsanız seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="e8638-432">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="e8638-433">İçinde **Güvenlik Uyarısı** ardından, görüntülenen iletişim seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="e8638-433">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e8638-434">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e8638-434">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e8638-435">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="e8638-435">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e8638-436">Bir tarayıcıda aşağıdaki URL'ye gidin: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="e8638-436">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e8638-437">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-437">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e8638-438">Uygulamayı başlatmak için **hata ayıklamayı başlat** > **Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-438">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="e8638-439">Mac için Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="e8638-439">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="e8638-440">HTTP 404 (bulunamadı) hatası döndürülür.</span><span class="sxs-lookup"><span data-stu-id="e8638-440">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="e8638-441">Append `/api/values` URL'sine (URL'yi `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="e8638-441">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="e8638-442">Aşağıdaki JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="e8638-442">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="e8638-443">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="e8638-443">Add a model class</span></span>

<span data-ttu-id="e8638-444">A *modeli* uygulamayı yöneten verilerini temsil eden sınıflar kümesidir.</span><span class="sxs-lookup"><span data-stu-id="e8638-444">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="e8638-445">Tek bir modeldir bu uygulama için `TodoItem` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e8638-445">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8638-446">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-446">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e8638-447">İçinde **Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e8638-447">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="e8638-448">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="e8638-448">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e8638-449">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="e8638-449">Name the folder *Models*.</span></span>

* <span data-ttu-id="e8638-450">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="e8638-450">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e8638-451">Sınıf adı *Todoıtem* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="e8638-451">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="e8638-452">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e8638-452">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e8638-453">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e8638-453">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e8638-454">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="e8638-454">Add a folder named *Models*.</span></span>

* <span data-ttu-id="e8638-455">Ekleme bir `TodoItem` sınıfının *modelleri* aşağıdaki kodla klasörü:</span><span class="sxs-lookup"><span data-stu-id="e8638-455">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e8638-456">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-456">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e8638-457">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e8638-457">Right-click the project.</span></span> <span data-ttu-id="e8638-458">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="e8638-458">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e8638-459">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="e8638-459">Name the folder *Models*.</span></span>

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="e8638-461">*Modeller* klasörüne sağ tıklayın ve > **yeni dosya** **ekle** ' yi > **genel** > **boş sınıf**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-461">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="e8638-462">Sınıf adı *Todoıtem*ve ardından **yeni**.</span><span class="sxs-lookup"><span data-stu-id="e8638-462">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="e8638-463">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e8638-463">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="e8638-464">`Id` Özelliği işlevlerinin bir ilişkisel veritabanında benzersiz anahtar.</span><span class="sxs-lookup"><span data-stu-id="e8638-464">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="e8638-465">Model sınıfları herhangi bir projede gidip ancak *modelleri* klasörü, kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e8638-465">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="e8638-466">Veritabanı bağlamı Ekle</span><span class="sxs-lookup"><span data-stu-id="e8638-466">Add a database context</span></span>

<span data-ttu-id="e8638-467">*Veritabanı bağlamı* koordine eden bir veri modeli için Entity Framework işlevsellik ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="e8638-467">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="e8638-468">Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e8638-468">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8638-469">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-469">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e8638-470">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="e8638-470">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e8638-471">Sınıf adı *TodoContext* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="e8638-471">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e8638-472">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-472">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e8638-473">Ekleme bir `TodoContext` sınıfının *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="e8638-473">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="e8638-474">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e8638-474">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="e8638-475">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="e8638-475">Register the database context</span></span>

<span data-ttu-id="e8638-476">ASP.NET Core DB bağlamı gibi hizmetler ile kaydedilmelidir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="e8638-476">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e8638-477">Kapsayıcı hizmeti denetleyicilerine sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8638-477">The container provides the service to controllers.</span></span>

<span data-ttu-id="e8638-478">Güncelleştirme *Startup.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="e8638-478">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="e8638-479">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="e8638-479">The preceding code:</span></span>

* <span data-ttu-id="e8638-480">Kullanılmayan kaldırır `using` bildirimleri.</span><span class="sxs-lookup"><span data-stu-id="e8638-480">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="e8638-481">Veritabanı bağlamı DI kapsayıcıya ekler.</span><span class="sxs-lookup"><span data-stu-id="e8638-481">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="e8638-482">Veritabanı bağlamı bir bellek içi veritabanına kullanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="e8638-482">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="e8638-483">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="e8638-483">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8638-484">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-484">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e8638-485">Sağ *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="e8638-485">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="e8638-486">> **Yeni öğe** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-486">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="e8638-487">İçinde **Yeni Öğe Ekle** iletişim kutusunda **API denetleyici sınıfı** şablonu.</span><span class="sxs-lookup"><span data-stu-id="e8638-487">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="e8638-488">Sınıf adı *TodoController*seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="e8638-488">Name the class *TodoController*, and select **Add**.</span></span>

  ![Yeni öğe iletişim denetleyicisiyle seçilen arama kutusu ve web API denetleyicisi Ekle](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e8638-490">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-490">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e8638-491">İçinde *denetleyicileri* klasör adında bir sınıf oluşturma `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="e8638-491">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="e8638-492">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e8638-492">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="e8638-493">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="e8638-493">The preceding code:</span></span>

* <span data-ttu-id="e8638-494">Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e8638-494">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="e8638-495">Sınıfı [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle işaretler.</span><span class="sxs-lookup"><span data-stu-id="e8638-495">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="e8638-496">Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="e8638-496">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="e8638-497">Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz. <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="e8638-497">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="e8638-498">Veritabanı bağlamı eklemesine DI kullanır (`TodoContext`) içine denetleyici.</span><span class="sxs-lookup"><span data-stu-id="e8638-498">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="e8638-499">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="e8638-499">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="e8638-500">Adlı bir öğe ekler `Item1` veritabanı boşsa veritabanı.</span><span class="sxs-lookup"><span data-stu-id="e8638-500">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="e8638-501">Her çalıştığında bu kod oluşturucusunun içinde yeni bir HTTP isteği olduğundan.</span><span class="sxs-lookup"><span data-stu-id="e8638-501">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="e8638-502">Tüm öğeleri silerseniz, oluşturucu oluşturur `Item1` API yöntemi çağrıldığında tekrar başlattığınızda.</span><span class="sxs-lookup"><span data-stu-id="e8638-502">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="e8638-503">Bu nedenle, gerçekten işe yaradı silme işlemi işe yaramadı gibi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="e8638-503">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="e8638-504">Get yöntemleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="e8638-504">Add Get methods</span></span>

<span data-ttu-id="e8638-505">Yapılacak iş öğeleri alır bir API sağlamak için aşağıdaki yöntemi ekleyin. `TodoController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="e8638-505">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="e8638-506">İki GET uç noktası bu yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="e8638-506">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="e8638-507">Hala çalışıyorsa uygulamayı durdurun.</span><span class="sxs-lookup"><span data-stu-id="e8638-507">Stop the app if it's still running.</span></span> <span data-ttu-id="e8638-508">Ardından, en son değişiklikleri dahil etmek için yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e8638-508">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="e8638-509">Bir tarayıcıdan iki uç nokta çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="e8638-509">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="e8638-510">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e8638-510">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="e8638-511">Şu HTTP yanıtı çağrısı tarafından üretilen `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="e8638-511">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="e8638-512">URL Yönlendirme ve yolları</span><span class="sxs-lookup"><span data-stu-id="e8638-512">Routing and URL paths</span></span>

<span data-ttu-id="e8638-513">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Özniteliği bir HTTP GET isteğine yanıt vermeden bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="e8638-513">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="e8638-514">Her yöntem için URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="e8638-514">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e8638-515">Denetleyicinin şablonu dizesi ile başlayıp `Route` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="e8638-515">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="e8638-516">Değiştirin `[controller]` denetleyicinin adı ile kural tarafından olduğu "Controller" soneki eksi denetleyici sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="e8638-516">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e8638-517">Bu örnek, denetleyici sınıfı adı olan **Todo**Denetleyici adı "todo" Bu nedenle denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="e8638-517">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="e8638-518">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="e8638-518">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="e8638-519">`[HttpGet]` özniteliğinin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e8638-519">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="e8638-520">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="e8638-520">This sample doesn't use a template.</span></span> <span data-ttu-id="e8638-521">Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="e8638-521">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e8638-522">Aşağıdaki `GetTodoItem` yöntemi `"{id}"` yapılacak iş öğesi benzersiz tanımlayıcısı için bir yer tutucu değişkendir.</span><span class="sxs-lookup"><span data-stu-id="e8638-522">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="e8638-523">Zaman `GetTodoItem` çağrılır, değerini `"{id}"` yöntemine URL'de sağlanan kendi`id` parametresi.</span><span class="sxs-lookup"><span data-stu-id="e8638-523">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="e8638-524">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="e8638-524">Return values</span></span>

<span data-ttu-id="e8638-525">Dönüş türünü `GetTodoItems` ve `GetTodoItem` yöntemler [actionresult öğesini\<T > türü](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="e8638-525">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="e8638-526">ASP.NET Core, nesneyi otomatik olarak serileştiren [JSON](https://www.json.org/) ve yanıt iletisinin gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="e8638-526">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e8638-527">Yanıt kodu 200 bu dönüş türü için olduğu varsayılırsa işlenmeyen özel durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="e8638-527">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e8638-528">İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.</span><span class="sxs-lookup"><span data-stu-id="e8638-528">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="e8638-529">`ActionResult` dönüş türleri, geniş HTTP durum kodları temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="e8638-529">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="e8638-530">Örneğin, `GetTodoItem` iki farklı durum değerleri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="e8638-530">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="e8638-531">Öğe istenen kimliği eşleşirse, yöntem bir 404 döndürür [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu.</span><span class="sxs-lookup"><span data-stu-id="e8638-531">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="e8638-532">Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="e8638-532">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e8638-533">Döndüren `item` sonuçları bir HTTP 200 yanıtı.</span><span class="sxs-lookup"><span data-stu-id="e8638-533">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="e8638-534">Test GetTodoItems yöntemi</span><span class="sxs-lookup"><span data-stu-id="e8638-534">Test the GetTodoItems method</span></span>

<span data-ttu-id="e8638-535">Bu öğreticide Postman web API'si test etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e8638-535">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="e8638-536">[Postman](https://www.getpostman.com/downloads/)'yi yükleme.</span><span class="sxs-lookup"><span data-stu-id="e8638-536">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="e8638-537">Web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="e8638-537">Start the web app.</span></span>
* <span data-ttu-id="e8638-538">Postman'i başlatın.</span><span class="sxs-lookup"><span data-stu-id="e8638-538">Start Postman.</span></span>
* <span data-ttu-id="e8638-539">**SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="e8638-539">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8638-540">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-540">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e8638-541">**Dosya** > **ayarları** ' ndan (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="e8638-541">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e8638-542">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8638-542">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e8638-543">**Postman** > **tercihleri** ' nden (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="e8638-543">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="e8638-544">Alternatif olarak, wranı seçin ve **Ayarlar**' ı seçip SSL sertifikası doğrulamasını devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="e8638-544">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="e8638-545">Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="e8638-545">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="e8638-546">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e8638-546">Create a new request.</span></span>
  * <span data-ttu-id="e8638-547">HTTP yöntemi kümesine **alma**.</span><span class="sxs-lookup"><span data-stu-id="e8638-547">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="e8638-548">İstek URL'si kümesine `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="e8638-548">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="e8638-549">Örneğin: `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="e8638-549">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="e8638-550">Ayarlama **iki bölme görünümü** postman'deki.</span><span class="sxs-lookup"><span data-stu-id="e8638-550">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="e8638-551">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-551">Select **Send**.</span></span>

![Get isteğiyle postman](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="e8638-553">Create yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="e8638-553">Add a Create method</span></span>

<span data-ttu-id="e8638-554">Aşağıdaki `PostTodoItem` yöntemini *Controllers/TodoController. cs*içine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e8638-554">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="e8638-555">Yukarıdaki kod, [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliğiyle gösterildiği gıbı bır http post yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="e8638-555">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="e8638-556">Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="e8638-556">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="e8638-557">`CreatedAtAction` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e8638-557">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="e8638-558">Başarılı olursa bir HTTP 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="e8638-558">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="e8638-559">HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="e8638-559">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="e8638-560">Yanıta bir `Location` üst bilgisi ekler.</span><span class="sxs-lookup"><span data-stu-id="e8638-560">Adds a `Location` header to the response.</span></span> <span data-ttu-id="e8638-561">`Location` üstbilgisi, yeni oluşturulan Yapılacaklar öğesinin URI 'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="e8638-561">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="e8638-562">Daha fazla bilgi için [10.2.2 201 oluşturuldu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="e8638-562">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="e8638-563">`Location` üst bilgisinin URI 'sini oluşturmak için `GetTodoItem` eyleme başvurur.</span><span class="sxs-lookup"><span data-stu-id="e8638-563">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="e8638-564">C# `nameof` anahtar sözcüğü, `CreatedAtAction` çağrısında eylem adının sabit kodlanmasını önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e8638-564">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="e8638-565">Test PostTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="e8638-565">Test the PostTodoItem method</span></span>

* <span data-ttu-id="e8638-566">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e8638-566">Build the project.</span></span>
* <span data-ttu-id="e8638-567">Postman HTTP yöntemi kümesine `POST`.</span><span class="sxs-lookup"><span data-stu-id="e8638-567">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="e8638-568">Seçin **gövdesi** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="e8638-568">Select the **Body** tab.</span></span>
* <span data-ttu-id="e8638-569">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="e8638-569">Select the **raw** radio button.</span></span>
* <span data-ttu-id="e8638-570">Tür kümesine **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="e8638-570">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="e8638-571">İstek gövdesinde bir yapılacak iş öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="e8638-571">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="e8638-572">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-572">Select **Send**.</span></span>

  ![Postman ile isteği oluştur](first-web-api/_static/create.png)

  <span data-ttu-id="e8638-574">Bir 405 yöntemine Izin verilmiyor hatası alırsanız, `PostTodoItem` yöntemi eklendikten sonra projenin derlenmesinin sonucu büyük olasılıkla oluşur.</span><span class="sxs-lookup"><span data-stu-id="e8638-574">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="e8638-575">Konum üst bilgisi URI test</span><span class="sxs-lookup"><span data-stu-id="e8638-575">Test the location header URI</span></span>

* <span data-ttu-id="e8638-576">Seçin **üstbilgileri** sekmesinde **yanıt** bölmesi.</span><span class="sxs-lookup"><span data-stu-id="e8638-576">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="e8638-577">Kopyalama **konumu** üst bilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="e8638-577">Copy the **Location** header value:</span></span>

  ![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmc2.png)

* <span data-ttu-id="e8638-579">Yöntemini GET öğesine Ayarla.</span><span class="sxs-lookup"><span data-stu-id="e8638-579">Set the method to GET.</span></span>
* <span data-ttu-id="e8638-580">URI 'yi yapıştırın (örneğin, `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="e8638-580">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="e8638-581">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-581">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="e8638-582">PutTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="e8638-582">Add a PutTodoItem method</span></span>

<span data-ttu-id="e8638-583">Aşağıdaki `PutTodoItem` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e8638-583">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="e8638-584">`PutTodoItem` benzer `PostTodoItem`, HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="e8638-584">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="e8638-585">Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e8638-585">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e8638-586">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca değişiklikler değil göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e8638-586">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="e8638-587">Kısmi güncelleştirmeleri desteklemek için kullanma [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="e8638-587">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="e8638-588">`PutTodoItem`çağırırken bir hata alırsanız, veritabanında bir öğe olduğundan emin olmak için `GET` çağırın.</span><span class="sxs-lookup"><span data-stu-id="e8638-588">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="e8638-589">Test PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="e8638-589">Test the PutTodoItem method</span></span>

<span data-ttu-id="e8638-590">Bu örnek, uygulama her başlatıldığında başlatılmış olması gereken bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="e8638-590">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="e8638-591">Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e8638-591">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="e8638-592">PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="e8638-592">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="e8638-593">Kimliğine sahip bir yapılacak iş öğesi güncelleştirme = 1 ve "balık akış için" adını girin:</span><span class="sxs-lookup"><span data-stu-id="e8638-593">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="e8638-594">Aşağıdaki görüntüde, Postman güncelleştirme gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="e8638-594">The following image shows the Postman update:</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="e8638-596">DeleteTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="e8638-596">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="e8638-597">Aşağıdaki `DeleteTodoItem` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e8638-597">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="e8638-598">`DeleteTodoItem` Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e8638-598">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="e8638-599">Test DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="e8638-599">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="e8638-600">Postman bir yapılacak iş öğesini silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="e8638-600">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="e8638-601">Yöntem kümesine `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="e8638-601">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="e8638-602">Silinecek nesnenin URI 'sini ayarlayın (örneğin `https://localhost:5001/api/todo/1`).</span><span class="sxs-lookup"><span data-stu-id="e8638-602">Set the URI of the object to delete (for example `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="e8638-603">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="e8638-603">Select **Send**.</span></span>

<span data-ttu-id="e8638-604">Örnek uygulama, tüm öğeleri silmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8638-604">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="e8638-605">Ancak, son öğe silindiğinde, API 'nin bir sonraki çağrılışında model sınıfı Oluşturucu tarafından yeni bir tane oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e8638-605">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="e8638-606">JavaScript ile Web API 'sini çağırma</span><span class="sxs-lookup"><span data-stu-id="e8638-606">Call the web API with JavaScript</span></span>

<span data-ttu-id="e8638-607">Bu bölümde, Web API 'sini çağırmak için JavaScript kullanan bir HTML sayfası eklenir.</span><span class="sxs-lookup"><span data-stu-id="e8638-607">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="e8638-608">jQuery isteği başlatır.</span><span class="sxs-lookup"><span data-stu-id="e8638-608">jQuery initiates the request.</span></span> <span data-ttu-id="e8638-609">JavaScript, sayfayı Web API 'sinin yanıtından alınan ayrıntılarla güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="e8638-609">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="e8638-610">Uygulamayı [statik dosyalara sunacak](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) şekilde yapılandırın ve aşağıdaki vurgulanmış kodla *Startup.cs* güncelleştirerek [varsayılan dosya eşlemesini etkinleştirin](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) :</span><span class="sxs-lookup"><span data-stu-id="e8638-610">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="e8638-611">Oluşturma bir *wwwroot* proje dizininde klasör.</span><span class="sxs-lookup"><span data-stu-id="e8638-611">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="e8638-612">Adlı bir HTML dosyası ekleyin *index.html* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="e8638-612">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="e8638-613">Dosyanın içeriğini aşağıdaki biçimlendirme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e8638-613">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="e8638-614">Adlı bir JavaScript dosyası ekleyin *site.js* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="e8638-614">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="e8638-615">Dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e8638-615">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="e8638-616">ASP.NET Core proje başlatma ayarlarında bir değişiklik HTML sayfasını yerel olarak test etmek için gerekli:</span><span class="sxs-lookup"><span data-stu-id="e8638-616">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="e8638-617">Açık *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e8638-617">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="e8638-618">Kaldırma `launchUrl` , açmak için uygulamayı zorlamak için özellik *index.html*&mdash;projenin varsayılan dosya.</span><span class="sxs-lookup"><span data-stu-id="e8638-618">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="e8638-619">Bu örnek, Web API 'sinin tüm CRUD yöntemlerini çağırır.</span><span class="sxs-lookup"><span data-stu-id="e8638-619">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="e8638-620">API çağrıları açıklamaları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e8638-620">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="e8638-621">Yapılacaklar öğelerinin bir listesini alın</span><span class="sxs-lookup"><span data-stu-id="e8638-621">Get a list of to-do items</span></span>

<span data-ttu-id="e8638-622">jQuery, bir to-do öğesi dizisini temsil eden JSON döndüren Web API 'sine bir HTTP GET isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="e8638-622">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="e8638-623">`success` İstek başarılı olursa geri çağırma işlevi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="e8638-623">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="e8638-624">Geri çağırma içinde DOM Yapılacaklar bilgilerle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e8638-624">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="e8638-625">Yapılacak İş Öğesi Ekle</span><span class="sxs-lookup"><span data-stu-id="e8638-625">Add a to-do item</span></span>

<span data-ttu-id="e8638-626">jQuery, istek gövdesinde Yapılacaklar öğesiyle bir HTTP POST isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="e8638-626">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="e8638-627">`accepts` Ve `contentType` seçeneklerini ayarlamak `application/json` gönderilen ve alınan medya türü belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="e8638-627">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="e8638-628">Yapılacak iş öğesi kullanarak JSON'a dönüştürülür [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="e8638-628">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="e8638-629">API'yi bir başarılı durum kodu döndürdüğünde `getData` işlevi HTML tablosu güncelleştirmek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="e8638-629">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="e8638-630">Yapılacak iş öğesini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="e8638-630">Update a to-do item</span></span>

<span data-ttu-id="e8638-631">Yapılacak iş öğesi güncelleştirilirken bir eklemeye benzerdir.</span><span class="sxs-lookup"><span data-stu-id="e8638-631">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="e8638-632">`url` Öğenin benzersiz tanıtıcısı eklemek için değişiklikleri ve `type` olduğu `PUT`.</span><span class="sxs-lookup"><span data-stu-id="e8638-632">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="e8638-633">Yapılacak iş öğesi silme</span><span class="sxs-lookup"><span data-stu-id="e8638-633">Delete a to-do item</span></span>

<span data-ttu-id="e8638-634">Yapılacak iş öğesi silme gerçekleştirilir ayarlayarak `type` AJAX çağrısı hedefi üzerinde `DELETE` ve öğenin benzersiz tanıtıcısı URL'yi belirterek.</span><span class="sxs-lookup"><span data-stu-id="e8638-634">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="e8638-635">Web API 'sine kimlik doğrulama desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="e8638-635">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources"></a><span data-ttu-id="e8638-636">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e8638-636">Additional resources</span></span>

<span data-ttu-id="e8638-637">[Görüntülemek veya Bu öğretici için örnek kodu indirdikten](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="e8638-637">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="e8638-638">Bkz: [nasıl indirileceğini](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="e8638-638">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="e8638-639">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="e8638-639">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="e8638-640">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="e8638-640">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
