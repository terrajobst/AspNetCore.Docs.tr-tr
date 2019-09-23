---
title: "Öğretici: ASP.NET Core ile Web API 'SI oluşturma"
author: rick-anderson
description: ASP.NET Core ile Web API 'SI oluşturmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 2d0eb24641c3d1f795b9e85ce10d42ee96d30846
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187313"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="61d00-103">Öğretici: ASP.NET Core ile Web API 'SI oluşturma</span><span class="sxs-lookup"><span data-stu-id="61d00-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="61d00-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="61d00-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="61d00-105">Bu öğretici, bir web API ASP.NET Core ile oluşturmaya ilişkin temel bilgileri öğretir.</span><span class="sxs-lookup"><span data-stu-id="61d00-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="61d00-106">Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="61d00-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="61d00-107">Bir Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="61d00-107">Create a web API project.</span></span>
> * <span data-ttu-id="61d00-108">Bir model sınıfı ve bir veritabanı bağlamı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="61d00-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="61d00-109">CRUD yöntemleriyle bir denetleyiciyi dolandırın.</span><span class="sxs-lookup"><span data-stu-id="61d00-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="61d00-110">Yönlendirmeyi, URL yollarını ve dönüş değerlerini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="61d00-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="61d00-111">Web API'si Postman ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="61d00-111">Call the web API with Postman.</span></span>

<span data-ttu-id="61d00-112">Sonunda, bir veritabanında depolanan "yapılacaklar" öğelerini yönetebilmek için bir Web API 'SI vardır.</span><span class="sxs-lookup"><span data-stu-id="61d00-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="61d00-113">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="61d00-113">Overview</span></span>

<span data-ttu-id="61d00-114">Bu öğretici yandaki API oluşturur:</span><span class="sxs-lookup"><span data-stu-id="61d00-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="61d00-115">API</span><span class="sxs-lookup"><span data-stu-id="61d00-115">API</span></span> | <span data-ttu-id="61d00-116">Açıklama</span><span class="sxs-lookup"><span data-stu-id="61d00-116">Description</span></span> | <span data-ttu-id="61d00-117">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="61d00-117">Request body</span></span> | <span data-ttu-id="61d00-118">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="61d00-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="61d00-119">/Api/TodoItems al</span><span class="sxs-lookup"><span data-stu-id="61d00-119">GET /api/TodoItems</span></span> | <span data-ttu-id="61d00-120">Tüm yapılacak iş öğeleri al</span><span class="sxs-lookup"><span data-stu-id="61d00-120">Get all to-do items</span></span> | <span data-ttu-id="61d00-121">Yok.</span><span class="sxs-lookup"><span data-stu-id="61d00-121">None</span></span> | <span data-ttu-id="61d00-122">Yapılacaklar öğelerinin bir dizisi</span><span class="sxs-lookup"><span data-stu-id="61d00-122">Array of to-do items</span></span>|
|<span data-ttu-id="61d00-123">/Api/TodoItems/{id} al</span><span class="sxs-lookup"><span data-stu-id="61d00-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="61d00-124">Bir öğeyi Kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="61d00-124">Get an item by ID</span></span> | <span data-ttu-id="61d00-125">Yok.</span><span class="sxs-lookup"><span data-stu-id="61d00-125">None</span></span> | <span data-ttu-id="61d00-126">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="61d00-126">To-do item</span></span>|
|<span data-ttu-id="61d00-127">POST/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="61d00-127">POST /api/TodoItems</span></span> | <span data-ttu-id="61d00-128">Yeni Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="61d00-128">Add a new item</span></span> | <span data-ttu-id="61d00-129">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="61d00-129">To-do item</span></span> | <span data-ttu-id="61d00-130">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="61d00-130">To-do item</span></span> |
|<span data-ttu-id="61d00-131">/Api/TodoItems/{id} koy</span><span class="sxs-lookup"><span data-stu-id="61d00-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="61d00-132">Mevcut öğeyi güncelleştirin &nbsp;</span><span class="sxs-lookup"><span data-stu-id="61d00-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="61d00-133">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="61d00-133">To-do item</span></span> | <span data-ttu-id="61d00-134">Yok.</span><span class="sxs-lookup"><span data-stu-id="61d00-134">None</span></span> |
|<span data-ttu-id="61d00-135">/Api/TodoItems/{id} &nbsp; Sil&nbsp;</span><span class="sxs-lookup"><span data-stu-id="61d00-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="61d00-136">Öğeyi Sil &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="61d00-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="61d00-137">Hiçbiri</span><span class="sxs-lookup"><span data-stu-id="61d00-137">None</span></span> | <span data-ttu-id="61d00-138">Hiçbiri</span><span class="sxs-lookup"><span data-stu-id="61d00-138">None</span></span>|

<span data-ttu-id="61d00-139">Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="61d00-139">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki bir kutu ile temsil edilir.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="61d00-145">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="61d00-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61d00-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="61d00-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="61d00-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="61d00-148">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="61d00-149">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="61d00-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61d00-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="61d00-151">**Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="61d00-152">**ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61d00-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="61d00-153">Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61d00-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="61d00-154">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,0** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="61d00-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="61d00-155">**API** şablonunu seçin ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61d00-155">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="61d00-156">**Docker desteğini etkinleştir** **' i seçmeyin** .</span><span class="sxs-lookup"><span data-stu-id="61d00-156">**Don't** select **Enable Docker Support**.</span></span>

