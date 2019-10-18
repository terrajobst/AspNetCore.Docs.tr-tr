---
title: "Öğretici: ASP.NET Core bir Web API 'SI oluşturma"
author: rick-anderson
description: ASP.NET Core ile Web API 'SI oluşturmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: tutorials/first-web-api
ms.openlocfilehash: b4c88f5dc7853396448a2a6122f3652f92079e68
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72541806"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="1acb7-103">Öğretici: ASP.NET Core bir Web API 'SI oluşturma</span><span class="sxs-lookup"><span data-stu-id="1acb7-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="1acb7-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike te son](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="1acb7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="1acb7-105">Bu öğreticide, ASP.NET Core ile Web API 'SI oluşturmanın temelleri öğretilir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="1acb7-106">Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="1acb7-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1acb7-107">Bir Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1acb7-107">Create a web API project.</span></span>
> * <span data-ttu-id="1acb7-108">Bir model sınıfı ve bir veritabanı bağlamı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="1acb7-109">CRUD yöntemleriyle bir denetleyiciyi dolandırın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="1acb7-110">Yönlendirmeyi, URL yollarını ve dönüş değerlerini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="1acb7-111">Postman ile Web API 'sini çağırın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-111">Call the web API with Postman.</span></span>

<span data-ttu-id="1acb7-112">Sonunda, bir veritabanında depolanan "yapılacaklar" öğelerini yönetebilmek için bir Web API 'SI vardır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="1acb7-113">Genel bakış</span><span class="sxs-lookup"><span data-stu-id="1acb7-113">Overview</span></span>

<span data-ttu-id="1acb7-114">Bu öğretici aşağıdaki uç noktaları içeren bir Web API 'SI oluşturur:</span><span class="sxs-lookup"><span data-stu-id="1acb7-114">This tutorial creates a web API containing the following endpoints:</span></span>

|<span data-ttu-id="1acb7-115">Uç Noktası</span><span class="sxs-lookup"><span data-stu-id="1acb7-115">Endpoint</span></span>                  |<span data-ttu-id="1acb7-116">Açıklama</span><span class="sxs-lookup"><span data-stu-id="1acb7-116">Description</span></span>            |<span data-ttu-id="1acb7-117">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="1acb7-117">Request body</span></span>|<span data-ttu-id="1acb7-118">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="1acb7-118">Response body</span></span>       |
|--------------------------|-----------------------|------------|--------------------|
|<span data-ttu-id="1acb7-119">/Api/TodoItems al</span><span class="sxs-lookup"><span data-stu-id="1acb7-119">GET /api/TodoItems</span></span>        |<span data-ttu-id="1acb7-120">Tüm yapılacaklar öğelerini Al</span><span class="sxs-lookup"><span data-stu-id="1acb7-120">Get all to-do items</span></span>    |<span data-ttu-id="1acb7-121">Yok.</span><span class="sxs-lookup"><span data-stu-id="1acb7-121">None</span></span>        |<span data-ttu-id="1acb7-122">Yapılacaklar öğeleri dizisi</span><span class="sxs-lookup"><span data-stu-id="1acb7-122">Array of to-do items</span></span>|
|<span data-ttu-id="1acb7-123">/Api/TodoItems/{id} al</span><span class="sxs-lookup"><span data-stu-id="1acb7-123">GET /api/TodoItems/{id}</span></span>   |<span data-ttu-id="1acb7-124">KIMLIĞE göre öğe al</span><span class="sxs-lookup"><span data-stu-id="1acb7-124">Get an item by ID</span></span>      |<span data-ttu-id="1acb7-125">Yok.</span><span class="sxs-lookup"><span data-stu-id="1acb7-125">None</span></span>        |<span data-ttu-id="1acb7-126">Yapılacaklar öğesi</span><span class="sxs-lookup"><span data-stu-id="1acb7-126">To-do item</span></span>          |
|<span data-ttu-id="1acb7-127">POST/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="1acb7-127">POST /api/TodoItems</span></span>       |<span data-ttu-id="1acb7-128">Yeni öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="1acb7-128">Add a new item</span></span>         |<span data-ttu-id="1acb7-129">Yapılacaklar öğesi</span><span class="sxs-lookup"><span data-stu-id="1acb7-129">To-do item</span></span>  |<span data-ttu-id="1acb7-130">Yapılacaklar öğesi</span><span class="sxs-lookup"><span data-stu-id="1acb7-130">To-do item</span></span>          |
|<span data-ttu-id="1acb7-131">/Api/TodoItems/{id} koy</span><span class="sxs-lookup"><span data-stu-id="1acb7-131">PUT /api/TodoItems/{id}</span></span>   |<span data-ttu-id="1acb7-132">Mevcut bir öğeyi güncelleştir</span><span class="sxs-lookup"><span data-stu-id="1acb7-132">Update an existing item</span></span>|<span data-ttu-id="1acb7-133">Yapılacaklar öğesi</span><span class="sxs-lookup"><span data-stu-id="1acb7-133">To-do item</span></span>  |<span data-ttu-id="1acb7-134">Yok.</span><span class="sxs-lookup"><span data-stu-id="1acb7-134">None</span></span>                |
|<span data-ttu-id="1acb7-135">/Api/TodoItems/{id} SIL</span><span class="sxs-lookup"><span data-stu-id="1acb7-135">DELETE /api/TodoItems/{id}</span></span>|<span data-ttu-id="1acb7-136">Öğe silme</span><span class="sxs-lookup"><span data-stu-id="1acb7-136">Delete an item</span></span>         |<span data-ttu-id="1acb7-137">Yok.</span><span class="sxs-lookup"><span data-stu-id="1acb7-137">None</span></span>        |<span data-ttu-id="1acb7-138">Yok.</span><span class="sxs-lookup"><span data-stu-id="1acb7-138">None</span></span>                |

