---
title: "Öğretici: ASP.NET Core bir Web API 'SI oluşturma"
author: rick-anderson
description: ASP.NET Core ile Web API 'SI oluşturmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 73e547b014d78dcbcbf1c887ebec16e0743d10b9
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294743"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="540f2-103">Öğretici: ASP.NET Core bir Web API 'SI oluşturma</span><span class="sxs-lookup"><span data-stu-id="540f2-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="540f2-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="540f2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="540f2-105">Bu öğretici, bir web API ASP.NET Core ile oluşturmaya ilişkin temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="540f2-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="540f2-106">Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="540f2-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="540f2-107">Bir Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="540f2-107">Create a web API project.</span></span>
> * <span data-ttu-id="540f2-108">Bir model sınıfı ve bir veritabanı bağlamı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="540f2-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="540f2-109">CRUD yöntemleriyle bir denetleyiciyi dolandırın.</span><span class="sxs-lookup"><span data-stu-id="540f2-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="540f2-110">Yönlendirmeyi, URL yollarını ve dönüş değerlerini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="540f2-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="540f2-111">Web API'si Postman ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="540f2-111">Call the web API with Postman.</span></span>

<span data-ttu-id="540f2-112">Sonunda, bir veritabanında depolanan "yapılacaklar" öğelerini yönetebilmek için bir Web API 'SI vardır.</span><span class="sxs-lookup"><span data-stu-id="540f2-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="540f2-113">Genel bakış</span><span class="sxs-lookup"><span data-stu-id="540f2-113">Overview</span></span>

<span data-ttu-id="540f2-114">Bu öğretici yandaki API oluşturur:</span><span class="sxs-lookup"><span data-stu-id="540f2-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="540f2-115">API</span><span class="sxs-lookup"><span data-stu-id="540f2-115">API</span></span> | <span data-ttu-id="540f2-116">Açıklama</span><span class="sxs-lookup"><span data-stu-id="540f2-116">Description</span></span> | <span data-ttu-id="540f2-117">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="540f2-117">Request body</span></span> | <span data-ttu-id="540f2-118">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="540f2-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="540f2-119">/Api/TodoItems al</span><span class="sxs-lookup"><span data-stu-id="540f2-119">GET /api/TodoItems</span></span> | <span data-ttu-id="540f2-120">Tüm yapılacak iş öğeleri al</span><span class="sxs-lookup"><span data-stu-id="540f2-120">Get all to-do items</span></span> | <span data-ttu-id="540f2-121">Yok.</span><span class="sxs-lookup"><span data-stu-id="540f2-121">None</span></span> | <span data-ttu-id="540f2-122">Yapılacaklar öğelerinin bir dizisi</span><span class="sxs-lookup"><span data-stu-id="540f2-122">Array of to-do items</span></span>|
|<span data-ttu-id="540f2-123">/Api/TodoItems/{id} al</span><span class="sxs-lookup"><span data-stu-id="540f2-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="540f2-124">Bir öğeyi Kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="540f2-124">Get an item by ID</span></span> | <span data-ttu-id="540f2-125">Yok.</span><span class="sxs-lookup"><span data-stu-id="540f2-125">None</span></span> | <span data-ttu-id="540f2-126">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="540f2-126">To-do item</span></span>|
|<span data-ttu-id="540f2-127">POST/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="540f2-127">POST /api/TodoItems</span></span> | <span data-ttu-id="540f2-128">Yeni Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="540f2-128">Add a new item</span></span> | <span data-ttu-id="540f2-129">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="540f2-129">To-do item</span></span> | <span data-ttu-id="540f2-130">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="540f2-130">To-do item</span></span> |
|<span data-ttu-id="540f2-131">/Api/TodoItems/{id} koy</span><span class="sxs-lookup"><span data-stu-id="540f2-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="540f2-132">Mevcut öğeyi güncelleştirin &nbsp;</span><span class="sxs-lookup"><span data-stu-id="540f2-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="540f2-133">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="540f2-133">To-do item</span></span> | <span data-ttu-id="540f2-134">Yok.</span><span class="sxs-lookup"><span data-stu-id="540f2-134">None</span></span> |
|<span data-ttu-id="540f2-135">/Api/TodoItems/{id} &nbsp; SIL &nbsp;</span><span class="sxs-lookup"><span data-stu-id="540f2-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="540f2-136">Öğeyi Sil &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="540f2-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="540f2-137">Yok.</span><span class="sxs-lookup"><span data-stu-id="540f2-137">None</span></span> | <span data-ttu-id="540f2-138">Yok.</span><span class="sxs-lookup"><span data-stu-id="540f2-138">None</span></span>|

<span data-ttu-id="540f2-139">Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="540f2-139">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki bir kutu ile temsil edilir.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="540f2-145">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="540f2-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540f2-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="540f2-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="540f2-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="540f2-148">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="540f2-149">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="540f2-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540f2-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="540f2-151">**Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="540f2-152">**ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="540f2-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="540f2-153">Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="540f2-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="540f2-154">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,1** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="540f2-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="540f2-155">**API** şablonunu seçin ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="540f2-155">Select the **API** template and click **Create**.</span></span>

