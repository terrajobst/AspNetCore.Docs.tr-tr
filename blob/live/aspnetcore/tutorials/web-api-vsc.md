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
ms.openlocfilehash: 40f9259101e5d006378562a27e97948641e29450
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="a335a-104">ASP.NET Core MVC ve Linux, macOS ve Windows Visual Studio Code ile Web API oluşturun</span><span class="sxs-lookup"><span data-stu-id="a335a-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="a335a-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="a335a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="a335a-106">Bu öğreticide, "Yapılacaklar" öğeleri listesini yönetmek için bir web API'si oluşturma.</span><span class="sxs-lookup"><span data-stu-id="a335a-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="a335a-107">Bir kullanıcı Arabirimi yapı olmaz.</span><span class="sxs-lookup"><span data-stu-id="a335a-107">You won’t build a UI.</span></span>

<span data-ttu-id="a335a-108">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="a335a-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="a335a-109">macOS, Linux, Windows: Visual Studio Code (Bu öğretici) ile Web API</span><span class="sxs-lookup"><span data-stu-id="a335a-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="a335a-110">macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="a335a-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="a335a-111">Windows: [Web API'si Visual Studio ile Windows için](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="a335a-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="a335a-112">Geliştirme ortamınızı ayarlama</span><span class="sxs-lookup"><span data-stu-id="a335a-112">Set up your development environment</span></span>

<span data-ttu-id="a335a-113">İndirin ve yükleyin:</span><span class="sxs-lookup"><span data-stu-id="a335a-113">Download and install:</span></span>
- <span data-ttu-id="a335a-114">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="a335a-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="a335a-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a335a-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="a335a-116">Visual Studio Code [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="a335a-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="a335a-117">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a335a-117">Create the project</span></span>

<span data-ttu-id="a335a-118">Bir konsoldan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a335a-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="a335a-119">Açık *TodoApi* Visual Studio Code (VS Code) ve select klasöründe *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="a335a-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="a335a-120">Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'TodoApi' eksik. ileti</span><span class="sxs-lookup"><span data-stu-id="a335a-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="a335a-121">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="a335a-121">Add them?"</span></span>
- <span data-ttu-id="a335a-122">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.</span><span class="sxs-lookup"><span data-stu-id="a335a-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Derleme ve hata ayıklama VS koduyla gerekli uyar varlıklar 'TodoApi' eksik.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="a335a-126">Tuşuna **hata ayıklama** derlemek ve programı çalıştırmak için (F5).</span><span class="sxs-lookup"><span data-stu-id="a335a-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="a335a-127">Bir tarayıcıda http://localhost: 5000/api/değerleri gidin.</span><span class="sxs-lookup"><span data-stu-id="a335a-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="a335a-128">Aşağıdaki görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="a335a-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="a335a-129">Bkz: [Visual Studio Code Yardım](#visual-studio-code-help) VS Code kullanma hakkında ipuçları için.</span><span class="sxs-lookup"><span data-stu-id="a335a-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="a335a-130">Entity Framework Çekirdek desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="a335a-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="a335a-131">.NET Core 2. 0 ' yeni bir proje oluşturma ekler 'Microsoft.AspNetCore.All' Sağlayıcısında *TodoApi.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="a335a-131">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="a335a-132">Yüklemek için gerek yoktur [Entity Framework Çekirdek Inmemory](https://docs.microsoft.com/ef/core/providers/in-memory/) sağlayıcı ayrı olarak veritabanı.</span><span class="sxs-lookup"><span data-stu-id="a335a-132">There is no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="a335a-133">Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="a335a-133">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="a335a-134">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="a335a-134">Add a model class</span></span>

<span data-ttu-id="a335a-135">Uygulamanızdaki verileri temsil eden bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="a335a-135">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="a335a-136">Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.</span><span class="sxs-lookup"><span data-stu-id="a335a-136">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="a335a-137">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="a335a-137">Add a folder named *Models*.</span></span> <span data-ttu-id="a335a-138">Model sınıfları projenizde, herhangi bir yere koyabilirsiniz ancak *modelleri* klasörü, kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a335a-138">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="a335a-139">Ekleme bir `TodoItem` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="a335a-139">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="a335a-140">Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a335a-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="a335a-141">Veritabanı bağlamı oluşturma</span><span class="sxs-lookup"><span data-stu-id="a335a-141">Create the database context</span></span>

<span data-ttu-id="a335a-142">*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="a335a-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="a335a-143">Bu sınıf türetme tarafından oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a335a-143">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="a335a-144">Ekleme bir `TodoContext` sınıfını *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="a335a-144">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="a335a-145">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="a335a-145">Add a controller</span></span>

<span data-ttu-id="a335a-146">İçinde *denetleyicileri* klasörünü adlı bir sınıf oluşturun `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="a335a-146">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="a335a-147">Aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a335a-147">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="a335a-148">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="a335a-148">Launch the app</span></span>

<span data-ttu-id="a335a-149">VS Code'da uygulamayı başlatmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="a335a-149">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="a335a-150">API/http://localhost: 5000/todo gidin ( `Todo` yeni oluşturduğumuz denetleyicisi).</span><span class="sxs-lookup"><span data-stu-id="a335a-150">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="a335a-151">Visual Studio Code Yardım</span><span class="sxs-lookup"><span data-stu-id="a335a-151">Visual Studio Code help</span></span>

- [<span data-ttu-id="a335a-152">Başlarken</span><span class="sxs-lookup"><span data-stu-id="a335a-152">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="a335a-153">Hata Ayıklama</span><span class="sxs-lookup"><span data-stu-id="a335a-153">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="a335a-154">Tümleşik terminal</span><span class="sxs-lookup"><span data-stu-id="a335a-154">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="a335a-155">Klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="a335a-155">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="a335a-156">Mac klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="a335a-156">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="a335a-157">Linux klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="a335a-157">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="a335a-158">Windows klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="a335a-158">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


