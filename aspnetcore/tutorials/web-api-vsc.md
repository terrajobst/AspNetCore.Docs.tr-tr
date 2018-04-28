---
title: ASP.NET Çekirdeği ve Visual Studio Code ile Web API oluşturma
author: rick-anderson
description: MacOS, Linux veya Windows ASP.NET Core MVC ve Visual Studio Code ile web API'si oluşturma
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 303016c7e1d27726faaa0b1546575255355d5275
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="12c23-103">ASP.NET Çekirdeği ve Visual Studio Code ile Web API oluşturma</span><span class="sxs-lookup"><span data-stu-id="12c23-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="12c23-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="12c23-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="12c23-105">Bu öğreticide, "Yapılacaklar" öğeleri listesini yönetmek için bir web API'si oluşturma.</span><span class="sxs-lookup"><span data-stu-id="12c23-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="12c23-106">Bir kullanıcı Arabirimi oluşturulan değil.</span><span class="sxs-lookup"><span data-stu-id="12c23-106">A UI isn't constructed.</span></span>

<span data-ttu-id="12c23-107">Bu öğretici için üç sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="12c23-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="12c23-108">macOS, Linux, Windows: Visual Studio Code (Bu öğretici) ile Web API</span><span class="sxs-lookup"><span data-stu-id="12c23-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="12c23-109">macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="12c23-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="12c23-110">Windows: [Web API'si Visual Studio ile Windows için](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="12c23-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="12c23-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="12c23-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="12c23-112">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="12c23-112">Create the project</span></span>

<span data-ttu-id="12c23-113">Bir konsoldan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="12c23-113">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="12c23-114">*TodoApi* klasörünü Visual Studio Code (VS Code) açar.</span><span class="sxs-lookup"><span data-stu-id="12c23-114">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="12c23-115">Seçin *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="12c23-115">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="12c23-116">Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'TodoApi' eksik. ileti</span><span class="sxs-lookup"><span data-stu-id="12c23-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="12c23-117">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="12c23-117">Add them?"</span></span>
* <span data-ttu-id="12c23-118">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.</span><span class="sxs-lookup"><span data-stu-id="12c23-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![Derleme ve hata ayıklama VS koduyla gerekli uyar varlıklar 'TodoApi' eksik.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="12c23-122">Tuşuna **hata ayıklama** derlemek ve programı çalıştırmak için (F5).</span><span class="sxs-lookup"><span data-stu-id="12c23-122">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="12c23-123">Bir tarayıcıda gidin http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="12c23-123">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="12c23-124">Şu çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="12c23-124">The following output is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="12c23-125">Bkz: [Visual Studio Code Yardım](#visual-studio-code-help) VS Code kullanma hakkında ipuçları için.</span><span class="sxs-lookup"><span data-stu-id="12c23-125">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="12c23-126">Entity Framework Çekirdek desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="12c23-126">Add support for Entity Framework Core</span></span>

:::moniker range="<= aspnetcore-2.0"
<span data-ttu-id="12c23-127">ASP.NET Core 2. 0 ' yeni bir proje oluşturma ekler [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) paketini referansı *TodoApi.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="12c23-127">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="12c23-128">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="12c23-128">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
<span data-ttu-id="12c23-129">ASP.NET Core 2.1 veya daha sonra yeni bir proje oluşturma ekler [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) paketini referansı *TodoApi.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="12c23-129">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="12c23-130">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="12c23-130">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end

<span data-ttu-id="12c23-131">Yüklemek için gerek yoktur [Entity Framework Çekirdek Inmemory](/ef/core/providers/in-memory/) sağlayıcı ayrı olarak veritabanı.</span><span class="sxs-lookup"><span data-stu-id="12c23-131">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="12c23-132">Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar.</span><span class="sxs-lookup"><span data-stu-id="12c23-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="12c23-133">Bir model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="12c23-133">Add a model class</span></span>

<span data-ttu-id="12c23-134">Uygulamanızdaki verileri temsil eden bir nesne modelidir.</span><span class="sxs-lookup"><span data-stu-id="12c23-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="12c23-135">Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.</span><span class="sxs-lookup"><span data-stu-id="12c23-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="12c23-136">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="12c23-136">Add a folder named *Models*.</span></span> <span data-ttu-id="12c23-137">Model sınıfları projenizde, herhangi bir yere koyabilirsiniz ancak *modelleri* klasörü, kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="12c23-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="12c23-138">Ekleme bir `TodoItem` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="12c23-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="12c23-139">Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="12c23-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="12c23-140">Veritabanı bağlamı oluşturma</span><span class="sxs-lookup"><span data-stu-id="12c23-140">Create the database context</span></span>

<span data-ttu-id="12c23-141">*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır.</span><span class="sxs-lookup"><span data-stu-id="12c23-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="12c23-142">Bu sınıf türetme tarafından oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="12c23-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="12c23-143">Ekleme bir `TodoContext` sınıfını *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="12c23-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="12c23-144">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="12c23-144">Add a controller</span></span>

<span data-ttu-id="12c23-145">İçinde *denetleyicileri* klasörünü adlı bir sınıf oluşturun `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="12c23-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="12c23-146">İçeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="12c23-146">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="12c23-147">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="12c23-147">Launch the app</span></span>

<span data-ttu-id="12c23-148">VS Code'da uygulamayı başlatmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="12c23-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="12c23-149">Gidin http://localhost:5000/api/todo ( `Todo` oluşturduğumuz denetleyicisi).</span><span class="sxs-lookup"><span data-stu-id="12c23-149">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="12c23-150">Visual Studio Code Yardım</span><span class="sxs-lookup"><span data-stu-id="12c23-150">Visual Studio Code help</span></span>

* [<span data-ttu-id="12c23-151">Başlarken</span><span class="sxs-lookup"><span data-stu-id="12c23-151">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="12c23-152">Hata Ayıklama</span><span class="sxs-lookup"><span data-stu-id="12c23-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="12c23-153">Tümleşik terminal</span><span class="sxs-lookup"><span data-stu-id="12c23-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="12c23-154">Klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="12c23-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="12c23-155">macOS klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="12c23-155">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="12c23-156">Linux klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="12c23-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="12c23-157">Windows klavye kısayolları</span><span class="sxs-lookup"><span data-stu-id="12c23-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
