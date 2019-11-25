---
title: Get started with ASP.NET Core Blazor
author: guardrex
description: Get started with Blazor by building a Blazor app with the tooling of your choice.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: 198093b37cb4f440eb7b520d18004304aea570a5
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239712"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="2618b-103">Get started with ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="2618b-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="2618b-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2618b-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="2618b-105">Get started with Blazor:</span><span class="sxs-lookup"><span data-stu-id="2618b-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="2618b-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="2618b-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="2618b-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span><span class="sxs-lookup"><span data-stu-id="2618b-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span></span> <span data-ttu-id="2618b-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span><span class="sxs-lookup"><span data-stu-id="2618b-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview3.19555.2
   ```

1. <span data-ttu-id="2618b-109">Follow the guidance for your choice of tooling:</span><span class="sxs-lookup"><span data-stu-id="2618b-109">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2618b-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2618b-110">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="2618b-111">1\.</span><span class="sxs-lookup"><span data-stu-id="2618b-111">1\.</span></span> <span data-ttu-id="2618b-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span><span class="sxs-lookup"><span data-stu-id="2618b-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="2618b-113">2\.</span><span class="sxs-lookup"><span data-stu-id="2618b-113">2\.</span></span> <span data-ttu-id="2618b-114">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2618b-114">Create a new project.</span></span>

   <span data-ttu-id="2618b-115">3\.</span><span class="sxs-lookup"><span data-stu-id="2618b-115">3\.</span></span> <span data-ttu-id="2618b-116">Select **Blazor App**.</span><span class="sxs-lookup"><span data-stu-id="2618b-116">Select **Blazor App**.</span></span> <span data-ttu-id="2618b-117">Select **Next**.</span><span class="sxs-lookup"><span data-stu-id="2618b-117">Select **Next**.</span></span>

   <span data-ttu-id="2618b-118">4\.</span><span class="sxs-lookup"><span data-stu-id="2618b-118">4\.</span></span> <span data-ttu-id="2618b-119">Provide a project name in the **Project name** field or accept the default project name.</span><span class="sxs-lookup"><span data-stu-id="2618b-119">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="2618b-120">Confirm the **Location** entry is correct or provide a location for the project.</span><span class="sxs-lookup"><span data-stu-id="2618b-120">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="2618b-121">Select **Create**.</span><span class="sxs-lookup"><span data-stu-id="2618b-121">Select **Create**.</span></span>

   <span data-ttu-id="2618b-122">5\.</span><span class="sxs-lookup"><span data-stu-id="2618b-122">5\.</span></span> <span data-ttu-id="2618b-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span><span class="sxs-lookup"><span data-stu-id="2618b-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="2618b-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span><span class="sxs-lookup"><span data-stu-id="2618b-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="2618b-125">Select **Create**.</span><span class="sxs-lookup"><span data-stu-id="2618b-125">Select **Create**.</span></span> <span data-ttu-id="2618b-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2618b-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2618b-127">6\.</span><span class="sxs-lookup"><span data-stu-id="2618b-127">6\.</span></span> <span data-ttu-id="2618b-128">Press **Ctrl**+**F5** to run the app.</span><span class="sxs-lookup"><span data-stu-id="2618b-128">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2618b-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span><span class="sxs-lookup"><span data-stu-id="2618b-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="2618b-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2618b-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2618b-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2618b-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="2618b-132">1\.</span><span class="sxs-lookup"><span data-stu-id="2618b-132">1\.</span></span> <span data-ttu-id="2618b-133">Install [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="2618b-133">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="2618b-134">2\.</span><span class="sxs-lookup"><span data-stu-id="2618b-134">2\.</span></span> <span data-ttu-id="2618b-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="2618b-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="2618b-136">3\.</span><span class="sxs-lookup"><span data-stu-id="2618b-136">3\.</span></span> <span data-ttu-id="2618b-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span><span class="sxs-lookup"><span data-stu-id="2618b-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="2618b-138">For a Blazor Server experience, execute the following command in a command shell:</span><span class="sxs-lookup"><span data-stu-id="2618b-138">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="2618b-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2618b-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2618b-140">4\.</span><span class="sxs-lookup"><span data-stu-id="2618b-140">4\.</span></span> <span data-ttu-id="2618b-141">Open the *WebApplication1* folder in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2618b-141">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="2618b-142">5\.</span><span class="sxs-lookup"><span data-stu-id="2618b-142">5\.</span></span> <span data-ttu-id="2618b-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span><span class="sxs-lookup"><span data-stu-id="2618b-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="2618b-144">Select **Yes**.</span><span class="sxs-lookup"><span data-stu-id="2618b-144">Select **Yes**.</span></span>

   <span data-ttu-id="2618b-145">6\.</span><span class="sxs-lookup"><span data-stu-id="2618b-145">6\.</span></span> <span data-ttu-id="2618b-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span><span class="sxs-lookup"><span data-stu-id="2618b-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="2618b-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span><span class="sxs-lookup"><span data-stu-id="2618b-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="2618b-148">7\.</span><span class="sxs-lookup"><span data-stu-id="2618b-148">7\.</span></span> <span data-ttu-id="2618b-149">In a browser, navigate to `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="2618b-149">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2618b-150">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2618b-150">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="2618b-151">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span><span class="sxs-lookup"><span data-stu-id="2618b-151">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="2618b-152">For a Blazor Server experience, execute the following commands in a command shell:</span><span class="sxs-lookup"><span data-stu-id="2618b-152">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="2618b-153">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2618b-153">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2618b-154">In a browser, navigate to `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="2618b-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="2618b-155">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span><span class="sxs-lookup"><span data-stu-id="2618b-155">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="2618b-156">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span><span class="sxs-lookup"><span data-stu-id="2618b-156">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview3.19555.2
   ```

