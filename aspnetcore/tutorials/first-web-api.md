---
title: "Öğretici: ASP.NET Core bir Web API 'SI oluşturma"
author: rick-anderson
description: ASP.NET Core ile Web API 'SI oluşturmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 2/25/2020
uid: tutorials/first-web-api
ms.openlocfilehash: 55dfc05b5c96f7fa060d537745bac969e92daa9b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655591"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="4cd04-103">Öğretici: ASP.NET Core bir Web API 'SI oluşturma</span><span class="sxs-lookup"><span data-stu-id="4cd04-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="4cd04-104">[Rick Anderson](https://twitter.com/RickAndMSFT), [Kirk Larkabağı](https://twitter.com/serpent5)ve [Mike te son](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="4cd04-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Kirk Larkin](https://twitter.com/serpent5),  and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="4cd04-105">Bu öğretici, bir web API ASP.NET Core ile oluşturmaya ilişkin temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4cd04-106">Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="4cd04-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4cd04-107">Bir Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4cd04-107">Create a web API project.</span></span>
> * <span data-ttu-id="4cd04-108">Bir model sınıfı ve bir veritabanı bağlamı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="4cd04-109">CRUD yöntemleriyle bir denetleyiciyi dolandırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="4cd04-110">Yönlendirmeyi, URL yollarını ve dönüş değerlerini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="4cd04-111">Web API'si Postman ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-111">Call the web API with Postman.</span></span>

<span data-ttu-id="4cd04-112">Sonunda, bir veritabanında depolanan "yapılacaklar" öğelerini yönetebilmek için bir Web API 'SI vardır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="4cd04-113">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="4cd04-113">Overview</span></span>

<span data-ttu-id="4cd04-114">Bu öğretici yandaki API oluşturur:</span><span class="sxs-lookup"><span data-stu-id="4cd04-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="4cd04-115">API</span><span class="sxs-lookup"><span data-stu-id="4cd04-115">API</span></span> | <span data-ttu-id="4cd04-116">Açıklama</span><span class="sxs-lookup"><span data-stu-id="4cd04-116">Description</span></span> | <span data-ttu-id="4cd04-117">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="4cd04-117">Request body</span></span> | <span data-ttu-id="4cd04-118">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="4cd04-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="4cd04-119">/Api/TodoItems al</span><span class="sxs-lookup"><span data-stu-id="4cd04-119">GET /api/TodoItems</span></span> | <span data-ttu-id="4cd04-120">Tüm yapılacak iş öğeleri al</span><span class="sxs-lookup"><span data-stu-id="4cd04-120">Get all to-do items</span></span> | <span data-ttu-id="4cd04-121">Yok</span><span class="sxs-lookup"><span data-stu-id="4cd04-121">None</span></span> | <span data-ttu-id="4cd04-122">Yapılacaklar öğelerinin bir dizisi</span><span class="sxs-lookup"><span data-stu-id="4cd04-122">Array of to-do items</span></span>|
|<span data-ttu-id="4cd04-123">/Api/TodoItems/{id} al</span><span class="sxs-lookup"><span data-stu-id="4cd04-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="4cd04-124">Bir öğeyi Kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="4cd04-124">Get an item by ID</span></span> | <span data-ttu-id="4cd04-125">Yok</span><span class="sxs-lookup"><span data-stu-id="4cd04-125">None</span></span> | <span data-ttu-id="4cd04-126">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="4cd04-126">To-do item</span></span>|
|<span data-ttu-id="4cd04-127">POST/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="4cd04-127">POST /api/TodoItems</span></span> | <span data-ttu-id="4cd04-128">Yeni Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="4cd04-128">Add a new item</span></span> | <span data-ttu-id="4cd04-129">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="4cd04-129">To-do item</span></span> | <span data-ttu-id="4cd04-130">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="4cd04-130">To-do item</span></span> |
|<span data-ttu-id="4cd04-131">/Api/TodoItems/{id} koy</span><span class="sxs-lookup"><span data-stu-id="4cd04-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="4cd04-132">Mevcut bir öğeyi güncelleştirin &nbsp;</span><span class="sxs-lookup"><span data-stu-id="4cd04-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="4cd04-133">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="4cd04-133">To-do item</span></span> | <span data-ttu-id="4cd04-134">Yok</span><span class="sxs-lookup"><span data-stu-id="4cd04-134">None</span></span> |
|<span data-ttu-id="4cd04-135">/Api/TodoItems/{id} &nbsp; SIL &nbsp;</span><span class="sxs-lookup"><span data-stu-id="4cd04-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="4cd04-136">Öğe &nbsp; &nbsp; silme</span><span class="sxs-lookup"><span data-stu-id="4cd04-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="4cd04-137">Yok</span><span class="sxs-lookup"><span data-stu-id="4cd04-137">None</span></span> | <span data-ttu-id="4cd04-138">Yok</span><span class="sxs-lookup"><span data-stu-id="4cd04-138">None</span></span>|

<span data-ttu-id="4cd04-139">Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-139">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki bir kutu ile temsil edilir.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="4cd04-145">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="4cd04-145">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4cd04-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="4cd04-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4cd04-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="4cd04-148">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="4cd04-149">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="4cd04-149">Create a web project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4cd04-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4cd04-151">**Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4cd04-152">**ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="4cd04-153">Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="4cd04-154">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,1** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="4cd04-155">**API** şablonunu seçin ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-155">Select the **API** template and click **Create**.</span></span>

![VS yeni proje iletişim kutusu](first-web-api/_static/vs3.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="4cd04-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4cd04-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4cd04-158">[Tümleşik terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)açın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="4cd04-159">Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="4cd04-160">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4cd04-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="4cd04-161">Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="4cd04-162">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="4cd04-162">The preceding commands:</span></span>

  * <span data-ttu-id="4cd04-163">Yeni bir Web API projesi oluşturur ve Visual Studio Code açar.</span><span class="sxs-lookup"><span data-stu-id="4cd04-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="4cd04-164">Sonraki bölümde gerekli olan NuGet paketlerini ekler.</span><span class="sxs-lookup"><span data-stu-id="4cd04-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="4cd04-165">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4cd04-166">**Yeni çözüm**> **Dosya** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-166">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="4cd04-168">**.NET Core** > **App** > **API** > ' **yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="4cd04-170">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **hedef Framework** \* *.NET Core 3,1*' i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.1*.</span></span>

* <span data-ttu-id="4cd04-171">**Proje adı** için *TodoApi* girin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="4cd04-173">Proje klasöründe bir komut terminali açın ve aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4cd04-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="4cd04-174">API'yi test etme</span><span class="sxs-lookup"><span data-stu-id="4cd04-174">Test the API</span></span>

<span data-ttu-id="4cd04-175">Proje şablonu bir `WeatherForecast` API 'SI oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4cd04-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="4cd04-176">Uygulamayı test etmek için bir tarayıcıdan `Get` yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4cd04-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4cd04-178">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="4cd04-179">Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>/WeatherForecast`gider.</span><span class="sxs-lookup"><span data-stu-id="4cd04-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="4cd04-180">IIS Express sertifikaya güvenip güvenmemeyi soran bir iletişim kutusu alırsanız **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="4cd04-181">Sonraki görüntülenen **güvenlik uyarısı** Iletişim kutusunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="4cd04-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4cd04-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4cd04-183">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="4cd04-184">Bir tarayıcıda aşağıdaki URL 'ye gidin: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="4cd04-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="4cd04-185">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="4cd04-186">Uygulamayı başlatmak için **hata ayıklamayı başlat** > **Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="4cd04-187">Mac için Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>`' a gider.</span><span class="sxs-lookup"><span data-stu-id="4cd04-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="4cd04-188">HTTP 404 (bulunamadı) hatası döndürülür.</span><span class="sxs-lookup"><span data-stu-id="4cd04-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="4cd04-189">URL 'ye `/WeatherForecast` ekleyin (URL 'yi `https://localhost:<port>/WeatherForecast`olarak değiştirin).</span><span class="sxs-lookup"><span data-stu-id="4cd04-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="4cd04-190">Aşağıdakine benzer bir JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="4cd04-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="4cd04-191">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="4cd04-191">Add a model class</span></span>

<span data-ttu-id="4cd04-192">*Model* , uygulamanın yönettiği verileri temsil eden bir sınıf kümesidir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="4cd04-193">Bu uygulamanın modeli, tek bir `TodoItem` sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4cd04-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4cd04-195">**Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="4cd04-196">**Yeni > klasör** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="4cd04-197">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="4cd04-198">*Modeller* klasörüne sağ tıklayın ve > **sınıfı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="4cd04-199">Sınıfı *TodoItem* olarak adlandırın ve **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="4cd04-200">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="4cd04-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4cd04-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4cd04-202">*Modeller*adlı bir klasör ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="4cd04-203">*Modeller* klasörüne aşağıdaki kodla bir `TodoItem` sınıfı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="4cd04-204">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4cd04-205">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-205">Right-click the project.</span></span> <span data-ttu-id="4cd04-206">**Yeni > klasör** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="4cd04-207">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-207">Name the folder *Models*.</span></span>

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="4cd04-209">*Modeller* klasörüne sağ tıklayın ve > **yeni dosya** **ekle** ' yi > **genel** > **boş sınıf**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="4cd04-210">Sınıfı *TodoItem*olarak adlandırın ve ardından **Yeni**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="4cd04-211">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs?name=snippet)]

<span data-ttu-id="4cd04-212">`Id` özelliği, ilişkisel bir veritabanındaki benzersiz anahtar olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="4cd04-213">Model sınıfları projede herhangi bir yere gidebilir, ancak *modeller* klasörü kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="4cd04-214">Veritabanı bağlamı Ekle</span><span class="sxs-lookup"><span data-stu-id="4cd04-214">Add a database context</span></span>

<span data-ttu-id="4cd04-215">*Veritabanı bağlamı* , bir veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="4cd04-216">Bu sınıf, `Microsoft.EntityFrameworkCore.DbContext` sınıfından türeterek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4cd04-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4cd04-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="4cd04-218">Microsoft. EntityFrameworkCore. SqlServer ekleyin</span><span class="sxs-lookup"><span data-stu-id="4cd04-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="4cd04-219">**Araçlar** menüsünde **nuget Paket Yöneticisi > çözüm Için NuGet Paketlerini Yönet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="4cd04-220">**Araştır** sekmesini seçin ve arama kutusuna **Microsoft. Entityframeworkcore. SqlServer** yazın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="4cd04-221">Sol bölmedeki **Microsoft. EntityFrameworkCore. SqlServer** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="4cd04-222">Sağ bölmedeki **Proje** onay kutusunu seçin ve ardından **Install**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="4cd04-223">`Microsoft.EntityFrameworkCore.InMemory` NuGet paketini eklemek için yukarıdaki yönergeleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet Paket Yöneticisi](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="4cd04-225">TodoContext veritabanı bağlamını ekleme</span><span class="sxs-lookup"><span data-stu-id="4cd04-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="4cd04-226">*Modeller* klasörüne sağ tıklayın ve > **sınıfı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="4cd04-227">Sınıfı *TodoContext* olarak adlandırın ve **Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="4cd04-228">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="4cd04-229">*Modeller* klasörüne bir `TodoContext` sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="4cd04-230">Aşağıdaki kodu girin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="4cd04-231">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="4cd04-231">Register the database context</span></span>

<span data-ttu-id="4cd04-232">ASP.NET Core, VERITABANı bağlamı gibi hizmetlerin [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısına kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="4cd04-233">Kapsayıcı hizmeti denetleyicilerine sağlar.</span><span class="sxs-lookup"><span data-stu-id="4cd04-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="4cd04-234">Aşağıdaki Vurgulanan kodla *Startup.cs* güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="4cd04-235">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="4cd04-235">The preceding code:</span></span>

* <span data-ttu-id="4cd04-236">Kullanılmayan `using` bildirimlerini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="4cd04-237">Veritabanı bağlamı DI kapsayıcıya ekler.</span><span class="sxs-lookup"><span data-stu-id="4cd04-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="4cd04-238">Veritabanı bağlamı bir bellek içi veritabanına kullanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="4cd04-239">Denetleyiciyi bir denetleyiciye katlama</span><span class="sxs-lookup"><span data-stu-id="4cd04-239">Scaffold a controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4cd04-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4cd04-241">*Denetleyiciler* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="4cd04-242">> **yeni yapı Iskelesi öğesi** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="4cd04-243">**Entity Framework kullanarak ve eylemler Içeren API denetleyicisi**' ni seçin ve ardından **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="4cd04-244">**API denetleyiciyi eylemler Ile Ekle ' de Entity Framework** iletişim kutusunu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="4cd04-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="4cd04-245">**Model sınıfında** **TodoItem (TodoApi. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="4cd04-246">**Veri bağlamı sınıfında** **TodoContext (TodoApi. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="4cd04-247">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="4cd04-248">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="4cd04-249">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4cd04-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="4cd04-250">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="4cd04-250">The preceding commands:</span></span>

* <span data-ttu-id="4cd04-251">Yapı iskelesi için gereken NuGet paketlerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="4cd04-252">Scafkatlama altyapısını (`dotnet-aspnet-codegenerator`) kurar.</span><span class="sxs-lookup"><span data-stu-id="4cd04-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="4cd04-253">`TodoItemsController`yapı iskelesi.</span><span class="sxs-lookup"><span data-stu-id="4cd04-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="4cd04-254">Oluşturulan kod:</span><span class="sxs-lookup"><span data-stu-id="4cd04-254">The generated code:</span></span>

* <span data-ttu-id="4cd04-255">Sınıfı [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle işaretler.</span><span class="sxs-lookup"><span data-stu-id="4cd04-255">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="4cd04-256">Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-256">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="4cd04-257">Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz. <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="4cd04-257">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="4cd04-258">Veritabanı bağlamını (`TodoContext`) denetleyiciye eklemek için DI kullanır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-258">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="4cd04-259">Veritabanı bağlamı, denetleyicideki [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) yöntemlerinde her birinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-259">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<span data-ttu-id="4cd04-260">İçin ASP.NET Core şablonları:</span><span class="sxs-lookup"><span data-stu-id="4cd04-260">The ASP.NET Core templates for:</span></span>

* <span data-ttu-id="4cd04-261">Görünümleri olan denetleyiciler yol şablonunda `[action]` içerir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-261">Controllers with views include `[action]` in the route template.</span></span>
* <span data-ttu-id="4cd04-262">API denetleyicileri yol şablonuna `[action]` içermez.</span><span class="sxs-lookup"><span data-stu-id="4cd04-262">API controllers don't include `[action]` in the route template.</span></span>

<span data-ttu-id="4cd04-263">`[action]` belirteci yol şablonunda olmadığında, [eylem](xref:mvc/controllers/routing#action) adı rotadan çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-263">When the `[action]` token isn't in the route template, the [action](xref:mvc/controllers/routing#action) name is excluded from the route.</span></span> <span data-ttu-id="4cd04-264">Diğer bir deyişle, eylemin ilişkili Yöntem adı eşleşen rotada kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="4cd04-264">That is, the action's associated method name isn't used in the matching route.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="4cd04-265">PostTodoItem Create metodunu inceleyin</span><span class="sxs-lookup"><span data-stu-id="4cd04-265">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="4cd04-266">`PostTodoItem` ' deki return ifadesini, [NameOf](/dotnet/csharp/language-reference/operators/nameof) işlecini kullanacak şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-266">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="4cd04-267">Yukarıdaki kod, [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliğiyle gösterildiği gıbı bır http post yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-267">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="4cd04-268">Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-268">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="4cd04-269"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4cd04-269">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="4cd04-270">Başarılı olursa bir HTTP 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="4cd04-270">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="4cd04-271">HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-271">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="4cd04-272">Yanıta bir [konum](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) üst bilgisi ekler.</span><span class="sxs-lookup"><span data-stu-id="4cd04-272">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="4cd04-273">`Location` üstbilgisi, yeni oluşturulan Yapılacaklar öğesinin [URI](https://developer.mozilla.org/docs/Glossary/URI) 'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-273">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="4cd04-274">Daha fazla bilgi için bkz. [10.2.2 201 oluşturma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="4cd04-274">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="4cd04-275">`Location` üst bilgisinin URI 'sini oluşturmak için `GetTodoItem` eyleme başvurur.</span><span class="sxs-lookup"><span data-stu-id="4cd04-275">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="4cd04-276">C# `nameof` anahtar sözcüğü, `CreatedAtAction` çağrısında eylem adının sabit kodlanmasını önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-276">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="4cd04-277">Postman yükleme</span><span class="sxs-lookup"><span data-stu-id="4cd04-277">Install Postman</span></span>

<span data-ttu-id="4cd04-278">Bu öğreticide Postman web API'si test etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-278">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="4cd04-279">[Postman](https://www.getpostman.com/downloads/) yükleme</span><span class="sxs-lookup"><span data-stu-id="4cd04-279">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="4cd04-280">Web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-280">Start the web app.</span></span>
* <span data-ttu-id="4cd04-281">Postman'i başlatın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-281">Start Postman.</span></span>
* <span data-ttu-id="4cd04-282">**SSL sertifikası doğrulamasını** devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="4cd04-282">Disable **SSL certificate verification**</span></span>
  * <span data-ttu-id="4cd04-283">**Dosya** > **ayarları** ' ndan (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-283">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="4cd04-284">Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-284">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="4cd04-285">Postman ile test PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="4cd04-285">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="4cd04-286">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4cd04-286">Create a new request.</span></span>
* <span data-ttu-id="4cd04-287">HTTP yöntemini `POST`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-287">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="4cd04-288">**Gövde** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-288">Select the **Body** tab.</span></span>
* <span data-ttu-id="4cd04-289">**Ham** radyo düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-289">Select the **raw** radio button.</span></span>
* <span data-ttu-id="4cd04-290">Türü **JSON (Application/JSON)** olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-290">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="4cd04-291">İstek gövdesinde bir yapılacak iş öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-291">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="4cd04-292">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-292">Select **Send**.</span></span>

  ![Postman ile isteği oluştur](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="4cd04-294">Konum üst bilgisi URI test</span><span class="sxs-lookup"><span data-stu-id="4cd04-294">Test the location header URI</span></span>

* <span data-ttu-id="4cd04-295">**Yanıt** bölmesinde **üstbilgiler** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-295">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="4cd04-296">**Konum** üst bilgisi değerini kopyalayın:</span><span class="sxs-lookup"><span data-stu-id="4cd04-296">Copy the **Location** header value:</span></span>

  ![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/3/create.png)

* <span data-ttu-id="4cd04-298">Yöntemini GET öğesine Ayarla.</span><span class="sxs-lookup"><span data-stu-id="4cd04-298">Set the method to GET.</span></span>
* <span data-ttu-id="4cd04-299">URI 'yi yapıştırın (örneğin, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="4cd04-299">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="4cd04-300">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-300">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="4cd04-301">GET yöntemlerini inceleyin</span><span class="sxs-lookup"><span data-stu-id="4cd04-301">Examine the GET methods</span></span>

<span data-ttu-id="4cd04-302">İki GET uç noktası bu yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="4cd04-302">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="4cd04-303">Tarayıcıdan veya Postman 'dan iki uç noktayı çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-303">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="4cd04-304">Örnek:</span><span class="sxs-lookup"><span data-stu-id="4cd04-304">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="4cd04-305">Aşağıdakine benzer bir yanıt, `GetTodoItems`çağrısı tarafından üretilir:</span><span class="sxs-lookup"><span data-stu-id="4cd04-305">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="4cd04-306">Postman ile test al</span><span class="sxs-lookup"><span data-stu-id="4cd04-306">Test Get with Postman</span></span>

* <span data-ttu-id="4cd04-307">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4cd04-307">Create a new request.</span></span>
* <span data-ttu-id="4cd04-308">**Almak**için http yöntemini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-308">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="4cd04-309">İstek URL 'sini `https://localhost:<port>/api/TodoItems`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-309">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="4cd04-310">Örneğin, `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="4cd04-310">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="4cd04-311">Postman 'da **iki bölme görünümü** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-311">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="4cd04-312">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-312">Select **Send**.</span></span>

<span data-ttu-id="4cd04-313">Bu uygulama, bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-313">This app uses an in-memory database.</span></span> <span data-ttu-id="4cd04-314">Uygulama durdurulup başlatılırsa, önceki GET isteği herhangi bir veri döndürmez.</span><span class="sxs-lookup"><span data-stu-id="4cd04-314">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="4cd04-315">Hiçbir veri döndürülmezse, verileri uygulamaya [gönderin](#post) .</span><span class="sxs-lookup"><span data-stu-id="4cd04-315">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="4cd04-316">URL Yönlendirme ve yolları</span><span class="sxs-lookup"><span data-stu-id="4cd04-316">Routing and URL paths</span></span>

<span data-ttu-id="4cd04-317">[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) ÖZNITELIĞI BIR HTTP GET isteğine yanıt veren bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-317">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="4cd04-318">Her yöntem için URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="4cd04-318">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="4cd04-319">Denetleyicinin `Route` özniteliğinde şablon dizesiyle başlayın:</span><span class="sxs-lookup"><span data-stu-id="4cd04-319">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="4cd04-320">`[controller]`, denetleyicinin adıyla değiştirin, bu kural, denetleyici sınıf adı "denetleyici" sonekidir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-320">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="4cd04-321">Bu örnek için denetleyici sınıfı adı **todoıtems**denetleyicisidir, bu nedenle denetleyicinin adı "todoıtems" olur.</span><span class="sxs-lookup"><span data-stu-id="4cd04-321">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="4cd04-322">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-322">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="4cd04-323">`[HttpGet]` özniteliğinin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-323">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="4cd04-324">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="4cd04-324">This sample doesn't use a template.</span></span> <span data-ttu-id="4cd04-325">Daha fazla bilgi için bkz. [http [fiil] öznitelikleriyle öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="4cd04-325">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="4cd04-326">Aşağıdaki `GetTodoItem` yönteminde, `"{id}"` Yapılacaklar öğesinin benzersiz tanımlayıcısı için bir yer tutucu değişkenidir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-326">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="4cd04-327">`GetTodoItem` çağrıldığında, URL 'deki `"{id}"` değeri `id` parametresindeki yöntemine sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-327">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="4cd04-328">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="4cd04-328">Return values</span></span>

<span data-ttu-id="4cd04-329">`GetTodoItems` ve `GetTodoItem` yöntemlerinin dönüş türü [actionresult\<t > türüdür](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="4cd04-329">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="4cd04-330">ASP.NET Core nesneyi [JSON](https://www.json.org/) 'a otomatik olarak serileştirir ve yanıt ILETISININ gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="4cd04-330">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="4cd04-331">Yanıt kodu 200 bu dönüş türü için olduğu varsayılırsa işlenmeyen özel durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-331">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="4cd04-332">İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-332">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="4cd04-333">`ActionResult` dönüş türleri, çok çeşitli HTTP durum kodlarını temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-333">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="4cd04-334">Örneğin, `GetTodoItem` iki farklı durum değeri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="4cd04-334">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="4cd04-335">İstenen KIMLIKLE eşleşen hiçbir öğe yoksa, yöntem 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="4cd04-335">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="4cd04-336">Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="4cd04-336">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="4cd04-337">`item` sonuçları bir HTTP 200 yanıtına döndürülüyor.</span><span class="sxs-lookup"><span data-stu-id="4cd04-337">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="4cd04-338">PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="4cd04-338">The PutTodoItem method</span></span>

<span data-ttu-id="4cd04-339">`PutTodoItem` yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-339">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="4cd04-340">`PutTodoItem`, HTTP PUT kullanması dışında `PostTodoItem`benzerdir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-340">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="4cd04-341">Yanıt 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="4cd04-341">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="4cd04-342">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca değişiklikler değil göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-342">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="4cd04-343">Kısmi güncelleştirmeleri desteklemek için [http Patch](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)kullanın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-343">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="4cd04-344">`PutTodoItem`çağırırken bir hata alırsanız, veritabanında bir öğe olduğundan emin olmak için `GET` çağırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-344">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="4cd04-345">Test PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="4cd04-345">Test the PutTodoItem method</span></span>

<span data-ttu-id="4cd04-346">Bu örnek, uygulama her başlatıldığında başlatılmış olması gereken bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-346">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="4cd04-347">Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-347">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="4cd04-348">PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-348">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="4cd04-349">ID = 1 olan Yapılacaklar öğesini güncelleştirin ve adını "Feed balık" olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="4cd04-349">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="4cd04-350">Aşağıdaki görüntüde, Postman güncelleştirme gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="4cd04-350">The following image shows the Postman update:</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="4cd04-352">DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="4cd04-352">The DeleteTodoItem method</span></span>

<span data-ttu-id="4cd04-353">`DeleteTodoItem` yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-353">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="4cd04-354">Test DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="4cd04-354">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="4cd04-355">Postman bir yapılacak iş öğesini silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="4cd04-355">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="4cd04-356">Yöntemini `DELETE`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-356">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="4cd04-357">Silinecek nesnenin URI 'sini ayarlayın (örneğin `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="4cd04-357">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="4cd04-358">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-358">Select **Send**.</span></span>

<a name="over-post"></a>

## <a name="prevent-over-posting"></a><span data-ttu-id="4cd04-359">Fazla nakletmeyi engelle</span><span class="sxs-lookup"><span data-stu-id="4cd04-359">Prevent over-posting</span></span>

<span data-ttu-id="4cd04-360">Şu anda örnek uygulama, tüm `TodoItem` nesnesini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="4cd04-360">Currently the sample app exposes the entire `TodoItem` object.</span></span> <span data-ttu-id="4cd04-361">Üretim uygulamaları tipik olarak, bir modelin alt kümesini kullanarak girdi ve döndürülen verileri sınırlandırır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-361">Productions apps typically limit the data that's input and returned using a subset of the model.</span></span> <span data-ttu-id="4cd04-362">Bunun arkasında birden çok neden vardır ve güvenlik önemli bir değer.</span><span class="sxs-lookup"><span data-stu-id="4cd04-362">There are multiple reasons behind this and security is a major one.</span></span> <span data-ttu-id="4cd04-363">Bir modelin alt kümesi genellikle Veri Aktarımı nesnesi (DTO), giriş modeli veya görünüm modeli olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-363">The subset of a model is usually referred to as a Data Transfer Object (DTO), input model, or view model.</span></span> <span data-ttu-id="4cd04-364">Bu makalede **DTO** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-364">**DTO** is used in this article.</span></span>

<span data-ttu-id="4cd04-365">Bir DTO için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="4cd04-365">A DTO may be used to:</span></span>

* <span data-ttu-id="4cd04-366">Fazla nakletmeyi önleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-366">Prevent over-posting.</span></span>
* <span data-ttu-id="4cd04-367">İstemcilerin görüntülemesi beklenen özellikleri gizleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-367">Hide properties that clients are not supposed to view.</span></span>
* <span data-ttu-id="4cd04-368">Yük boyutunu azaltmak için bazı özellikleri atlayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-368">Omit some properties in order to reduce payload size.</span></span>
* <span data-ttu-id="4cd04-369">İç içe geçmiş nesneler içeren nesne grafiklerini düzleştirin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-369">Flatten object graphs that contain nested objects.</span></span> <span data-ttu-id="4cd04-370">Düzleştirilmiş nesne grafikleri istemciler için daha uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-370">Flattened object graphs can be more convenient for clients.</span></span>

<span data-ttu-id="4cd04-371">DTO yaklaşımını göstermek için `TodoItem` sınıfını gizli bir alan içerecek şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-371">To demonstrate the DTO approach, update the `TodoItem` class to include a secret field:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Models/TodoItem.cs?name=snippet&highlight=6)]

<span data-ttu-id="4cd04-372">Gizli alanın bu uygulamadan gizlenmesi gerekir, ancak bir yönetim uygulaması onu kullanıma sunmayı seçebilir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-372">The secret field needs to be hidden from this app, but an administrative app could choose to expose it.</span></span>

<span data-ttu-id="4cd04-373">Gizli dizi alanını nakledebildiğinizi ve alabilirim.</span><span class="sxs-lookup"><span data-stu-id="4cd04-373">Verify you can post and get the secret field.</span></span>

<span data-ttu-id="4cd04-374">Bir DTO modeli oluşturun:</span><span class="sxs-lookup"><span data-stu-id="4cd04-374">Create a DTO model:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Models/TodoItemDTO.cs?name=snippet)]

<span data-ttu-id="4cd04-375">`TodoItemsController` `TodoItemDTO`kullanacak şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-375">Update the `TodoItemsController` to use `TodoItemDTO`:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet)]

<span data-ttu-id="4cd04-376">Gizli dizi alanını nakledemeyeceğinizi veya alamazsınız.</span><span class="sxs-lookup"><span data-stu-id="4cd04-376">Verify you can't post or get the secret field.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="4cd04-377">JavaScript ile Web API 'sini çağırma</span><span class="sxs-lookup"><span data-stu-id="4cd04-377">Call the web API with JavaScript</span></span>

<span data-ttu-id="4cd04-378">Bkz. [öğretici: JavaScript ile ASP.NET Core Web API 'Si çağırma](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="4cd04-378">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4cd04-379">Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="4cd04-379">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4cd04-380">Bir Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4cd04-380">Create a web API project.</span></span>
> * <span data-ttu-id="4cd04-381">Bir model sınıfı ve bir veritabanı bağlamı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-381">Add a model class and a database context.</span></span>
> * <span data-ttu-id="4cd04-382">Bir denetleyici ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="4cd04-382">Add a controller.</span></span>
> * <span data-ttu-id="4cd04-383">CRUD yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-383">Add CRUD methods.</span></span>
> * <span data-ttu-id="4cd04-384">Yönlendirmeyi Yapılandırma ve URL yolu.</span><span class="sxs-lookup"><span data-stu-id="4cd04-384">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="4cd04-385">Dönüş değerleri belirtin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-385">Specify return values.</span></span>
> * <span data-ttu-id="4cd04-386">Web API'si Postman ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-386">Call the web API with Postman.</span></span>
> * <span data-ttu-id="4cd04-387">JavaScript ile Web API 'sini çağırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-387">Call the web API with JavaScript.</span></span>

<span data-ttu-id="4cd04-388">Sonunda, web API'si "Yapılacaklar" öğelerini ilişkisel bir veritabanında depolanan yönetebileceği sahip.</span><span class="sxs-lookup"><span data-stu-id="4cd04-388">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="4cd04-389">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="4cd04-389">Overview</span></span>

<span data-ttu-id="4cd04-390">Bu öğretici yandaki API oluşturur:</span><span class="sxs-lookup"><span data-stu-id="4cd04-390">This tutorial creates the following API:</span></span>

|<span data-ttu-id="4cd04-391">API</span><span class="sxs-lookup"><span data-stu-id="4cd04-391">API</span></span> | <span data-ttu-id="4cd04-392">Açıklama</span><span class="sxs-lookup"><span data-stu-id="4cd04-392">Description</span></span> | <span data-ttu-id="4cd04-393">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="4cd04-393">Request body</span></span> | <span data-ttu-id="4cd04-394">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="4cd04-394">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="4cd04-395">/Api/TodoItems al</span><span class="sxs-lookup"><span data-stu-id="4cd04-395">GET /api/TodoItems</span></span> | <span data-ttu-id="4cd04-396">Tüm yapılacak iş öğeleri al</span><span class="sxs-lookup"><span data-stu-id="4cd04-396">Get all to-do items</span></span> | <span data-ttu-id="4cd04-397">Yok</span><span class="sxs-lookup"><span data-stu-id="4cd04-397">None</span></span> | <span data-ttu-id="4cd04-398">Yapılacaklar öğelerinin bir dizisi</span><span class="sxs-lookup"><span data-stu-id="4cd04-398">Array of to-do items</span></span>|
|<span data-ttu-id="4cd04-399">/Api/TodoItems/{id} al</span><span class="sxs-lookup"><span data-stu-id="4cd04-399">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="4cd04-400">Bir öğeyi Kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="4cd04-400">Get an item by ID</span></span> | <span data-ttu-id="4cd04-401">Yok</span><span class="sxs-lookup"><span data-stu-id="4cd04-401">None</span></span> | <span data-ttu-id="4cd04-402">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="4cd04-402">To-do item</span></span>|
|<span data-ttu-id="4cd04-403">POST/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="4cd04-403">POST /api/TodoItems</span></span> | <span data-ttu-id="4cd04-404">Yeni Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="4cd04-404">Add a new item</span></span> | <span data-ttu-id="4cd04-405">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="4cd04-405">To-do item</span></span> | <span data-ttu-id="4cd04-406">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="4cd04-406">To-do item</span></span> |
|<span data-ttu-id="4cd04-407">/Api/TodoItems/{id} koy</span><span class="sxs-lookup"><span data-stu-id="4cd04-407">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="4cd04-408">Mevcut bir öğeyi güncelleştirin &nbsp;</span><span class="sxs-lookup"><span data-stu-id="4cd04-408">Update an existing item &nbsp;</span></span> | <span data-ttu-id="4cd04-409">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="4cd04-409">To-do item</span></span> | <span data-ttu-id="4cd04-410">Yok</span><span class="sxs-lookup"><span data-stu-id="4cd04-410">None</span></span> |
|<span data-ttu-id="4cd04-411">/Api/TodoItems/{id} &nbsp; SIL &nbsp;</span><span class="sxs-lookup"><span data-stu-id="4cd04-411">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="4cd04-412">Öğe &nbsp; &nbsp; silme</span><span class="sxs-lookup"><span data-stu-id="4cd04-412">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="4cd04-413">Yok</span><span class="sxs-lookup"><span data-stu-id="4cd04-413">None</span></span> | <span data-ttu-id="4cd04-414">Yok</span><span class="sxs-lookup"><span data-stu-id="4cd04-414">None</span></span>|

<span data-ttu-id="4cd04-415">Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-415">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki bir kutu ile temsil edilir.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="4cd04-421">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="4cd04-421">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4cd04-422">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-422">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="4cd04-423">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4cd04-423">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="4cd04-424">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-424">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="4cd04-425">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="4cd04-425">Create a web project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4cd04-426">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-426">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4cd04-427">**Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-427">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4cd04-428">**ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-428">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="4cd04-429">Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-429">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="4cd04-430">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 2,2** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-430">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="4cd04-431">**API** şablonunu seçin ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-431">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="4cd04-432">**Docker desteğini etkinleştir** **' i seçmeyin** .</span><span class="sxs-lookup"><span data-stu-id="4cd04-432">**Don't** select **Enable Docker Support**.</span></span>

![VS yeni proje iletişim kutusu](first-web-api/_static/vs.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="4cd04-434">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4cd04-434">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4cd04-435">[Tümleşik terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)açın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-435">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="4cd04-436">Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-436">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="4cd04-437">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4cd04-437">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="4cd04-438">Bu komutlar yeni bir Web API projesi oluşturur ve yeni proje klasöründe Visual Studio Code yeni bir örneğini açar.</span><span class="sxs-lookup"><span data-stu-id="4cd04-438">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="4cd04-439">Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-439">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="4cd04-440">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4cd04-441">**Yeni çözüm**> **Dosya** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-441">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="4cd04-443">**.NET Core** > **App** > **API** > ' **yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-443">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="4cd04-445">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, \* *.NET Core 2,2*' un varsayılan **hedef çerçevesini** kabul edin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-445">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="4cd04-446">**Proje adı** için *TodoApi* girin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-446">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="4cd04-448">API'yi test etme</span><span class="sxs-lookup"><span data-stu-id="4cd04-448">Test the API</span></span>

<span data-ttu-id="4cd04-449">Proje şablonu bir `values` API 'SI oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4cd04-449">The project template creates a `values` API.</span></span> <span data-ttu-id="4cd04-450">Uygulamayı test etmek için bir tarayıcıdan `Get` yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-450">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4cd04-451">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-451">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4cd04-452">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-452">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="4cd04-453">Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>/api/values`gider.</span><span class="sxs-lookup"><span data-stu-id="4cd04-453">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="4cd04-454">IIS Express sertifikaya güvenip güvenmemeyi soran bir iletişim kutusu alırsanız **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-454">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="4cd04-455">Sonraki görüntülenen **güvenlik uyarısı** Iletişim kutusunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-455">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="4cd04-456">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4cd04-456">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4cd04-457">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-457">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="4cd04-458">Bir tarayıcıda aşağıdaki URL 'ye gidin: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="4cd04-458">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="4cd04-459">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-459">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="4cd04-460">Uygulamayı başlatmak için **hata ayıklamayı başlat** > **Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-460">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="4cd04-461">Mac için Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>`' a gider.</span><span class="sxs-lookup"><span data-stu-id="4cd04-461">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="4cd04-462">HTTP 404 (bulunamadı) hatası döndürülür.</span><span class="sxs-lookup"><span data-stu-id="4cd04-462">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="4cd04-463">URL 'ye `/api/values` ekleyin (URL 'yi `https://localhost:<port>/api/values`olarak değiştirin).</span><span class="sxs-lookup"><span data-stu-id="4cd04-463">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="4cd04-464">Aşağıdaki JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="4cd04-464">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="4cd04-465">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="4cd04-465">Add a model class</span></span>

<span data-ttu-id="4cd04-466">*Model* , uygulamanın yönettiği verileri temsil eden bir sınıf kümesidir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-466">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="4cd04-467">Bu uygulamanın modeli, tek bir `TodoItem` sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-467">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4cd04-468">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-468">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4cd04-469">**Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-469">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="4cd04-470">**Yeni > klasör** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-470">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="4cd04-471">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-471">Name the folder *Models*.</span></span>

* <span data-ttu-id="4cd04-472">*Modeller* klasörüne sağ tıklayın ve > **sınıfı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-472">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="4cd04-473">Sınıfı *TodoItem* olarak adlandırın ve **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-473">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="4cd04-474">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-474">Replace the template code with the following code:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="4cd04-475">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4cd04-475">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4cd04-476">*Modeller*adlı bir klasör ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-476">Add a folder named *Models*.</span></span>

* <span data-ttu-id="4cd04-477">*Modeller* klasörüne aşağıdaki kodla bir `TodoItem` sınıfı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-477">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="4cd04-478">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-478">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4cd04-479">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-479">Right-click the project.</span></span> <span data-ttu-id="4cd04-480">**Yeni > klasör** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-480">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="4cd04-481">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-481">Name the folder *Models*.</span></span>

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="4cd04-483">*Modeller* klasörüne sağ tıklayın ve > **yeni dosya** **ekle** ' yi > **genel** > **boş sınıf**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-483">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="4cd04-484">Sınıfı *TodoItem*olarak adlandırın ve ardından **Yeni**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-484">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="4cd04-485">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-485">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="4cd04-486">`Id` özelliği, ilişkisel bir veritabanındaki benzersiz anahtar olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-486">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="4cd04-487">Model sınıfları projede herhangi bir yere gidebilir, ancak *modeller* klasörü kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-487">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="4cd04-488">Veritabanı bağlamı Ekle</span><span class="sxs-lookup"><span data-stu-id="4cd04-488">Add a database context</span></span>

<span data-ttu-id="4cd04-489">*Veritabanı bağlamı* , bir veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-489">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="4cd04-490">Bu sınıf, `Microsoft.EntityFrameworkCore.DbContext` sınıfından türeterek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4cd04-490">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4cd04-491">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-491">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4cd04-492">*Modeller* klasörüne sağ tıklayın ve > **sınıfı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-492">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="4cd04-493">Sınıfı *TodoContext* olarak adlandırın ve **Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-493">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="4cd04-494">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-494">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="4cd04-495">*Modeller* klasörüne bir `TodoContext` sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-495">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="4cd04-496">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-496">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="4cd04-497">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="4cd04-497">Register the database context</span></span>

<span data-ttu-id="4cd04-498">ASP.NET Core, VERITABANı bağlamı gibi hizmetlerin [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısına kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-498">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="4cd04-499">Kapsayıcı hizmeti denetleyicilerine sağlar.</span><span class="sxs-lookup"><span data-stu-id="4cd04-499">The container provides the service to controllers.</span></span>

<span data-ttu-id="4cd04-500">Aşağıdaki Vurgulanan kodla *Startup.cs* güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-500">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="4cd04-501">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="4cd04-501">The preceding code:</span></span>

* <span data-ttu-id="4cd04-502">Kullanılmayan `using` bildirimlerini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-502">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="4cd04-503">Veritabanı bağlamı DI kapsayıcıya ekler.</span><span class="sxs-lookup"><span data-stu-id="4cd04-503">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="4cd04-504">Veritabanı bağlamı bir bellek içi veritabanına kullanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-504">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="4cd04-505">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="4cd04-505">Add a controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4cd04-506">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-506">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4cd04-507">*Denetleyiciler* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-507">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="4cd04-508">> **Yeni öğe** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-508">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="4cd04-509">**Yeni öğe Ekle** iletişim kutusunda, **API denetleyici sınıfı** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-509">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="4cd04-510">Sınıfı *TodoController*olarak adlandırın ve **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-510">Name the class *TodoController*, and select **Add**.</span></span>

  ![Yeni öğe iletişim denetleyicisiyle seçilen arama kutusu ve web API denetleyicisi Ekle](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="4cd04-512">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-512">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="4cd04-513">*Denetleyiciler* klasöründe `TodoController`adlı bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4cd04-513">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="4cd04-514">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-514">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="4cd04-515">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="4cd04-515">The preceding code:</span></span>

* <span data-ttu-id="4cd04-516">Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4cd04-516">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="4cd04-517">Sınıfı [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle işaretler.</span><span class="sxs-lookup"><span data-stu-id="4cd04-517">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="4cd04-518">Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-518">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="4cd04-519">Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz. <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="4cd04-519">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="4cd04-520">Veritabanı bağlamını (`TodoContext`) denetleyiciye eklemek için DI kullanır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-520">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="4cd04-521">Veritabanı bağlamı, denetleyicideki [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) yöntemlerinde her birinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-521">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="4cd04-522">Veritabanı boşsa, veritabanına `Item1` adlı bir öğe ekler.</span><span class="sxs-lookup"><span data-stu-id="4cd04-522">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="4cd04-523">Her çalıştığında bu kod oluşturucusunun içinde yeni bir HTTP isteği olduğundan.</span><span class="sxs-lookup"><span data-stu-id="4cd04-523">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="4cd04-524">Tüm öğeleri silerseniz, bir API yönteminin bir sonraki çağrılışında Oluşturucu `Item1` yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4cd04-524">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="4cd04-525">Bu nedenle, gerçekten işe yaradı silme işlemi işe yaramadı gibi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-525">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="4cd04-526">Get yöntemleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="4cd04-526">Add Get methods</span></span>

<span data-ttu-id="4cd04-527">Yapılacaklar öğelerini alan bir API sağlamak için, `TodoController` sınıfına aşağıdaki yöntemleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-527">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="4cd04-528">İki GET uç noktası bu yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="4cd04-528">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="4cd04-529">Hala çalışıyorsa uygulamayı durdurun.</span><span class="sxs-lookup"><span data-stu-id="4cd04-529">Stop the app if it's still running.</span></span> <span data-ttu-id="4cd04-530">Ardından, en son değişiklikleri dahil etmek için yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-530">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="4cd04-531">Bir tarayıcıdan iki uç nokta çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-531">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="4cd04-532">Örnek:</span><span class="sxs-lookup"><span data-stu-id="4cd04-532">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="4cd04-533">Aşağıdaki HTTP yanıtı `GetTodoItems`çağrısı tarafından üretilir:</span><span class="sxs-lookup"><span data-stu-id="4cd04-533">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="4cd04-534">URL Yönlendirme ve yolları</span><span class="sxs-lookup"><span data-stu-id="4cd04-534">Routing and URL paths</span></span>

<span data-ttu-id="4cd04-535">[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) ÖZNITELIĞI BIR HTTP GET isteğine yanıt veren bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-535">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="4cd04-536">Her yöntem için URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="4cd04-536">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="4cd04-537">Denetleyicinin `Route` özniteliğinde şablon dizesiyle başlayın:</span><span class="sxs-lookup"><span data-stu-id="4cd04-537">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="4cd04-538">`[controller]`, denetleyicinin adıyla değiştirin, bu kural, denetleyici sınıf adı "denetleyici" sonekidir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-538">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="4cd04-539">Bu örnek için denetleyici sınıf adı **Todo**Controller olduğundan, denetleyici adı "Todo" olur.</span><span class="sxs-lookup"><span data-stu-id="4cd04-539">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="4cd04-540">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-540">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="4cd04-541">`[HttpGet]` özniteliğinin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-541">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="4cd04-542">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="4cd04-542">This sample doesn't use a template.</span></span> <span data-ttu-id="4cd04-543">Daha fazla bilgi için bkz. [http [fiil] öznitelikleriyle öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="4cd04-543">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="4cd04-544">Aşağıdaki `GetTodoItem` yönteminde, `"{id}"` Yapılacaklar öğesinin benzersiz tanımlayıcısı için bir yer tutucu değişkenidir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-544">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="4cd04-545">`GetTodoItem` çağrıldığında, URL 'deki `"{id}"` değeri`id` parametresindeki yöntemine sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-545">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="4cd04-546">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="4cd04-546">Return values</span></span>

<span data-ttu-id="4cd04-547">`GetTodoItems` ve `GetTodoItem` yöntemlerinin dönüş türü [actionresult\<t > türüdür](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="4cd04-547">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="4cd04-548">ASP.NET Core nesneyi [JSON](https://www.json.org/) 'a otomatik olarak serileştirir ve yanıt ILETISININ gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="4cd04-548">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="4cd04-549">Yanıt kodu 200 bu dönüş türü için olduğu varsayılırsa işlenmeyen özel durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-549">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="4cd04-550">İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-550">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="4cd04-551">`ActionResult` dönüş türleri, çok çeşitli HTTP durum kodlarını temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-551">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="4cd04-552">Örneğin, `GetTodoItem` iki farklı durum değeri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="4cd04-552">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="4cd04-553">İstenen KIMLIKLE eşleşen hiçbir öğe yoksa, yöntem 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="4cd04-553">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="4cd04-554">Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="4cd04-554">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="4cd04-555">`item` sonuçları bir HTTP 200 yanıtına döndürülüyor.</span><span class="sxs-lookup"><span data-stu-id="4cd04-555">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="4cd04-556">Test GetTodoItems yöntemi</span><span class="sxs-lookup"><span data-stu-id="4cd04-556">Test the GetTodoItems method</span></span>

<span data-ttu-id="4cd04-557">Bu öğreticide Postman web API'si test etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-557">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="4cd04-558">[Postman](https://www.getpostman.com/downloads/)'yi yükleme.</span><span class="sxs-lookup"><span data-stu-id="4cd04-558">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="4cd04-559">Web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-559">Start the web app.</span></span>
* <span data-ttu-id="4cd04-560">Postman'i başlatın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-560">Start Postman.</span></span>
* <span data-ttu-id="4cd04-561">**SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-561">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4cd04-562">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-562">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4cd04-563">**Dosya** > **ayarları** ' ndan (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-563">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="4cd04-564">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd04-564">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="4cd04-565">**Postman** > **tercihleri** ' nden (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-565">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="4cd04-566">Alternatif olarak, wranı seçin ve **Ayarlar**' ı seçip SSL sertifikası doğrulamasını devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-566">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="4cd04-567">Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-567">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="4cd04-568">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4cd04-568">Create a new request.</span></span>
  * <span data-ttu-id="4cd04-569">**Almak**için http yöntemini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-569">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="4cd04-570">İstek URL 'sini `https://localhost:<port>/api/todo`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-570">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="4cd04-571">Örneğin, `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="4cd04-571">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="4cd04-572">Postman 'da **iki bölme görünümü** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-572">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="4cd04-573">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-573">Select **Send**.</span></span>

![Get isteğiyle postman](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="4cd04-575">Create yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="4cd04-575">Add a Create method</span></span>

<span data-ttu-id="4cd04-576">Aşağıdaki `PostTodoItem` yöntemini *Controllers/TodoController. cs*içine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-576">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="4cd04-577">Yukarıdaki kod, [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliğiyle gösterildiği gıbı bır http post yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-577">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="4cd04-578">Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-578">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="4cd04-579">`CreatedAtAction` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4cd04-579">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="4cd04-580">Başarılı olursa bir HTTP 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="4cd04-580">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="4cd04-581">HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-581">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="4cd04-582">Yanıta bir `Location` üst bilgisi ekler.</span><span class="sxs-lookup"><span data-stu-id="4cd04-582">Adds a `Location` header to the response.</span></span> <span data-ttu-id="4cd04-583">`Location` üstbilgisi, yeni oluşturulan Yapılacaklar öğesinin URI 'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-583">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="4cd04-584">Daha fazla bilgi için bkz. [10.2.2 201 oluşturma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="4cd04-584">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="4cd04-585">`Location` üst bilgisinin URI 'sini oluşturmak için `GetTodoItem` eyleme başvurur.</span><span class="sxs-lookup"><span data-stu-id="4cd04-585">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="4cd04-586">C# `nameof` anahtar sözcüğü, `CreatedAtAction` çağrısında eylem adının sabit kodlanmasını önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-586">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="4cd04-587">Test PostTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="4cd04-587">Test the PostTodoItem method</span></span>

* <span data-ttu-id="4cd04-588">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4cd04-588">Build the project.</span></span>
* <span data-ttu-id="4cd04-589">Postman 'da HTTP yöntemini `POST`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-589">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="4cd04-590">**Gövde** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-590">Select the **Body** tab.</span></span>
* <span data-ttu-id="4cd04-591">**Ham** radyo düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-591">Select the **raw** radio button.</span></span>
* <span data-ttu-id="4cd04-592">Türü **JSON (Application/JSON)** olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-592">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="4cd04-593">İstek gövdesinde bir yapılacak iş öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-593">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="4cd04-594">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-594">Select **Send**.</span></span>

  ![Postman ile isteği oluştur](first-web-api/_static/create.png)

  <span data-ttu-id="4cd04-596">Bir 405 yöntemine Izin verilmiyor hatası alırsanız, `PostTodoItem` yöntemi eklendikten sonra projenin derlenmesinin sonucu büyük olasılıkla oluşur.</span><span class="sxs-lookup"><span data-stu-id="4cd04-596">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="4cd04-597">Konum üst bilgisi URI test</span><span class="sxs-lookup"><span data-stu-id="4cd04-597">Test the location header URI</span></span>

* <span data-ttu-id="4cd04-598">**Yanıt** bölmesinde **üstbilgiler** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-598">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="4cd04-599">**Konum** üst bilgisi değerini kopyalayın:</span><span class="sxs-lookup"><span data-stu-id="4cd04-599">Copy the **Location** header value:</span></span>

  ![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmc2.png)

* <span data-ttu-id="4cd04-601">Yöntemini GET öğesine Ayarla.</span><span class="sxs-lookup"><span data-stu-id="4cd04-601">Set the method to GET.</span></span>
* <span data-ttu-id="4cd04-602">URI 'yi yapıştırın (örneğin, `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="4cd04-602">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="4cd04-603">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-603">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="4cd04-604">PutTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="4cd04-604">Add a PutTodoItem method</span></span>

<span data-ttu-id="4cd04-605">Aşağıdaki `PutTodoItem` yöntemi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-605">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="4cd04-606">`PutTodoItem`, HTTP PUT kullanması dışında `PostTodoItem`benzerdir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-606">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="4cd04-607">Yanıt 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="4cd04-607">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="4cd04-608">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca değişiklikler değil göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-608">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="4cd04-609">Kısmi güncelleştirmeleri desteklemek için [http Patch](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)kullanın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-609">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="4cd04-610">`PutTodoItem`çağırırken bir hata alırsanız, veritabanında bir öğe olduğundan emin olmak için `GET` çağırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-610">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="4cd04-611">Test PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="4cd04-611">Test the PutTodoItem method</span></span>

<span data-ttu-id="4cd04-612">Bu örnek, uygulama her başlatıldığında başlatılmış olması gereken bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-612">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="4cd04-613">Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-613">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="4cd04-614">PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-614">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="4cd04-615">Kimliğine sahip bir yapılacak iş öğesi güncelleştirme = 1 ve "balık akış için" adını girin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-615">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="4cd04-616">Aşağıdaki görüntüde, Postman güncelleştirme gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="4cd04-616">The following image shows the Postman update:</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="4cd04-618">DeleteTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="4cd04-618">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="4cd04-619">Aşağıdaki `DeleteTodoItem` yöntemi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-619">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="4cd04-620">`DeleteTodoItem` yanıtı 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="4cd04-620">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="4cd04-621">Test DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="4cd04-621">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="4cd04-622">Postman bir yapılacak iş öğesini silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="4cd04-622">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="4cd04-623">Yöntemini `DELETE`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-623">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="4cd04-624">Silinecek nesnenin URI 'sini ayarlayın (örneğin `https://localhost:5001/api/todo/1`).</span><span class="sxs-lookup"><span data-stu-id="4cd04-624">Set the URI of the object to delete (for example `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="4cd04-625">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-625">Select **Send**.</span></span>

<span data-ttu-id="4cd04-626">Örnek uygulama, tüm öğeleri silmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="4cd04-626">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="4cd04-627">Ancak, son öğe silindiğinde, API 'nin bir sonraki çağrılışında model sınıfı Oluşturucu tarafından yeni bir tane oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4cd04-627">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="4cd04-628">JavaScript ile Web API 'sini çağırma</span><span class="sxs-lookup"><span data-stu-id="4cd04-628">Call the web API with JavaScript</span></span>

<span data-ttu-id="4cd04-629">Bu bölümde, Web API 'sini çağırmak için JavaScript kullanan bir HTML sayfası eklenir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-629">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="4cd04-630">jQuery isteği başlatır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-630">jQuery initiates the request.</span></span> <span data-ttu-id="4cd04-631">JavaScript, sayfayı Web API 'sinin yanıtından alınan ayrıntılarla güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-631">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="4cd04-632">Uygulamayı [statik dosyalara sunacak](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) şekilde yapılandırın ve aşağıdaki vurgulanmış kodla *Startup.cs* güncelleştirerek [varsayılan dosya eşlemesini etkinleştirin](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) :</span><span class="sxs-lookup"><span data-stu-id="4cd04-632">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="4cd04-633">Proje dizininde bir *Wwwroot* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4cd04-633">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="4cd04-634">*Wwwroot* dizinine *index. HTML* adlı bir HTML dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-634">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="4cd04-635">Dosyanın içeriğini aşağıdaki biçimlendirme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-635">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="4cd04-636">*Wwwroot* dizinine *site. js* adlı bir JavaScript dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4cd04-636">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="4cd04-637">Dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4cd04-637">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="4cd04-638">ASP.NET Core proje başlatma ayarlarında bir değişiklik HTML sayfasını yerel olarak test etmek için gerekli:</span><span class="sxs-lookup"><span data-stu-id="4cd04-638">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="4cd04-639">*Properties\launchSettings.JSON*'i açın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-639">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="4cd04-640">Uygulamayı, projenin varsayılan dosyası&mdash;*Dizin. html* ' de açmaya zorlamak için `launchUrl` özelliğini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="4cd04-640">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="4cd04-641">Bu örnek, Web API 'sinin tüm CRUD yöntemlerini çağırır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-641">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="4cd04-642">API çağrıları açıklamaları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-642">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="4cd04-643">Yapılacaklar öğelerinin bir listesini alın</span><span class="sxs-lookup"><span data-stu-id="4cd04-643">Get a list of to-do items</span></span>

<span data-ttu-id="4cd04-644">jQuery, bir to-do öğesi dizisini temsil eden JSON döndüren Web API 'sine bir HTTP GET isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-644">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="4cd04-645">`success` geri çağırma işlevi, istek başarılı olursa çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-645">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="4cd04-646">Geri çağırma içinde DOM Yapılacaklar bilgilerle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-646">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="4cd04-647">Yapılacak İş Öğesi Ekle</span><span class="sxs-lookup"><span data-stu-id="4cd04-647">Add a to-do item</span></span>

<span data-ttu-id="4cd04-648">jQuery, istek gövdesinde Yapılacaklar öğesiyle bir HTTP POST isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-648">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="4cd04-649">`accepts` ve `contentType` seçenekleri, alınan ve gönderilen medya türünü belirtmek için `application/json` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-649">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="4cd04-650">Yapılacaklar öğesi [JSON. stringbelirt](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)kullanılarak JSON 'a dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="4cd04-650">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="4cd04-651">API başarılı bir durum kodu döndürdüğünde, `getData` işlevi HTML tablosunu güncelleştirmek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4cd04-651">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="4cd04-652">Yapılacak iş öğesini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="4cd04-652">Update a to-do item</span></span>

<span data-ttu-id="4cd04-653">Yapılacak iş öğesi güncelleştirilirken bir eklemeye benzerdir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-653">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="4cd04-654">`url` öğenin benzersiz tanımlayıcısını eklemek için değişir ve `type` `PUT`.</span><span class="sxs-lookup"><span data-stu-id="4cd04-654">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="4cd04-655">Yapılacak iş öğesi silme</span><span class="sxs-lookup"><span data-stu-id="4cd04-655">Delete a to-do item</span></span>

<span data-ttu-id="4cd04-656">Bir yapılacaklar öğesinin silinmesi, AJAX çağrısının `DELETE` `type` ayarlanarak ve öğenin URL 'sindeki benzersiz tanımlayıcısını belirterek gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4cd04-656">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="4cd04-657">Web API 'sine kimlik doğrulama desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="4cd04-657">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources"></a><span data-ttu-id="4cd04-658">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4cd04-658">Additional resources</span></span>

<span data-ttu-id="4cd04-659">[Bu öğretici için örnek kodu görüntüleyin veya indirin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="4cd04-659">[View or download sample code for this tutorial](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="4cd04-660">Bkz. [indirme](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="4cd04-660">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="4cd04-661">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="4cd04-661">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="4cd04-662">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="4cd04-662">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