<span data-ttu-id="1acb7-139">Aşağıdaki diyagramda uygulamanın tasarımı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-139">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki bir kutu ile temsil edilir.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="1acb7-145">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="1acb7-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1acb7-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1acb7-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1acb7-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1acb7-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1acb7-148">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1acb7-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="1acb7-149">Web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="1acb7-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1acb7-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1acb7-150">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="1acb7-151">**Dosya** menüsünden **Yeni**  > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-151">From the **File** menu, select **New** > **Project**.</span></span>
1. <span data-ttu-id="1acb7-152">**ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
1. <span data-ttu-id="1acb7-153">Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-153">Name the project *TodoApi* and click **Create**.</span></span>
1. <span data-ttu-id="1acb7-154">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,0** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="1acb7-155">**API** şablonunu seçin ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-155">Select the **API** template and click **Create**.</span></span>

![VS Yeni proje iletişim kutusu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1acb7-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1acb7-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="1acb7-158">[Tümleşik terminali](https://code.visualstudio.com/docs/editor/integrated-terminal)açın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
1. <span data-ttu-id="1acb7-159">Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
1. <span data-ttu-id="1acb7-160">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1acb7-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

1. <span data-ttu-id="1acb7-161">Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="1acb7-162">Önceki komutlar:</span><span class="sxs-lookup"><span data-stu-id="1acb7-162">The preceding commands:</span></span>

  * <span data-ttu-id="1acb7-163">Yeni bir Web API projesi oluşturun ve Visual Studio Code açın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-163">Create a new web API project and open it in Visual Studio Code.</span></span>
  * <span data-ttu-id="1acb7-164">Sonraki bölümde gerekli olan NuGet paketlerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-164">Add the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1acb7-165">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1acb7-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="1acb7-166">**Yeni çözüm** >  **Dosya** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-166">Select **File** > **New Solution**.</span></span>

    ![macOS yeni çözüm](first-web-api-mac/_static/sln.png)

1. <span data-ttu-id="1acb7-168">**.NET Core**  > **App**  > **API**  >  '**yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

    ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
1. <span data-ttu-id="1acb7-170">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **hedef Framework** \* *.NET Core 3,0*' i seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>
1. <span data-ttu-id="1acb7-171">**Proje adı** için *TodoApi* girin ve ardından **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="1acb7-173">Proje klasöründe bir komut terminali açın ve aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1acb7-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="1acb7-174">API 'YI test etme</span><span class="sxs-lookup"><span data-stu-id="1acb7-174">Test the API</span></span>

<span data-ttu-id="1acb7-175">Proje şablonu bir `WeatherForecast` API 'SI oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1acb7-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="1acb7-176">Uygulamayı test etmek için bir tarayıcıdan `Get` yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1acb7-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1acb7-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1acb7-178">Uygulamayı çalıştırmak için <kbd>CTRL + F5</kbd> tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-178">Press <kbd>Ctrl+F5</kbd> to run the app.</span></span> <span data-ttu-id="1acb7-179">Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>/WeatherForecast` gider.</span><span class="sxs-lookup"><span data-stu-id="1acb7-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="1acb7-180">IIS Express sertifikaya güvenip güvenmemeyi soran bir iletişim kutusu alırsanız **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="1acb7-181">Sonraki görüntülenen **güvenlik uyarısı** Iletişim kutusunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1acb7-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1acb7-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1acb7-183">Uygulamayı çalıştırmak için <kbd>CTRL + F5</kbd> tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-183">Press <kbd>Ctrl+F5</kbd> to run the app.</span></span> <span data-ttu-id="1acb7-184">Bir tarayıcıda aşağıdaki URL 'ye gidin: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="1acb7-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1acb7-185">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1acb7-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1acb7-186">Uygulamayı başlatmak için**hata ayıklamayı başlat**  >  **Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="1acb7-187">Mac için Visual Studio bir tarayıcı başlatır ve `<port>` rastgele seçilmiş bir bağlantı noktası numarası olduğu `https://localhost:<port>` ' a gider.</span><span class="sxs-lookup"><span data-stu-id="1acb7-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="1acb7-188">HTTP 404 (bulunamadı) hatası döndürüldü.</span><span class="sxs-lookup"><span data-stu-id="1acb7-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="1acb7-189">URL 'ye `/WeatherForecast` ekleyin (URL 'yi `https://localhost:<port>/WeatherForecast` olarak değiştirin).</span><span class="sxs-lookup"><span data-stu-id="1acb7-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="1acb7-190">Aşağıdakine benzer bir JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="1acb7-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="1acb7-191">Model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="1acb7-191">Add a model class</span></span>

<span data-ttu-id="1acb7-192">*Model* , uygulamanın yönettiği verileri temsil eden bir sınıf kümesidir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="1acb7-193">Bu uygulamanın modeli, tek bir `TodoItem` sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1acb7-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1acb7-194">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="1acb7-195">**Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="1acb7-196">**Yeni  >  klasör** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="1acb7-197">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-197">Name the folder *Models*.</span></span>
1. <span data-ttu-id="1acb7-198">*Modeller* klasörüne sağ tıklayın ve  > **sınıfı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="1acb7-199">Sınıfı *TodoItem* olarak adlandırın ve **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-199">Name the class *TodoItem* and select **Add**.</span></span>
1. <span data-ttu-id="1acb7-200">Şablon kodunu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="1acb7-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1acb7-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1acb7-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="1acb7-202">*Modeller*adlı bir klasör ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-202">Add a folder named *Models*.</span></span>
1. <span data-ttu-id="1acb7-203">*Modeller* klasörüne aşağıdaki kodla bir `TodoItem` sınıfı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="1acb7-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1acb7-204">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1acb7-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="1acb7-205">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-205">Right-click the project.</span></span> <span data-ttu-id="1acb7-206">**Yeni  >  klasör** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="1acb7-207">Klasör *modellerini*adlandırın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-207">Name the folder *Models*.</span></span>

    ![Yeni klasör](first-web-api-mac/_static/folder.png)

1. <span data-ttu-id="1acb7-209">*Modeller* klasörüne sağ tıklayın ve  > **yeni dosya** **ekle** ' yi  > **genel**  > **boş sınıf**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>
1. <span data-ttu-id="1acb7-210">Sınıfı *TodoItem*olarak adlandırın ve ardından **Yeni**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-210">Name the class *TodoItem*, and then click **New**.</span></span>
1. <span data-ttu-id="1acb7-211">Şablon kodunu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="1acb7-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="1acb7-212">@No__t_0 özelliği, ilişkisel bir veritabanındaki benzersiz anahtar olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="1acb7-213">Model sınıfları projede herhangi bir yere gidebilir, ancak *modeller* klasörü kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="1acb7-214">Veritabanı bağlamı ekleme</span><span class="sxs-lookup"><span data-stu-id="1acb7-214">Add a database context</span></span>

<span data-ttu-id="1acb7-215">*Veritabanı bağlamı* , bir veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="1acb7-216">Bu sınıf, `Microsoft.EntityFrameworkCore.DbContext` sınıfından türeterek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1acb7-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1acb7-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1acb7-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="1acb7-218">Microsoft. EntityFrameworkCore. SqlServer ekleyin</span><span class="sxs-lookup"><span data-stu-id="1acb7-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

1. <span data-ttu-id="1acb7-219">**Araçlar** menüsünde **nuget Paket Yöneticisi > çözüm Için NuGet Paketlerini Yönet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
1. <span data-ttu-id="1acb7-220">**Araştır** sekmesini seçin ve arama kutusuna **Microsoft. Entityframeworkcore. SqlServer** yazın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
1. <span data-ttu-id="1acb7-221">Sol bölmedeki **Microsoft. EntityFrameworkCore. SqlServer** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
1. <span data-ttu-id="1acb7-222">Sağ bölmedeki **Proje** onay kutusunu seçin ve ardından **Install**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
1. <span data-ttu-id="1acb7-223">@No__t_0 NuGet paketini eklemek için yukarıdaki yönergeleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet Paket Yöneticisi](first-web-api/_static/vs3nuget.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="1acb7-225">TodoContext veritabanı bağlamını ekleme</span><span class="sxs-lookup"><span data-stu-id="1acb7-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="1acb7-226">*Modeller* klasörüne sağ tıklayın ve  > **sınıfı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="1acb7-227">Sınıfı *TodoContext* olarak adlandırın ve **Ekle**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="1acb7-228">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1acb7-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="1acb7-229">*Modeller* klasörüne bir `TodoContext` sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="1acb7-230">Aşağıdaki kodu girin:</span><span class="sxs-lookup"><span data-stu-id="1acb7-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="1acb7-231">Veritabanı bağlamını kaydetme</span><span class="sxs-lookup"><span data-stu-id="1acb7-231">Register the database context</span></span>

<span data-ttu-id="1acb7-232">ASP.NET Core, veritabanı bağlamı gibi hizmetlerin [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısına kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-232">In ASP.NET Core, services such as the database context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="1acb7-233">Kapsayıcı hizmeti denetleyicilere sağlar.</span><span class="sxs-lookup"><span data-stu-id="1acb7-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="1acb7-234">Aşağıdaki Vurgulanan kodla *Startup.cs* güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="1acb7-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="1acb7-235">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="1acb7-235">The preceding code:</span></span>

* <span data-ttu-id="1acb7-236">Kullanılmayan `using` bildirimlerini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="1acb7-237">Veritabanı bağlamını dı kapsayıcısına ekler.</span><span class="sxs-lookup"><span data-stu-id="1acb7-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="1acb7-238">Veritabanı bağlamının bellek içi bir veritabanını kullanacağı belirtir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="1acb7-239">Denetleyiciyi bir denetleyiciye katlama</span><span class="sxs-lookup"><span data-stu-id="1acb7-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1acb7-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1acb7-240">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="1acb7-241">*Denetleyiciler* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-241">Right-click the *Controllers* folder.</span></span>
1. <span data-ttu-id="1acb7-242">@No__t_1**yeni yapı Iskelesi öğesi** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-242">Select **Add** > **New Scaffolded Item**.</span></span>
1. <span data-ttu-id="1acb7-243">**Entity Framework kullanarak ve eylemler Içeren API denetleyicisi**' ni seçin ve ardından **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
1. <span data-ttu-id="1acb7-244">**API denetleyiciyi eylemler Ile Ekle ' de Entity Framework** iletişim kutusunu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="1acb7-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>
    * <span data-ttu-id="1acb7-245">**Model sınıfında** **TodoItem (TodoApi. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
    * <span data-ttu-id="1acb7-246">**Veri bağlamı sınıfında** **TodoContext (TodoApi. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
    * <span data-ttu-id="1acb7-247">**Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="1acb7-248">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1acb7-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="1acb7-249">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1acb7-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="1acb7-250">Önceki komutlar:</span><span class="sxs-lookup"><span data-stu-id="1acb7-250">The preceding commands:</span></span>

* <span data-ttu-id="1acb7-251">Yapı iskelesi için gereken NuGet paketlerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="1acb7-252">Scafkatlama altyapısını (`dotnet-aspnet-codegenerator`) kurar.</span><span class="sxs-lookup"><span data-stu-id="1acb7-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="1acb7-253">@No__t_0 yapı iskelesi.</span><span class="sxs-lookup"><span data-stu-id="1acb7-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="1acb7-254">Oluşturulan kod:</span><span class="sxs-lookup"><span data-stu-id="1acb7-254">The generated code:</span></span>

* <span data-ttu-id="1acb7-255">Yöntemler olmadan bir API denetleyici sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="1acb7-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="1acb7-256">Sınıfı [[Apicontroller]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliğiyle süsler.</span><span class="sxs-lookup"><span data-stu-id="1acb7-256">Decorates the class with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute.</span></span> <span data-ttu-id="1acb7-257">Bu öznitelik, denetleyicinin Web API isteklerine yanıt verdiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="1acb7-258">Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz. <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="1acb7-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="1acb7-259">Veritabanı bağlamını (`TodoContext`) denetleyiciye eklemek için DI kullanır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="1acb7-260">Veritabanı bağlamı, denetleyicideki [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) yöntemlerinde her birinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="1acb7-261">PostTodoItem Create metodunu inceleyin</span><span class="sxs-lookup"><span data-stu-id="1acb7-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="1acb7-262">@No__t_0 ' deki return ifadesini, [NameOf](/dotnet/csharp/language-reference/operators/nameof) işlecini kullanacak şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="1acb7-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="1acb7-263">Yukarıdaki kod, [[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) özniteliğiyle gösterildiği gıbı bır http post yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-263">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) attribute.</span></span> <span data-ttu-id="1acb7-264">Yöntemi, HTTP isteğinin gövdesinden Yapılacaklar öğesinin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="1acb7-265">@No__t_0 yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1acb7-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="1acb7-266">Başarılı olursa bir HTTP 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="1acb7-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="1acb7-267">HTTP 201, sunucuda yeni bir kaynak oluşturan HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="1acb7-268">Yanıta bir [konum](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) üst bilgisi ekler.</span><span class="sxs-lookup"><span data-stu-id="1acb7-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="1acb7-269">@No__t_0 üstbilgisi, yeni oluşturulan Yapılacaklar öğesinin [URI](https://developer.mozilla.org/docs/Glossary/URI) 'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="1acb7-270">Daha fazla bilgi için bkz. [10.2.2 201 oluşturma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="1acb7-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="1acb7-271">@No__t_1 üst bilgisinin URI 'sini oluşturmak için `GetTodoItem` eyleme başvurur.</span><span class="sxs-lookup"><span data-stu-id="1acb7-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="1acb7-272">C# @No__t_1 anahtar sözcüğü, `CreatedAtAction` çağrısında eylem adının sabit kodlanmasını önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="1acb7-273">Postman yükleme</span><span class="sxs-lookup"><span data-stu-id="1acb7-273">Install Postman</span></span>

<span data-ttu-id="1acb7-274">Bu öğretici, Web API 'sini test etmek için Postman kullanır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-274">This tutorial uses Postman to test the web API.</span></span>

1. <span data-ttu-id="1acb7-275">[Postman](https://www.getpostman.com/downloads/) yükleme</span><span class="sxs-lookup"><span data-stu-id="1acb7-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
1. <span data-ttu-id="1acb7-276">Web uygulamasını başlatın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-276">Start the web app.</span></span>
1. <span data-ttu-id="1acb7-277">Postman 'ı başlatın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-277">Start Postman.</span></span>
1. <span data-ttu-id="1acb7-278">**SSL sertifikası doğrulamasını** devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="1acb7-278">Disable **SSL certificate verification**</span></span>
1. <span data-ttu-id="1acb7-279">**Dosya**  > **ayarları** ' ndan (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-279">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="1acb7-280">Denetleyiciyi test ettikten sonra SSL sertifikası doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="1acb7-281">Postman ile test PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="1acb7-281">Test PostTodoItem with Postman</span></span>

1. <span data-ttu-id="1acb7-282">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1acb7-282">Create a new request.</span></span>
1. <span data-ttu-id="1acb7-283">HTTP yöntemini `POST` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-283">Set the HTTP method to `POST`.</span></span>
1. <span data-ttu-id="1acb7-284">**Gövde** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-284">Select the **Body** tab.</span></span>
1. <span data-ttu-id="1acb7-285">**Ham** radyo düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-285">Select the **raw** radio button.</span></span>
1. <span data-ttu-id="1acb7-286">Türü **JSON (Application/JSON)** olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-286">Set the type to **JSON (application/json)**.</span></span>
1. <span data-ttu-id="1acb7-287">İstek gövdesinde, bir yapılacaklar öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="1acb7-287">In the request body, enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

1. <span data-ttu-id="1acb7-288">**Gönder**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-288">Select **Send**.</span></span>

  ![Oluşturma isteğiyle Postman](first-web-api/_static/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="1acb7-290">Konum üst bilgisi URI 'sini test etme</span><span class="sxs-lookup"><span data-stu-id="1acb7-290">Test the location header URI</span></span>

1. <span data-ttu-id="1acb7-291">**Yanıt** bölmesinde **üstbilgiler** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-291">Select the **Headers** tab in the **Response** pane.</span></span>
1. <span data-ttu-id="1acb7-292">**Konum** üst bilgisi değerini kopyalayın:</span><span class="sxs-lookup"><span data-stu-id="1acb7-292">Copy the **Location** header value:</span></span>

    ![Postman konsolunun üstbilgiler sekmesi](first-web-api/_static/create.png)

1. <span data-ttu-id="1acb7-294">ALıNACAK yöntemi ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-294">Set the method to GET.</span></span>
1. <span data-ttu-id="1acb7-295">URI 'yi yapıştırın (örneğin, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="1acb7-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
1. <span data-ttu-id="1acb7-296">**Gönder**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="1acb7-297">GET yöntemlerini inceleyin</span><span class="sxs-lookup"><span data-stu-id="1acb7-297">Examine the GET methods</span></span>

<span data-ttu-id="1acb7-298">Bu yöntemler iki al uç noktası uygular:</span><span class="sxs-lookup"><span data-stu-id="1acb7-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="1acb7-299">Tarayıcıdan veya Postman 'dan iki uç noktayı çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="1acb7-300">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1acb7-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="1acb7-301">Aşağıdakine benzer bir yanıt, `GetTodoItems` çağrısı tarafından üretilir:</span><span class="sxs-lookup"><span data-stu-id="1acb7-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="1acb7-302">Postman ile test al</span><span class="sxs-lookup"><span data-stu-id="1acb7-302">Test Get with Postman</span></span>

1. <span data-ttu-id="1acb7-303">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1acb7-303">Create a new request.</span></span>
1. <span data-ttu-id="1acb7-304">**Almak**için http yöntemini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-304">Set the HTTP method to **GET**.</span></span>
1. <span data-ttu-id="1acb7-305">İstek URL 'sini `https://localhost:<port>/api/TodoItems` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="1acb7-306">Örneğin, `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="1acb7-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
1. <span data-ttu-id="1acb7-307">Postman 'da **iki bölme görünümü** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-307">Set **Two pane view** in Postman.</span></span>
1. <span data-ttu-id="1acb7-308">**Gönder**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-308">Select **Send**.</span></span>

<span data-ttu-id="1acb7-309">Bu uygulama, bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-309">This app uses an in-memory database.</span></span> <span data-ttu-id="1acb7-310">Uygulama durdurulup başlatılırsa, önceki GET isteği herhangi bir veri döndürmez.</span><span class="sxs-lookup"><span data-stu-id="1acb7-310">If the app is stopped and started, the preceding GET request won't return any data.</span></span> <span data-ttu-id="1acb7-311">Hiçbir veri döndürülmezse, verileri uygulamaya [gönderin](#post) .</span><span class="sxs-lookup"><span data-stu-id="1acb7-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="1acb7-312">Yönlendirme ve URL yolları</span><span class="sxs-lookup"><span data-stu-id="1acb7-312">Routing and URL paths</span></span>

<span data-ttu-id="1acb7-313">[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) ÖZNITELIĞI BIR HTTP GET isteğine yanıt veren bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-313">The [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="1acb7-314">Her yöntemin URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="1acb7-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="1acb7-315">Denetleyicinin `Route` özniteliğinde şablon dizesiyle başlayın:</span><span class="sxs-lookup"><span data-stu-id="1acb7-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="1acb7-316">@No__t_0, denetleyicinin adıyla değiştirin, bu kural, denetleyici sınıf adı "denetleyici" sonekidir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="1acb7-317">Bu örnek için denetleyici sınıfı adı **todoıtems**denetleyicisidir, bu nedenle denetleyicinin adı "todoıtems" olur.</span><span class="sxs-lookup"><span data-stu-id="1acb7-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="1acb7-318">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="1acb7-319">@No__t_0 özniteliğinin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="1acb7-320">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="1acb7-320">This sample doesn't use a template.</span></span> <span data-ttu-id="1acb7-321">Daha fazla bilgi için bkz. [http [fiil] öznitelikleriyle öznitelik yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="1acb7-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="1acb7-322">Aşağıdaki `GetTodoItem` yönteminde, `"{id}"` Yapılacaklar öğesinin benzersiz tanımlayıcısı için bir yer tutucu değişkenidir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="1acb7-323">@No__t_0 çağrıldığında, URL 'deki `"{id}"` değeri `id` parametresindeki yöntemine sağlanır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="1acb7-324">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="1acb7-324">Return values</span></span>

<span data-ttu-id="1acb7-325">@No__t_0 ve `GetTodoItem` yöntemlerinin dönüş türü [ActionResult \<T > türüdür](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="1acb7-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="1acb7-326">ASP.NET Core nesneyi [JSON](https://www.json.org/) 'a otomatik olarak serileştirir ve yanıt ILETISININ gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="1acb7-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="1acb7-327">Bu dönüş türü için yanıt kodu, işlenmemiş özel durum olmadığı varsayılarak 200 ' dir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="1acb7-328">İşlenmemiş özel durumlar 5 xx hataya çevrilir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="1acb7-329">`ActionResult` dönüş türleri, çok çeşitli HTTP durum kodlarını temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="1acb7-330">Örneğin, `GetTodoItem` iki farklı durum değeri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="1acb7-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="1acb7-331">İstenen KIMLIKLE eşleşen hiçbir öğe yoksa, yöntem 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound> hata kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="1acb7-331">If no item matches the requested ID, the method returns a 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound> error code.</span></span>
* <span data-ttu-id="1acb7-332">Aksi takdirde, yöntemi bir JSON yanıt gövdesi ile 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="1acb7-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="1acb7-333">@No__t_0 sonuçları bir HTTP 200 yanıtına döndürülüyor.</span><span class="sxs-lookup"><span data-stu-id="1acb7-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="1acb7-334">PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="1acb7-334">The PutTodoItem method</span></span>

<span data-ttu-id="1acb7-335">@No__t_0 yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="1acb7-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="1acb7-336">`PutTodoItem`, HTTP PUT kullanması dışında `PostTodoItem` benzerdir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="1acb7-337">Yanıt 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="1acb7-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="1acb7-338">HTTP belirtimine göre bir PUT isteği, istemcinin yalnızca değişiklikleri değil, tüm güncelleştirilmiş varlığı göndermesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1acb7-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="1acb7-339">Kısmi güncelleştirmeleri desteklemek için [http Patch](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)kullanın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="1acb7-340">@No__t_0 çağırırken bir hata alırsanız, veritabanında bir öğe olduğundan emin olmak için `GET` çağırın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="1acb7-341">PutTodoItem yöntemini test etme</span><span class="sxs-lookup"><span data-stu-id="1acb7-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="1acb7-342">Bu örnek, uygulama her başlatıldığında başlatılmış olması gereken bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-342">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="1acb7-343">Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1acb7-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="1acb7-344">PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="1acb7-345">KIMLIĞI 1 olan Yapılacaklar öğesini güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-345">Update the to-do item that has an ID of 1.</span></span> <span data-ttu-id="1acb7-346">Adını "Feed balık" olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="1acb7-346">Set its name to "feed fish":</span></span>

```json
{
  "ID":1,
  "name":"feed fish",
  "isComplete":true
}
```

<span data-ttu-id="1acb7-347">Aşağıdaki görüntüde Postman güncelleştirmesi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="1acb7-347">The following image shows the Postman update:</span></span>

![204 (Içerik yok) yanıtı gösteren Postman konsolu](first-web-api/_static/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="1acb7-349">DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="1acb7-349">The DeleteTodoItem method</span></span>

<span data-ttu-id="1acb7-350">@No__t_0 yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="1acb7-350">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="1acb7-351">@No__t_0 yanıtı 204 ' dir [(Içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="1acb7-351">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="1acb7-352">DeleteTodoItem yöntemini test etme</span><span class="sxs-lookup"><span data-stu-id="1acb7-352">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="1acb7-353">Bir yapılacaklar öğesini silmek için Postman kullanın:</span><span class="sxs-lookup"><span data-stu-id="1acb7-353">Use Postman to delete a to-do item:</span></span>

1. <span data-ttu-id="1acb7-354">Yöntemini `DELETE` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="1acb7-354">Set the method to `DELETE`.</span></span>
1. <span data-ttu-id="1acb7-355">Silinecek nesnenin URI 'sini ayarlayın (örneğin, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="1acb7-355">Set the URI of the object to delete (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
1. <span data-ttu-id="1acb7-356">**Gönder**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="1acb7-356">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="1acb7-357">JavaScript ile Web API 'sini çağırma</span><span class="sxs-lookup"><span data-stu-id="1acb7-357">Call the web API with JavaScript</span></span>

<span data-ttu-id="1acb7-358">Bkz. [öğretici: JavaScript ile ASP.NET Core Web API 'Si çağırma](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="1acb7-358">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="1acb7-359">Web API 'sine kimlik doğrulama desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="1acb7-359">Add authentication support to a web API</span></span>

<span data-ttu-id="1acb7-360">Bkz. [ıdentityserver4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) öğreticisi.</span><span class="sxs-lookup"><span data-stu-id="1acb7-360">See the [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1acb7-361">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1acb7-361">Additional resources</span></span>

<span data-ttu-id="1acb7-362">[Bu öğretici için örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="1acb7-362">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="1acb7-363">Bkz. [indirme](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="1acb7-363">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="1acb7-364">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="1acb7-364">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="1acb7-365">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="1acb7-365">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
