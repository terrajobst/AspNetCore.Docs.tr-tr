---
title: Windows için ASP.NET Core ve Visual Studio ile Web API oluşturma
author: rick-anderson
description: ASP.NET Core MVC ve Windows için Visual Studio ile web API'si oluşturma
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: c264ae6a04e46d029f8c710af9cbce4f8437ba7c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="43661-103">Windows için ASP.NET Core ve Visual Studio ile Web API oluşturma</span><span class="sxs-lookup"><span data-stu-id="43661-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="43661-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="43661-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="43661-105">Bu öğretici, "Yapılacaklar" öğeleri listesini yönetmek için bir web API oluşturur.</span><span class="sxs-lookup"><span data-stu-id="43661-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="43661-106">Bir kullanıcı arabirimi (UI) oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="43661-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="43661-107">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="43661-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="43661-108">Windows: Web API ile Windows için Visual Studio (Bu öğretici)</span><span class="sxs-lookup"><span data-stu-id="43661-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="43661-109">macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="43661-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="43661-110">macOS, Linux, Windows: [Visual Studio Code ile Web API](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="43661-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE [intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="43661-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="43661-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="43661-112">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="43661-112">Create the project</span></span>

<span data-ttu-id="43661-113">Visual Studio'dan seçin **dosya** menüsünde > **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="43661-113">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="43661-114">Seçin **.NET Core** > **ASP.NET çekirdek Web uygulaması** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="43661-114">Select **.NET Core** > **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="43661-115">Proje adı `TodoApi` seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="43661-115">Name the project `TodoApi` and select **OK**.</span></span>

![Yeni Proje iletişim kutusu](first-web-api/_static/new-project.png)

<span data-ttu-id="43661-117">İçinde **yeni ASP.NET çekirdek Web uygulaması - TodoApi** iletişim kutusunda **API** şablonu.</span><span class="sxs-lookup"><span data-stu-id="43661-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **API** template.</span></span> <span data-ttu-id="43661-118">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="43661-118">Select **OK**.</span></span> <span data-ttu-id="43661-119">Yapmak **değil** seçin **Docker desteğini etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="43661-119">Do **not** select **Enable Docker Support**.</span></span>

![ASP.NET Core şablonlardan seçili Web API projesi şablonuyla yeni ASP.NET Web uygulaması iletişim kutusu](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="43661-121">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="43661-121">Launch the app</span></span>

<span data-ttu-id="43661-122">Visual Studio'da uygulamayı başlatmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="43661-122">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="43661-123">Visual Studio bir tarayıcı ile başlatarak `http://localhost:port/api/values`, burada *bağlantı noktası* rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="43661-123">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="43661-124">Chrome, Microsoft Edge ve Firefox aşağıdaki çıkış görüntüler:</span><span class="sxs-lookup"><span data-stu-id="43661-124">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="43661-125">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="43661-125">Add a model class</span></span>

<span data-ttu-id="43661-126">Uygulama verileri temsil eden bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="43661-126">A model is an object that represents the data in the app.</span></span> <span data-ttu-id="43661-127">Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.</span><span class="sxs-lookup"><span data-stu-id="43661-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="43661-128">"Modelleri" adlı bir klasör ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43661-128">Add a folder named "Models".</span></span> <span data-ttu-id="43661-129">Çözüm Gezgini'nde projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="43661-129">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="43661-130">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="43661-130">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="43661-131">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="43661-131">Name the folder *Models*.</span></span>

<span data-ttu-id="43661-132">Not: Modeli sınıfları projede herhangi bir yere gidin.</span><span class="sxs-lookup"><span data-stu-id="43661-132">Note: The model classes go anywhere in the project.</span></span> <span data-ttu-id="43661-133">*Modelleri* klasörü kural tarafından model sınıfları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="43661-133">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="43661-134">Ekleme bir `TodoItem` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="43661-134">Add a `TodoItem` class.</span></span> <span data-ttu-id="43661-135">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="43661-135">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="43661-136">Sınıf adını `TodoItem` seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="43661-136">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="43661-137">Güncelleştirme `TodoItem` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="43661-137">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="43661-138">Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="43661-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="43661-139">Veritabanı bağlamı oluşturma</span><span class="sxs-lookup"><span data-stu-id="43661-139">Create the database context</span></span>

<span data-ttu-id="43661-140">*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="43661-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="43661-141">Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="43661-141">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="43661-142">Ekleme bir `TodoContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="43661-142">Add a `TodoContext` class.</span></span> <span data-ttu-id="43661-143">Sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="43661-143">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="43661-144">Sınıf adını `TodoContext` seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="43661-144">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="43661-145">Sınıfı aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="43661-145">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="43661-146">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="43661-146">Add a controller</span></span>

<span data-ttu-id="43661-147">Çözüm Gezgini'nde sağ *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="43661-147">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="43661-148">Seçin **ekleme** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="43661-148">Select **Add** > **New Item**.</span></span> <span data-ttu-id="43661-149">İçinde **Yeni Öğe Ekle** iletişim kutusunda **Web API denetleyicisi sınıfı** şablonu.</span><span class="sxs-lookup"><span data-stu-id="43661-149">In the **Add New Item** dialog, select the **Web API Controller Class** template.</span></span> <span data-ttu-id="43661-150">Sınıf adını `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="43661-150">Name the class `TodoController`.</span></span>

![Yeni öğe iletişim denetleyicisiyle seçili arama kutusu ve web API denetleyicisi ekleyin](first-web-api/_static/new_controller.png)

<span data-ttu-id="43661-152">Sınıfı aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="43661-152">Replace the class with the following code:</span></span>

[!INCLUDE [code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="43661-153">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="43661-153">Launch the app</span></span>

<span data-ttu-id="43661-154">Visual Studio'da uygulamayı başlatmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="43661-154">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="43661-155">Visual Studio bir tarayıcı ile başlatarak `http://localhost:port/api/values`, burada *bağlantı noktası* rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="43661-155">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="43661-156">Gidin `Todo` denetleyicisinde `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="43661-156">Navigate to the `Todo` controller at `http://localhost:port/api/todo`.</span></span>

[!INCLUDE [last part of web API](../includes/webApi/end.md)]

[!INCLUDE [next steps](../includes/webApi/next.md)]

