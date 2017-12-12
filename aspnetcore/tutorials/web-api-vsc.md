---
title: "ASP.NET Çekirdeği ve VS Code'u Web API'si oluşturma"
description: "MacOS, Linux veya Windows ASP.NET Core MVC ve Visual Studio Code ile web API'si oluşturma"
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
keywords: "ASP.NET Core, Webapı, Web API'si, REST, Mac, Linux, HTTP, hizmeti, HTTP hizmeti, VS Code"
manager: wpickett
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
uid: tutorials/web-api-vsc
ms.openlocfilehash: caf40ee1c2d45d2fbf33b07d707fa4f1be98d31c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="66827-104">ASP.NET Core MVC ve Linux, macOS ve Windows Visual Studio Code ile Web API oluşturun</span><span class="sxs-lookup"><span data-stu-id="66827-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="66827-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="66827-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="66827-106">Bu öğreticide, "Yapılacaklar" öğeleri listesini yönetmek için bir web API'si oluşturma.</span><span class="sxs-lookup"><span data-stu-id="66827-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="66827-107">Bir kullanıcı Arabirimi yapı olmaz.</span><span class="sxs-lookup"><span data-stu-id="66827-107">You won’t build a UI.</span></span>

<span data-ttu-id="66827-108">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="66827-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="66827-109">macOS, Linux, Windows: Visual Studio Code (Bu öğretici) ile Web API</span><span class="sxs-lookup"><span data-stu-id="66827-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="66827-110">macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="66827-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="66827-111">Windows: [Web API'si Visual Studio ile Windows için](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="66827-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="66827-112">Geliştirme ortamınızı ayarlama</span><span class="sxs-lookup"><span data-stu-id="66827-112">Set up your development environment</span></span>

<span data-ttu-id="66827-113">İndirin ve yükleyin:</span><span class="sxs-lookup"><span data-stu-id="66827-113">Download and install:</span></span>
- <span data-ttu-id="66827-114">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="66827-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="66827-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="66827-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="66827-116">Visual Studio Code [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="66827-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="66827-117">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="66827-117">Create the project</span></span>

<span data-ttu-id="66827-118">Bir konsoldan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="66827-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="66827-119">Açık *TodoApi* Visual Studio Code (VS Code) ve select klasöründe *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="66827-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="66827-120">Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'TodoApi' eksik. ileti</span><span class="sxs-lookup"><span data-stu-id="66827-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="66827-121">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="66827-121">Add them?"</span></span>
- <span data-ttu-id="66827-122">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.</span><span class="sxs-lookup"><span data-stu-id="66827-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Derleme ve hata ayıklama VS koduyla gerekli uyar varlıklar 'TodoApi' eksik.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="66827-126">Tuşuna **hata ayıklama** derlemek ve programı çalıştırmak için (F5).</span><span class="sxs-lookup"><span data-stu-id="66827-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="66827-127">Bir tarayıcıda http://localhost: 5000/api/değerleri gidin.</span><span class="sxs-lookup"><span data-stu-id="66827-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="66827-128">Aşağıdaki görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="66827-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="66827-129">Bkz: [Visual Studio Code Yardım](#visual-studio-code-help) VS Code kullanma hakkında ipuçları için.</span><span class="sxs-lookup"><span data-stu-id="66827-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="66827-130">Entity Framework Çekirdek desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="66827-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="66827-131">Düzen *TodoApi.csproj* yüklemek için dosya [Entity Framework Çekirdek Inmemory](https://docs.microsoft.com/ef/core/providers/in-memory/) veritabanı sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="66827-131">Edit the *TodoApi.csproj* file to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="66827-132">Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="66827-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

<span data-ttu-id="66827-133">Çalıştırma `dotnet restore` indirip EF çekirdek Inmemory DB Sağlayıcısı'nı yükleyin.</span><span class="sxs-lookup"><span data-stu-id="66827-133">Run `dotnet restore` to download and install the EF Core InMemory DB provider.</span></span> <span data-ttu-id="66827-134">Çalıştırabilirsiniz `dotnet restore` terminalde girin veya `⌘⇧P` (macOS) veya `Ctrl+Shift+P` (Linux) VS Code ve ardından yazın **.NET**.</span><span class="sxs-lookup"><span data-stu-id="66827-134">You can run `dotnet restore` from the terminal or enter `⌘⇧P` (macOS) or `Ctrl+Shift+P` (Linux) in VS Code and then type **.NET**.</span></span> <span data-ttu-id="66827-135">Seçin **.NET: geri yükleme paketleri**.</span><span class="sxs-lookup"><span data-stu-id="66827-135">Select **.NET: Restore Packages**.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="66827-136">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="66827-136">Add a model class</span></span>

<span data-ttu-id="66827-137">Uygulamanızdaki verileri temsil eden bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="66827-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="66827-138">Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.</span><span class="sxs-lookup"><span data-stu-id="66827-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="66827-139">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="66827-139">Add a folder named *Models*.</span></span> <span data-ttu-id="66827-140">Model sınıfları projenizde, herhangi bir yere koyabilirsiniz ancak *modelleri* klasörü, kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="66827-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="66827-141">Ekleme bir `TodoItem` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="66827-141">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="66827-142">Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="66827-142">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="66827-143">Veritabanı bağlamı oluşturma</span><span class="sxs-lookup"><span data-stu-id="66827-143">Create the database context</span></span>

<span data-ttu-id="66827-144">*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="66827-144">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="66827-145">Bu sınıf türetme tarafından oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="66827-145">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="66827-146">Ekleme bir `TodoContext` sınıfını *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="66827-146">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="66827-147">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="66827-147">Add a controller</span></span>

<span data-ttu-id="66827-148">İçinde *denetleyicileri* klasörünü adlı bir sınıf oluşturun `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="66827-148">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="66827-149">Aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="66827-149">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="66827-150">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="66827-150">Launch the app</span></span>

<span data-ttu-id="66827-151">VS Code'da uygulamayı başlatmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="66827-151">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="66827-152">API/http://localhost: 5000/todo gidin ( `Todo` yeni oluşturduğumuz denetleyicisi).</span><span class="sxs-lookup"><span data-stu-id="66827-152">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="66827-153">Visual Studio Code Yardım</span><span class="sxs-lookup"><span data-stu-id="66827-153">Visual Studio Code help</span></span>

- [<span data-ttu-id="66827-154">Başlarken</span><span class="sxs-lookup"><span data-stu-id="66827-154">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="66827-155">Hata Ayıklama</span><span class="sxs-lookup"><span data-stu-id="66827-155">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="66827-156">Tümleşik terminal</span><span class="sxs-lookup"><span data-stu-id="66827-156">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="66827-157">Klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="66827-157">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="66827-158">Mac klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="66827-158">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="66827-159">Linux klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="66827-159">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="66827-160">Windows klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="66827-160">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