![VS yeni proje iletişim kutusu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="540f2-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="540f2-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="540f2-158">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="540f2-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="540f2-159">Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="540f2-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="540f2-160">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="540f2-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="540f2-161">Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="540f2-162">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="540f2-162">The preceding commands:</span></span>

  * <span data-ttu-id="540f2-163">Yeni bir Web API projesi oluşturur ve Visual Studio Code açar.</span><span class="sxs-lookup"><span data-stu-id="540f2-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="540f2-164">Sonraki bölümde gerekli olan NuGet paketlerini ekler.</span><span class="sxs-lookup"><span data-stu-id="540f2-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="540f2-165">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="540f2-166">**Yeni çözüm**> **Dosya** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-166">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="540f2-168">**.NET Core** > **App** > **API** > ' **yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="540f2-170">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **hedef Framework** \* *.NET Core 3,1*' i seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.1*.</span></span>

* <span data-ttu-id="540f2-171">Girin *TodoApi* için **proje adı** seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="540f2-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="540f2-173">Proje klasöründe bir komut terminali açın ve aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="540f2-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="540f2-174">API'yi test etme</span><span class="sxs-lookup"><span data-stu-id="540f2-174">Test the API</span></span>

<span data-ttu-id="540f2-175">Proje şablonu oluşturur bir `WeatherForecast` API.</span><span class="sxs-lookup"><span data-stu-id="540f2-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="540f2-176">Çağrı `Get` uygulamayı test etmek için bir tarayıcıdan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="540f2-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540f2-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="540f2-178">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="540f2-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="540f2-179">Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>/WeatherForecast`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="540f2-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="540f2-180">IIS Express sertifika güven varsa soran bir iletişim kutusu alırsanız seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="540f2-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="540f2-181">İçinde **Güvenlik Uyarısı** ardından, görüntülenen iletişim seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="540f2-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="540f2-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="540f2-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="540f2-183">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="540f2-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="540f2-184">Bir tarayıcıda aşağıdaki URL'ye gidin: [ https://localhost:5001/WeatherForecast ](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="540f2-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="540f2-185">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="540f2-186">Uygulamayı başlatmak için **hata ayıklamayı başlat** > **Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="540f2-187">Mac için Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="540f2-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="540f2-188">HTTP 404 (bulunamadı) hatası döndürülür.</span><span class="sxs-lookup"><span data-stu-id="540f2-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="540f2-189">Append `/WeatherForecast` URL'sine (URL'yi `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="540f2-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="540f2-190">Aşağıdakine benzer bir JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="540f2-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="540f2-191">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="540f2-191">Add a model class</span></span>

<span data-ttu-id="540f2-192">A *modeli* uygulamayı yöneten verilerini temsil eden sınıflar kümesidir.</span><span class="sxs-lookup"><span data-stu-id="540f2-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="540f2-193">Tek bir modeldir bu uygulama için `TodoItem` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="540f2-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540f2-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="540f2-195">İçinde **Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="540f2-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="540f2-196">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="540f2-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="540f2-197">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="540f2-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="540f2-198">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="540f2-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="540f2-199">Sınıf adı *Todoıtem* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="540f2-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="540f2-200">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="540f2-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="540f2-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="540f2-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="540f2-202">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="540f2-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="540f2-203">Ekleme bir `TodoItem` sınıfının *modelleri* aşağıdaki kodla klasörü:</span><span class="sxs-lookup"><span data-stu-id="540f2-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="540f2-204">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="540f2-205">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="540f2-205">Right-click the project.</span></span> <span data-ttu-id="540f2-206">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="540f2-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="540f2-207">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="540f2-207">Name the folder *Models*.</span></span>

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="540f2-209">*Modeller* klasörüne sağ tıklayın ve > **yeni dosya** **ekle** ' yi > **genel** > **boş sınıf**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="540f2-210">Sınıf adı *Todoıtem*ve ardından **yeni**.</span><span class="sxs-lookup"><span data-stu-id="540f2-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="540f2-211">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="540f2-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="540f2-212">`Id` Özelliği işlevlerinin bir ilişkisel veritabanında benzersiz anahtar.</span><span class="sxs-lookup"><span data-stu-id="540f2-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="540f2-213">Model sınıfları herhangi bir projede gidip ancak *modelleri* klasörü, kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="540f2-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="540f2-214">Veritabanı bağlamı Ekle</span><span class="sxs-lookup"><span data-stu-id="540f2-214">Add a database context</span></span>

<span data-ttu-id="540f2-215">*Veritabanı bağlamı* koordine eden bir veri modeli için Entity Framework işlevsellik ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="540f2-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="540f2-216">Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="540f2-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540f2-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="540f2-218">Microsoft. EntityFrameworkCore. SqlServer ekleyin</span><span class="sxs-lookup"><span data-stu-id="540f2-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="540f2-219">**Araçlar** menüsünde **nuget Paket Yöneticisi > çözüm Için NuGet Paketlerini Yönet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="540f2-220">**Araştır** sekmesini seçin ve arama kutusuna **Microsoft. Entityframeworkcore. SqlServer** yazın.</span><span class="sxs-lookup"><span data-stu-id="540f2-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="540f2-221">Sol bölmedeki **Microsoft. EntityFrameworkCore. SqlServer** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="540f2-222">Sağ bölmedeki **Proje** onay kutusunu seçin ve ardından **Install**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="540f2-223">`Microsoft.EntityFrameworkCore.InMemory` NuGet paketini eklemek için yukarıdaki yönergeleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="540f2-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet Paket Yöneticisi](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="540f2-225">TodoContext veritabanı bağlamını ekleme</span><span class="sxs-lookup"><span data-stu-id="540f2-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="540f2-226">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="540f2-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="540f2-227">Sınıf adı *TodoContext* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="540f2-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="540f2-228">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="540f2-229">Ekleme bir `TodoContext` sınıfının *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="540f2-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="540f2-230">Aşağıdaki kodu girin:</span><span class="sxs-lookup"><span data-stu-id="540f2-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="540f2-231">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="540f2-231">Register the database context</span></span>

<span data-ttu-id="540f2-232">ASP.NET Core DB bağlamı gibi hizmetler ile kaydedilmelidir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="540f2-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="540f2-233">Kapsayıcı hizmeti denetleyicilerine sağlar.</span><span class="sxs-lookup"><span data-stu-id="540f2-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="540f2-234">Güncelleştirme *Startup.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="540f2-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="540f2-235">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="540f2-235">The preceding code:</span></span>

* <span data-ttu-id="540f2-236">Kullanılmayan kaldırır `using` bildirimleri.</span><span class="sxs-lookup"><span data-stu-id="540f2-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="540f2-237">Veritabanı bağlamı DI kapsayıcıya ekler.</span><span class="sxs-lookup"><span data-stu-id="540f2-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="540f2-238">Veritabanı bağlamı bir bellek içi veritabanına kullanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="540f2-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="540f2-239">Denetleyiciyi bir denetleyiciye katlama</span><span class="sxs-lookup"><span data-stu-id="540f2-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540f2-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="540f2-241">Sağ *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="540f2-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="540f2-242">> **yeni yapı Iskelesi öğesi** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="540f2-243">**Entity Framework kullanarak ve eylemler Içeren API denetleyicisi**' ni seçin ve ardından **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="540f2-244">**API denetleyiciyi eylemler Ile Ekle ' de Entity Framework** iletişim kutusunu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="540f2-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="540f2-245">**Model sınıfında** **TodoItem (TodoApi. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="540f2-246">**Veri bağlamı sınıfında** **TodoContext (TodoApi. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="540f2-247">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="540f2-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="540f2-248">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="540f2-249">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="540f2-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="540f2-250">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="540f2-250">The preceding commands:</span></span>

* <span data-ttu-id="540f2-251">Yapı iskelesi için gereken NuGet paketlerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="540f2-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="540f2-252">Scafkatlama altyapısını (`dotnet-aspnet-codegenerator`) kurar.</span><span class="sxs-lookup"><span data-stu-id="540f2-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="540f2-253">`TodoItemsController`yapı iskelesi.</span><span class="sxs-lookup"><span data-stu-id="540f2-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="540f2-254">Oluşturulan kod:</span><span class="sxs-lookup"><span data-stu-id="540f2-254">The generated code:</span></span>

* <span data-ttu-id="540f2-255">Sınıfı [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle işaretler.</span><span class="sxs-lookup"><span data-stu-id="540f2-255">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="540f2-256">Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="540f2-256">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="540f2-257">Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz. <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="540f2-257">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="540f2-258">Veritabanı bağlamı eklemesine DI kullanır (`TodoContext`) içine denetleyici.</span><span class="sxs-lookup"><span data-stu-id="540f2-258">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="540f2-259">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="540f2-259">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="540f2-260">PostTodoItem Create metodunu inceleyin</span><span class="sxs-lookup"><span data-stu-id="540f2-260">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="540f2-261">`PostTodoItem` ' deki return ifadesini, [NameOf](/dotnet/csharp/language-reference/operators/nameof) işlecini kullanacak şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="540f2-261">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="540f2-262">Yukarıdaki kod, [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliğiyle gösterildiği gıbı bır http post yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="540f2-262">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="540f2-263">Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="540f2-263">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="540f2-264"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="540f2-264">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="540f2-265">Başarılı olursa bir HTTP 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="540f2-265">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="540f2-266">HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="540f2-266">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="540f2-267">Yanıta bir [konum](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) üst bilgisi ekler.</span><span class="sxs-lookup"><span data-stu-id="540f2-267">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="540f2-268">`Location` üstbilgisi, yeni oluşturulan Yapılacaklar öğesinin [URI](https://developer.mozilla.org/docs/Glossary/URI) 'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="540f2-268">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="540f2-269">Daha fazla bilgi için [10.2.2 201 oluşturuldu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="540f2-269">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="540f2-270">`Location` üst bilgisinin URI 'sini oluşturmak için `GetTodoItem` eyleme başvurur.</span><span class="sxs-lookup"><span data-stu-id="540f2-270">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="540f2-271">C# `nameof` anahtar sözcüğü, `CreatedAtAction` çağrısında eylem adının sabit kodlanmasını önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="540f2-271">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="540f2-272">Postman yükleme</span><span class="sxs-lookup"><span data-stu-id="540f2-272">Install Postman</span></span>

<span data-ttu-id="540f2-273">Bu öğreticide Postman web API'si test etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="540f2-273">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="540f2-274">Yükleme [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="540f2-274">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="540f2-275">Web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="540f2-275">Start the web app.</span></span>
* <span data-ttu-id="540f2-276">Postman'i başlatın.</span><span class="sxs-lookup"><span data-stu-id="540f2-276">Start Postman.</span></span>
* <span data-ttu-id="540f2-277">Devre dışı **SSL sertifika doğrulama**</span><span class="sxs-lookup"><span data-stu-id="540f2-277">Disable **SSL certificate verification**</span></span>
  * <span data-ttu-id="540f2-278">**Dosya** > **ayarları** ' ndan (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="540f2-278">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="540f2-279">Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="540f2-279">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="540f2-280">Postman ile test PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="540f2-280">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="540f2-281">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="540f2-281">Create a new request.</span></span>
* <span data-ttu-id="540f2-282">HTTP yöntemini `POST`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="540f2-282">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="540f2-283">Seçin **gövdesi** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="540f2-283">Select the **Body** tab.</span></span>
* <span data-ttu-id="540f2-284">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="540f2-284">Select the **raw** radio button.</span></span>
* <span data-ttu-id="540f2-285">Tür kümesine **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="540f2-285">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="540f2-286">İstek gövdesinde bir yapılacak iş öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="540f2-286">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="540f2-287">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-287">Select **Send**.</span></span>

  ![Postman ile isteği oluştur](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="540f2-289">Konum üst bilgisi URI test</span><span class="sxs-lookup"><span data-stu-id="540f2-289">Test the location header URI</span></span>

* <span data-ttu-id="540f2-290">Seçin **üstbilgileri** sekmesinde **yanıt** bölmesi.</span><span class="sxs-lookup"><span data-stu-id="540f2-290">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="540f2-291">Kopyalama **konumu** üst bilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="540f2-291">Copy the **Location** header value:</span></span>

  ![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/3/create.png)

* <span data-ttu-id="540f2-293">Yöntemini GET öğesine Ayarla.</span><span class="sxs-lookup"><span data-stu-id="540f2-293">Set the method to GET.</span></span>
* <span data-ttu-id="540f2-294">URI 'yi yapıştırın (örneğin, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="540f2-294">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="540f2-295">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-295">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="540f2-296">GET yöntemlerini inceleyin</span><span class="sxs-lookup"><span data-stu-id="540f2-296">Examine the GET methods</span></span>

<span data-ttu-id="540f2-297">İki GET uç noktası bu yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="540f2-297">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="540f2-298">Tarayıcıdan veya Postman 'dan iki uç noktayı çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="540f2-298">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="540f2-299">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="540f2-299">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="540f2-300">Aşağıdakine benzer bir yanıt, `GetTodoItems`çağrısı tarafından üretilir:</span><span class="sxs-lookup"><span data-stu-id="540f2-300">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="540f2-301">Postman ile test al</span><span class="sxs-lookup"><span data-stu-id="540f2-301">Test Get with Postman</span></span>

* <span data-ttu-id="540f2-302">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="540f2-302">Create a new request.</span></span>
* <span data-ttu-id="540f2-303">HTTP yöntemi kümesine **alma**.</span><span class="sxs-lookup"><span data-stu-id="540f2-303">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="540f2-304">İstek URL'si kümesine `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="540f2-304">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="540f2-305">Örneğin: `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="540f2-305">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="540f2-306">Ayarlama **iki bölme görünümü** postman'deki.</span><span class="sxs-lookup"><span data-stu-id="540f2-306">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="540f2-307">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-307">Select **Send**.</span></span>

<span data-ttu-id="540f2-308">Bu uygulama, bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="540f2-308">This app uses an in-memory database.</span></span> <span data-ttu-id="540f2-309">Uygulama durdurulup başlatılırsa, önceki GET isteği herhangi bir veri döndürmez.</span><span class="sxs-lookup"><span data-stu-id="540f2-309">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="540f2-310">Hiçbir veri döndürülmezse, verileri uygulamaya [gönderin](#post) .</span><span class="sxs-lookup"><span data-stu-id="540f2-310">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="540f2-311">URL Yönlendirme ve yolları</span><span class="sxs-lookup"><span data-stu-id="540f2-311">Routing and URL paths</span></span>

<span data-ttu-id="540f2-312">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Özniteliği bir HTTP GET isteğine yanıt vermeden bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="540f2-312">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="540f2-313">Her yöntem için URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="540f2-313">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="540f2-314">Denetleyicinin şablonu dizesi ile başlayıp `Route` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="540f2-314">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="540f2-315">Değiştirin `[controller]` denetleyicinin adı ile kural tarafından olduğu "Controller" soneki eksi denetleyici sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="540f2-315">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="540f2-316">Bu örnek için denetleyici sınıfı adı **todoıtems**denetleyicisidir, bu nedenle denetleyicinin adı "todoıtems" olur.</span><span class="sxs-lookup"><span data-stu-id="540f2-316">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="540f2-317">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="540f2-317">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="540f2-318">`[HttpGet]` özniteliğinin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin.</span><span class="sxs-lookup"><span data-stu-id="540f2-318">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="540f2-319">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="540f2-319">This sample doesn't use a template.</span></span> <span data-ttu-id="540f2-320">Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="540f2-320">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="540f2-321">Aşağıdaki `GetTodoItem` yöntemi `"{id}"` yapılacak iş öğesi benzersiz tanımlayıcısı için bir yer tutucu değişkendir.</span><span class="sxs-lookup"><span data-stu-id="540f2-321">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="540f2-322">`GetTodoItem` çağrıldığında, URL 'deki `"{id}"` değeri `id` parametresindeki yöntemine sağlanır.</span><span class="sxs-lookup"><span data-stu-id="540f2-322">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="540f2-323">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="540f2-323">Return values</span></span>

<span data-ttu-id="540f2-324">Dönüş türünü `GetTodoItems` ve `GetTodoItem` yöntemler [actionresult öğesini\<T > türü](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="540f2-324">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="540f2-325">ASP.NET Core, nesneyi otomatik olarak serileştiren [JSON](https://www.json.org/) ve yanıt iletisinin gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="540f2-325">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="540f2-326">Yanıt kodu 200 bu dönüş türü için olduğu varsayılırsa işlenmeyen özel durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="540f2-326">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="540f2-327">İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.</span><span class="sxs-lookup"><span data-stu-id="540f2-327">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="540f2-328">`ActionResult` dönüş türleri, geniş HTTP durum kodları temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="540f2-328">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="540f2-329">Örneğin, `GetTodoItem` iki farklı durum değerleri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="540f2-329">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="540f2-330">Öğe istenen kimliği eşleşirse, yöntem bir 404 döndürür [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu.</span><span class="sxs-lookup"><span data-stu-id="540f2-330">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="540f2-331">Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="540f2-331">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="540f2-332">Döndüren `item` sonuçları bir HTTP 200 yanıtı.</span><span class="sxs-lookup"><span data-stu-id="540f2-332">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="540f2-333">PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="540f2-333">The PutTodoItem method</span></span>

<span data-ttu-id="540f2-334">`PutTodoItem` yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="540f2-334">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="540f2-335">`PutTodoItem` benzer `PostTodoItem`, HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="540f2-335">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="540f2-336">Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="540f2-336">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="540f2-337">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca değişiklikler değil göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="540f2-337">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="540f2-338">Kısmi güncelleştirmeleri desteklemek için kullanma [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="540f2-338">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="540f2-339">`PutTodoItem`çağırırken bir hata alırsanız, veritabanında bir öğe olduğundan emin olmak için `GET` çağırın.</span><span class="sxs-lookup"><span data-stu-id="540f2-339">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="540f2-340">Test PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="540f2-340">Test the PutTodoItem method</span></span>

<span data-ttu-id="540f2-341">Bu örnek, uygulama her başlatıldığında başlatılmış olması gereken bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="540f2-341">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="540f2-342">Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="540f2-342">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="540f2-343">PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="540f2-343">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="540f2-344">ID = 1 olan Yapılacaklar öğesini güncelleştirin ve adını "Feed balık" olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="540f2-344">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="540f2-345">Aşağıdaki görüntüde, Postman güncelleştirme gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="540f2-345">The following image shows the Postman update:</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="540f2-347">DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="540f2-347">The DeleteTodoItem method</span></span>

<span data-ttu-id="540f2-348">`DeleteTodoItem` yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="540f2-348">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="540f2-349">Test DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="540f2-349">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="540f2-350">Postman bir yapılacak iş öğesini silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="540f2-350">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="540f2-351">Yöntem kümesine `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="540f2-351">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="540f2-352">Silinecek nesnenin URI 'sini ayarlayın (örneğin `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="540f2-352">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="540f2-353">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-353">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="540f2-354">JavaScript ile Web API 'sini çağırma</span><span class="sxs-lookup"><span data-stu-id="540f2-354">Call the web API with JavaScript</span></span>

<span data-ttu-id="540f2-355">Bkz. [öğretici: JavaScript ile ASP.NET Core Web API 'Si çağırma](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="540f2-355">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="540f2-356">Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="540f2-356">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="540f2-357">Bir Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="540f2-357">Create a web API project.</span></span>
> * <span data-ttu-id="540f2-358">Bir model sınıfı ve bir veritabanı bağlamı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="540f2-358">Add a model class and a database context.</span></span>
> * <span data-ttu-id="540f2-359">Bir denetleyici ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="540f2-359">Add a controller.</span></span>
> * <span data-ttu-id="540f2-360">CRUD yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="540f2-360">Add CRUD methods.</span></span>
> * <span data-ttu-id="540f2-361">Yönlendirmeyi Yapılandırma ve URL yolu.</span><span class="sxs-lookup"><span data-stu-id="540f2-361">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="540f2-362">Dönüş değerleri belirtin.</span><span class="sxs-lookup"><span data-stu-id="540f2-362">Specify return values.</span></span>
> * <span data-ttu-id="540f2-363">Web API'si Postman ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="540f2-363">Call the web API with Postman.</span></span>
> * <span data-ttu-id="540f2-364">JavaScript ile Web API 'sini çağırın.</span><span class="sxs-lookup"><span data-stu-id="540f2-364">Call the web API with JavaScript.</span></span>

<span data-ttu-id="540f2-365">Sonunda, web API'si "Yapılacaklar" öğelerini ilişkisel bir veritabanında depolanan yönetebileceği sahip.</span><span class="sxs-lookup"><span data-stu-id="540f2-365">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="540f2-366">Genel bakış</span><span class="sxs-lookup"><span data-stu-id="540f2-366">Overview</span></span>

<span data-ttu-id="540f2-367">Bu öğretici yandaki API oluşturur:</span><span class="sxs-lookup"><span data-stu-id="540f2-367">This tutorial creates the following API:</span></span>

|<span data-ttu-id="540f2-368">API</span><span class="sxs-lookup"><span data-stu-id="540f2-368">API</span></span> | <span data-ttu-id="540f2-369">Açıklama</span><span class="sxs-lookup"><span data-stu-id="540f2-369">Description</span></span> | <span data-ttu-id="540f2-370">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="540f2-370">Request body</span></span> | <span data-ttu-id="540f2-371">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="540f2-371">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="540f2-372">/Api/TodoItems al</span><span class="sxs-lookup"><span data-stu-id="540f2-372">GET /api/TodoItems</span></span> | <span data-ttu-id="540f2-373">Tüm yapılacak iş öğeleri al</span><span class="sxs-lookup"><span data-stu-id="540f2-373">Get all to-do items</span></span> | <span data-ttu-id="540f2-374">Yok.</span><span class="sxs-lookup"><span data-stu-id="540f2-374">None</span></span> | <span data-ttu-id="540f2-375">Yapılacaklar öğelerinin bir dizisi</span><span class="sxs-lookup"><span data-stu-id="540f2-375">Array of to-do items</span></span>|
|<span data-ttu-id="540f2-376">/Api/TodoItems/{id} al</span><span class="sxs-lookup"><span data-stu-id="540f2-376">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="540f2-377">Bir öğeyi Kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="540f2-377">Get an item by ID</span></span> | <span data-ttu-id="540f2-378">Yok.</span><span class="sxs-lookup"><span data-stu-id="540f2-378">None</span></span> | <span data-ttu-id="540f2-379">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="540f2-379">To-do item</span></span>|
|<span data-ttu-id="540f2-380">POST/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="540f2-380">POST /api/TodoItems</span></span> | <span data-ttu-id="540f2-381">Yeni Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="540f2-381">Add a new item</span></span> | <span data-ttu-id="540f2-382">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="540f2-382">To-do item</span></span> | <span data-ttu-id="540f2-383">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="540f2-383">To-do item</span></span> |
|<span data-ttu-id="540f2-384">/Api/TodoItems/{id} koy</span><span class="sxs-lookup"><span data-stu-id="540f2-384">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="540f2-385">Mevcut öğeyi güncelleştirin &nbsp;</span><span class="sxs-lookup"><span data-stu-id="540f2-385">Update an existing item &nbsp;</span></span> | <span data-ttu-id="540f2-386">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="540f2-386">To-do item</span></span> | <span data-ttu-id="540f2-387">Yok.</span><span class="sxs-lookup"><span data-stu-id="540f2-387">None</span></span> |
|<span data-ttu-id="540f2-388">/Api/TodoItems/{id} &nbsp; SIL &nbsp;</span><span class="sxs-lookup"><span data-stu-id="540f2-388">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="540f2-389">Öğeyi Sil &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="540f2-389">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="540f2-390">Yok.</span><span class="sxs-lookup"><span data-stu-id="540f2-390">None</span></span> | <span data-ttu-id="540f2-391">Yok.</span><span class="sxs-lookup"><span data-stu-id="540f2-391">None</span></span>|

<span data-ttu-id="540f2-392">Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="540f2-392">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki bir kutu ile temsil edilir.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="540f2-398">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="540f2-398">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540f2-399">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-399">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="540f2-400">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="540f2-400">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="540f2-401">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-401">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="540f2-402">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="540f2-402">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540f2-403">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-403">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="540f2-404">**Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-404">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="540f2-405">**ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="540f2-405">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="540f2-406">Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="540f2-406">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="540f2-407">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 2,2** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="540f2-407">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="540f2-408">**API** şablonunu seçin ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="540f2-408">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="540f2-409">**Docker desteğini etkinleştir** **' i seçmeyin** .</span><span class="sxs-lookup"><span data-stu-id="540f2-409">**Don't** select **Enable Docker Support**.</span></span>

![VS yeni proje iletişim kutusu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="540f2-411">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="540f2-411">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="540f2-412">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="540f2-412">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="540f2-413">Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="540f2-413">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="540f2-414">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="540f2-414">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="540f2-415">Bu komutlar yeni bir Web API projesi oluşturur ve yeni proje klasöründe Visual Studio Code yeni bir örneğini açar.</span><span class="sxs-lookup"><span data-stu-id="540f2-415">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="540f2-416">Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-416">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="540f2-417">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-417">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="540f2-418">**Yeni çözüm**> **Dosya** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-418">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="540f2-420">**.NET Core** > **App** > **API** > ' **yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-420">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="540f2-422">İçinde **, yeni ASP.NET Core Web API'sini yapılandırma** iletişim kutusunda varsayılan değerleri kabul **hedef Framework'ü** , \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="540f2-422">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="540f2-423">Girin *TodoApi* için **proje adı** seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="540f2-423">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="540f2-425">API'yi test etme</span><span class="sxs-lookup"><span data-stu-id="540f2-425">Test the API</span></span>

<span data-ttu-id="540f2-426">Proje şablonu oluşturur bir `values` API.</span><span class="sxs-lookup"><span data-stu-id="540f2-426">The project template creates a `values` API.</span></span> <span data-ttu-id="540f2-427">Çağrı `Get` uygulamayı test etmek için bir tarayıcıdan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="540f2-427">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540f2-428">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-428">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="540f2-429">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="540f2-429">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="540f2-430">Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>/api/values`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="540f2-430">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="540f2-431">IIS Express sertifika güven varsa soran bir iletişim kutusu alırsanız seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="540f2-431">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="540f2-432">İçinde **Güvenlik Uyarısı** ardından, görüntülenen iletişim seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="540f2-432">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="540f2-433">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="540f2-433">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="540f2-434">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="540f2-434">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="540f2-435">Bir tarayıcıda aşağıdaki URL'ye gidin: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="540f2-435">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="540f2-436">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-436">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="540f2-437">Uygulamayı başlatmak için **hata ayıklamayı başlat** > **Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-437">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="540f2-438">Mac için Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="540f2-438">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="540f2-439">HTTP 404 (bulunamadı) hatası döndürülür.</span><span class="sxs-lookup"><span data-stu-id="540f2-439">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="540f2-440">Append `/api/values` URL'sine (URL'yi `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="540f2-440">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="540f2-441">Aşağıdaki JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="540f2-441">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="540f2-442">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="540f2-442">Add a model class</span></span>

<span data-ttu-id="540f2-443">A *modeli* uygulamayı yöneten verilerini temsil eden sınıflar kümesidir.</span><span class="sxs-lookup"><span data-stu-id="540f2-443">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="540f2-444">Tek bir modeldir bu uygulama için `TodoItem` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="540f2-444">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540f2-445">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-445">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="540f2-446">İçinde **Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="540f2-446">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="540f2-447">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="540f2-447">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="540f2-448">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="540f2-448">Name the folder *Models*.</span></span>

* <span data-ttu-id="540f2-449">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="540f2-449">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="540f2-450">Sınıf adı *Todoıtem* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="540f2-450">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="540f2-451">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="540f2-451">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="540f2-452">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="540f2-452">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="540f2-453">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="540f2-453">Add a folder named *Models*.</span></span>

* <span data-ttu-id="540f2-454">Ekleme bir `TodoItem` sınıfının *modelleri* aşağıdaki kodla klasörü:</span><span class="sxs-lookup"><span data-stu-id="540f2-454">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="540f2-455">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-455">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="540f2-456">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="540f2-456">Right-click the project.</span></span> <span data-ttu-id="540f2-457">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="540f2-457">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="540f2-458">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="540f2-458">Name the folder *Models*.</span></span>

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="540f2-460">*Modeller* klasörüne sağ tıklayın ve > **yeni dosya** **ekle** ' yi > **genel** > **boş sınıf**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-460">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="540f2-461">Sınıf adı *Todoıtem*ve ardından **yeni**.</span><span class="sxs-lookup"><span data-stu-id="540f2-461">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="540f2-462">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="540f2-462">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="540f2-463">`Id` Özelliği işlevlerinin bir ilişkisel veritabanında benzersiz anahtar.</span><span class="sxs-lookup"><span data-stu-id="540f2-463">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="540f2-464">Model sınıfları herhangi bir projede gidip ancak *modelleri* klasörü, kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="540f2-464">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="540f2-465">Veritabanı bağlamı Ekle</span><span class="sxs-lookup"><span data-stu-id="540f2-465">Add a database context</span></span>

<span data-ttu-id="540f2-466">*Veritabanı bağlamı* koordine eden bir veri modeli için Entity Framework işlevsellik ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="540f2-466">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="540f2-467">Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="540f2-467">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540f2-468">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-468">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="540f2-469">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="540f2-469">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="540f2-470">Sınıf adı *TodoContext* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="540f2-470">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="540f2-471">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-471">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="540f2-472">Ekleme bir `TodoContext` sınıfının *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="540f2-472">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="540f2-473">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="540f2-473">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="540f2-474">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="540f2-474">Register the database context</span></span>

<span data-ttu-id="540f2-475">ASP.NET Core DB bağlamı gibi hizmetler ile kaydedilmelidir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="540f2-475">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="540f2-476">Kapsayıcı hizmeti denetleyicilerine sağlar.</span><span class="sxs-lookup"><span data-stu-id="540f2-476">The container provides the service to controllers.</span></span>

<span data-ttu-id="540f2-477">Güncelleştirme *Startup.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="540f2-477">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="540f2-478">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="540f2-478">The preceding code:</span></span>

* <span data-ttu-id="540f2-479">Kullanılmayan kaldırır `using` bildirimleri.</span><span class="sxs-lookup"><span data-stu-id="540f2-479">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="540f2-480">Veritabanı bağlamı DI kapsayıcıya ekler.</span><span class="sxs-lookup"><span data-stu-id="540f2-480">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="540f2-481">Veritabanı bağlamı bir bellek içi veritabanına kullanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="540f2-481">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="540f2-482">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="540f2-482">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540f2-483">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-483">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="540f2-484">Sağ *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="540f2-484">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="540f2-485">> **Yeni öğe** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-485">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="540f2-486">İçinde **Yeni Öğe Ekle** iletişim kutusunda **API denetleyici sınıfı** şablonu.</span><span class="sxs-lookup"><span data-stu-id="540f2-486">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="540f2-487">Sınıf adı *TodoController*seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="540f2-487">Name the class *TodoController*, and select **Add**.</span></span>

  ![Yeni öğe iletişim denetleyicisiyle seçilen arama kutusu ve web API denetleyicisi Ekle](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="540f2-489">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-489">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="540f2-490">İçinde *denetleyicileri* klasör adında bir sınıf oluşturma `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="540f2-490">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="540f2-491">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="540f2-491">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="540f2-492">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="540f2-492">The preceding code:</span></span>

* <span data-ttu-id="540f2-493">Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.</span><span class="sxs-lookup"><span data-stu-id="540f2-493">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="540f2-494">Sınıfı [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle işaretler.</span><span class="sxs-lookup"><span data-stu-id="540f2-494">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="540f2-495">Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="540f2-495">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="540f2-496">Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz. <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="540f2-496">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="540f2-497">Veritabanı bağlamı eklemesine DI kullanır (`TodoContext`) içine denetleyici.</span><span class="sxs-lookup"><span data-stu-id="540f2-497">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="540f2-498">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="540f2-498">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="540f2-499">Adlı bir öğe ekler `Item1` veritabanı boşsa veritabanı.</span><span class="sxs-lookup"><span data-stu-id="540f2-499">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="540f2-500">Her çalıştığında bu kod oluşturucusunun içinde yeni bir HTTP isteği olduğundan.</span><span class="sxs-lookup"><span data-stu-id="540f2-500">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="540f2-501">Tüm öğeleri silerseniz, oluşturucu oluşturur `Item1` API yöntemi çağrıldığında tekrar başlattığınızda.</span><span class="sxs-lookup"><span data-stu-id="540f2-501">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="540f2-502">Bu nedenle, gerçekten işe yaradı silme işlemi işe yaramadı gibi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="540f2-502">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="540f2-503">Get yöntemleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="540f2-503">Add Get methods</span></span>

<span data-ttu-id="540f2-504">Yapılacak iş öğeleri alır bir API sağlamak için aşağıdaki yöntemi ekleyin. `TodoController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="540f2-504">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="540f2-505">İki GET uç noktası bu yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="540f2-505">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="540f2-506">Hala çalışıyorsa uygulamayı durdurun.</span><span class="sxs-lookup"><span data-stu-id="540f2-506">Stop the app if it's still running.</span></span> <span data-ttu-id="540f2-507">Ardından, en son değişiklikleri dahil etmek için yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="540f2-507">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="540f2-508">Bir tarayıcıdan iki uç nokta çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="540f2-508">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="540f2-509">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="540f2-509">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="540f2-510">Şu HTTP yanıtı çağrısı tarafından üretilen `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="540f2-510">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="540f2-511">URL Yönlendirme ve yolları</span><span class="sxs-lookup"><span data-stu-id="540f2-511">Routing and URL paths</span></span>

<span data-ttu-id="540f2-512">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Özniteliği bir HTTP GET isteğine yanıt vermeden bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="540f2-512">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="540f2-513">Her yöntem için URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="540f2-513">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="540f2-514">Denetleyicinin şablonu dizesi ile başlayıp `Route` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="540f2-514">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="540f2-515">Değiştirin `[controller]` denetleyicinin adı ile kural tarafından olduğu "Controller" soneki eksi denetleyici sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="540f2-515">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="540f2-516">Bu örnek, denetleyici sınıfı adı olan **Todo**Denetleyici adı "todo" Bu nedenle denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="540f2-516">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="540f2-517">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="540f2-517">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="540f2-518">`[HttpGet]` özniteliğinin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin.</span><span class="sxs-lookup"><span data-stu-id="540f2-518">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="540f2-519">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="540f2-519">This sample doesn't use a template.</span></span> <span data-ttu-id="540f2-520">Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="540f2-520">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="540f2-521">Aşağıdaki `GetTodoItem` yöntemi `"{id}"` yapılacak iş öğesi benzersiz tanımlayıcısı için bir yer tutucu değişkendir.</span><span class="sxs-lookup"><span data-stu-id="540f2-521">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="540f2-522">Zaman `GetTodoItem` çağrılır, değerini `"{id}"` yöntemine URL'de sağlanan kendi`id` parametresi.</span><span class="sxs-lookup"><span data-stu-id="540f2-522">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="540f2-523">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="540f2-523">Return values</span></span>

<span data-ttu-id="540f2-524">Dönüş türünü `GetTodoItems` ve `GetTodoItem` yöntemler [actionresult öğesini\<T > türü](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="540f2-524">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="540f2-525">ASP.NET Core, nesneyi otomatik olarak serileştiren [JSON](https://www.json.org/) ve yanıt iletisinin gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="540f2-525">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="540f2-526">Yanıt kodu 200 bu dönüş türü için olduğu varsayılırsa işlenmeyen özel durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="540f2-526">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="540f2-527">İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.</span><span class="sxs-lookup"><span data-stu-id="540f2-527">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="540f2-528">`ActionResult` dönüş türleri, geniş HTTP durum kodları temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="540f2-528">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="540f2-529">Örneğin, `GetTodoItem` iki farklı durum değerleri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="540f2-529">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="540f2-530">Öğe istenen kimliği eşleşirse, yöntem bir 404 döndürür [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu.</span><span class="sxs-lookup"><span data-stu-id="540f2-530">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="540f2-531">Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="540f2-531">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="540f2-532">Döndüren `item` sonuçları bir HTTP 200 yanıtı.</span><span class="sxs-lookup"><span data-stu-id="540f2-532">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="540f2-533">Test GetTodoItems yöntemi</span><span class="sxs-lookup"><span data-stu-id="540f2-533">Test the GetTodoItems method</span></span>

<span data-ttu-id="540f2-534">Bu öğreticide Postman web API'si test etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="540f2-534">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="540f2-535">[Postman](https://www.getpostman.com/downloads/)'yi yükleme.</span><span class="sxs-lookup"><span data-stu-id="540f2-535">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="540f2-536">Web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="540f2-536">Start the web app.</span></span>
* <span data-ttu-id="540f2-537">Postman'i başlatın.</span><span class="sxs-lookup"><span data-stu-id="540f2-537">Start Postman.</span></span>
* <span data-ttu-id="540f2-538">**SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="540f2-538">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540f2-539">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-539">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="540f2-540">**Dosya** > **ayarları** ' ndan (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="540f2-540">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="540f2-541">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540f2-541">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="540f2-542">**Postman** > **tercihleri** ' nden (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="540f2-542">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="540f2-543">Alternatif olarak, wranı seçin ve **Ayarlar**' ı seçip SSL sertifikası doğrulamasını devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="540f2-543">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="540f2-544">Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="540f2-544">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="540f2-545">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="540f2-545">Create a new request.</span></span>
  * <span data-ttu-id="540f2-546">HTTP yöntemi kümesine **alma**.</span><span class="sxs-lookup"><span data-stu-id="540f2-546">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="540f2-547">İstek URL'si kümesine `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="540f2-547">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="540f2-548">Örneğin: `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="540f2-548">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="540f2-549">Ayarlama **iki bölme görünümü** postman'deki.</span><span class="sxs-lookup"><span data-stu-id="540f2-549">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="540f2-550">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-550">Select **Send**.</span></span>

![Get isteğiyle postman](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="540f2-552">Create yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="540f2-552">Add a Create method</span></span>

<span data-ttu-id="540f2-553">Aşağıdaki `PostTodoItem` yöntemini *Controllers/TodoController. cs*içine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="540f2-553">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="540f2-554">Yukarıdaki kod, [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliğiyle gösterildiği gıbı bır http post yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="540f2-554">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="540f2-555">Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="540f2-555">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="540f2-556">`CreatedAtAction` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="540f2-556">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="540f2-557">Başarılı olursa bir HTTP 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="540f2-557">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="540f2-558">HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="540f2-558">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="540f2-559">Yanıta bir `Location` üst bilgisi ekler.</span><span class="sxs-lookup"><span data-stu-id="540f2-559">Adds a `Location` header to the response.</span></span> <span data-ttu-id="540f2-560">`Location` üstbilgisi, yeni oluşturulan Yapılacaklar öğesinin URI 'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="540f2-560">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="540f2-561">Daha fazla bilgi için [10.2.2 201 oluşturuldu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="540f2-561">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="540f2-562">`Location` üst bilgisinin URI 'sini oluşturmak için `GetTodoItem` eyleme başvurur.</span><span class="sxs-lookup"><span data-stu-id="540f2-562">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="540f2-563">C# `nameof` anahtar sözcüğü, `CreatedAtAction` çağrısında eylem adının sabit kodlanmasını önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="540f2-563">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="540f2-564">Test PostTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="540f2-564">Test the PostTodoItem method</span></span>

* <span data-ttu-id="540f2-565">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="540f2-565">Build the project.</span></span>
* <span data-ttu-id="540f2-566">Postman HTTP yöntemi kümesine `POST`.</span><span class="sxs-lookup"><span data-stu-id="540f2-566">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="540f2-567">Seçin **gövdesi** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="540f2-567">Select the **Body** tab.</span></span>
* <span data-ttu-id="540f2-568">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="540f2-568">Select the **raw** radio button.</span></span>
* <span data-ttu-id="540f2-569">Tür kümesine **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="540f2-569">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="540f2-570">İstek gövdesinde bir yapılacak iş öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="540f2-570">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="540f2-571">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-571">Select **Send**.</span></span>

  ![Postman ile isteği oluştur](first-web-api/_static/create.png)

  <span data-ttu-id="540f2-573">Bir 405 yöntemine Izin verilmiyor hatası alırsanız, `PostTodoItem` yöntemi eklendikten sonra projenin derlenmesinin sonucu büyük olasılıkla oluşur.</span><span class="sxs-lookup"><span data-stu-id="540f2-573">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="540f2-574">Konum üst bilgisi URI test</span><span class="sxs-lookup"><span data-stu-id="540f2-574">Test the location header URI</span></span>

* <span data-ttu-id="540f2-575">Seçin **üstbilgileri** sekmesinde **yanıt** bölmesi.</span><span class="sxs-lookup"><span data-stu-id="540f2-575">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="540f2-576">Kopyalama **konumu** üst bilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="540f2-576">Copy the **Location** header value:</span></span>

  ![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmc2.png)

* <span data-ttu-id="540f2-578">Yöntemini GET öğesine Ayarla.</span><span class="sxs-lookup"><span data-stu-id="540f2-578">Set the method to GET.</span></span>
* <span data-ttu-id="540f2-579">URI 'yi yapıştırın (örneğin, `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="540f2-579">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="540f2-580">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-580">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="540f2-581">PutTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="540f2-581">Add a PutTodoItem method</span></span>

<span data-ttu-id="540f2-582">Aşağıdaki `PutTodoItem` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="540f2-582">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="540f2-583">`PutTodoItem` benzer `PostTodoItem`, HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="540f2-583">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="540f2-584">Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="540f2-584">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="540f2-585">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca değişiklikler değil göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="540f2-585">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="540f2-586">Kısmi güncelleştirmeleri desteklemek için kullanma [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="540f2-586">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="540f2-587">`PutTodoItem`çağırırken bir hata alırsanız, veritabanında bir öğe olduğundan emin olmak için `GET` çağırın.</span><span class="sxs-lookup"><span data-stu-id="540f2-587">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="540f2-588">Test PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="540f2-588">Test the PutTodoItem method</span></span>

<span data-ttu-id="540f2-589">Bu örnek, uygulama her başlatıldığında başlatılmış olması gereken bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="540f2-589">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="540f2-590">Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="540f2-590">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="540f2-591">PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="540f2-591">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="540f2-592">Kimliğine sahip bir yapılacak iş öğesi güncelleştirme = 1 ve "balık akış için" adını girin:</span><span class="sxs-lookup"><span data-stu-id="540f2-592">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="540f2-593">Aşağıdaki görüntüde, Postman güncelleştirme gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="540f2-593">The following image shows the Postman update:</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="540f2-595">DeleteTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="540f2-595">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="540f2-596">Aşağıdaki `DeleteTodoItem` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="540f2-596">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="540f2-597">`DeleteTodoItem` Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="540f2-597">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="540f2-598">Test DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="540f2-598">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="540f2-599">Postman bir yapılacak iş öğesini silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="540f2-599">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="540f2-600">Yöntem kümesine `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="540f2-600">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="540f2-601">Silinecek nesnenin URI 'sini ayarlayın (örneğin `https://localhost:5001/api/todo/1`).</span><span class="sxs-lookup"><span data-stu-id="540f2-601">Set the URI of the object to delete (for example `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="540f2-602">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="540f2-602">Select **Send**.</span></span>

<span data-ttu-id="540f2-603">Örnek uygulama, tüm öğeleri silmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="540f2-603">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="540f2-604">Ancak, son öğe silindiğinde, API 'nin bir sonraki çağrılışında model sınıfı Oluşturucu tarafından yeni bir tane oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="540f2-604">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="540f2-605">JavaScript ile Web API 'sini çağırma</span><span class="sxs-lookup"><span data-stu-id="540f2-605">Call the web API with JavaScript</span></span>

<span data-ttu-id="540f2-606">Bu bölümde, Web API 'sini çağırmak için JavaScript kullanan bir HTML sayfası eklenir.</span><span class="sxs-lookup"><span data-stu-id="540f2-606">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="540f2-607">jQuery isteği başlatır.</span><span class="sxs-lookup"><span data-stu-id="540f2-607">jQuery initiates the request.</span></span> <span data-ttu-id="540f2-608">JavaScript, sayfayı Web API 'sinin yanıtından alınan ayrıntılarla güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="540f2-608">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="540f2-609">Uygulamayı [statik dosyalara sunacak](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) şekilde yapılandırın ve aşağıdaki vurgulanmış kodla *Startup.cs* güncelleştirerek [varsayılan dosya eşlemesini etkinleştirin](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) :</span><span class="sxs-lookup"><span data-stu-id="540f2-609">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="540f2-610">Oluşturma bir *wwwroot* proje dizininde klasör.</span><span class="sxs-lookup"><span data-stu-id="540f2-610">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="540f2-611">Adlı bir HTML dosyası ekleyin *index.html* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="540f2-611">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="540f2-612">Dosyanın içeriğini aşağıdaki biçimlendirme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="540f2-612">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="540f2-613">Adlı bir JavaScript dosyası ekleyin *site.js* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="540f2-613">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="540f2-614">Dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="540f2-614">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="540f2-615">ASP.NET Core proje başlatma ayarlarında bir değişiklik HTML sayfasını yerel olarak test etmek için gerekli:</span><span class="sxs-lookup"><span data-stu-id="540f2-615">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="540f2-616">Açık *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="540f2-616">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="540f2-617">Kaldırma `launchUrl` , açmak için uygulamayı zorlamak için özellik *index.html*&mdash;projenin varsayılan dosya.</span><span class="sxs-lookup"><span data-stu-id="540f2-617">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="540f2-618">Bu örnek, Web API 'sinin tüm CRUD yöntemlerini çağırır.</span><span class="sxs-lookup"><span data-stu-id="540f2-618">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="540f2-619">API çağrıları açıklamaları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="540f2-619">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="540f2-620">Yapılacaklar öğelerinin bir listesini alın</span><span class="sxs-lookup"><span data-stu-id="540f2-620">Get a list of to-do items</span></span>

<span data-ttu-id="540f2-621">jQuery, bir to-do öğesi dizisini temsil eden JSON döndüren Web API 'sine bir HTTP GET isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="540f2-621">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="540f2-622">`success` İstek başarılı olursa geri çağırma işlevi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="540f2-622">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="540f2-623">Geri çağırma içinde DOM Yapılacaklar bilgilerle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="540f2-623">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="540f2-624">Yapılacak İş Öğesi Ekle</span><span class="sxs-lookup"><span data-stu-id="540f2-624">Add a to-do item</span></span>

<span data-ttu-id="540f2-625">jQuery, istek gövdesinde Yapılacaklar öğesiyle bir HTTP POST isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="540f2-625">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="540f2-626">`accepts` Ve `contentType` seçeneklerini ayarlamak `application/json` gönderilen ve alınan medya türü belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="540f2-626">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="540f2-627">Yapılacak iş öğesi kullanarak JSON'a dönüştürülür [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="540f2-627">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="540f2-628">API'yi bir başarılı durum kodu döndürdüğünde `getData` işlevi HTML tablosu güncelleştirmek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="540f2-628">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="540f2-629">Yapılacak iş öğesini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="540f2-629">Update a to-do item</span></span>

<span data-ttu-id="540f2-630">Yapılacak iş öğesi güncelleştirilirken bir eklemeye benzerdir.</span><span class="sxs-lookup"><span data-stu-id="540f2-630">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="540f2-631">`url` Öğenin benzersiz tanıtıcısı eklemek için değişiklikleri ve `type` olduğu `PUT`.</span><span class="sxs-lookup"><span data-stu-id="540f2-631">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="540f2-632">Yapılacak iş öğesi silme</span><span class="sxs-lookup"><span data-stu-id="540f2-632">Delete a to-do item</span></span>

<span data-ttu-id="540f2-633">Yapılacak iş öğesi silme gerçekleştirilir ayarlayarak `type` AJAX çağrısı hedefi üzerinde `DELETE` ve öğenin benzersiz tanıtıcısı URL'yi belirterek.</span><span class="sxs-lookup"><span data-stu-id="540f2-633">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="540f2-634">Web API 'sine kimlik doğrulama desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="540f2-634">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources"></a><span data-ttu-id="540f2-635">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="540f2-635">Additional resources</span></span>

<span data-ttu-id="540f2-636">[Görüntülemek veya Bu öğretici için örnek kodu indirdikten](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="540f2-636">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="540f2-637">Bkz: [nasıl indirileceğini](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="540f2-637">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="540f2-638">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="540f2-638">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="540f2-639">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="540f2-639">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
