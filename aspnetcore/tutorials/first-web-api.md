---
title: "Öğretici: ASP.NET Core bir Web API 'SI oluşturma"
author: rick-anderson
description: ASP.NET Core ile Web API 'SI oluşturmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 09/29/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 6f2d62600da828261ecfc3a1df688ce914eccf33
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72590023"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="751d5-103">Öğretici: ASP.NET Core bir Web API 'SI oluşturma</span><span class="sxs-lookup"><span data-stu-id="751d5-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="751d5-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike te son](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="751d5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="751d5-105">Bu öğreticide, ASP.NET Core ile Web API 'SI oluşturmanın temelleri öğretilir.</span><span class="sxs-lookup"><span data-stu-id="751d5-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="751d5-106">Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="751d5-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="751d5-107">Bir Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="751d5-107">Create a web API project.</span></span>
> * <span data-ttu-id="751d5-108">Bir model sınıfı ve bir veritabanı bağlamı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="751d5-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="751d5-109">CRUD yöntemleriyle bir denetleyiciyi dolandırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="751d5-110">Yönlendirmeyi, URL yollarını ve dönüş değerlerini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="751d5-111">Postman ile Web API 'sini çağırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-111">Call the web API with Postman.</span></span>

<span data-ttu-id="751d5-112">Sonunda, bir veritabanında depolanan "yapılacaklar" öğelerini yönetebilmek için bir Web API 'SI vardır.</span><span class="sxs-lookup"><span data-stu-id="751d5-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="751d5-113">Genel bakış</span><span class="sxs-lookup"><span data-stu-id="751d5-113">Overview</span></span>

<span data-ttu-id="751d5-114">Bu öğretici aşağıdaki API 'YI oluşturur:</span><span class="sxs-lookup"><span data-stu-id="751d5-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="751d5-115">API</span><span class="sxs-lookup"><span data-stu-id="751d5-115">API</span></span> | <span data-ttu-id="751d5-116">Açıklama</span><span class="sxs-lookup"><span data-stu-id="751d5-116">Description</span></span> | <span data-ttu-id="751d5-117">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="751d5-117">Request body</span></span> | <span data-ttu-id="751d5-118">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="751d5-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="751d5-119">/Api/TodoItems al</span><span class="sxs-lookup"><span data-stu-id="751d5-119">GET /api/TodoItems</span></span> | <span data-ttu-id="751d5-120">Tüm yapılacaklar öğelerini Al</span><span class="sxs-lookup"><span data-stu-id="751d5-120">Get all to-do items</span></span> | <span data-ttu-id="751d5-121">Yok.</span><span class="sxs-lookup"><span data-stu-id="751d5-121">None</span></span> | <span data-ttu-id="751d5-122">Yapılacaklar öğeleri dizisi</span><span class="sxs-lookup"><span data-stu-id="751d5-122">Array of to-do items</span></span>|
|<span data-ttu-id="751d5-123">/Api/TodoItems/{id} al</span><span class="sxs-lookup"><span data-stu-id="751d5-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="751d5-124">KIMLIĞE göre öğe al</span><span class="sxs-lookup"><span data-stu-id="751d5-124">Get an item by ID</span></span> | <span data-ttu-id="751d5-125">Yok.</span><span class="sxs-lookup"><span data-stu-id="751d5-125">None</span></span> | <span data-ttu-id="751d5-126">Yapılacaklar öğesi</span><span class="sxs-lookup"><span data-stu-id="751d5-126">To-do item</span></span>|
|<span data-ttu-id="751d5-127">POST/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="751d5-127">POST /api/TodoItems</span></span> | <span data-ttu-id="751d5-128">Yeni öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="751d5-128">Add a new item</span></span> | <span data-ttu-id="751d5-129">Yapılacaklar öğesi</span><span class="sxs-lookup"><span data-stu-id="751d5-129">To-do item</span></span> | <span data-ttu-id="751d5-130">Yapılacaklar öğesi</span><span class="sxs-lookup"><span data-stu-id="751d5-130">To-do item</span></span> |
|<span data-ttu-id="751d5-131">/Api/TodoItems/{id} koy</span><span class="sxs-lookup"><span data-stu-id="751d5-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="751d5-132">Mevcut bir öğeyi güncelleştirin &nbsp;</span><span class="sxs-lookup"><span data-stu-id="751d5-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="751d5-133">Yapılacaklar öğesi</span><span class="sxs-lookup"><span data-stu-id="751d5-133">To-do item</span></span> | <span data-ttu-id="751d5-134">Yok.</span><span class="sxs-lookup"><span data-stu-id="751d5-134">None</span></span> |
|<span data-ttu-id="751d5-135">/Api/TodoItems/{id} &nbsp; SIL &nbsp;</span><span class="sxs-lookup"><span data-stu-id="751d5-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="751d5-136">Öğe &nbsp; &nbsp; silme</span><span class="sxs-lookup"><span data-stu-id="751d5-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="751d5-137">Yok.</span><span class="sxs-lookup"><span data-stu-id="751d5-137">None</span></span> | <span data-ttu-id="751d5-138">Yok.</span><span class="sxs-lookup"><span data-stu-id="751d5-138">None</span></span>|

<span data-ttu-id="751d5-139">Aşağıdaki diyagramda uygulamanın tasarımı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="751d5-139">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki bir kutu ile temsil edilir.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="751d5-145">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="751d5-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="751d5-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="751d5-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="751d5-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="751d5-148">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="751d5-149">Web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="751d5-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="751d5-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="751d5-151">**Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="751d5-152">**ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="751d5-153">Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="751d5-154">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,0** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="751d5-155">**API** şablonunu seçin ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-155">Select the **API** template and click **Create**.</span></span>

