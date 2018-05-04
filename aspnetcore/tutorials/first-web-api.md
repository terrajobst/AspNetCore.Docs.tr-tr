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
ms.openlocfilehash: 962c24a7e654328df7e8893e589e45b19e87b931
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="29535-103">Windows için ASP.NET Core ve Visual Studio ile Web API oluşturma</span><span class="sxs-lookup"><span data-stu-id="29535-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="29535-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="29535-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

::: moniker range="= aspnetcore-2.1"
[!INCLUDE[](~/includes/2.1.md)]
::: moniker-end

<span data-ttu-id="29535-106">Bu öğretici, "Yapılacaklar" öğeleri listesini yönetmek için bir web API oluşturur.</span><span class="sxs-lookup"><span data-stu-id="29535-106">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="29535-107">Bir kullanıcı arabirimi (UI) oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="29535-107">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="29535-108">Bu öğretici için üç sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="29535-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="29535-109">Windows: Web API ile Windows için Visual Studio (Bu öğretici)</span><span class="sxs-lookup"><span data-stu-id="29535-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="29535-110">macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="29535-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="29535-111">macOS, Linux, Windows: [Visual Studio Code ile Web API](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="29535-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="29535-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="29535-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="29535-113">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="29535-113">Create the project</span></span>

<span data-ttu-id="29535-114">Visual Studio'da aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="29535-114">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="29535-115">Gelen **dosya** menüsünde, select **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="29535-115">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="29535-116">Seçin **ASP.NET çekirdek Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="29535-116">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="29535-117">Proje adı *TodoApi* tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="29535-117">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="29535-118">İçinde **yeni ASP.NET çekirdek Web uygulaması - TodoApi** iletişim kutusunda, ASP.NET Core sürümünü seçin.</span><span class="sxs-lookup"><span data-stu-id="29535-118">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="29535-119">Seçin **API** şablonu ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="29535-119">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="29535-120">Yapmak **değil** seçin **Docker desteğini etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="29535-120">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="29535-121">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="29535-121">Launch the app</span></span>

<span data-ttu-id="29535-122">Visual Studio'da uygulamayı başlatmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="29535-122">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="29535-123">Visual Studio bir tarayıcı ile başlatarak `http://localhost:<port>/api/values`, burada `<port>` rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="29535-123">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="29535-124">Chrome, Microsoft Edge ve Firefox aşağıdaki çıkış görüntüler:</span><span class="sxs-lookup"><span data-stu-id="29535-124">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="29535-125">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="29535-125">Add a model class</span></span>

<span data-ttu-id="29535-126">Uygulama verileri temsil eden bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="29535-126">A model is an object representing the data in the app.</span></span> <span data-ttu-id="29535-127">Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.</span><span class="sxs-lookup"><span data-stu-id="29535-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="29535-128">Çözüm Gezgini'nde projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="29535-128">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="29535-129">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="29535-129">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="29535-130">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="29535-130">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="29535-131">Model sınıfları herhangi bir yere projede gidebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="29535-131">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="29535-132">*Modelleri* klasörü kural tarafından model sınıfları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="29535-132">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="29535-133">Çözüm Gezgini'nde sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="29535-133">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="29535-134">Sınıf adını *Todoıtem* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="29535-134">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="29535-135">Güncelleştirme `TodoItem` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="29535-135">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="29535-136">Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="29535-136">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="29535-137">Veritabanı bağlamı oluşturma</span><span class="sxs-lookup"><span data-stu-id="29535-137">Create the database context</span></span>

<span data-ttu-id="29535-138">*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="29535-138">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="29535-139">Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="29535-139">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="29535-140">Çözüm Gezgini'nde sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="29535-140">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="29535-141">Sınıf adını *TodoContext* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="29535-141">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="29535-142">Sınıfı aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="29535-142">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="29535-143">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="29535-143">Add a controller</span></span>

<span data-ttu-id="29535-144">Çözüm Gezgini'nde sağ *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="29535-144">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="29535-145">Seçin **ekleme** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="29535-145">Select **Add** > **New Item**.</span></span> <span data-ttu-id="29535-146">İçinde **Yeni Öğe Ekle** iletişim kutusunda **API denetleyicisi sınıfı** şablonu.</span><span class="sxs-lookup"><span data-stu-id="29535-146">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="29535-147">Sınıf adını *TodoController*, tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="29535-147">Name the class *TodoController*, and click **Add**.</span></span>

![Yeni öğe iletişim denetleyicisiyle seçili arama kutusu ve web API denetleyicisi ekleyin](first-web-api/_static/new_controller.png)

<span data-ttu-id="29535-149">Sınıfı aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="29535-149">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="29535-150">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="29535-150">Launch the app</span></span>

<span data-ttu-id="29535-151">Visual Studio'da uygulamayı başlatmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="29535-151">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="29535-152">Visual Studio bir tarayıcı ile başlatarak `http://localhost:<port>/api/values`, burada `<port>` rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="29535-152">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="29535-153">Gidin `Todo` denetleyicisinde `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="29535-153">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