1. <span data-ttu-id="2618b-157">Follow the guidance for your choice of tooling:</span><span class="sxs-lookup"><span data-stu-id="2618b-157">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2618b-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2618b-158">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="2618b-159">1\.</span><span class="sxs-lookup"><span data-stu-id="2618b-159">1\.</span></span> <span data-ttu-id="2618b-160">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span><span class="sxs-lookup"><span data-stu-id="2618b-160">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="2618b-161">2\.</span><span class="sxs-lookup"><span data-stu-id="2618b-161">2\.</span></span> <span data-ttu-id="2618b-162">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span><span class="sxs-lookup"><span data-stu-id="2618b-162">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="2618b-163">3\.</span><span class="sxs-lookup"><span data-stu-id="2618b-163">3\.</span></span> <span data-ttu-id="2618b-164">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2618b-164">Create a new project.</span></span>

   <span data-ttu-id="2618b-165">4\.</span><span class="sxs-lookup"><span data-stu-id="2618b-165">4\.</span></span> <span data-ttu-id="2618b-166">Select **Blazor App**.</span><span class="sxs-lookup"><span data-stu-id="2618b-166">Select **Blazor App**.</span></span> <span data-ttu-id="2618b-167">Select **Next**.</span><span class="sxs-lookup"><span data-stu-id="2618b-167">Select **Next**.</span></span>

   <span data-ttu-id="2618b-168">5\.</span><span class="sxs-lookup"><span data-stu-id="2618b-168">5\.</span></span> <span data-ttu-id="2618b-169">Provide a project name in the **Project name** field or accept the default project name.</span><span class="sxs-lookup"><span data-stu-id="2618b-169">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="2618b-170">Confirm the **Location** entry is correct or provide a location for the project.</span><span class="sxs-lookup"><span data-stu-id="2618b-170">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="2618b-171">Select **Create**.</span><span class="sxs-lookup"><span data-stu-id="2618b-171">Select **Create**.</span></span>

   <span data-ttu-id="2618b-172">6\.</span><span class="sxs-lookup"><span data-stu-id="2618b-172">6\.</span></span> <span data-ttu-id="2618b-173">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span><span class="sxs-lookup"><span data-stu-id="2618b-173">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="2618b-174">For a Blazor Server experience, choose the **Blazor Server App** template.</span><span class="sxs-lookup"><span data-stu-id="2618b-174">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="2618b-175">Select **Create**.</span><span class="sxs-lookup"><span data-stu-id="2618b-175">Select **Create**.</span></span> <span data-ttu-id="2618b-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2618b-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2618b-177">7\.</span><span class="sxs-lookup"><span data-stu-id="2618b-177">7\.</span></span> <span data-ttu-id="2618b-178">Press **F5** to run the app.</span><span class="sxs-lookup"><span data-stu-id="2618b-178">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2618b-179">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span><span class="sxs-lookup"><span data-stu-id="2618b-179">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="2618b-180">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2618b-180">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2618b-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2618b-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="2618b-182">1\.</span><span class="sxs-lookup"><span data-stu-id="2618b-182">1\.</span></span> <span data-ttu-id="2618b-183">Install [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="2618b-183">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="2618b-184">2\.</span><span class="sxs-lookup"><span data-stu-id="2618b-184">2\.</span></span> <span data-ttu-id="2618b-185">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="2618b-185">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="2618b-186">3\.</span><span class="sxs-lookup"><span data-stu-id="2618b-186">3\.</span></span> <span data-ttu-id="2618b-187">For a Blazor WebAssembly experience, execute the following command in a command shell:</span><span class="sxs-lookup"><span data-stu-id="2618b-187">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="2618b-188">For a Blazor Server experience, execute the following command in a command shell:</span><span class="sxs-lookup"><span data-stu-id="2618b-188">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="2618b-189">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2618b-189">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2618b-190">4\.</span><span class="sxs-lookup"><span data-stu-id="2618b-190">4\.</span></span> <span data-ttu-id="2618b-191">Open the *WebApplication1* folder in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2618b-191">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="2618b-192">5\.</span><span class="sxs-lookup"><span data-stu-id="2618b-192">5\.</span></span> <span data-ttu-id="2618b-193">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span><span class="sxs-lookup"><span data-stu-id="2618b-193">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="2618b-194">Select **Yes**.</span><span class="sxs-lookup"><span data-stu-id="2618b-194">Select **Yes**.</span></span>

   <span data-ttu-id="2618b-195">6\.</span><span class="sxs-lookup"><span data-stu-id="2618b-195">6\.</span></span> <span data-ttu-id="2618b-196">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span><span class="sxs-lookup"><span data-stu-id="2618b-196">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="2618b-197">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span><span class="sxs-lookup"><span data-stu-id="2618b-197">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="2618b-198">7\.</span><span class="sxs-lookup"><span data-stu-id="2618b-198">7\.</span></span> <span data-ttu-id="2618b-199">In a browser, navigate to `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="2618b-199">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2618b-200">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2618b-200">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="2618b-201">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span><span class="sxs-lookup"><span data-stu-id="2618b-201">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="2618b-202">For a Blazor Server experience, execute the following commands in a command shell:</span><span class="sxs-lookup"><span data-stu-id="2618b-202">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="2618b-203">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2618b-203">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2618b-204">In a browser, navigate to `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="2618b-204">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="2618b-205">Multiple pages are available from tabs in the sidebar:</span><span class="sxs-lookup"><span data-stu-id="2618b-205">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="2618b-206">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="2618b-206">Home</span></span>
* <span data-ttu-id="2618b-207">Sayaç</span><span class="sxs-lookup"><span data-stu-id="2618b-207">Counter</span></span>
* <span data-ttu-id="2618b-208">Fetch data</span><span class="sxs-lookup"><span data-stu-id="2618b-208">Fetch data</span></span>

<span data-ttu-id="2618b-209">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span><span class="sxs-lookup"><span data-stu-id="2618b-209">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="2618b-210">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span><span class="sxs-lookup"><span data-stu-id="2618b-210">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="2618b-211">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="2618b-211">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="2618b-212">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span><span class="sxs-lookup"><span data-stu-id="2618b-212">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="2618b-213">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span><span class="sxs-lookup"><span data-stu-id="2618b-213">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="2618b-214">Each time the **Click me** button is selected:</span><span class="sxs-lookup"><span data-stu-id="2618b-214">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="2618b-215">The `onclick` event is fired.</span><span class="sxs-lookup"><span data-stu-id="2618b-215">The `onclick` event is fired.</span></span>
* <span data-ttu-id="2618b-216">The `IncrementCount` method is called.</span><span class="sxs-lookup"><span data-stu-id="2618b-216">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="2618b-217">The `currentCount` is incremented.</span><span class="sxs-lookup"><span data-stu-id="2618b-217">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="2618b-218">The component is rendered again.</span><span class="sxs-lookup"><span data-stu-id="2618b-218">The component is rendered again.</span></span>

<span data-ttu-id="2618b-219">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="2618b-219">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="2618b-220">Add a component to another component using HTML syntax.</span><span class="sxs-lookup"><span data-stu-id="2618b-220">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="2618b-221">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span><span class="sxs-lookup"><span data-stu-id="2618b-221">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="2618b-222">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="2618b-222">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="2618b-223">Run the app.</span><span class="sxs-lookup"><span data-stu-id="2618b-223">Run the app.</span></span> <span data-ttu-id="2618b-224">The homepage has its own counter provided by the `Counter` component.</span><span class="sxs-lookup"><span data-stu-id="2618b-224">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="2618b-225">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span><span class="sxs-lookup"><span data-stu-id="2618b-225">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="2618b-226">To add a parameter to the `Counter` component, update the component's `@code` block:</span><span class="sxs-lookup"><span data-stu-id="2618b-226">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="2618b-227">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span><span class="sxs-lookup"><span data-stu-id="2618b-227">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="2618b-228">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="2618b-228">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="2618b-229">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="2618b-229">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="2618b-230">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span><span class="sxs-lookup"><span data-stu-id="2618b-230">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="2618b-231">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="2618b-231">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="2618b-232">Run the app.</span><span class="sxs-lookup"><span data-stu-id="2618b-232">Run the app.</span></span> <span data-ttu-id="2618b-233">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span><span class="sxs-lookup"><span data-stu-id="2618b-233">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="2618b-234">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span><span class="sxs-lookup"><span data-stu-id="2618b-234">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2618b-235">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="2618b-235">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="2618b-236">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2618b-236">Additional resources</span></span>

* <xref:signalr/introduction>
