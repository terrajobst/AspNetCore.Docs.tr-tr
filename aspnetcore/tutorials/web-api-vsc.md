---
title: ASP.NET Çekirdeği ve Visual Studio Code ile Web API oluşturma
author: rick-anderson
description: MacOS, Linux veya Windows ASP.NET Core MVC ve Visual Studio Code ile web API'si oluşturma
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 705524a62018b9f0fbd8c40fa1b70d4c62ee236e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="96c54-103">ASP.NET Çekirdeği ve Visual Studio Code ile Web API oluşturma</span><span class="sxs-lookup"><span data-stu-id="96c54-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="96c54-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="96c54-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="96c54-105">Bu öğreticide, "Yapılacaklar" öğeleri listesini yönetmek için bir web API'si oluşturma.</span><span class="sxs-lookup"><span data-stu-id="96c54-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="96c54-106">Bir kullanıcı Arabirimi oluşturulan değil.</span><span class="sxs-lookup"><span data-stu-id="96c54-106">A UI isn't constructed.</span></span>

<span data-ttu-id="96c54-107">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="96c54-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="96c54-108">macOS, Linux, Windows: Visual Studio Code (Bu öğretici) ile Web API</span><span class="sxs-lookup"><span data-stu-id="96c54-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="96c54-109">macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="96c54-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="96c54-110">Windows: [Web API'si Visual Studio ile Windows için](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="96c54-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE [template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="96c54-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="96c54-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="96c54-112">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="96c54-112">Create the project</span></span>

<span data-ttu-id="96c54-113">Bir konsoldan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="96c54-113">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="96c54-114">Açık *TodoApi* Visual Studio Code (VS Code) ve select klasöründe *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="96c54-114">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="96c54-115">Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'TodoApi' eksik. ileti</span><span class="sxs-lookup"><span data-stu-id="96c54-115">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="96c54-116">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="96c54-116">Add them?"</span></span>
- <span data-ttu-id="96c54-117">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.</span><span class="sxs-lookup"><span data-stu-id="96c54-117">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Derleme ve hata ayıklama VS koduyla gerekli uyar varlıklar 'TodoApi' eksik.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="96c54-121">Tuşuna **hata ayıklama** derlemek ve programı çalıştırmak için (F5).</span><span class="sxs-lookup"><span data-stu-id="96c54-121">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="96c54-122">Bir tarayıcıda gidin http://localhost:5000/api/values .</span><span class="sxs-lookup"><span data-stu-id="96c54-122">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="96c54-123">Aşağıdaki görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="96c54-123">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="96c54-124">Bkz: [Visual Studio Code Yardım](#visual-studio-code-help) VS Code kullanma hakkında ipuçları için.</span><span class="sxs-lookup"><span data-stu-id="96c54-124">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="96c54-125">Entity Framework Çekirdek desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="96c54-125">Add support for Entity Framework Core</span></span>

<span data-ttu-id="96c54-126">.NET Core 2. 0 ' yeni bir proje oluşturma ekler 'Microsoft.AspNetCore.All' Sağlayıcısında *TodoApi.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="96c54-126">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="96c54-127">Yüklemek için gerek yoktur [Entity Framework Çekirdek Inmemory](https://docs.microsoft.com/ef/core/providers/in-memory/) sağlayıcı ayrı olarak veritabanı.</span><span class="sxs-lookup"><span data-stu-id="96c54-127">There's no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="96c54-128">Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="96c54-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="96c54-129">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="96c54-129">Add a model class</span></span>

<span data-ttu-id="96c54-130">Uygulamanızdaki verileri temsil eden bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="96c54-130">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="96c54-131">Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.</span><span class="sxs-lookup"><span data-stu-id="96c54-131">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="96c54-132">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="96c54-132">Add a folder named *Models*.</span></span> <span data-ttu-id="96c54-133">Model sınıfları projenizde, herhangi bir yere koyabilirsiniz ancak *modelleri* klasörü, kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="96c54-133">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="96c54-134">Ekleme bir `TodoItem` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="96c54-134">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="96c54-135">Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="96c54-135">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="96c54-136">Veritabanı bağlamı oluşturma</span><span class="sxs-lookup"><span data-stu-id="96c54-136">Create the database context</span></span>

<span data-ttu-id="96c54-137">*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="96c54-137">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="96c54-138">Bu sınıf türetme tarafından oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="96c54-138">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="96c54-139">Ekleme bir `TodoContext` sınıfını *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="96c54-139">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="96c54-140">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="96c54-140">Add a controller</span></span>

<span data-ttu-id="96c54-141">İçinde *denetleyicileri* klasörünü adlı bir sınıf oluşturun `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="96c54-141">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="96c54-142">Aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="96c54-142">Add the following code:</span></span>

[!INCLUDE [code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="96c54-143">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="96c54-143">Launch the app</span></span>

<span data-ttu-id="96c54-144">VS Code'da uygulamayı başlatmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="96c54-144">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="96c54-145">Gidin http://localhost:5000/api/todo ( `Todo` yeni oluşturduğumuz denetleyicisi).</span><span class="sxs-lookup"><span data-stu-id="96c54-145">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE [last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="96c54-146">Visual Studio Code help</span><span class="sxs-lookup"><span data-stu-id="96c54-146">Visual Studio Code help</span></span>

- [<span data-ttu-id="96c54-147">Başlarken</span><span class="sxs-lookup"><span data-stu-id="96c54-147">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="96c54-148">Hata Ayıklama</span><span class="sxs-lookup"><span data-stu-id="96c54-148">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="96c54-149">Tümleşik terminal</span><span class="sxs-lookup"><span data-stu-id="96c54-149">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="96c54-150">Klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="96c54-150">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="96c54-151">macOS klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="96c54-151">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="96c54-152">Linux klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="96c54-152">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="96c54-153">Windows klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="96c54-153">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE [next steps](../includes/webApi/next.md)]

