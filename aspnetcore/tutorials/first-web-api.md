---
title: Windows için ASP.NET Core ve Visual Studio ile Web API oluşturma
author: rick-anderson
description: ASP.NET Core MVC ve Windows için Visual Studio ile web API'si oluşturma
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 71f1e10dee9fbae8cf76733d33533be832fb5769
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="6f43f-103">Windows için ASP.NET Core ve Visual Studio ile Web API oluşturma</span><span class="sxs-lookup"><span data-stu-id="6f43f-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="6f43f-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="6f43f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="6f43f-105">Bu öğretici, "Yapılacaklar" öğeleri listesini yönetmek için bir web API oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6f43f-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="6f43f-106">Bir kullanıcı arabirimi (UI) oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="6f43f-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="6f43f-107">Bu öğretici için üç sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="6f43f-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="6f43f-108">Windows: Web API ile Windows için Visual Studio (Bu öğretici)</span><span class="sxs-lookup"><span data-stu-id="6f43f-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="6f43f-109">macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="6f43f-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="6f43f-110">macOS, Linux, Windows: [Visual Studio Code ile Web API](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="6f43f-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="6f43f-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="6f43f-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="6f43f-112">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="6f43f-112">Create the project</span></span>

<span data-ttu-id="6f43f-113">Visual Studio'dan seçin **dosya** menüsünde > **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="6f43f-113">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="6f43f-114">Seçin **.NET Core** > **ASP.NET çekirdek Web uygulaması** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="6f43f-114">Select **.NET Core** > **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="6f43f-115">Proje adı *TodoApi* seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="6f43f-115">Name the project *TodoApi* and select **OK**.</span></span>

![Yeni Proje iletişim kutusu](first-web-api/_static/new-project.png)

<span data-ttu-id="6f43f-117">İçinde **yeni ASP.NET çekirdek Web uygulaması - TodoApi** iletişim kutusunda, ASP.NET Core sürümünü seçin.</span><span class="sxs-lookup"><span data-stu-id="6f43f-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="6f43f-118">Seçin **API** şablonu.</span><span class="sxs-lookup"><span data-stu-id="6f43f-118">Select the **API** template.</span></span> <span data-ttu-id="6f43f-119">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="6f43f-119">Select **OK**.</span></span> <span data-ttu-id="6f43f-120">Yapmak **değil** seçin **Docker desteğini etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="6f43f-120">Do **not** select **Enable Docker Support**.</span></span>

![ASP.NET Core şablonlardan seçili Web API projesi şablonuyla yeni ASP.NET Web uygulaması iletişim kutusu](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="6f43f-122">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="6f43f-122">Launch the app</span></span>

<span data-ttu-id="6f43f-123">Visual Studio'da uygulamayı başlatmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="6f43f-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="6f43f-124">Visual Studio bir tarayıcı ile başlatarak `http://localhost:<port>/api/values`, burada `<port>` rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="6f43f-124">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="6f43f-125">Chrome, Microsoft Edge ve Firefox aşağıdaki çıkış görüntüler:</span><span class="sxs-lookup"><span data-stu-id="6f43f-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="6f43f-126">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="6f43f-126">Add a model class</span></span>

<span data-ttu-id="6f43f-127">Uygulama verileri temsil eden bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="6f43f-127">A model is an object representing the data in the app.</span></span> <span data-ttu-id="6f43f-128">Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.</span><span class="sxs-lookup"><span data-stu-id="6f43f-128">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="6f43f-129">Çözüm Gezgini'nde projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6f43f-129">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="6f43f-130">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="6f43f-130">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="6f43f-131">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="6f43f-131">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="6f43f-132">Model sınıfları herhangi bir yere projede gidebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6f43f-132">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="6f43f-133">*Modelleri* klasörü kural tarafından model sınıfları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6f43f-133">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="6f43f-134">Çözüm Gezgini'nde sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="6f43f-134">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="6f43f-135">Sınıf adını *Todoıtem* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="6f43f-135">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="6f43f-136">Güncelleştirme `TodoItem` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="6f43f-136">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="6f43f-137">Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6f43f-137">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="6f43f-138">Veritabanı bağlamı oluşturma</span><span class="sxs-lookup"><span data-stu-id="6f43f-138">Create the database context</span></span>

<span data-ttu-id="6f43f-139">*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="6f43f-139">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="6f43f-140">Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6f43f-140">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="6f43f-141">Çözüm Gezgini'nde sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="6f43f-141">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="6f43f-142">Sınıf adını *TodoContext* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="6f43f-142">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="6f43f-143">Sınıfı aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="6f43f-143">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="6f43f-144">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="6f43f-144">Add a controller</span></span>

<span data-ttu-id="6f43f-145">Çözüm Gezgini'nde sağ *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="6f43f-145">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="6f43f-146">Seçin **ekleme** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="6f43f-146">Select **Add** > **New Item**.</span></span> <span data-ttu-id="6f43f-147">İçinde **Yeni Öğe Ekle** iletişim kutusunda **API denetleyicisi sınıfı** şablonu.</span><span class="sxs-lookup"><span data-stu-id="6f43f-147">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="6f43f-148">Sınıf adını *TodoController*, tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="6f43f-148">Name the class *TodoController*, and click **Add**.</span></span>

![Yeni öğe iletişim denetleyicisiyle seçili arama kutusu ve web API denetleyicisi ekleyin](first-web-api/_static/new_controller.png)

<span data-ttu-id="6f43f-150">Sınıfı aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="6f43f-150">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="6f43f-151">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="6f43f-151">Launch the app</span></span>

<span data-ttu-id="6f43f-152">Visual Studio'da uygulamayı başlatmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="6f43f-152">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="6f43f-153">Visual Studio bir tarayıcı ile başlatarak `http://localhost:<port>/api/values`, burada `<port>` rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="6f43f-153">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="6f43f-154">Gidin `Todo` denetleyicisinde `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="6f43f-154">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