![VS Yeni proje iletişim kutusu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="751d5-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="751d5-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="751d5-158">[Tümleşik terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)açın.</span><span class="sxs-lookup"><span data-stu-id="751d5-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="751d5-159">Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="751d5-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="751d5-160">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="751d5-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="751d5-161">Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="751d5-162">Önceki komutlar:</span><span class="sxs-lookup"><span data-stu-id="751d5-162">The preceding commands:</span></span>

  * <span data-ttu-id="751d5-163">Yeni bir Web API projesi oluşturur ve Visual Studio Code açar.</span><span class="sxs-lookup"><span data-stu-id="751d5-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="751d5-164">Sonraki bölümde gerekli olan NuGet paketlerini ekler.</span><span class="sxs-lookup"><span data-stu-id="751d5-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="751d5-165">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="751d5-166">**Dosya** > **yeni çözüm**seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-166">Select **File** > **New Solution**.</span></span>

  ![macOS yeni çözüm](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="751d5-168">**.NET Core** > **App** > **API** > ' **yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="751d5-170">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **hedef Framework** \* *.NET Core 3,0*' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="751d5-171">**Proje adı** için *TodoApi* girin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="751d5-173">Proje klasöründe bir komut terminali açın ve aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="751d5-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="751d5-174">API 'YI test etme</span><span class="sxs-lookup"><span data-stu-id="751d5-174">Test the API</span></span>

<span data-ttu-id="751d5-175">Proje şablonu bir `WeatherForecast` API 'SI oluşturur.</span><span class="sxs-lookup"><span data-stu-id="751d5-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="751d5-176">Uygulamayı test etmek için bir tarayıcıdan `Get` yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="751d5-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="751d5-178">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="751d5-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="751d5-179">Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>/WeatherForecast` gider.</span><span class="sxs-lookup"><span data-stu-id="751d5-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="751d5-180">IIS Express sertifikaya güvenip güvenmemeyi soran bir iletişim kutusu alırsanız **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="751d5-181">Sonraki görüntülenen **güvenlik uyarısı** Iletişim kutusunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="751d5-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="751d5-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="751d5-183">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="751d5-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="751d5-184">Bir tarayıcıda aşağıdaki URL 'ye gidin: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="751d5-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="751d5-185">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="751d5-186">Uygulamayı başlatmak için**hata ayıklamayı başlat**  >  **Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="751d5-187">Mac için Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>` ' a gider.</span><span class="sxs-lookup"><span data-stu-id="751d5-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="751d5-188">HTTP 404 (bulunamadı) hatası döndürüldü.</span><span class="sxs-lookup"><span data-stu-id="751d5-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="751d5-189">URL 'ye `/WeatherForecast` ekleyin (URL 'yi `https://localhost:<port>/WeatherForecast` olarak değiştirin).</span><span class="sxs-lookup"><span data-stu-id="751d5-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="751d5-190">Aşağıdakine benzer bir JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="751d5-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="751d5-191">Model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="751d5-191">Add a model class</span></span>

<span data-ttu-id="751d5-192">*Model* , uygulamanın yönettiği verileri temsil eden bir sınıf kümesidir.</span><span class="sxs-lookup"><span data-stu-id="751d5-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="751d5-193">Bu uygulamanın modeli, tek bir `TodoItem` sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="751d5-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="751d5-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="751d5-195">**Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="751d5-196">**Yeni  >  klasör** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="751d5-197">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="751d5-198">*Modeller* klasörüne sağ tıklayın ve  > **sınıfı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="751d5-199">Sınıfı *TodoItem* olarak adlandırın ve **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="751d5-200">Şablon kodunu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="751d5-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="751d5-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="751d5-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="751d5-202">*Modeller*adlı bir klasör ekleyin.</span><span class="sxs-lookup"><span data-stu-id="751d5-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="751d5-203">*Modeller* klasörüne aşağıdaki kodla bir `TodoItem` sınıfı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="751d5-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="751d5-204">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="751d5-205">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-205">Right-click the project.</span></span> <span data-ttu-id="751d5-206">**Yeni  >  klasör** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="751d5-207">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-207">Name the folder *Models*.</span></span>

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="751d5-209">*Modeller* klasörüne sağ tıklayın ve > **yeni dosya** **ekle** ' yi > **genel** > **boş sınıf**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="751d5-210">Sınıfı *TodoItem*olarak adlandırın ve ardından **Yeni**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="751d5-211">Şablon kodunu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="751d5-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="751d5-212">@No__t_0 özelliği, ilişkisel bir veritabanındaki benzersiz anahtar olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="751d5-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="751d5-213">Model sınıfları projede herhangi bir yere gidebilir, ancak *modeller* klasörü kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="751d5-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="751d5-214">Veritabanı bağlamı ekleme</span><span class="sxs-lookup"><span data-stu-id="751d5-214">Add a database context</span></span>

<span data-ttu-id="751d5-215">*Veritabanı bağlamı* , bir veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="751d5-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="751d5-216">Bu sınıf, `Microsoft.EntityFrameworkCore.DbContext` sınıfından türeterek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="751d5-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="751d5-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="751d5-218">Microsoft. EntityFrameworkCore. SqlServer ekleyin</span><span class="sxs-lookup"><span data-stu-id="751d5-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="751d5-219">**Araçlar** menüsünde **nuget Paket Yöneticisi > çözüm Için NuGet Paketlerini Yönet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="751d5-220">**Araştır** sekmesini seçin ve arama kutusuna **Microsoft. Entityframeworkcore. SqlServer** yazın.</span><span class="sxs-lookup"><span data-stu-id="751d5-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="751d5-221">Sol bölmedeki **Microsoft. EntityFrameworkCore. SqlServer** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="751d5-222">Sağ bölmedeki **Proje** onay kutusunu seçin ve ardından **Install**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="751d5-223">@No__t_0 NuGet paketini eklemek için yukarıdaki yönergeleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="751d5-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet Paket Yöneticisi](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="751d5-225">TodoContext veritabanı bağlamını ekleme</span><span class="sxs-lookup"><span data-stu-id="751d5-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="751d5-226">*Modeller* klasörüne sağ tıklayın ve  > **sınıfı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="751d5-227">Sınıfı *TodoContext* olarak adlandırın ve **Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="751d5-228">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="751d5-229">*Modeller* klasörüne bir `TodoContext` sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="751d5-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="751d5-230">Aşağıdaki kodu girin:</span><span class="sxs-lookup"><span data-stu-id="751d5-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="751d5-231">Veritabanı bağlamını kaydetme</span><span class="sxs-lookup"><span data-stu-id="751d5-231">Register the database context</span></span>

<span data-ttu-id="751d5-232">ASP.NET Core, VERITABANı bağlamı gibi hizmetlerin [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısına kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="751d5-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="751d5-233">Kapsayıcı hizmeti denetleyicilere sağlar.</span><span class="sxs-lookup"><span data-stu-id="751d5-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="751d5-234">Aşağıdaki Vurgulanan kodla *Startup.cs* güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="751d5-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="751d5-235">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="751d5-235">The preceding code:</span></span>

* <span data-ttu-id="751d5-236">Kullanılmayan `using` bildirimlerini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="751d5-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="751d5-237">Veritabanı bağlamını dı kapsayıcısına ekler.</span><span class="sxs-lookup"><span data-stu-id="751d5-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="751d5-238">Veritabanı bağlamının bellek içi bir veritabanını kullanacağı belirtir.</span><span class="sxs-lookup"><span data-stu-id="751d5-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="751d5-239">Denetleyiciyi bir denetleyiciye katlama</span><span class="sxs-lookup"><span data-stu-id="751d5-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="751d5-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="751d5-241">*Denetleyiciler* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="751d5-242">@No__t_1 **yeni yapı Iskelesi öğesi** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="751d5-243">**Entity Framework kullanarak ve eylemler Içeren API denetleyicisi**' ni seçin ve ardından **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="751d5-244">**API denetleyiciyi eylemler Ile Ekle ' de Entity Framework** iletişim kutusunu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="751d5-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="751d5-245">**Model sınıfında** **TodoItem (TodoApi. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="751d5-246">**Veri bağlamı sınıfında** **TodoContext (TodoApi. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="751d5-247">**Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="751d5-248">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="751d5-249">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="751d5-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="751d5-250">Önceki komutlar:</span><span class="sxs-lookup"><span data-stu-id="751d5-250">The preceding commands:</span></span>

* <span data-ttu-id="751d5-251">Yapı iskelesi için gereken NuGet paketlerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="751d5-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="751d5-252">Scafkatlama altyapısını (`dotnet-aspnet-codegenerator`) kurar.</span><span class="sxs-lookup"><span data-stu-id="751d5-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="751d5-253">@No__t_0 yapı iskelesi.</span><span class="sxs-lookup"><span data-stu-id="751d5-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="751d5-254">Oluşturulan kod:</span><span class="sxs-lookup"><span data-stu-id="751d5-254">The generated code:</span></span>

* <span data-ttu-id="751d5-255">Yöntemler olmadan bir API denetleyici sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="751d5-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="751d5-256">Sınıfı [[Apicontroller]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle süsler.</span><span class="sxs-lookup"><span data-stu-id="751d5-256">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="751d5-257">Bu öznitelik, denetleyicinin Web API isteklerine yanıt verdiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="751d5-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="751d5-258">Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz. <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="751d5-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="751d5-259">Veritabanı bağlamını (`TodoContext`) denetleyiciye eklemek için DI kullanır.</span><span class="sxs-lookup"><span data-stu-id="751d5-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="751d5-260">Veritabanı bağlamı, denetleyicideki [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) yöntemlerinde her birinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="751d5-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="751d5-261">PostTodoItem Create metodunu inceleyin</span><span class="sxs-lookup"><span data-stu-id="751d5-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="751d5-262">@No__t_0 ' deki return ifadesini, [NameOf](/dotnet/csharp/language-reference/operators/nameof) işlecini kullanacak şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="751d5-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="751d5-263">Yukarıdaki kod, [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliğiyle gösterildiği gıbı bır http post yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="751d5-263">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="751d5-264">Yöntemi, HTTP isteğinin gövdesinden Yapılacaklar öğesinin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="751d5-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="751d5-265">@No__t_0 yöntemi:</span><span class="sxs-lookup"><span data-stu-id="751d5-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="751d5-266">Başarılı olursa bir HTTP 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="751d5-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="751d5-267">HTTP 201, sunucuda yeni bir kaynak oluşturan HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="751d5-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="751d5-268">Yanıta bir [konum](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) üst bilgisi ekler.</span><span class="sxs-lookup"><span data-stu-id="751d5-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="751d5-269">@No__t_0 üstbilgisi, yeni oluşturulan Yapılacaklar öğesinin [URI](https://developer.mozilla.org/docs/Glossary/URI) 'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="751d5-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="751d5-270">Daha fazla bilgi için bkz. [10.2.2 201 oluşturma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="751d5-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="751d5-271">@No__t_1 üst bilgisinin URI 'sini oluşturmak için `GetTodoItem` eyleme başvurur.</span><span class="sxs-lookup"><span data-stu-id="751d5-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="751d5-272">C# @No__t_1 anahtar sözcüğü, `CreatedAtAction` çağrısında eylem adının sabit kodlanmasını önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="751d5-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="751d5-273">Postman yükleme</span><span class="sxs-lookup"><span data-stu-id="751d5-273">Install Postman</span></span>

<span data-ttu-id="751d5-274">Bu öğretici, Web API 'sini test etmek için Postman kullanır.</span><span class="sxs-lookup"><span data-stu-id="751d5-274">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="751d5-275">[Postman](https://www.getpostman.com/downloads/) yükleme</span><span class="sxs-lookup"><span data-stu-id="751d5-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="751d5-276">Web uygulamasını başlatın.</span><span class="sxs-lookup"><span data-stu-id="751d5-276">Start the web app.</span></span>
* <span data-ttu-id="751d5-277">Postman 'ı başlatın.</span><span class="sxs-lookup"><span data-stu-id="751d5-277">Start Postman.</span></span>
* <span data-ttu-id="751d5-278">**SSL sertifikası doğrulamasını** devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="751d5-278">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="751d5-279">**Dosya** > **ayarları** ' ndan (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="751d5-279">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="751d5-280">Denetleyiciyi test ettikten sonra SSL sertifikası doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="751d5-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="751d5-281">Postman ile test PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="751d5-281">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="751d5-282">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="751d5-282">Create a new request.</span></span>
* <span data-ttu-id="751d5-283">HTTP yöntemini `POST` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-283">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="751d5-284">**Gövde** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-284">Select the **Body** tab.</span></span>
* <span data-ttu-id="751d5-285">**Ham** radyo düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-285">Select the **raw** radio button.</span></span>
* <span data-ttu-id="751d5-286">Türü **JSON (Application/JSON)** olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-286">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="751d5-287">İstek gövdesinde, bir yapılacaklar öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="751d5-287">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="751d5-288">**Gönder**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-288">Select **Send**.</span></span>

  ![Oluşturma isteğiyle Postman](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="751d5-290">Konum üst bilgisi URI 'sini test etme</span><span class="sxs-lookup"><span data-stu-id="751d5-290">Test the location header URI</span></span>

* <span data-ttu-id="751d5-291">**Yanıt** bölmesinde **üstbilgiler** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-291">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="751d5-292">**Konum** üst bilgisi değerini kopyalayın:</span><span class="sxs-lookup"><span data-stu-id="751d5-292">Copy the **Location** header value:</span></span>

  ![Postman konsolunun üstbilgiler sekmesi](first-web-api/_static/3/create.png)

* <span data-ttu-id="751d5-294">ALıNACAK yöntemi ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-294">Set the method to GET.</span></span>
* <span data-ttu-id="751d5-295">URI 'yi yapıştırın (örneğin, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="751d5-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="751d5-296">**Gönder**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="751d5-297">GET yöntemlerini inceleyin</span><span class="sxs-lookup"><span data-stu-id="751d5-297">Examine the GET methods</span></span>

<span data-ttu-id="751d5-298">Bu yöntemler iki al uç noktası uygular:</span><span class="sxs-lookup"><span data-stu-id="751d5-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="751d5-299">Tarayıcıdan veya Postman 'dan iki uç noktayı çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="751d5-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="751d5-300">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="751d5-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="751d5-301">Aşağıdakine benzer bir yanıt, `GetTodoItems` çağrısı tarafından üretilir:</span><span class="sxs-lookup"><span data-stu-id="751d5-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="751d5-302">Postman ile test al</span><span class="sxs-lookup"><span data-stu-id="751d5-302">Test Get with Postman</span></span>

* <span data-ttu-id="751d5-303">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="751d5-303">Create a new request.</span></span>
* <span data-ttu-id="751d5-304">**Almak**için http yöntemini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-304">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="751d5-305">İstek URL 'sini `https://localhost:<port>/api/TodoItems` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="751d5-306">Örneğin, `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="751d5-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="751d5-307">Postman 'da **iki bölme görünümü** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-307">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="751d5-308">**Gönder**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-308">Select **Send**.</span></span>

<span data-ttu-id="751d5-309">Bu uygulama, bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="751d5-309">This app uses an in-memory database.</span></span> <span data-ttu-id="751d5-310">Uygulama durdurulup başlatılırsa, önceki GET isteği herhangi bir veri döndürmez.</span><span class="sxs-lookup"><span data-stu-id="751d5-310">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="751d5-311">Hiçbir veri döndürülmezse, verileri uygulamaya [gönderin](#post) .</span><span class="sxs-lookup"><span data-stu-id="751d5-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="751d5-312">Yönlendirme ve URL yolları</span><span class="sxs-lookup"><span data-stu-id="751d5-312">Routing and URL paths</span></span>

<span data-ttu-id="751d5-313">[@No__t_1](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) ÖZNITELIĞI BIR HTTP GET isteğine yanıt veren bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="751d5-313">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="751d5-314">Her yöntemin URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="751d5-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="751d5-315">Denetleyicinin `Route` özniteliğinde şablon dizesiyle başlayın:</span><span class="sxs-lookup"><span data-stu-id="751d5-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="751d5-316">@No__t_0, denetleyicinin adıyla değiştirin, bu kural, denetleyici sınıf adı "denetleyici" sonekidir.</span><span class="sxs-lookup"><span data-stu-id="751d5-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="751d5-317">Bu örnek için denetleyici sınıfı adı **todoıtems**denetleyicisidir, bu nedenle denetleyicinin adı "todoıtems" olur.</span><span class="sxs-lookup"><span data-stu-id="751d5-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="751d5-318">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="751d5-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="751d5-319">@No__t_0 özniteliğinin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin.</span><span class="sxs-lookup"><span data-stu-id="751d5-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="751d5-320">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="751d5-320">This sample doesn't use a template.</span></span> <span data-ttu-id="751d5-321">Daha fazla bilgi için bkz. [http [fiil] öznitelikleriyle öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="751d5-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="751d5-322">Aşağıdaki `GetTodoItem` yönteminde, `"{id}"` Yapılacaklar öğesinin benzersiz tanımlayıcısı için bir yer tutucu değişkenidir.</span><span class="sxs-lookup"><span data-stu-id="751d5-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="751d5-323">@No__t_0 çağrıldığında, URL 'deki `"{id}"` değeri `id` parametresindeki yöntemine sağlanır.</span><span class="sxs-lookup"><span data-stu-id="751d5-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="751d5-324">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="751d5-324">Return values</span></span>

<span data-ttu-id="751d5-325">@No__t_0 ve `GetTodoItem` yöntemlerinin dönüş türü [ActionResult \<T > türüdür](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="751d5-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="751d5-326">ASP.NET Core nesneyi [JSON](https://www.json.org/) 'a otomatik olarak serileştirir ve yanıt ILETISININ gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="751d5-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="751d5-327">Bu dönüş türü için yanıt kodu, işlenmemiş özel durum olmadığı varsayılarak 200 ' dir.</span><span class="sxs-lookup"><span data-stu-id="751d5-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="751d5-328">İşlenmemiş özel durumlar 5 xx hataya çevrilir.</span><span class="sxs-lookup"><span data-stu-id="751d5-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="751d5-329">`ActionResult` dönüş türleri, çok çeşitli HTTP durum kodlarını temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="751d5-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="751d5-330">Örneğin, `GetTodoItem` iki farklı durum değeri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="751d5-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="751d5-331">İstenen KIMLIKLE eşleşen hiçbir öğe yoksa, yöntem 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="751d5-331">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="751d5-332">Aksi takdirde, yöntemi bir JSON yanıt gövdesi ile 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="751d5-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="751d5-333">@No__t_0 sonuçları bir HTTP 200 yanıtına döndürülüyor.</span><span class="sxs-lookup"><span data-stu-id="751d5-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="751d5-334">PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="751d5-334">The PutTodoItem method</span></span>

<span data-ttu-id="751d5-335">@No__t_0 yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="751d5-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="751d5-336">`PutTodoItem`, HTTP PUT kullanması dışında `PostTodoItem` benzerdir.</span><span class="sxs-lookup"><span data-stu-id="751d5-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="751d5-337">Yanıt 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="751d5-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="751d5-338">HTTP belirtimine göre bir PUT isteği, istemcinin yalnızca değişiklikleri değil, tüm güncelleştirilmiş varlığı göndermesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="751d5-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="751d5-339">Kısmi güncelleştirmeleri desteklemek için [http Patch](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)kullanın.</span><span class="sxs-lookup"><span data-stu-id="751d5-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="751d5-340">@No__t_0 çağırırken bir hata alırsanız, veritabanında bir öğe olduğundan emin olmak için `GET` çağırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="751d5-341">PutTodoItem yöntemini test etme</span><span class="sxs-lookup"><span data-stu-id="751d5-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="751d5-342">Bu örnek, uygulama her başlatıldığında başlatılmış olması gereken bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="751d5-342">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="751d5-343">Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="751d5-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="751d5-344">PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="751d5-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="751d5-345">ID = 1 olan Yapılacaklar öğesini güncelleştirin ve adını "Feed balık" olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="751d5-345">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="751d5-346">Aşağıdaki görüntüde Postman güncelleştirmesi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="751d5-346">The following image shows the Postman update:</span></span>

![204 (Içerik yok) yanıtı gösteren Postman konsolu](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="751d5-348">DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="751d5-348">The DeleteTodoItem method</span></span>

<span data-ttu-id="751d5-349">@No__t_0 yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="751d5-349">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="751d5-350">@No__t_0 yanıtı 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="751d5-350">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="751d5-351">DeleteTodoItem yöntemini test etme</span><span class="sxs-lookup"><span data-stu-id="751d5-351">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="751d5-352">Bir yapılacaklar öğesini silmek için Postman kullanın:</span><span class="sxs-lookup"><span data-stu-id="751d5-352">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="751d5-353">Yöntemini `DELETE` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-353">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="751d5-354">Silinecek nesnenin URI 'sini ayarlayın (örneğin `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="751d5-354">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="751d5-355">**Gönder**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-355">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="751d5-356">JavaScript ile Web API 'sini çağırma</span><span class="sxs-lookup"><span data-stu-id="751d5-356">Call the web API with JavaScript</span></span>

<span data-ttu-id="751d5-357">Bkz. [öğretici: JavaScript ile ASP.NET Core Web API 'Si çağırma](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="751d5-357">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="751d5-358">Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="751d5-358">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="751d5-359">Bir Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="751d5-359">Create a web API project.</span></span>
> * <span data-ttu-id="751d5-360">Bir model sınıfı ve bir veritabanı bağlamı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="751d5-360">Add a model class and a database context.</span></span>
> * <span data-ttu-id="751d5-361">Denetleyici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="751d5-361">Add a controller.</span></span>
> * <span data-ttu-id="751d5-362">CRUD yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="751d5-362">Add CRUD methods.</span></span>
> * <span data-ttu-id="751d5-363">Yönlendirme ve URL yollarını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-363">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="751d5-364">Dönüş değerlerini belirtin.</span><span class="sxs-lookup"><span data-stu-id="751d5-364">Specify return values.</span></span>
> * <span data-ttu-id="751d5-365">Postman ile Web API 'sini çağırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-365">Call the web API with Postman.</span></span>
> * <span data-ttu-id="751d5-366">JavaScript ile Web API 'sini çağırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-366">Call the web API with JavaScript.</span></span>

<span data-ttu-id="751d5-367">Sonunda, ilişkisel bir veritabanında depolanan "yapılacaklar" öğelerini yönetebilmek için bir Web API 'SI vardır.</span><span class="sxs-lookup"><span data-stu-id="751d5-367">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="751d5-368">Genel bakış</span><span class="sxs-lookup"><span data-stu-id="751d5-368">Overview</span></span>

<span data-ttu-id="751d5-369">Bu öğretici aşağıdaki API 'YI oluşturur:</span><span class="sxs-lookup"><span data-stu-id="751d5-369">This tutorial creates the following API:</span></span>

|<span data-ttu-id="751d5-370">API</span><span class="sxs-lookup"><span data-stu-id="751d5-370">API</span></span> | <span data-ttu-id="751d5-371">Açıklama</span><span class="sxs-lookup"><span data-stu-id="751d5-371">Description</span></span> | <span data-ttu-id="751d5-372">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="751d5-372">Request body</span></span> | <span data-ttu-id="751d5-373">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="751d5-373">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="751d5-374">/Api/TodoItems al</span><span class="sxs-lookup"><span data-stu-id="751d5-374">GET /api/TodoItems</span></span> | <span data-ttu-id="751d5-375">Tüm yapılacaklar öğelerini Al</span><span class="sxs-lookup"><span data-stu-id="751d5-375">Get all to-do items</span></span> | <span data-ttu-id="751d5-376">Yok.</span><span class="sxs-lookup"><span data-stu-id="751d5-376">None</span></span> | <span data-ttu-id="751d5-377">Yapılacaklar öğeleri dizisi</span><span class="sxs-lookup"><span data-stu-id="751d5-377">Array of to-do items</span></span>|
|<span data-ttu-id="751d5-378">/Api/TodoItems/{id} al</span><span class="sxs-lookup"><span data-stu-id="751d5-378">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="751d5-379">KIMLIĞE göre öğe al</span><span class="sxs-lookup"><span data-stu-id="751d5-379">Get an item by ID</span></span> | <span data-ttu-id="751d5-380">Yok.</span><span class="sxs-lookup"><span data-stu-id="751d5-380">None</span></span> | <span data-ttu-id="751d5-381">Yapılacaklar öğesi</span><span class="sxs-lookup"><span data-stu-id="751d5-381">To-do item</span></span>|
|<span data-ttu-id="751d5-382">POST/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="751d5-382">POST /api/TodoItems</span></span> | <span data-ttu-id="751d5-383">Yeni öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="751d5-383">Add a new item</span></span> | <span data-ttu-id="751d5-384">Yapılacaklar öğesi</span><span class="sxs-lookup"><span data-stu-id="751d5-384">To-do item</span></span> | <span data-ttu-id="751d5-385">Yapılacaklar öğesi</span><span class="sxs-lookup"><span data-stu-id="751d5-385">To-do item</span></span> |
|<span data-ttu-id="751d5-386">/Api/TodoItems/{id} koy</span><span class="sxs-lookup"><span data-stu-id="751d5-386">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="751d5-387">Mevcut bir öğeyi güncelleştirin &nbsp;</span><span class="sxs-lookup"><span data-stu-id="751d5-387">Update an existing item &nbsp;</span></span> | <span data-ttu-id="751d5-388">Yapılacaklar öğesi</span><span class="sxs-lookup"><span data-stu-id="751d5-388">To-do item</span></span> | <span data-ttu-id="751d5-389">Yok.</span><span class="sxs-lookup"><span data-stu-id="751d5-389">None</span></span> |
|<span data-ttu-id="751d5-390">/Api/TodoItems/{id} &nbsp; SIL &nbsp;</span><span class="sxs-lookup"><span data-stu-id="751d5-390">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="751d5-391">Öğe &nbsp; &nbsp; silme</span><span class="sxs-lookup"><span data-stu-id="751d5-391">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="751d5-392">Yok.</span><span class="sxs-lookup"><span data-stu-id="751d5-392">None</span></span> | <span data-ttu-id="751d5-393">Yok.</span><span class="sxs-lookup"><span data-stu-id="751d5-393">None</span></span>|

<span data-ttu-id="751d5-394">Aşağıdaki diyagramda uygulamanın tasarımı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="751d5-394">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki bir kutu ile temsil edilir.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="751d5-400">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="751d5-400">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="751d5-401">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-401">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="751d5-402">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="751d5-402">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="751d5-403">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-403">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="751d5-404">Web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="751d5-404">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="751d5-405">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-405">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="751d5-406">**Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-406">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="751d5-407">**ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-407">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="751d5-408">Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-408">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="751d5-409">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 2,2** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-409">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="751d5-410">**API** şablonunu seçin ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-410">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="751d5-411">**Docker desteğini etkinleştir** **' i seçmeyin** .</span><span class="sxs-lookup"><span data-stu-id="751d5-411">**Don't** select **Enable Docker Support**.</span></span>

![VS Yeni proje iletişim kutusu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="751d5-413">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="751d5-413">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="751d5-414">[Tümleşik terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)açın.</span><span class="sxs-lookup"><span data-stu-id="751d5-414">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="751d5-415">Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="751d5-415">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="751d5-416">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="751d5-416">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="751d5-417">Bu komutlar yeni bir Web API projesi oluşturur ve yeni proje klasöründe Visual Studio Code yeni bir örneğini açar.</span><span class="sxs-lookup"><span data-stu-id="751d5-417">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="751d5-418">Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-418">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="751d5-419">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-419">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="751d5-420">**Dosya** > **yeni çözüm**seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-420">Select **File** > **New Solution**.</span></span>

  ![macOS yeni çözüm](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="751d5-422">**.NET Core** > **App** > **API** > ' **yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-422">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="751d5-424">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, \* *.NET Core 2,2*' un varsayılan **hedef çerçevesini** kabul edin.</span><span class="sxs-lookup"><span data-stu-id="751d5-424">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="751d5-425">**Proje adı** için *TodoApi* girin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-425">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="751d5-427">API 'YI test etme</span><span class="sxs-lookup"><span data-stu-id="751d5-427">Test the API</span></span>

<span data-ttu-id="751d5-428">Proje şablonu bir `values` API 'SI oluşturur.</span><span class="sxs-lookup"><span data-stu-id="751d5-428">The project template creates a `values` API.</span></span> <span data-ttu-id="751d5-429">Uygulamayı test etmek için bir tarayıcıdan `Get` yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-429">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="751d5-430">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-430">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="751d5-431">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="751d5-431">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="751d5-432">Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>/api/values` gider.</span><span class="sxs-lookup"><span data-stu-id="751d5-432">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="751d5-433">IIS Express sertifikaya güvenip güvenmemeyi soran bir iletişim kutusu alırsanız **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-433">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="751d5-434">Sonraki görüntülenen **güvenlik uyarısı** Iletişim kutusunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-434">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="751d5-435">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="751d5-435">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="751d5-436">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="751d5-436">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="751d5-437">Bir tarayıcıda aşağıdaki URL 'ye gidin: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="751d5-437">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="751d5-438">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-438">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="751d5-439">Uygulamayı başlatmak için**hata ayıklamayı başlat**  >  **Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-439">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="751d5-440">Mac için Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>` ' a gider.</span><span class="sxs-lookup"><span data-stu-id="751d5-440">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="751d5-441">HTTP 404 (bulunamadı) hatası döndürüldü.</span><span class="sxs-lookup"><span data-stu-id="751d5-441">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="751d5-442">URL 'ye `/api/values` ekleyin (URL 'yi `https://localhost:<port>/api/values` olarak değiştirin).</span><span class="sxs-lookup"><span data-stu-id="751d5-442">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="751d5-443">Aşağıdaki JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="751d5-443">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="751d5-444">Model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="751d5-444">Add a model class</span></span>

<span data-ttu-id="751d5-445">*Model* , uygulamanın yönettiği verileri temsil eden bir sınıf kümesidir.</span><span class="sxs-lookup"><span data-stu-id="751d5-445">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="751d5-446">Bu uygulamanın modeli, tek bir `TodoItem` sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="751d5-446">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="751d5-447">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-447">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="751d5-448">**Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-448">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="751d5-449">**Yeni  >  klasör** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-449">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="751d5-450">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-450">Name the folder *Models*.</span></span>

* <span data-ttu-id="751d5-451">*Modeller* klasörüne sağ tıklayın ve  > **sınıfı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-451">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="751d5-452">Sınıfı *TodoItem* olarak adlandırın ve **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-452">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="751d5-453">Şablon kodunu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="751d5-453">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="751d5-454">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="751d5-454">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="751d5-455">*Modeller*adlı bir klasör ekleyin.</span><span class="sxs-lookup"><span data-stu-id="751d5-455">Add a folder named *Models*.</span></span>

* <span data-ttu-id="751d5-456">*Modeller* klasörüne aşağıdaki kodla bir `TodoItem` sınıfı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="751d5-456">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="751d5-457">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-457">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="751d5-458">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-458">Right-click the project.</span></span> <span data-ttu-id="751d5-459">**Yeni  >  klasör** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-459">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="751d5-460">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-460">Name the folder *Models*.</span></span>

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="751d5-462">*Modeller* klasörüne sağ tıklayın ve > **yeni dosya** **ekle** ' yi > **genel** > **boş sınıf**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-462">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="751d5-463">Sınıfı *TodoItem*olarak adlandırın ve ardından **Yeni**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-463">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="751d5-464">Şablon kodunu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="751d5-464">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="751d5-465">@No__t_0 özelliği, ilişkisel bir veritabanındaki benzersiz anahtar olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="751d5-465">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="751d5-466">Model sınıfları projede herhangi bir yere gidebilir, ancak *modeller* klasörü kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="751d5-466">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="751d5-467">Veritabanı bağlamı ekleme</span><span class="sxs-lookup"><span data-stu-id="751d5-467">Add a database context</span></span>

<span data-ttu-id="751d5-468">*Veritabanı bağlamı* , bir veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="751d5-468">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="751d5-469">Bu sınıf, `Microsoft.EntityFrameworkCore.DbContext` sınıfından türeterek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="751d5-469">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="751d5-470">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-470">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="751d5-471">*Modeller* klasörüne sağ tıklayın ve  > **sınıfı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-471">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="751d5-472">Sınıfı *TodoContext* olarak adlandırın ve **Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-472">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="751d5-473">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-473">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="751d5-474">*Modeller* klasörüne bir `TodoContext` sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="751d5-474">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="751d5-475">Şablon kodunu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="751d5-475">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="751d5-476">Veritabanı bağlamını kaydetme</span><span class="sxs-lookup"><span data-stu-id="751d5-476">Register the database context</span></span>

<span data-ttu-id="751d5-477">ASP.NET Core, VERITABANı bağlamı gibi hizmetlerin [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısına kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="751d5-477">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="751d5-478">Kapsayıcı hizmeti denetleyicilere sağlar.</span><span class="sxs-lookup"><span data-stu-id="751d5-478">The container provides the service to controllers.</span></span>

<span data-ttu-id="751d5-479">Aşağıdaki Vurgulanan kodla *Startup.cs* güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="751d5-479">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="751d5-480">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="751d5-480">The preceding code:</span></span>

* <span data-ttu-id="751d5-481">Kullanılmayan `using` bildirimlerini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="751d5-481">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="751d5-482">Veritabanı bağlamını dı kapsayıcısına ekler.</span><span class="sxs-lookup"><span data-stu-id="751d5-482">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="751d5-483">Veritabanı bağlamının bellek içi bir veritabanını kullanacağı belirtir.</span><span class="sxs-lookup"><span data-stu-id="751d5-483">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="751d5-484">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="751d5-484">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="751d5-485">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-485">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="751d5-486">*Denetleyiciler* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-486">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="751d5-487">@No__t_1 **Yeni öğe** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-487">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="751d5-488">**Yeni öğe Ekle** iletişim kutusunda, **API denetleyici sınıfı** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-488">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="751d5-489">Sınıfı *TodoController*olarak adlandırın ve **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-489">Name the class *TodoController*, and select **Add**.</span></span>

  ![Arama kutusu ve Web API denetleyicisi seçiliyken denetleyiciyi içeren yeni öğe iletişim kutusu Ekle](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="751d5-491">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-491">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="751d5-492">*Denetleyiciler* klasöründe `TodoController` adlı bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="751d5-492">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="751d5-493">Şablon kodunu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="751d5-493">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="751d5-494">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="751d5-494">The preceding code:</span></span>

* <span data-ttu-id="751d5-495">Yöntemler olmadan bir API denetleyici sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="751d5-495">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="751d5-496">Sınıfı [[Apicontroller]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle süsler.</span><span class="sxs-lookup"><span data-stu-id="751d5-496">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="751d5-497">Bu öznitelik, denetleyicinin Web API isteklerine yanıt verdiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="751d5-497">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="751d5-498">Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz. <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="751d5-498">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="751d5-499">Veritabanı bağlamını (`TodoContext`) denetleyiciye eklemek için DI kullanır.</span><span class="sxs-lookup"><span data-stu-id="751d5-499">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="751d5-500">Veritabanı bağlamı, denetleyicideki [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) yöntemlerinde her birinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="751d5-500">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="751d5-501">Veritabanı boşsa, veritabanına `Item1` adlı bir öğe ekler.</span><span class="sxs-lookup"><span data-stu-id="751d5-501">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="751d5-502">Bu kod oluşturucuda yer aldığı için, her yeni HTTP isteği olduğunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="751d5-502">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="751d5-503">Tüm öğeleri silerseniz, bir API yönteminin bir sonraki çağrılışında Oluşturucu `Item1` yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="751d5-503">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="751d5-504">Bu nedenle, aslında çalışırken silme işe yaramadı gibi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="751d5-504">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="751d5-505">Get yöntemleri ekleme</span><span class="sxs-lookup"><span data-stu-id="751d5-505">Add Get methods</span></span>

<span data-ttu-id="751d5-506">Yapılacaklar öğelerini alan bir API sağlamak için, `TodoController` sınıfına aşağıdaki yöntemleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="751d5-506">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="751d5-507">Bu yöntemler iki al uç noktası uygular:</span><span class="sxs-lookup"><span data-stu-id="751d5-507">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="751d5-508">Hala çalışıyorsa uygulamayı durdurun.</span><span class="sxs-lookup"><span data-stu-id="751d5-508">Stop the app if it's still running.</span></span> <span data-ttu-id="751d5-509">Ardından, en son değişiklikleri dahil etmek için yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-509">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="751d5-510">Bir tarayıcıdan iki uç noktayı çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="751d5-510">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="751d5-511">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="751d5-511">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="751d5-512">Aşağıdaki HTTP yanıtı `GetTodoItems` çağrısı tarafından üretilir:</span><span class="sxs-lookup"><span data-stu-id="751d5-512">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="751d5-513">Yönlendirme ve URL yolları</span><span class="sxs-lookup"><span data-stu-id="751d5-513">Routing and URL paths</span></span>

<span data-ttu-id="751d5-514">[@No__t_1](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) ÖZNITELIĞI BIR HTTP GET isteğine yanıt veren bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="751d5-514">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="751d5-515">Her yöntemin URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="751d5-515">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="751d5-516">Denetleyicinin `Route` özniteliğinde şablon dizesiyle başlayın:</span><span class="sxs-lookup"><span data-stu-id="751d5-516">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="751d5-517">@No__t_0, denetleyicinin adıyla değiştirin, bu kural, denetleyici sınıf adı "denetleyici" sonekidir.</span><span class="sxs-lookup"><span data-stu-id="751d5-517">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="751d5-518">Bu örnek için denetleyici sınıf adı **Todo**Controller olduğundan, denetleyici adı "Todo" olur.</span><span class="sxs-lookup"><span data-stu-id="751d5-518">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="751d5-519">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="751d5-519">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="751d5-520">@No__t_0 özniteliğinin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin.</span><span class="sxs-lookup"><span data-stu-id="751d5-520">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="751d5-521">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="751d5-521">This sample doesn't use a template.</span></span> <span data-ttu-id="751d5-522">Daha fazla bilgi için bkz. [http [fiil] öznitelikleriyle öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="751d5-522">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="751d5-523">Aşağıdaki `GetTodoItem` yönteminde, `"{id}"` Yapılacaklar öğesinin benzersiz tanımlayıcısı için bir yer tutucu değişkenidir.</span><span class="sxs-lookup"><span data-stu-id="751d5-523">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="751d5-524">@No__t_0 çağrıldığında, URL 'deki `"{id}"` değeri `id` parametresindeki yöntemine sağlanır.</span><span class="sxs-lookup"><span data-stu-id="751d5-524">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="751d5-525">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="751d5-525">Return values</span></span>

<span data-ttu-id="751d5-526">@No__t_0 ve `GetTodoItem` yöntemlerinin dönüş türü [ActionResult \<T > türüdür](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="751d5-526">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="751d5-527">ASP.NET Core nesneyi [JSON](https://www.json.org/) 'a otomatik olarak serileştirir ve yanıt ILETISININ gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="751d5-527">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="751d5-528">Bu dönüş türü için yanıt kodu, işlenmemiş özel durum olmadığı varsayılarak 200 ' dir.</span><span class="sxs-lookup"><span data-stu-id="751d5-528">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="751d5-529">İşlenmemiş özel durumlar 5 xx hataya çevrilir.</span><span class="sxs-lookup"><span data-stu-id="751d5-529">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="751d5-530">`ActionResult` dönüş türleri, çok çeşitli HTTP durum kodlarını temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="751d5-530">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="751d5-531">Örneğin, `GetTodoItem` iki farklı durum değeri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="751d5-531">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="751d5-532">İstenen KIMLIKLE eşleşen hiçbir öğe yoksa, yöntem 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="751d5-532">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="751d5-533">Aksi takdirde, yöntemi bir JSON yanıt gövdesi ile 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="751d5-533">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="751d5-534">@No__t_0 sonuçları bir HTTP 200 yanıtına döndürülüyor.</span><span class="sxs-lookup"><span data-stu-id="751d5-534">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="751d5-535">GetTodoItems yöntemini test etme</span><span class="sxs-lookup"><span data-stu-id="751d5-535">Test the GetTodoItems method</span></span>

<span data-ttu-id="751d5-536">Bu öğretici, Web API 'sini test etmek için Postman kullanır.</span><span class="sxs-lookup"><span data-stu-id="751d5-536">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="751d5-537">[Postman](https://www.getpostman.com/downloads/)'yi yükleme.</span><span class="sxs-lookup"><span data-stu-id="751d5-537">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="751d5-538">Web uygulamasını başlatın.</span><span class="sxs-lookup"><span data-stu-id="751d5-538">Start the web app.</span></span>
* <span data-ttu-id="751d5-539">Postman 'ı başlatın.</span><span class="sxs-lookup"><span data-stu-id="751d5-539">Start Postman.</span></span>
* <span data-ttu-id="751d5-540">**SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="751d5-540">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="751d5-541">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-541">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="751d5-542">**Dosya** > **ayarları** ' ndan (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="751d5-542">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="751d5-543">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="751d5-543">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="751d5-544">**Postman**  > **tercihleri** ' nden (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="751d5-544">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="751d5-545">Alternatif olarak, wranı seçin ve **Ayarlar**' ı seçip SSL sertifikası doğrulamasını devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="751d5-545">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="751d5-546">Denetleyiciyi test ettikten sonra SSL sertifikası doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="751d5-546">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="751d5-547">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="751d5-547">Create a new request.</span></span>
  * <span data-ttu-id="751d5-548">**Almak**için http yöntemini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-548">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="751d5-549">İstek URL 'sini `https://localhost:<port>/api/todo` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-549">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="751d5-550">Örneğin, `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="751d5-550">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="751d5-551">Postman 'da **iki bölme görünümü** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-551">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="751d5-552">**Gönder**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-552">Select **Send**.</span></span>

![Get isteği ile Postman](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="751d5-554">Create yöntemi ekle</span><span class="sxs-lookup"><span data-stu-id="751d5-554">Add a Create method</span></span>

<span data-ttu-id="751d5-555">Aşağıdaki `PostTodoItem` yöntemini *Controllers/TodoController. cs*içine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="751d5-555">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="751d5-556">Yukarıdaki kod, [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliğiyle gösterildiği gıbı bır http post yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="751d5-556">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="751d5-557">Yöntemi, HTTP isteğinin gövdesinden Yapılacaklar öğesinin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="751d5-557">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="751d5-558">@No__t_0 yöntemi:</span><span class="sxs-lookup"><span data-stu-id="751d5-558">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="751d5-559">Başarılı olursa bir HTTP 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="751d5-559">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="751d5-560">HTTP 201, sunucuda yeni bir kaynak oluşturan HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="751d5-560">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="751d5-561">Yanıta bir `Location` üst bilgisi ekler.</span><span class="sxs-lookup"><span data-stu-id="751d5-561">Adds a `Location` header to the response.</span></span> <span data-ttu-id="751d5-562">@No__t_0 üstbilgisi, yeni oluşturulan Yapılacaklar öğesinin URI 'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="751d5-562">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="751d5-563">Daha fazla bilgi için bkz. [10.2.2 201 oluşturma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="751d5-563">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="751d5-564">@No__t_1 üst bilgisinin URI 'sini oluşturmak için `GetTodoItem` eyleme başvurur.</span><span class="sxs-lookup"><span data-stu-id="751d5-564">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="751d5-565">C# @No__t_1 anahtar sözcüğü, `CreatedAtAction` çağrısında eylem adının sabit kodlanmasını önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="751d5-565">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="751d5-566">PostTodoItem yöntemini test etme</span><span class="sxs-lookup"><span data-stu-id="751d5-566">Test the PostTodoItem method</span></span>

* <span data-ttu-id="751d5-567">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="751d5-567">Build the project.</span></span>
* <span data-ttu-id="751d5-568">Postman 'da HTTP yöntemini `POST` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-568">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="751d5-569">**Gövde** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-569">Select the **Body** tab.</span></span>
* <span data-ttu-id="751d5-570">**Ham** radyo düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-570">Select the **raw** radio button.</span></span>
* <span data-ttu-id="751d5-571">Türü **JSON (Application/JSON)** olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-571">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="751d5-572">İstek gövdesinde, bir yapılacaklar öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="751d5-572">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="751d5-573">**Gönder**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-573">Select **Send**.</span></span>

  ![Oluşturma isteğiyle Postman](first-web-api/_static/create.png)

  <span data-ttu-id="751d5-575">Bir 405 yöntemine Izin verilmiyor hatası alırsanız, `PostTodoItem` yöntemi eklendikten sonra projenin derlenmesinin sonucu büyük olasılıkla oluşur.</span><span class="sxs-lookup"><span data-stu-id="751d5-575">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="751d5-576">Konum üst bilgisi URI 'sini test etme</span><span class="sxs-lookup"><span data-stu-id="751d5-576">Test the location header URI</span></span>

* <span data-ttu-id="751d5-577">**Yanıt** bölmesinde **üstbilgiler** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-577">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="751d5-578">**Konum** üst bilgisi değerini kopyalayın:</span><span class="sxs-lookup"><span data-stu-id="751d5-578">Copy the **Location** header value:</span></span>

  ![Postman konsolunun üstbilgiler sekmesi](first-web-api/_static/pmc2.png)

* <span data-ttu-id="751d5-580">ALıNACAK yöntemi ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-580">Set the method to GET.</span></span>
* <span data-ttu-id="751d5-581">URI 'yi yapıştırın (örneğin, `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="751d5-581">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="751d5-582">**Gönder**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-582">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="751d5-583">PutTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="751d5-583">Add a PutTodoItem method</span></span>

<span data-ttu-id="751d5-584">Aşağıdaki `PutTodoItem` yöntemi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="751d5-584">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="751d5-585">`PutTodoItem`, HTTP PUT kullanması dışında `PostTodoItem` benzerdir.</span><span class="sxs-lookup"><span data-stu-id="751d5-585">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="751d5-586">Yanıt 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="751d5-586">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="751d5-587">HTTP belirtimine göre bir PUT isteği, istemcinin yalnızca değişiklikleri değil, tüm güncelleştirilmiş varlığı göndermesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="751d5-587">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="751d5-588">Kısmi güncelleştirmeleri desteklemek için [http Patch](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)kullanın.</span><span class="sxs-lookup"><span data-stu-id="751d5-588">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="751d5-589">@No__t_0 çağırırken bir hata alırsanız, veritabanında bir öğe olduğundan emin olmak için `GET` çağırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-589">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="751d5-590">PutTodoItem yöntemini test etme</span><span class="sxs-lookup"><span data-stu-id="751d5-590">Test the PutTodoItem method</span></span>

<span data-ttu-id="751d5-591">Bu örnek, uygulama her başlatıldığında başlatılmış olması gereken bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="751d5-591">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="751d5-592">Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="751d5-592">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="751d5-593">PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="751d5-593">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="751d5-594">ID = 1 olan Yapılacaklar öğesini güncelleştirin ve adını "Feed balık" olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="751d5-594">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="751d5-595">Aşağıdaki görüntüde Postman güncelleştirmesi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="751d5-595">The following image shows the Postman update:</span></span>

![204 (Içerik yok) yanıtı gösteren Postman konsolu](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="751d5-597">DeleteTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="751d5-597">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="751d5-598">Aşağıdaki `DeleteTodoItem` yöntemi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="751d5-598">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="751d5-599">@No__t_0 yanıtı 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="751d5-599">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="751d5-600">DeleteTodoItem yöntemini test etme</span><span class="sxs-lookup"><span data-stu-id="751d5-600">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="751d5-601">Bir yapılacaklar öğesini silmek için Postman kullanın:</span><span class="sxs-lookup"><span data-stu-id="751d5-601">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="751d5-602">Yöntemini `DELETE` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="751d5-602">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="751d5-603">Silinecek nesnenin URI 'sini ayarlayın (örneğin `https://localhost:5001/api/todo/1`).</span><span class="sxs-lookup"><span data-stu-id="751d5-603">Set the URI of the object to delete (for example `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="751d5-604">**Gönder**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="751d5-604">Select **Send**.</span></span>

<span data-ttu-id="751d5-605">Örnek uygulama, tüm öğeleri silmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="751d5-605">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="751d5-606">Ancak, son öğe silindiğinde, API 'nin bir sonraki çağrılışında model sınıfı Oluşturucu tarafından yeni bir tane oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="751d5-606">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="751d5-607">JavaScript ile Web API 'sini çağırma</span><span class="sxs-lookup"><span data-stu-id="751d5-607">Call the web API with JavaScript</span></span>

<span data-ttu-id="751d5-608">Bu bölümde, Web API 'sini çağırmak için JavaScript kullanan bir HTML sayfası eklenir.</span><span class="sxs-lookup"><span data-stu-id="751d5-608">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="751d5-609">jQuery isteği başlatır.</span><span class="sxs-lookup"><span data-stu-id="751d5-609">jQuery initiates the request.</span></span> <span data-ttu-id="751d5-610">JavaScript, sayfayı Web API 'sinin yanıtından alınan ayrıntılarla güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="751d5-610">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="751d5-611">Uygulamayı [statik dosyalara sunacak](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) şekilde yapılandırın ve aşağıdaki vurgulanmış kodla *Startup.cs* güncelleştirerek [varsayılan dosya eşlemesini etkinleştirin](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) :</span><span class="sxs-lookup"><span data-stu-id="751d5-611">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="751d5-612">Proje dizininde bir *Wwwroot* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="751d5-612">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="751d5-613">*Wwwroot* dizinine *index. HTML* adlı bir HTML dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="751d5-613">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="751d5-614">İçeriğini aşağıdaki biçimlendirmeyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="751d5-614">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="751d5-615">*Wwwroot* dizinine *site. js* adlı bir JavaScript dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="751d5-615">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="751d5-616">İçeriğini şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="751d5-616">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="751d5-617">HTML sayfasını yerel olarak test etmek için ASP.NET Core projesinin başlatma ayarlarındaki bir değişikliğin yapılması gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="751d5-617">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="751d5-618">*Properties\launchSettings.JSON*'i açın.</span><span class="sxs-lookup"><span data-stu-id="751d5-618">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="751d5-619">Uygulamanın *index. html* &mdash;the projenin varsayılan dosyasında açılmasını zorlamak için `launchUrl` özelliğini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="751d5-619">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="751d5-620">Bu örnek, Web API 'sinin tüm CRUD yöntemlerini çağırır.</span><span class="sxs-lookup"><span data-stu-id="751d5-620">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="751d5-621">API çağrılarının açıklamaları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="751d5-621">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="751d5-622">Yapılacaklar öğelerinin bir listesini alın</span><span class="sxs-lookup"><span data-stu-id="751d5-622">Get a list of to-do items</span></span>

<span data-ttu-id="751d5-623">jQuery, bir to-do öğesi dizisini temsil eden JSON döndüren Web API 'sine bir HTTP GET isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="751d5-623">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="751d5-624">@No__t_0 geri çağırma işlevi, istek başarılı olursa çağrılır.</span><span class="sxs-lookup"><span data-stu-id="751d5-624">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="751d5-625">Geri aramada, DOM, yapılacaklar bilgileriyle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="751d5-625">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="751d5-626">Yapılacaklar öğesi ekleme</span><span class="sxs-lookup"><span data-stu-id="751d5-626">Add a to-do item</span></span>

<span data-ttu-id="751d5-627">jQuery, istek gövdesinde Yapılacaklar öğesiyle bir HTTP POST isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="751d5-627">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="751d5-628">@No__t_0 ve `contentType` seçenekleri, alınan ve gönderilen medya türünü belirtmek için `application/json` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="751d5-628">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="751d5-629">Yapılacaklar öğesi [JSON. stringbelirt](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)kullanılarak JSON 'a dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="751d5-629">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="751d5-630">API başarılı bir durum kodu döndürdüğünde, `getData` işlevi HTML tablosunu güncelleştirmek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="751d5-630">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="751d5-631">Yapılacaklar öğesini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="751d5-631">Update a to-do item</span></span>

<span data-ttu-id="751d5-632">Bir yapılacaklar öğesinin güncelleştirilmesi, bir tane eklemeye benzer.</span><span class="sxs-lookup"><span data-stu-id="751d5-632">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="751d5-633">@No__t_0 öğenin benzersiz tanımlayıcısını eklemek için değişir ve `type` `PUT`.</span><span class="sxs-lookup"><span data-stu-id="751d5-633">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="751d5-634">Bir yapılacaklar öğesini silme</span><span class="sxs-lookup"><span data-stu-id="751d5-634">Delete a to-do item</span></span>

<span data-ttu-id="751d5-635">Bir yapılacaklar öğesinin silinmesi, AJAX çağrısının `DELETE` `type` ayarlanarak ve öğenin URL 'sindeki benzersiz tanımlayıcısını belirterek gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="751d5-635">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="751d5-636">Web API 'sine kimlik doğrulama desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="751d5-636">Add authentication support to a web API</span></span>

<span data-ttu-id="751d5-637">Bkz. [ıdentityserver4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) öğreticisi.</span><span class="sxs-lookup"><span data-stu-id="751d5-637">See the [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="751d5-638">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="751d5-638">Additional resources</span></span>

<span data-ttu-id="751d5-639">[Bu öğretici için örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="751d5-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="751d5-640">Bkz. [indirme](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="751d5-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="751d5-641">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="751d5-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="751d5-642">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="751d5-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