![VS yeni proje iletişim kutusu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="61d00-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="61d00-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="61d00-159">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="61d00-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="61d00-160">Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="61d00-160">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="61d00-161">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="61d00-161">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="61d00-162">Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-162">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="61d00-163">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="61d00-163">The preceding commands:</span></span>

  * <span data-ttu-id="61d00-164">Yeni bir Web API projesi oluşturur ve Visual Studio Code açar.</span><span class="sxs-lookup"><span data-stu-id="61d00-164">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="61d00-165">Sonraki bölümde gerekli olan NuGet paketlerini ekler.</span><span class="sxs-lookup"><span data-stu-id="61d00-165">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="61d00-166">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="61d00-167">**Dosya** > **yeni çözüm**' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-167">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="61d00-169">**.NET Core** > **uygulama** **API 'si** ileri ' **yi**seçin. > ></span><span class="sxs-lookup"><span data-stu-id="61d00-169">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="61d00-171">**Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **hedef Framework** \* *.NET Core 3,0*' i seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-171">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="61d00-172">Girin *TodoApi* için **proje adı** seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="61d00-172">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="61d00-174">Proje klasöründe bir komut terminali açın ve aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="61d00-174">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="61d00-175">API'yi test etme</span><span class="sxs-lookup"><span data-stu-id="61d00-175">Test the API</span></span>

<span data-ttu-id="61d00-176">Proje şablonu oluşturur bir `WeatherForecast` API.</span><span class="sxs-lookup"><span data-stu-id="61d00-176">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="61d00-177">Çağrı `Get` uygulamayı test etmek için bir tarayıcıdan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="61d00-177">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61d00-178">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-178">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="61d00-179">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="61d00-179">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="61d00-180">Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>/WeatherForecast`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="61d00-180">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="61d00-181">IIS Express sertifika güven varsa soran bir iletişim kutusu alırsanız seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="61d00-181">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="61d00-182">İçinde **Güvenlik Uyarısı** ardından, görüntülenen iletişim seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="61d00-182">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="61d00-183">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="61d00-183">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="61d00-184">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="61d00-184">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="61d00-185">Bir tarayıcıda aşağıdaki URL'ye gidin: [ https://localhost:5001/WeatherForecast ](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="61d00-185">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="61d00-186">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-186">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="61d00-187">Uygulamayı başlatmak için**hata ayıklamayı Başlat** ' **ı seçin.**  > </span><span class="sxs-lookup"><span data-stu-id="61d00-187">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="61d00-188">Mac için Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="61d00-188">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="61d00-189">HTTP 404 (bulunamadı) hatası döndürülür.</span><span class="sxs-lookup"><span data-stu-id="61d00-189">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="61d00-190">Append `/WeatherForecast` URL'sine (URL'yi `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="61d00-190">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="61d00-191">Aşağıdakine benzer bir JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="61d00-191">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="61d00-192">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="61d00-192">Add a model class</span></span>

<span data-ttu-id="61d00-193">A *modeli* uygulamayı yöneten verilerini temsil eden sınıflar kümesidir.</span><span class="sxs-lookup"><span data-stu-id="61d00-193">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="61d00-194">Tek bir modeldir bu uygulama için `TodoItem` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="61d00-194">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61d00-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-195">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="61d00-196">İçinde **Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61d00-196">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="61d00-197">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="61d00-197">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="61d00-198">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="61d00-198">Name the folder *Models*.</span></span>

* <span data-ttu-id="61d00-199">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="61d00-199">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="61d00-200">Sınıf adı *Todoıtem* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="61d00-200">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="61d00-201">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="61d00-201">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="61d00-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="61d00-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="61d00-203">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="61d00-203">Add a folder named *Models*.</span></span>

* <span data-ttu-id="61d00-204">Ekleme bir `TodoItem` sınıfının *modelleri* aşağıdaki kodla klasörü:</span><span class="sxs-lookup"><span data-stu-id="61d00-204">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="61d00-205">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-205">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="61d00-206">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61d00-206">Right-click the project.</span></span> <span data-ttu-id="61d00-207">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="61d00-207">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="61d00-208">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="61d00-208">Name the folder *Models*.</span></span>

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="61d00-210">*Modeller* klasörüne sağ tıklayın ve **yeni dosya** > **Ekle** > **genel** > **boş sınıfı**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-210">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="61d00-211">Sınıf adı *Todoıtem*ve ardından **yeni**.</span><span class="sxs-lookup"><span data-stu-id="61d00-211">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="61d00-212">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="61d00-212">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="61d00-213">`Id` Özelliği işlevlerinin bir ilişkisel veritabanında benzersiz anahtar.</span><span class="sxs-lookup"><span data-stu-id="61d00-213">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="61d00-214">Model sınıfları herhangi bir projede gidip ancak *modelleri* klasörü, kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="61d00-214">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="61d00-215">Veritabanı bağlamı Ekle</span><span class="sxs-lookup"><span data-stu-id="61d00-215">Add a database context</span></span>

<span data-ttu-id="61d00-216">*Veritabanı bağlamı* koordine eden bir veri modeli için Entity Framework işlevsellik ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="61d00-216">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="61d00-217">Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="61d00-217">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61d00-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-218">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="61d00-219">Microsoft. EntityFrameworkCore. SqlServer ekleyin</span><span class="sxs-lookup"><span data-stu-id="61d00-219">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="61d00-220">**Araçlar** menüsünde **nuget Paket Yöneticisi > çözüm Için NuGet Paketlerini Yönet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-220">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="61d00-221">**Ön sürümü dahil et** onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-221">Select the **Include prerelease** checkbox.</span></span>
* <span data-ttu-id="61d00-222">**Araştır** sekmesini seçin ve arama kutusuna **Microsoft. Entityframeworkcore. SqlServer** yazın.</span><span class="sxs-lookup"><span data-stu-id="61d00-222">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="61d00-223">Sol bölmedeki **Microsoft. EntityFrameworkCore. SqlServer v 3.0.0-Preview** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-223">Select  **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** in the left pane.</span></span>
* <span data-ttu-id="61d00-224">Sağ bölmedeki **Proje** onay kutusunu seçin ve ardından **Install**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-224">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="61d00-225">Önceki yönergeleri kullanarak `Microsoft.EntityFrameworkCore.InMemory` NuGet paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="61d00-225">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet Paket Yöneticisi](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="61d00-227">TodoContext veritabanı bağlamını ekleme</span><span class="sxs-lookup"><span data-stu-id="61d00-227">Add the TodoContext database context</span></span>

* <span data-ttu-id="61d00-228">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="61d00-228">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="61d00-229">Sınıf adı *TodoContext* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="61d00-229">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="61d00-230">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-230">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="61d00-231">Ekleme bir `TodoContext` sınıfının *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="61d00-231">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="61d00-232">Aşağıdaki kodu girin:</span><span class="sxs-lookup"><span data-stu-id="61d00-232">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="61d00-233">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="61d00-233">Register the database context</span></span>

<span data-ttu-id="61d00-234">ASP.NET Core DB bağlamı gibi hizmetler ile kaydedilmelidir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="61d00-234">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="61d00-235">Kapsayıcı hizmeti denetleyicilerine sağlar.</span><span class="sxs-lookup"><span data-stu-id="61d00-235">The container provides the service to controllers.</span></span>

<span data-ttu-id="61d00-236">Güncelleştirme *Startup.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="61d00-236">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="61d00-237">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="61d00-237">The preceding code:</span></span>

* <span data-ttu-id="61d00-238">Kullanılmayan kaldırır `using` bildirimleri.</span><span class="sxs-lookup"><span data-stu-id="61d00-238">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="61d00-239">Veritabanı bağlamı DI kapsayıcıya ekler.</span><span class="sxs-lookup"><span data-stu-id="61d00-239">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="61d00-240">Veritabanı bağlamı bir bellek içi veritabanına kullanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="61d00-240">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="61d00-241">Denetleyiciyi bir denetleyiciye katlama</span><span class="sxs-lookup"><span data-stu-id="61d00-241">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61d00-242">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-242">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="61d00-243">Sağ *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="61d00-243">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="61d00-244">**Yeni yapı iskelesi öğesi** **Ekle** > öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-244">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="61d00-245">**Entity Framework kullanarak ve eylemler Içeren API denetleyicisi**' ni seçin ve ardından **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-245">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="61d00-246">**API denetleyiciyi eylemler Ile Ekle ' de Entity Framework** iletişim kutusunu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="61d00-246">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="61d00-247">**Model sınıfında** **TodoItem (TodoAPI. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-247">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="61d00-248">**Veri bağlamı sınıfında** **TodoContext (TodoAPI. modeller)** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-248">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="61d00-249">**Ekle** 'yi seçin</span><span class="sxs-lookup"><span data-stu-id="61d00-249">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="61d00-250">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-250">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="61d00-251">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="61d00-251">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="61d00-252">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="61d00-252">The preceding commands:</span></span>

* <span data-ttu-id="61d00-253">Yapı iskelesi için gereken NuGet paketlerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="61d00-253">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="61d00-254">Scafkatlama altyapısını (`dotnet-aspnet-codegenerator`) kurar.</span><span class="sxs-lookup"><span data-stu-id="61d00-254">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="61d00-255">Yapı iskelesi `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="61d00-255">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="61d00-256">Oluşturulan kod:</span><span class="sxs-lookup"><span data-stu-id="61d00-256">The generated code:</span></span>

* <span data-ttu-id="61d00-257">Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.</span><span class="sxs-lookup"><span data-stu-id="61d00-257">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="61d00-258">Sınıfı [[Apicontroller]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle süsler.</span><span class="sxs-lookup"><span data-stu-id="61d00-258">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="61d00-259">Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="61d00-259">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="61d00-260">Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="61d00-260">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="61d00-261">Veritabanı bağlamı eklemesine DI kullanır (`TodoContext`) içine denetleyici.</span><span class="sxs-lookup"><span data-stu-id="61d00-261">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="61d00-262">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="61d00-262">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="61d00-263">PostTodoItem Create metodunu inceleyin</span><span class="sxs-lookup"><span data-stu-id="61d00-263">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="61d00-264">İçindeki `PostTodoItem` return ifadesini, [NameOf](/dotnet/csharp/language-reference/operators/nameof) işlecini kullanmak için değiştirin:</span><span class="sxs-lookup"><span data-stu-id="61d00-264">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="61d00-265">Yukarıdaki kod tarafından belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="61d00-265">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="61d00-266">Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="61d00-266">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="61d00-267"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="61d00-267">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="61d00-268">Başarılı olursa bir HTTP 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="61d00-268">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="61d00-269">HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="61d00-269">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="61d00-270">Yanıta bir [konum](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) üst bilgisi ekler.</span><span class="sxs-lookup"><span data-stu-id="61d00-270">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="61d00-271">Üst bilgi, yeni oluşturulan Yapılacaklar öğesinin URI 'sini belirtir. [](https://developer.mozilla.org/docs/Glossary/URI) `Location`</span><span class="sxs-lookup"><span data-stu-id="61d00-271">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="61d00-272">Daha fazla bilgi için [10.2.2 201 oluşturuldu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="61d00-272">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="61d00-273">`Location` Üstbilginin URI 'sini oluşturma eylemine başvurur. `GetTodoItem`</span><span class="sxs-lookup"><span data-stu-id="61d00-273">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="61d00-274">Anahtar sözcüğü, `CreatedAtAction` çağrıda eylem adının sabit kodlanmasını önlemek için kullanılır. C# `nameof`</span><span class="sxs-lookup"><span data-stu-id="61d00-274">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="61d00-275">Postman yükleme</span><span class="sxs-lookup"><span data-stu-id="61d00-275">Install Postman</span></span>

<span data-ttu-id="61d00-276">Bu öğreticide Postman web API'si test etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="61d00-276">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="61d00-277">Yükleme [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="61d00-277">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="61d00-278">Web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="61d00-278">Start the web app.</span></span>
* <span data-ttu-id="61d00-279">Postman'i başlatın.</span><span class="sxs-lookup"><span data-stu-id="61d00-279">Start Postman.</span></span>
* <span data-ttu-id="61d00-280">Devre dışı **SSL sertifika doğrulama**</span><span class="sxs-lookup"><span data-stu-id="61d00-280">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="61d00-281">Gelen **Dosya > Ayarlar** (\**genel* sekmesinde), devre dışı **SSL sertifika doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="61d00-281">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="61d00-282">Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="61d00-282">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="61d00-283">Postman ile test PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="61d00-283">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="61d00-284">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="61d00-284">Create a new request.</span></span>
* <span data-ttu-id="61d00-285">HTTP yöntemini olarak `POST`ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="61d00-285">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="61d00-286">Seçin **gövdesi** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="61d00-286">Select the **Body** tab.</span></span>
* <span data-ttu-id="61d00-287">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="61d00-287">Select the **raw** radio button.</span></span>
* <span data-ttu-id="61d00-288">Tür kümesine **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="61d00-288">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="61d00-289">İstek gövdesinde bir yapılacak iş öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="61d00-289">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="61d00-290">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-290">Select **Send**.</span></span>

  ![Postman ile isteği oluştur](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="61d00-292">Konum üst bilgisi URI test</span><span class="sxs-lookup"><span data-stu-id="61d00-292">Test the location header URI</span></span>

* <span data-ttu-id="61d00-293">Seçin **üstbilgileri** sekmesinde **yanıt** bölmesi.</span><span class="sxs-lookup"><span data-stu-id="61d00-293">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="61d00-294">Kopyalama **konumu** üst bilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="61d00-294">Copy the **Location** header value:</span></span>

  ![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/3/create.png)

* <span data-ttu-id="61d00-296">Yöntemini GET öğesine Ayarla.</span><span class="sxs-lookup"><span data-stu-id="61d00-296">Set the method to GET.</span></span>
* <span data-ttu-id="61d00-297">URİ'sini yapıştırın (örneğin, `https://localhost:5001/api/TodoItems/1`)</span><span class="sxs-lookup"><span data-stu-id="61d00-297">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="61d00-298">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-298">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="61d00-299">GET yöntemlerini inceleyin</span><span class="sxs-lookup"><span data-stu-id="61d00-299">Examine the GET methods</span></span>

<span data-ttu-id="61d00-300">İki GET uç noktası bu yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="61d00-300">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="61d00-301">Tarayıcıdan veya Postman 'dan iki uç noktayı çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="61d00-301">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="61d00-302">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="61d00-302">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="61d00-303">Şuna benzer bir yanıt, şu çağrı `GetTodoItems`tarafından üretilir:</span><span class="sxs-lookup"><span data-stu-id="61d00-303">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="61d00-304">Postman ile test al</span><span class="sxs-lookup"><span data-stu-id="61d00-304">Test Get with Postman</span></span>

* <span data-ttu-id="61d00-305">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="61d00-305">Create a new request.</span></span>
* <span data-ttu-id="61d00-306">HTTP yöntemi kümesine **alma**.</span><span class="sxs-lookup"><span data-stu-id="61d00-306">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="61d00-307">İstek URL'si kümesine `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="61d00-307">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="61d00-308">Örneğin: `https://localhost:5001/api/TodoItems`</span><span class="sxs-lookup"><span data-stu-id="61d00-308">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="61d00-309">Ayarlama **iki bölme görünümü** postman'deki.</span><span class="sxs-lookup"><span data-stu-id="61d00-309">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="61d00-310">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-310">Select **Send**.</span></span>

<span data-ttu-id="61d00-311">Bu uygulama, bellek içi bir veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="61d00-311">This app uses an in-memory database.</span></span> <span data-ttu-id="61d00-312">Uygulama durdurulup başlatılırsa, önceki GET isteği herhangi bir veri döndürmez.</span><span class="sxs-lookup"><span data-stu-id="61d00-312">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="61d00-313">Hiçbir veri döndürülmezse, verileri uygulamaya [gönderin](#post) .</span><span class="sxs-lookup"><span data-stu-id="61d00-313">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="61d00-314">URL Yönlendirme ve yolları</span><span class="sxs-lookup"><span data-stu-id="61d00-314">Routing and URL paths</span></span>

<span data-ttu-id="61d00-315">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Özniteliği bir HTTP GET isteğine yanıt vermeden bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="61d00-315">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="61d00-316">Her yöntem için URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="61d00-316">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="61d00-317">Denetleyicinin şablonu dizesi ile başlayıp `Route` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="61d00-317">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="61d00-318">Değiştirin `[controller]` denetleyicinin adı ile kural tarafından olduğu "Controller" soneki eksi denetleyici sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="61d00-318">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="61d00-319">Bu örnek için denetleyici sınıfı adı **todoıtems**denetleyicisidir, bu nedenle denetleyicinin adı "todoıtems" olur.</span><span class="sxs-lookup"><span data-stu-id="61d00-319">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="61d00-320">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="61d00-320">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="61d00-321">Özniteliğin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin. `[HttpGet]`</span><span class="sxs-lookup"><span data-stu-id="61d00-321">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="61d00-322">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="61d00-322">This sample doesn't use a template.</span></span> <span data-ttu-id="61d00-323">Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="61d00-323">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="61d00-324">Aşağıdaki `GetTodoItem` yöntemi `"{id}"` yapılacak iş öğesi benzersiz tanımlayıcısı için bir yer tutucu değişkendir.</span><span class="sxs-lookup"><span data-stu-id="61d00-324">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="61d00-325">Zaman `GetTodoItem` çağrılır, değerini `"{id}"` yöntemine URL'de sağlanan kendi`id` parametresi.</span><span class="sxs-lookup"><span data-stu-id="61d00-325">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="61d00-326">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="61d00-326">Return values</span></span>

<span data-ttu-id="61d00-327">Dönüş türünü `GetTodoItems` ve `GetTodoItem` yöntemler [actionresult öğesini\<T > türü](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="61d00-327">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="61d00-328">ASP.NET Core, nesneyi otomatik olarak serileştiren [JSON](https://www.json.org/) ve yanıt iletisinin gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="61d00-328">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="61d00-329">Yanıt kodu 200 bu dönüş türü için olduğu varsayılırsa işlenmeyen özel durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="61d00-329">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="61d00-330">İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.</span><span class="sxs-lookup"><span data-stu-id="61d00-330">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="61d00-331">`ActionResult` dönüş türleri, geniş HTTP durum kodları temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="61d00-331">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="61d00-332">Örneğin, `GetTodoItem` iki farklı durum değerleri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="61d00-332">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="61d00-333">Öğe istenen kimliği eşleşirse, yöntem bir 404 döndürür [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu.</span><span class="sxs-lookup"><span data-stu-id="61d00-333">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="61d00-334">Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="61d00-334">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="61d00-335">Döndüren `item` sonuçları bir HTTP 200 yanıtı.</span><span class="sxs-lookup"><span data-stu-id="61d00-335">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="61d00-336">PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="61d00-336">The PutTodoItem method</span></span>

<span data-ttu-id="61d00-337">`PutTodoItem` Yöntemi inceleyin:</span><span class="sxs-lookup"><span data-stu-id="61d00-337">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="61d00-338">`PutTodoItem` benzer `PostTodoItem`, HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="61d00-338">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="61d00-339">Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="61d00-339">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="61d00-340">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca değişiklikler değil göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="61d00-340">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="61d00-341">Kısmi güncelleştirmeleri desteklemek için kullanma [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="61d00-341">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="61d00-342">Çağrılırken `PutTodoItem`bir hata alırsanız, veritabanında bir öğe `GET` olduğundan emin olmak için çağırın.</span><span class="sxs-lookup"><span data-stu-id="61d00-342">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="61d00-343">Test PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="61d00-343">Test the PutTodoItem method</span></span>

<span data-ttu-id="61d00-344">Bu örnek, uygulama her başlatıldığında yeniden başlatılması gereken bellek içi veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="61d00-344">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="61d00-345">Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="61d00-345">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="61d00-346">PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="61d00-346">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="61d00-347">ID = 1 olan Yapılacaklar öğesini güncelleştirin ve adını "Feed balık" olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="61d00-347">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="61d00-348">Aşağıdaki görüntüde, Postman güncelleştirme gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="61d00-348">The following image shows the Postman update:</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="61d00-350">DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="61d00-350">The DeleteTodoItem method</span></span>

<span data-ttu-id="61d00-351">`DeleteTodoItem` Yöntemi inceleyin:</span><span class="sxs-lookup"><span data-stu-id="61d00-351">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="61d00-352">`DeleteTodoItem` Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="61d00-352">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="61d00-353">Test DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="61d00-353">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="61d00-354">Postman bir yapılacak iş öğesini silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="61d00-354">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="61d00-355">Yöntem kümesine `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="61d00-355">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="61d00-356">Silmek, örneğin nesnenin URI ayarlayın `https://localhost:5001/api/TodoItems/1`</span><span class="sxs-lookup"><span data-stu-id="61d00-356">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="61d00-357">Seçin **Gönder**</span><span class="sxs-lookup"><span data-stu-id="61d00-357">Select **Send**</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="61d00-358">JavaScript ile Web API 'sini çağırma</span><span class="sxs-lookup"><span data-stu-id="61d00-358">Call the web API with JavaScript</span></span>

<span data-ttu-id="61d00-359">Öğreticiye bakın [: JavaScript](xref:tutorials/web-api-javascript)ile ASP.NET Core Web API 'si çağırın.</span><span class="sxs-lookup"><span data-stu-id="61d00-359">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="61d00-360">Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="61d00-360">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="61d00-361">Bir Web API projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="61d00-361">Create a web API project.</span></span>
> * <span data-ttu-id="61d00-362">Bir model sınıfı ve bir veritabanı bağlamı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="61d00-362">Add a model class and a database context.</span></span>
> * <span data-ttu-id="61d00-363">Bir denetleyici ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="61d00-363">Add a controller.</span></span>
> * <span data-ttu-id="61d00-364">CRUD yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="61d00-364">Add CRUD methods.</span></span>
> * <span data-ttu-id="61d00-365">Yönlendirmeyi Yapılandırma ve URL yolu.</span><span class="sxs-lookup"><span data-stu-id="61d00-365">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="61d00-366">Dönüş değerleri belirtin.</span><span class="sxs-lookup"><span data-stu-id="61d00-366">Specify return values.</span></span>
> * <span data-ttu-id="61d00-367">Web API'si Postman ile çağırın.</span><span class="sxs-lookup"><span data-stu-id="61d00-367">Call the web API with Postman.</span></span>
> * <span data-ttu-id="61d00-368">JavaScript ile Web API 'sini çağırın.</span><span class="sxs-lookup"><span data-stu-id="61d00-368">Call the web API with JavaScript.</span></span>

<span data-ttu-id="61d00-369">Sonunda, web API'si "Yapılacaklar" öğelerini ilişkisel bir veritabanında depolanan yönetebileceği sahip.</span><span class="sxs-lookup"><span data-stu-id="61d00-369">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="61d00-370">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="61d00-370">Overview</span></span>

<span data-ttu-id="61d00-371">Bu öğretici yandaki API oluşturur:</span><span class="sxs-lookup"><span data-stu-id="61d00-371">This tutorial creates the following API:</span></span>

|<span data-ttu-id="61d00-372">API</span><span class="sxs-lookup"><span data-stu-id="61d00-372">API</span></span> | <span data-ttu-id="61d00-373">Açıklama</span><span class="sxs-lookup"><span data-stu-id="61d00-373">Description</span></span> | <span data-ttu-id="61d00-374">İstek gövdesi</span><span class="sxs-lookup"><span data-stu-id="61d00-374">Request body</span></span> | <span data-ttu-id="61d00-375">Yanıt gövdesi</span><span class="sxs-lookup"><span data-stu-id="61d00-375">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="61d00-376">/Api/TodoItems al</span><span class="sxs-lookup"><span data-stu-id="61d00-376">GET /api/TodoItems</span></span> | <span data-ttu-id="61d00-377">Tüm yapılacak iş öğeleri al</span><span class="sxs-lookup"><span data-stu-id="61d00-377">Get all to-do items</span></span> | <span data-ttu-id="61d00-378">Yok.</span><span class="sxs-lookup"><span data-stu-id="61d00-378">None</span></span> | <span data-ttu-id="61d00-379">Yapılacaklar öğelerinin bir dizisi</span><span class="sxs-lookup"><span data-stu-id="61d00-379">Array of to-do items</span></span>|
|<span data-ttu-id="61d00-380">/Api/TodoItems/{id} al</span><span class="sxs-lookup"><span data-stu-id="61d00-380">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="61d00-381">Bir öğeyi Kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="61d00-381">Get an item by ID</span></span> | <span data-ttu-id="61d00-382">Yok.</span><span class="sxs-lookup"><span data-stu-id="61d00-382">None</span></span> | <span data-ttu-id="61d00-383">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="61d00-383">To-do item</span></span>|
|<span data-ttu-id="61d00-384">POST/api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="61d00-384">POST /api/TodoItems</span></span> | <span data-ttu-id="61d00-385">Yeni Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="61d00-385">Add a new item</span></span> | <span data-ttu-id="61d00-386">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="61d00-386">To-do item</span></span> | <span data-ttu-id="61d00-387">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="61d00-387">To-do item</span></span> |
|<span data-ttu-id="61d00-388">/Api/TodoItems/{id} koy</span><span class="sxs-lookup"><span data-stu-id="61d00-388">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="61d00-389">Mevcut öğeyi güncelleştirin &nbsp;</span><span class="sxs-lookup"><span data-stu-id="61d00-389">Update an existing item &nbsp;</span></span> | <span data-ttu-id="61d00-390">Yapılacak iş öğesi</span><span class="sxs-lookup"><span data-stu-id="61d00-390">To-do item</span></span> | <span data-ttu-id="61d00-391">Yok.</span><span class="sxs-lookup"><span data-stu-id="61d00-391">None</span></span> |
|<span data-ttu-id="61d00-392">/Api/TodoItems/{id} &nbsp; Sil&nbsp;</span><span class="sxs-lookup"><span data-stu-id="61d00-392">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="61d00-393">Öğeyi Sil &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="61d00-393">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="61d00-394">Hiçbiri</span><span class="sxs-lookup"><span data-stu-id="61d00-394">None</span></span> | <span data-ttu-id="61d00-395">Hiçbiri</span><span class="sxs-lookup"><span data-stu-id="61d00-395">None</span></span>|

<span data-ttu-id="61d00-396">Aşağıdaki diyagramda, bu uygulamanın tasarımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="61d00-396">The following diagram shows the design of the app.</span></span>

![İstemci, sol taraftaki bir kutu ile temsil edilir.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="61d00-402">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="61d00-402">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61d00-403">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-403">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="61d00-404">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="61d00-404">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="61d00-405">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-405">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="61d00-406">Bir web projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="61d00-406">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61d00-407">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-407">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="61d00-408">**Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-408">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="61d00-409">**ASP.NET Core Web uygulaması** şablonunu seçin ve **İleri**' ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61d00-409">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="61d00-410">Projeyi *TodoApi* olarak adlandırın ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61d00-410">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="61d00-411">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 2,2** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="61d00-411">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="61d00-412">**API** şablonunu seçin ve **Oluştur**' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61d00-412">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="61d00-413">**Docker desteğini etkinleştir** **' i seçmeyin** .</span><span class="sxs-lookup"><span data-stu-id="61d00-413">**Don't** select **Enable Docker Support**.</span></span>

![VS yeni proje iletişim kutusu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="61d00-415">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="61d00-415">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="61d00-416">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="61d00-416">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="61d00-417">Dizinleri (`cd`) proje klasörünü içerecek olan klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="61d00-417">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="61d00-418">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="61d00-418">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="61d00-419">Bu komutlar yeni bir Web API projesi oluşturur ve yeni proje klasöründe Visual Studio Code yeni bir örneğini açar.</span><span class="sxs-lookup"><span data-stu-id="61d00-419">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="61d00-420">Bir iletişim kutusu projeye gerekli varlıkları eklemek isteyip istemediğinizi sorduğunda **Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-420">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="61d00-421">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-421">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="61d00-422">**Dosya** > **yeni çözüm**' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-422">Select **File** > **New Solution**.</span></span>

  ![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="61d00-424">**.NET Core** > **uygulama** **API 'si** ileri ' **yi**seçin. > ></span><span class="sxs-lookup"><span data-stu-id="61d00-424">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="61d00-426">İçinde **, yeni ASP.NET Core Web API'sini yapılandırma** iletişim kutusunda varsayılan değerleri kabul **hedef Framework'ü** , \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="61d00-426">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="61d00-427">Girin *TodoApi* için **proje adı** seçip **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="61d00-427">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="61d00-429">API'yi test etme</span><span class="sxs-lookup"><span data-stu-id="61d00-429">Test the API</span></span>

<span data-ttu-id="61d00-430">Proje şablonu oluşturur bir `values` API.</span><span class="sxs-lookup"><span data-stu-id="61d00-430">The project template creates a `values` API.</span></span> <span data-ttu-id="61d00-431">Çağrı `Get` uygulamayı test etmek için bir tarayıcıdan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="61d00-431">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61d00-432">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-432">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="61d00-433">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="61d00-433">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="61d00-434">Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>/api/values`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="61d00-434">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="61d00-435">IIS Express sertifika güven varsa soran bir iletişim kutusu alırsanız seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="61d00-435">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="61d00-436">İçinde **Güvenlik Uyarısı** ardından, görüntülenen iletişim seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="61d00-436">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="61d00-437">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="61d00-437">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="61d00-438">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="61d00-438">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="61d00-439">Bir tarayıcıda aşağıdaki URL'ye gidin: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="61d00-439">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="61d00-440">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="61d00-441">Uygulamayı başlatmak için**hata ayıklamayı Başlat** ' **ı seçin.**  > </span><span class="sxs-lookup"><span data-stu-id="61d00-441">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="61d00-442">Mac için Visual Studio bir tarayıcı ile başlatarak `https://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="61d00-442">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="61d00-443">HTTP 404 (bulunamadı) hatası döndürülür.</span><span class="sxs-lookup"><span data-stu-id="61d00-443">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="61d00-444">Append `/api/values` URL'sine (URL'yi `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="61d00-444">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="61d00-445">Aşağıdaki JSON döndürülür:</span><span class="sxs-lookup"><span data-stu-id="61d00-445">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="61d00-446">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="61d00-446">Add a model class</span></span>

<span data-ttu-id="61d00-447">A *modeli* uygulamayı yöneten verilerini temsil eden sınıflar kümesidir.</span><span class="sxs-lookup"><span data-stu-id="61d00-447">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="61d00-448">Tek bir modeldir bu uygulama için `TodoItem` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="61d00-448">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61d00-449">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-449">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="61d00-450">İçinde **Çözüm Gezgini**, projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61d00-450">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="61d00-451">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="61d00-451">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="61d00-452">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="61d00-452">Name the folder *Models*.</span></span>

* <span data-ttu-id="61d00-453">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="61d00-453">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="61d00-454">Sınıf adı *Todoıtem* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="61d00-454">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="61d00-455">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="61d00-455">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="61d00-456">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="61d00-456">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="61d00-457">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="61d00-457">Add a folder named *Models*.</span></span>

* <span data-ttu-id="61d00-458">Ekleme bir `TodoItem` sınıfının *modelleri* aşağıdaki kodla klasörü:</span><span class="sxs-lookup"><span data-stu-id="61d00-458">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="61d00-459">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-459">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="61d00-460">Projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="61d00-460">Right-click the project.</span></span> <span data-ttu-id="61d00-461">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="61d00-461">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="61d00-462">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="61d00-462">Name the folder *Models*.</span></span>

  ![Yeni klasör](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="61d00-464">*Modeller* klasörüne sağ tıklayın ve **yeni dosya** > **Ekle** > **genel** > **boş sınıfı**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-464">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="61d00-465">Sınıf adı *Todoıtem*ve ardından **yeni**.</span><span class="sxs-lookup"><span data-stu-id="61d00-465">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="61d00-466">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="61d00-466">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="61d00-467">`Id` Özelliği işlevlerinin bir ilişkisel veritabanında benzersiz anahtar.</span><span class="sxs-lookup"><span data-stu-id="61d00-467">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="61d00-468">Model sınıfları herhangi bir projede gidip ancak *modelleri* klasörü, kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="61d00-468">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="61d00-469">Veritabanı bağlamı Ekle</span><span class="sxs-lookup"><span data-stu-id="61d00-469">Add a database context</span></span>

<span data-ttu-id="61d00-470">*Veritabanı bağlamı* koordine eden bir veri modeli için Entity Framework işlevsellik ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="61d00-470">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="61d00-471">Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="61d00-471">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61d00-472">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-472">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="61d00-473">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="61d00-473">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="61d00-474">Sınıf adı *TodoContext* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="61d00-474">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="61d00-475">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-475">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="61d00-476">Ekleme bir `TodoContext` sınıfının *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="61d00-476">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="61d00-477">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="61d00-477">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="61d00-478">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="61d00-478">Register the database context</span></span>

<span data-ttu-id="61d00-479">ASP.NET Core DB bağlamı gibi hizmetler ile kaydedilmelidir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="61d00-479">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="61d00-480">Kapsayıcı hizmeti denetleyicilerine sağlar.</span><span class="sxs-lookup"><span data-stu-id="61d00-480">The container provides the service to controllers.</span></span>

<span data-ttu-id="61d00-481">Güncelleştirme *Startup.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="61d00-481">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="61d00-482">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="61d00-482">The preceding code:</span></span>

* <span data-ttu-id="61d00-483">Kullanılmayan kaldırır `using` bildirimleri.</span><span class="sxs-lookup"><span data-stu-id="61d00-483">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="61d00-484">Veritabanı bağlamı DI kapsayıcıya ekler.</span><span class="sxs-lookup"><span data-stu-id="61d00-484">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="61d00-485">Veritabanı bağlamı bir bellek içi veritabanına kullanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="61d00-485">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="61d00-486">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="61d00-486">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61d00-487">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-487">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="61d00-488">Sağ *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="61d00-488">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="61d00-489">**Yeni öğe** **Ekle** > ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-489">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="61d00-490">İçinde **Yeni Öğe Ekle** iletişim kutusunda **API denetleyici sınıfı** şablonu.</span><span class="sxs-lookup"><span data-stu-id="61d00-490">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="61d00-491">Sınıf adı *TodoController*seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="61d00-491">Name the class *TodoController*, and select **Add**.</span></span>

  ![Yeni öğe iletişim denetleyicisiyle seçilen arama kutusu ve web API denetleyicisi Ekle](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="61d00-493">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-493">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="61d00-494">İçinde *denetleyicileri* klasör adında bir sınıf oluşturma `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="61d00-494">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="61d00-495">Şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="61d00-495">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="61d00-496">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="61d00-496">The preceding code:</span></span>

* <span data-ttu-id="61d00-497">Bir API denetleyicisi sınıfı yöntemleri olmadan tanımlar.</span><span class="sxs-lookup"><span data-stu-id="61d00-497">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="61d00-498">Sınıfı [[Apicontroller]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) özniteliğiyle süsler.</span><span class="sxs-lookup"><span data-stu-id="61d00-498">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="61d00-499">Bu öznitelik, denetleyicinin web API'si isteklerine yanıt verdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="61d00-499">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="61d00-500">Özniteliğin izin aldığı belirli davranışlar hakkında daha fazla bilgi için bkz <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="61d00-500">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="61d00-501">Veritabanı bağlamı eklemesine DI kullanır (`TodoContext`) içine denetleyici.</span><span class="sxs-lookup"><span data-stu-id="61d00-501">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="61d00-502">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="61d00-502">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="61d00-503">Adlı bir öğe ekler `Item1` veritabanı boşsa veritabanı.</span><span class="sxs-lookup"><span data-stu-id="61d00-503">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="61d00-504">Her çalıştığında bu kod oluşturucusunun içinde yeni bir HTTP isteği olduğundan.</span><span class="sxs-lookup"><span data-stu-id="61d00-504">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="61d00-505">Tüm öğeleri silerseniz, oluşturucu oluşturur `Item1` API yöntemi çağrıldığında tekrar başlattığınızda.</span><span class="sxs-lookup"><span data-stu-id="61d00-505">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="61d00-506">Bu nedenle, gerçekten işe yaradı silme işlemi işe yaramadı gibi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="61d00-506">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="61d00-507">Get yöntemleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="61d00-507">Add Get methods</span></span>

<span data-ttu-id="61d00-508">Yapılacak iş öğeleri alır bir API sağlamak için aşağıdaki yöntemi ekleyin. `TodoController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="61d00-508">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="61d00-509">İki GET uç noktası bu yöntemleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="61d00-509">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="61d00-510">Hala çalışıyorsa uygulamayı durdurun.</span><span class="sxs-lookup"><span data-stu-id="61d00-510">Stop the app if it's still running.</span></span> <span data-ttu-id="61d00-511">Ardından, en son değişiklikleri dahil etmek için yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="61d00-511">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="61d00-512">Bir tarayıcıdan iki uç nokta çağırarak uygulamayı test edin.</span><span class="sxs-lookup"><span data-stu-id="61d00-512">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="61d00-513">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="61d00-513">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="61d00-514">Şu HTTP yanıtı çağrısı tarafından üretilen `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="61d00-514">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="61d00-515">URL Yönlendirme ve yolları</span><span class="sxs-lookup"><span data-stu-id="61d00-515">Routing and URL paths</span></span>

<span data-ttu-id="61d00-516">[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Özniteliği bir HTTP GET isteğine yanıt vermeden bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="61d00-516">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="61d00-517">Her yöntem için URL yolu şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="61d00-517">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="61d00-518">Denetleyicinin şablonu dizesi ile başlayıp `Route` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="61d00-518">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="61d00-519">Değiştirin `[controller]` denetleyicinin adı ile kural tarafından olduğu "Controller" soneki eksi denetleyici sınıfı adı.</span><span class="sxs-lookup"><span data-stu-id="61d00-519">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="61d00-520">Bu örnek, denetleyici sınıfı adı olan **Todo**Denetleyici adı "todo" Bu nedenle denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="61d00-520">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="61d00-521">ASP.NET Core [yönlendirme](xref:mvc/controllers/routing) büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="61d00-521">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="61d00-522">Özniteliğin bir yol şablonu varsa (örneğin, `[HttpGet("products")]`), yola ekleyin. `[HttpGet]`</span><span class="sxs-lookup"><span data-stu-id="61d00-522">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="61d00-523">Bu örnek, bir şablon kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="61d00-523">This sample doesn't use a template.</span></span> <span data-ttu-id="61d00-524">Daha fazla bilgi için [özniteliği Http [eylem] özniteliği ile yönlendirme](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="61d00-524">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="61d00-525">Aşağıdaki `GetTodoItem` yöntemi `"{id}"` yapılacak iş öğesi benzersiz tanımlayıcısı için bir yer tutucu değişkendir.</span><span class="sxs-lookup"><span data-stu-id="61d00-525">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="61d00-526">Zaman `GetTodoItem` çağrılır, değerini `"{id}"` yöntemine URL'de sağlanan kendi`id` parametresi.</span><span class="sxs-lookup"><span data-stu-id="61d00-526">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="61d00-527">Döndürülen değerler</span><span class="sxs-lookup"><span data-stu-id="61d00-527">Return values</span></span>

<span data-ttu-id="61d00-528">Dönüş türünü `GetTodoItems` ve `GetTodoItem` yöntemler [actionresult öğesini\<T > türü](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="61d00-528">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="61d00-529">ASP.NET Core, nesneyi otomatik olarak serileştiren [JSON](https://www.json.org/) ve yanıt iletisinin gövdesine JSON yazar.</span><span class="sxs-lookup"><span data-stu-id="61d00-529">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="61d00-530">Yanıt kodu 200 bu dönüş türü için olduğu varsayılırsa işlenmeyen özel durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="61d00-530">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="61d00-531">İşlenmeyen özel durumları 5xx hatalarla karşılaşırsanız çevrilir.</span><span class="sxs-lookup"><span data-stu-id="61d00-531">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="61d00-532">`ActionResult` dönüş türleri, geniş HTTP durum kodları temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="61d00-532">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="61d00-533">Örneğin, `GetTodoItem` iki farklı durum değerleri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="61d00-533">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="61d00-534">Öğe istenen kimliği eşleşirse, yöntem bir 404 döndürür [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) hata kodu.</span><span class="sxs-lookup"><span data-stu-id="61d00-534">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="61d00-535">Aksi takdirde yöntem bir JSON yanıt gövdesine 200 döndürür.</span><span class="sxs-lookup"><span data-stu-id="61d00-535">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="61d00-536">Döndüren `item` sonuçları bir HTTP 200 yanıtı.</span><span class="sxs-lookup"><span data-stu-id="61d00-536">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="61d00-537">Test GetTodoItems yöntemi</span><span class="sxs-lookup"><span data-stu-id="61d00-537">Test the GetTodoItems method</span></span>

<span data-ttu-id="61d00-538">Bu öğreticide Postman web API'si test etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="61d00-538">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="61d00-539">Yükleme [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="61d00-539">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="61d00-540">Web uygulaması başlatın.</span><span class="sxs-lookup"><span data-stu-id="61d00-540">Start the web app.</span></span>
* <span data-ttu-id="61d00-541">Postman'i başlatın.</span><span class="sxs-lookup"><span data-stu-id="61d00-541">Start Postman.</span></span>
* <span data-ttu-id="61d00-542">Devre dışı **SSL sertifika doğrulama**</span><span class="sxs-lookup"><span data-stu-id="61d00-542">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61d00-543">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-543">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="61d00-544">**Dosya** ayarlarından (Genel sekmesinden) SSL sertifikası doğrulamasını devre dışı bırakın. ></span><span class="sxs-lookup"><span data-stu-id="61d00-544">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="61d00-545">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61d00-545">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="61d00-546">**Postman** > **tercihleri** ' nden (**genel** sekmesinden) **SSL sertifikası doğrulamasını**devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="61d00-546">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="61d00-547">Alternatif olarak, wranı seçin ve **Ayarlar**' ı seçip SSL sertifikası doğrulamasını devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="61d00-547">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="61d00-548">Test denetleyicisi sonra SSL sertifika doğrulamasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="61d00-548">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="61d00-549">Yeni bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="61d00-549">Create a new request.</span></span>
  * <span data-ttu-id="61d00-550">HTTP yöntemi kümesine **alma**.</span><span class="sxs-lookup"><span data-stu-id="61d00-550">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="61d00-551">İstek URL'si kümesine `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="61d00-551">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="61d00-552">Örneğin: `https://localhost:5001/api/todo`</span><span class="sxs-lookup"><span data-stu-id="61d00-552">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="61d00-553">Ayarlama **iki bölme görünümü** postman'deki.</span><span class="sxs-lookup"><span data-stu-id="61d00-553">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="61d00-554">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-554">Select **Send**.</span></span>

![Get isteğiyle postman](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="61d00-556">Create yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="61d00-556">Add a Create method</span></span>

<span data-ttu-id="61d00-557">`PostTodoItem` *Controllers/TodoController. cs*içindeki şu yöntemi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="61d00-557">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="61d00-558">Yukarıdaki kod tarafından belirtildiği gibi bir HTTP POST yöntemi olup [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="61d00-558">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="61d00-559">Yöntemi, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.</span><span class="sxs-lookup"><span data-stu-id="61d00-559">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="61d00-560">`CreatedAtAction` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="61d00-560">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="61d00-561">Başarılı olursa bir HTTP 201 durum kodu döndürür.</span><span class="sxs-lookup"><span data-stu-id="61d00-561">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="61d00-562">HTTP 201 sunucuda yeni bir kaynak oluşturan bir HTTP POST yöntemi için standart yanıttır.</span><span class="sxs-lookup"><span data-stu-id="61d00-562">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="61d00-563">Yanıta bir `Location` üst bilgi ekler.</span><span class="sxs-lookup"><span data-stu-id="61d00-563">Adds a `Location` header to the response.</span></span> <span data-ttu-id="61d00-564">`Location` Üst bilgi, yeni oluşturulan Yapılacaklar öğesinin URI 'sini belirtir.</span><span class="sxs-lookup"><span data-stu-id="61d00-564">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="61d00-565">Daha fazla bilgi için [10.2.2 201 oluşturuldu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="61d00-565">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="61d00-566">`Location` Üstbilginin URI 'sini oluşturma eylemine başvurur. `GetTodoItem`</span><span class="sxs-lookup"><span data-stu-id="61d00-566">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="61d00-567">Anahtar sözcüğü, `CreatedAtAction` çağrıda eylem adının sabit kodlanmasını önlemek için kullanılır. C# `nameof`</span><span class="sxs-lookup"><span data-stu-id="61d00-567">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="61d00-568">Test PostTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="61d00-568">Test the PostTodoItem method</span></span>

* <span data-ttu-id="61d00-569">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="61d00-569">Build the project.</span></span>
* <span data-ttu-id="61d00-570">Postman HTTP yöntemi kümesine `POST`.</span><span class="sxs-lookup"><span data-stu-id="61d00-570">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="61d00-571">Seçin **gövdesi** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="61d00-571">Select the **Body** tab.</span></span>
* <span data-ttu-id="61d00-572">Seçin **ham** radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="61d00-572">Select the **raw** radio button.</span></span>
* <span data-ttu-id="61d00-573">Tür kümesine **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="61d00-573">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="61d00-574">İstek gövdesinde bir yapılacak iş öğesi için JSON girin:</span><span class="sxs-lookup"><span data-stu-id="61d00-574">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="61d00-575">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-575">Select **Send**.</span></span>

  ![Postman ile isteği oluştur](first-web-api/_static/create.png)

  <span data-ttu-id="61d00-577">Bir 405 yöntemine izin verilmiyor hatası alırsanız, `PostTodoItem` Yöntem eklendikten sonra projeyi derlememe sonucu büyük olasılıkla oluşur.</span><span class="sxs-lookup"><span data-stu-id="61d00-577">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="61d00-578">Konum üst bilgisi URI test</span><span class="sxs-lookup"><span data-stu-id="61d00-578">Test the location header URI</span></span>

* <span data-ttu-id="61d00-579">Seçin **üstbilgileri** sekmesinde **yanıt** bölmesi.</span><span class="sxs-lookup"><span data-stu-id="61d00-579">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="61d00-580">Kopyalama **konumu** üst bilgi değeri:</span><span class="sxs-lookup"><span data-stu-id="61d00-580">Copy the **Location** header value:</span></span>

  ![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmc2.png)

* <span data-ttu-id="61d00-582">Yöntemini GET öğesine Ayarla.</span><span class="sxs-lookup"><span data-stu-id="61d00-582">Set the method to GET.</span></span>
* <span data-ttu-id="61d00-583">URİ'sini yapıştırın (örneğin, `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="61d00-583">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="61d00-584">**Gönder**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="61d00-584">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="61d00-585">PutTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="61d00-585">Add a PutTodoItem method</span></span>

<span data-ttu-id="61d00-586">Aşağıdaki `PutTodoItem` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="61d00-586">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="61d00-587">`PutTodoItem` benzer `PostTodoItem`, HTTP PUT kullanır.</span><span class="sxs-lookup"><span data-stu-id="61d00-587">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="61d00-588">Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="61d00-588">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="61d00-589">HTTP belirtimine göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca değişiklikler değil göndermek istemci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="61d00-589">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="61d00-590">Kısmi güncelleştirmeleri desteklemek için kullanma [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="61d00-590">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="61d00-591">Çağrılırken `PutTodoItem`bir hata alırsanız, veritabanında bir öğe `GET` olduğundan emin olmak için çağırın.</span><span class="sxs-lookup"><span data-stu-id="61d00-591">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="61d00-592">Test PutTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="61d00-592">Test the PutTodoItem method</span></span>

<span data-ttu-id="61d00-593">Bu örnek, uygulama her başlatıldığında yeniden başlatılması gereken bellek içi veritabanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="61d00-593">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="61d00-594">Bir PUT çağrısı yapmadan önce veritabanında bir öğe olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="61d00-594">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="61d00-595">PUT çağrısı yapmadan önce veritabanında bir öğe olduğundan emin olmak için GET çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="61d00-595">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="61d00-596">Kimliğine sahip bir yapılacak iş öğesi güncelleştirme = 1 ve "balık akış için" adını girin:</span><span class="sxs-lookup"><span data-stu-id="61d00-596">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="61d00-597">Aşağıdaki görüntüde, Postman güncelleştirme gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="61d00-597">The following image shows the Postman update:</span></span>

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="61d00-599">DeleteTodoItem yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="61d00-599">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="61d00-600">Aşağıdaki `DeleteTodoItem` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="61d00-600">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="61d00-601">`DeleteTodoItem` Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="61d00-601">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="61d00-602">Test DeleteTodoItem yöntemi</span><span class="sxs-lookup"><span data-stu-id="61d00-602">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="61d00-603">Postman bir yapılacak iş öğesini silmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="61d00-603">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="61d00-604">Yöntem kümesine `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="61d00-604">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="61d00-605">Silmek, örneğin nesnenin URI ayarlayın `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="61d00-605">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="61d00-606">Seçin **Gönder**</span><span class="sxs-lookup"><span data-stu-id="61d00-606">Select **Send**</span></span>

<span data-ttu-id="61d00-607">Örnek uygulama, tüm öğeleri silmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="61d00-607">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="61d00-608">Ancak, son öğe silindiğinde, API 'nin bir sonraki çağrılışında model sınıfı Oluşturucu tarafından yeni bir tane oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="61d00-608">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="61d00-609">JavaScript ile Web API 'sini çağırma</span><span class="sxs-lookup"><span data-stu-id="61d00-609">Call the web API with JavaScript</span></span>

<span data-ttu-id="61d00-610">Bu bölümde, Web API 'sini çağırmak için JavaScript kullanan bir HTML sayfası eklenir.</span><span class="sxs-lookup"><span data-stu-id="61d00-610">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="61d00-611">Fetch API 'SI isteği başlatır.</span><span class="sxs-lookup"><span data-stu-id="61d00-611">The Fetch API initiates the request.</span></span> <span data-ttu-id="61d00-612">JavaScript, sayfayı Web API 'sinin yanıtından alınan ayrıntılarla güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="61d00-612">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="61d00-613">Uygulamayı [statik dosyalara sunacak](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) şekilde yapılandırın ve aşağıdaki vurgulanmış kodla *Startup.cs* güncelleştirerek [varsayılan dosya eşlemesini etkinleştirin](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) :</span><span class="sxs-lookup"><span data-stu-id="61d00-613">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="61d00-614">Oluşturma bir *wwwroot* proje dizininde klasör.</span><span class="sxs-lookup"><span data-stu-id="61d00-614">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="61d00-615">Adlı bir HTML dosyası ekleyin *index.html* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="61d00-615">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="61d00-616">Dosyanın içeriğini aşağıdaki biçimlendirme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="61d00-616">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="61d00-617">Adlı bir JavaScript dosyası ekleyin *site.js* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="61d00-617">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="61d00-618">Dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="61d00-618">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="61d00-619">ASP.NET Core proje başlatma ayarlarında bir değişiklik HTML sayfasını yerel olarak test etmek için gerekli:</span><span class="sxs-lookup"><span data-stu-id="61d00-619">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="61d00-620">Açık *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="61d00-620">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="61d00-621">Kaldırma `launchUrl` , açmak için uygulamayı zorlamak için özellik *index.html*&mdash;projenin varsayılan dosya.</span><span class="sxs-lookup"><span data-stu-id="61d00-621">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="61d00-622">Bu örnek, Web API 'sinin tüm CRUD yöntemlerini çağırır.</span><span class="sxs-lookup"><span data-stu-id="61d00-622">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="61d00-623">API çağrıları açıklamaları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="61d00-623">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="61d00-624">Yapılacaklar öğelerinin bir listesini alın</span><span class="sxs-lookup"><span data-stu-id="61d00-624">Get a list of to-do items</span></span>

<span data-ttu-id="61d00-625">Fetch, Web API 'sine bir HTTP GET isteği gönderir ve bu, Yapılacaklar öğeleri dizisini temsil eden JSON döndürür.</span><span class="sxs-lookup"><span data-stu-id="61d00-625">Fetch sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="61d00-626">`success` İstek başarılı olursa geri çağırma işlevi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="61d00-626">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="61d00-627">Geri çağırma içinde DOM Yapılacaklar bilgilerle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="61d00-627">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="61d00-628">Yapılacak İş Öğesi Ekle</span><span class="sxs-lookup"><span data-stu-id="61d00-628">Add a to-do item</span></span>

<span data-ttu-id="61d00-629">Fetch, istek gövdesinde Yapılacaklar öğesiyle bir HTTP POST isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="61d00-629">Fetch sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="61d00-630">`accepts` Ve `contentType` seçeneklerini ayarlamak `application/json` gönderilen ve alınan medya türü belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="61d00-630">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="61d00-631">Yapılacak iş öğesi kullanarak JSON'a dönüştürülür [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="61d00-631">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="61d00-632">API'yi bir başarılı durum kodu döndürdüğünde `getData` işlevi HTML tablosu güncelleştirmek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="61d00-632">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="61d00-633">Yapılacak iş öğesini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="61d00-633">Update a to-do item</span></span>

<span data-ttu-id="61d00-634">Yapılacak iş öğesi güncelleştirilirken bir eklemeye benzerdir.</span><span class="sxs-lookup"><span data-stu-id="61d00-634">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="61d00-635">`url` Öğenin benzersiz tanıtıcısı eklemek için değişiklikleri ve `type` olduğu `PUT`.</span><span class="sxs-lookup"><span data-stu-id="61d00-635">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="61d00-636">Yapılacak iş öğesi silme</span><span class="sxs-lookup"><span data-stu-id="61d00-636">Delete a to-do item</span></span>

<span data-ttu-id="61d00-637">Yapılacak iş öğesi silme gerçekleştirilir ayarlayarak `type` AJAX çağrısı hedefi üzerinde `DELETE` ve öğenin benzersiz tanıtıcısı URL'yi belirterek.</span><span class="sxs-lookup"><span data-stu-id="61d00-637">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="61d00-638">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="61d00-638">Additional resources</span></span>

<span data-ttu-id="61d00-639">[Görüntülemek veya Bu öğretici için örnek kodu indirdikten](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="61d00-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="61d00-640">Bkz: [nasıl indirileceğini](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="61d00-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="61d00-641">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="61d00-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="61d00-642">Bu öğreticinin YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="61d00-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
