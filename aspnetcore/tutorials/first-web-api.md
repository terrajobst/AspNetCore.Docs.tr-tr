---
title: ASP.NET Core ve Visual Studio ile Web API'si oluşturma
author: rick-anderson
description: ASP.NET Core MVC ve Windows üzerinde Visual Studio ile bir web API'si oluşturma
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: eb23d5376742e04712f714263815582fddc5d18e
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450703"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a><span data-ttu-id="b16a9-103">ASP.NET Core ve Visual Studio ile Web API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="b16a9-103">Create a Web API with ASP.NET Core and Visual Studio</span></span>

<span data-ttu-id="b16a9-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="b16a9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="b16a9-105">Bu öğreticide web API'si "Yapılacaklar" öğelerinin listesini yönetmek için oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b16a9-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="b16a9-106">Bir kullanıcı arabirimi (UI) oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="b16a9-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="b16a9-107">Bu öğretici üç sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="b16a9-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="b16a9-108">Windows: Web API Windows üzerinde Visual Studio (Bu öğretici, bkz: [videosu](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span><span class="sxs-lookup"><span data-stu-id="b16a9-108">Windows: Web API with Visual Studio on Windows (This tutorial, see the [video version](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span></span>
* <span data-ttu-id="b16a9-109">macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="b16a9-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="b16a9-110">macOS, Linux, Windows: [Visual Studio Code ile Web API](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="b16a9-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

> [!NOTE]
> <span data-ttu-id="b16a9-111">ASP.NET Core içindekiler tablosuna yönelik önerilmiş olan yeni bir yapının kullanılabilirliğini test ediyoruz.</span><span class="sxs-lookup"><span data-stu-id="b16a9-111">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="b16a9-112">Geçerli veya önerilen içindekiler tablosunda 7 farklı konuyu bulmaya ilişkin alıştırmayı denemek için vaktiniz varsa lütfen [çalışmaya katılmak için buraya tıklayın](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="b16a9-112">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="b16a9-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b16a9-113">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="b16a9-114">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="b16a9-114">Create the project</span></span>

<span data-ttu-id="b16a9-115">Visual Studio aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="b16a9-115">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="b16a9-116">Gelen **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="b16a9-116">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b16a9-117">Seçin **ASP.NET Core Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="b16a9-117">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="b16a9-118">Projeyi adlandırın *TodoApi* tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="b16a9-118">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="b16a9-119">İçinde **yeni ASP.NET Core Web uygulaması - TodoApi** iletişim kutusunda, ASP.NET Core sürümünü seçin.</span><span class="sxs-lookup"><span data-stu-id="b16a9-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="b16a9-120">Seçin **API** şablonu ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="b16a9-120">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="b16a9-121">Yapmak **değil** seçin **Docker desteğini etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="b16a9-121">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="b16a9-122">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="b16a9-122">Launch the app</span></span>

<span data-ttu-id="b16a9-123">Visual Studio'da uygulamayı başlatmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="b16a9-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="b16a9-124">Visual Studio bir tarayıcı ile başlatarak `http://localhost:<port>/api/values`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="b16a9-124">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="b16a9-125">Chrome, Microsoft Edge ve Firefox aşağıdaki çıkışı görüntüler:</span><span class="sxs-lookup"><span data-stu-id="b16a9-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="b16a9-126">Internet Explorer kullanıyorsanız, kaydetmek için istenir bir *values.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="b16a9-126">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="b16a9-127">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="b16a9-127">Add a model class</span></span>

<span data-ttu-id="b16a9-128">Bir uygulamadaki verileri temsil eden bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="b16a9-128">A model is an object representing the data in the app.</span></span> <span data-ttu-id="b16a9-129">Bu durumda, bir yapılacak iş öğesi tek model budur.</span><span class="sxs-lookup"><span data-stu-id="b16a9-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="b16a9-130">Çözüm Gezgini'nde projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b16a9-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="b16a9-131">Seçin **ekleme** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="b16a9-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="b16a9-132">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="b16a9-132">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="b16a9-133">Model sınıfları herhangi bir projede gidebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b16a9-133">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="b16a9-134">*Modelleri* klasör kural tarafından model sınıfları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b16a9-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="b16a9-135">Çözüm Gezgini'nde sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="b16a9-135">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="b16a9-136">Sınıf adı *Todoıtem* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="b16a9-136">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="b16a9-137">Güncelleştirme `TodoItem` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="b16a9-137">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="b16a9-138">Veritabanı oluşturur `Id` olduğunda bir `TodoItem` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b16a9-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="b16a9-139">Veritabanı bağlamı oluşturur</span><span class="sxs-lookup"><span data-stu-id="b16a9-139">Create the database context</span></span>

<span data-ttu-id="b16a9-140">*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="b16a9-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="b16a9-141">Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b16a9-141">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="b16a9-142">Çözüm Gezgini'nde sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="b16a9-142">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="b16a9-143">Sınıf adı *TodoContext* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="b16a9-143">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="b16a9-144">Sınıfı, aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="b16a9-144">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="b16a9-145">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="b16a9-145">Add a controller</span></span>

<span data-ttu-id="b16a9-146">Çözüm Gezgini'nde sağ *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="b16a9-146">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="b16a9-147">Seçin **ekleme** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="b16a9-147">Select **Add** > **New Item**.</span></span> <span data-ttu-id="b16a9-148">İçinde **Yeni Öğe Ekle** iletişim kutusunda **API denetleyici sınıfı** şablonu.</span><span class="sxs-lookup"><span data-stu-id="b16a9-148">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="b16a9-149">Sınıf adı *TodoController*, tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="b16a9-149">Name the class *TodoController*, and click **Add**.</span></span>

![Yeni öğe iletişim denetleyicisiyle seçilen arama kutusu ve web API denetleyicisi Ekle](first-web-api/_static/new_controller.png)

<span data-ttu-id="b16a9-151">Sınıfı, aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="b16a9-151">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="b16a9-152">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="b16a9-152">Launch the app</span></span>

<span data-ttu-id="b16a9-153">Visual Studio'da uygulamayı başlatmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="b16a9-153">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="b16a9-154">Visual Studio bir tarayıcı ile başlatarak `http://localhost:<port>/api/values`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="b16a9-154">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="b16a9-155">Gidin `Todo` denetleyicisinde `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="b16a9-155">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
