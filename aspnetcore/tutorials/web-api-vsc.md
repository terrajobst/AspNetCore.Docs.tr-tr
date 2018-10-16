---
title: ASP.NET Core ve Visual Studio Code ile Web API'si oluşturma
author: rick-anderson
description: MacOS, Linux veya Windows ASP.NET Core MVC ve Visual Studio Code ile bir web API'si oluşturma
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: b8e5c8b7d3dc04513997997d903295853dd1ff46
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348435"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="ce619-103">ASP.NET Core ve Visual Studio Code ile Web API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="ce619-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="ce619-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="ce619-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="ce619-105">Bu öğreticide, bir "Yapılacaklar" öğelerinin listesi yönetmek için bir web API'si oluşturma.</span><span class="sxs-lookup"><span data-stu-id="ce619-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="ce619-106">Bir kullanıcı Arabirimi oluşturulur değil.</span><span class="sxs-lookup"><span data-stu-id="ce619-106">A UI isn't constructed.</span></span>

<span data-ttu-id="ce619-107">Bu öğretici üç sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="ce619-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="ce619-108">macOS, Linux, Windows: Visual Studio Code (Bu öğreticide) ile Web API</span><span class="sxs-lookup"><span data-stu-id="ce619-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="ce619-109">macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="ce619-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="ce619-110">Windows: [Windows için Visual Studio ile API Web](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="ce619-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="ce619-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="ce619-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

<span data-ttu-id="ce619-112">Bkz: [Visual Studio Code Yardım](#visual-studio-code-help) için VS Code kullanma hakkında ipuçları.</span><span class="sxs-lookup"><span data-stu-id="ce619-112">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="ce619-113">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="ce619-113">Create the project</span></span>

<span data-ttu-id="ce619-114">Bir konsoldan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ce619-114">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="ce619-115">*TodoApi* Visual Studio Code'da (VS Code) klasörünü açar.</span><span class="sxs-lookup"><span data-stu-id="ce619-115">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="ce619-116">Seçin *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="ce619-116">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="ce619-117">Seçin **Evet** için **uyar** ileti "derleme ve hata ayıklamak için gerekli varlıkları 'TodoApi' eksik.</span><span class="sxs-lookup"><span data-stu-id="ce619-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="ce619-118">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="ce619-118">Add them?"</span></span>
* <span data-ttu-id="ce619-119">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıklar vardır" iletisi.</span><span class="sxs-lookup"><span data-stu-id="ce619-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Derleme ve hata ayıklamak için gerekli uyar varlıklar ile VS Code 'TodoApi' eksik.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="ce619-123">Tuşuna **hata ayıklama** oluşturup programı çalıştırın (F5).</span><span class="sxs-lookup"><span data-stu-id="ce619-123">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="ce619-124">Bir tarayıcıda gidin http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="ce619-124">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="ce619-125">Aşağıdaki çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="ce619-125">The following output is displayed:</span></span>

```json
["value1","value2"]
```



## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="ce619-126">Entity Framework Core desteği eklendi</span><span class="sxs-lookup"><span data-stu-id="ce619-126">Add support for Entity Framework Core</span></span>

:::moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ce619-127">ASP.NET Core 2.1 veya daha sonra yeni proje oluşturma ekler [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) paketini başvuru *TodoApi.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ce619-127">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file:</span></span>

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

<span data-ttu-id="ce619-128">ASP.NET Core 2.0 sürümünde yeni proje oluşturma ekler [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) paketini başvuru *TodoApi.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ce619-128">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

<span data-ttu-id="ce619-129">Yüklemek için gerek yoktur [Entity Framework Core Inmemory](/ef/core/providers/in-memory/) sağlayıcısı ayrıca veritabanı.</span><span class="sxs-lookup"><span data-stu-id="ce619-129">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="ce619-130">Bu veritabanı sağlayıcısı, bir bellek içi veritabanı ile kullanılacak Entity Framework Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="ce619-130">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="ce619-131">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="ce619-131">Add a model class</span></span>

<span data-ttu-id="ce619-132">Uygulamanızda veri temsil eden bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="ce619-132">A model is an object representing the data in your app.</span></span> <span data-ttu-id="ce619-133">Bu durumda, bir yapılacak iş öğesi tek model budur.</span><span class="sxs-lookup"><span data-stu-id="ce619-133">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="ce619-134">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="ce619-134">Add a folder named *Models*.</span></span> <span data-ttu-id="ce619-135">Model sınıfları, projenizdeki herhangi bir yere koyabilirsiniz ancak *modelleri* klasörü, kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ce619-135">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="ce619-136">Ekleme bir `TodoItem` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ce619-136">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="ce619-137">Veritabanı oluşturur `Id` olduğunda bir `TodoItem` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ce619-137">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="ce619-138">Veritabanı bağlamı oluşturur</span><span class="sxs-lookup"><span data-stu-id="ce619-138">Create the database context</span></span>

<span data-ttu-id="ce619-139">*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="ce619-139">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="ce619-140">Türeterek Bu sınıf oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ce619-140">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="ce619-141">Ekleme bir `TodoContext` sınıfını *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="ce619-141">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="ce619-142">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="ce619-142">Add a controller</span></span>

<span data-ttu-id="ce619-143">İçinde *denetleyicileri* klasör adında bir sınıf oluşturma `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="ce619-143">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="ce619-144">Dosyanın içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="ce619-144">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="ce619-145">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="ce619-145">Launch the app</span></span>

<span data-ttu-id="ce619-146">VS Code'da uygulamasını başlatmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="ce619-146">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="ce619-147">Gidin http://localhost:5000/api/todo ( `Todo` oluşturduğumuz denetleyicisi).</span><span class="sxs-lookup"><span data-stu-id="ce619-147">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="ce619-148">Visual Studio Code Yardım</span><span class="sxs-lookup"><span data-stu-id="ce619-148">Visual Studio Code help</span></span>

* [<span data-ttu-id="ce619-149">Başlarken</span><span class="sxs-lookup"><span data-stu-id="ce619-149">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="ce619-150">Hata Ayıklama</span><span class="sxs-lookup"><span data-stu-id="ce619-150">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="ce619-151">Tümleşik terminal</span><span class="sxs-lookup"><span data-stu-id="ce619-151">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="ce619-152">Klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="ce619-152">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="ce619-153">macOS klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="ce619-153">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="ce619-154">Linux klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="ce619-154">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="ce619-155">Windows klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="ce619-155">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
