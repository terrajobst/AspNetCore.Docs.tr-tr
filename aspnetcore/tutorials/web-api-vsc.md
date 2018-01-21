---
title: "ASP.NET Çekirdeği ve VS Code'u Web API'si oluşturma"
description: "MacOS, Linux veya Windows ASP.NET Core MVC ve Visual Studio Code ile web API'si oluşturma"
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
manager: wpickett
uid: tutorials/web-api-vsc
ms.openlocfilehash: e5e0f7c5704036057db33bc8a705da9d4cd8c004
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="80515-103">ASP.NET Core MVC ve Linux, macOS ve Windows Visual Studio Code ile Web API oluşturun</span><span class="sxs-lookup"><span data-stu-id="80515-103">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="80515-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="80515-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="80515-105">Bu öğreticide, "Yapılacaklar" öğeleri listesini yönetmek için bir web API'si oluşturma.</span><span class="sxs-lookup"><span data-stu-id="80515-105">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="80515-106">Bir kullanıcı Arabirimi yapı olmaz.</span><span class="sxs-lookup"><span data-stu-id="80515-106">You won’t build a UI.</span></span>

<span data-ttu-id="80515-107">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="80515-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="80515-108">macOS, Linux, Windows: Visual Studio Code (Bu öğretici) ile Web API</span><span class="sxs-lookup"><span data-stu-id="80515-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="80515-109">macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="80515-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="80515-110">Windows: [Web API'si Visual Studio ile Windows için](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="80515-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="80515-111">Geliştirme ortamınızı ayarlama</span><span class="sxs-lookup"><span data-stu-id="80515-111">Set up your development environment</span></span>

<span data-ttu-id="80515-112">İndirin ve yükleyin:</span><span class="sxs-lookup"><span data-stu-id="80515-112">Download and install:</span></span>
- <span data-ttu-id="80515-113">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="80515-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="80515-114">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="80515-114">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="80515-115">Visual Studio Code [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="80515-115">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="80515-116">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="80515-116">Create the project</span></span>

<span data-ttu-id="80515-117">Bir konsoldan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="80515-117">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="80515-118">Açık *TodoApi* Visual Studio Code (VS Code) ve select klasöründe *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="80515-118">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="80515-119">Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'TodoApi' eksik. ileti</span><span class="sxs-lookup"><span data-stu-id="80515-119">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="80515-120">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="80515-120">Add them?"</span></span>
- <span data-ttu-id="80515-121">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.</span><span class="sxs-lookup"><span data-stu-id="80515-121">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Derleme ve hata ayıklama VS koduyla gerekli uyar varlıklar 'TodoApi' eksik.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="80515-125">Tuşuna **hata ayıklama** derlemek ve programı çalıştırmak için (F5).</span><span class="sxs-lookup"><span data-stu-id="80515-125">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="80515-126">Bir tarayıcıda http://localhost: 5000/api/değerleri gidin.</span><span class="sxs-lookup"><span data-stu-id="80515-126">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="80515-127">Aşağıdaki görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="80515-127">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="80515-128">Bkz: [Visual Studio Code Yardım](#visual-studio-code-help) VS Code kullanma hakkında ipuçları için.</span><span class="sxs-lookup"><span data-stu-id="80515-128">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="80515-129">Entity Framework Çekirdek desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="80515-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="80515-130">.NET Core 2. 0 ' yeni bir proje oluşturma ekler 'Microsoft.AspNetCore.All' Sağlayıcısında *TodoApi.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="80515-130">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="80515-131">Yüklemek için gerek yoktur [Entity Framework Çekirdek Inmemory](https://docs.microsoft.com/ef/core/providers/in-memory/) sağlayıcı ayrı olarak veritabanı.</span><span class="sxs-lookup"><span data-stu-id="80515-131">There is no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="80515-132">Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="80515-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="80515-133">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="80515-133">Add a model class</span></span>

<span data-ttu-id="80515-134">Uygulamanızdaki verileri temsil eden bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="80515-134">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="80515-135">Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.</span><span class="sxs-lookup"><span data-stu-id="80515-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="80515-136">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="80515-136">Add a folder named *Models*.</span></span> <span data-ttu-id="80515-137">Model sınıfları projenizde, herhangi bir yere koyabilirsiniz ancak *modelleri* klasörü, kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="80515-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="80515-138">Ekleme bir `TodoItem` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="80515-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="80515-139">Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="80515-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="80515-140">Veritabanı bağlamı oluşturma</span><span class="sxs-lookup"><span data-stu-id="80515-140">Create the database context</span></span>

<span data-ttu-id="80515-141">*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="80515-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="80515-142">Bu sınıf türetme tarafından oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="80515-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="80515-143">Ekleme bir `TodoContext` sınıfını *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="80515-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="80515-144">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="80515-144">Add a controller</span></span>

<span data-ttu-id="80515-145">İçinde *denetleyicileri* klasörünü adlı bir sınıf oluşturun `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="80515-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="80515-146">Aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="80515-146">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="80515-147">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="80515-147">Launch the app</span></span>

<span data-ttu-id="80515-148">VS Code'da uygulamayı başlatmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="80515-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="80515-149">API/http://localhost: 5000/todo gidin ( `Todo` yeni oluşturduğumuz denetleyicisi).</span><span class="sxs-lookup"><span data-stu-id="80515-149">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="80515-150">Visual Studio Code help</span><span class="sxs-lookup"><span data-stu-id="80515-150">Visual Studio Code help</span></span>

- [<span data-ttu-id="80515-151">Başlarken</span><span class="sxs-lookup"><span data-stu-id="80515-151">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="80515-152">Hata Ayıklama</span><span class="sxs-lookup"><span data-stu-id="80515-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="80515-153">Tümleşik terminal</span><span class="sxs-lookup"><span data-stu-id="80515-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="80515-154">Klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="80515-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="80515-155">Mac klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="80515-155">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="80515-156">Linux klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="80515-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="80515-157">Windows klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="80515-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


