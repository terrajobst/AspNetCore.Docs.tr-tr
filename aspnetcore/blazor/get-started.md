---
title: ASP.NET Core Blazor kullanmaya başlama
author: guardrex
description: Seçtiğiniz araç ile Blazor bir uygulama oluşturarak Blazor kullanmaya başlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: 9b4aee0be30568f098c756e9ab4cb5298e9a049b
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963003"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="3dc63-103">ASP.NET Core Blazor kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="3dc63-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="3dc63-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="3dc63-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="3dc63-105">Blazorkullanmaya başlayın:</span><span class="sxs-lookup"><span data-stu-id="3dc63-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="3dc63-106">[.NET Core 3,1 Preview SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.</span><span class="sxs-lookup"><span data-stu-id="3dc63-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="3dc63-107">Komut kabuğu 'nda aşağıdaki komutu çalıştırarak [WebAssembly şablonunuBlazor](xref:blazor/hosting-models#blazor-webassembly) .</span><span class="sxs-lookup"><span data-stu-id="3dc63-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span></span> <span data-ttu-id="3dc63-108">[Microsoft. AspNetCore.Blazor. Şablon](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) paketinin önizleme sürümü varsa Blazor WebAssembly önizleme aşamasındadır.</span><span class="sxs-lookup"><span data-stu-id="3dc63-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview2.19528.8
   ```

1. <span data-ttu-id="3dc63-109">Araç seçiminiz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="3dc63-109">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3dc63-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3dc63-110">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="3dc63-111">1 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-111">1\.</span></span> <span data-ttu-id="3dc63-112">**ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 16,4 Preview 2 veya üstünü yükledikten sonra](https://visualstudio.microsoft.com/vs/preview/) .</span><span class="sxs-lookup"><span data-stu-id="3dc63-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="3dc63-113">2 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-113">2\.</span></span> <span data-ttu-id="3dc63-114">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3dc63-114">Create a new project.</span></span>

   <span data-ttu-id="3dc63-115">3 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-115">3\.</span></span> <span data-ttu-id="3dc63-116">**Blazor uygulama**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-116">Select **Blazor App**.</span></span> <span data-ttu-id="3dc63-117">**İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-117">Select **Next**.</span></span>

   <span data-ttu-id="3dc63-118">4 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-118">4\.</span></span> <span data-ttu-id="3dc63-119">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-119">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="3dc63-120">**Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-120">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="3dc63-121">**Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-121">Select **Create**.</span></span>

   <span data-ttu-id="3dc63-122">5 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-122">5\.</span></span> <span data-ttu-id="3dc63-123">Blazor Weelsembly deneyimi için, **Webassembly uygulama şablonunuBlazor** seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="3dc63-124">Blazor sunucusu deneyimi için **Blazor sunucusu uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="3dc63-125">**Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-125">Select **Create**.</span></span> <span data-ttu-id="3dc63-126">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3dc63-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="3dc63-127">6 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-127">6\.</span></span> <span data-ttu-id="3dc63-128">Uygulamayı çalıştırmak için **Ctrl** +**F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="3dc63-128">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3dc63-129">Blazor Visual Studio uzantısını ASP.NET Core Blazor önceki bir önizleme sürümü için yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3dc63-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="3dc63-130">Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzeye eklemek yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="3dc63-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3dc63-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3dc63-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="3dc63-132">1 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-132">1\.</span></span> <span data-ttu-id="3dc63-133">[Visual Studio Code](https://code.visualstudio.com/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="3dc63-133">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="3dc63-134">2 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-134">2\.</span></span> <span data-ttu-id="3dc63-135">En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.</span><span class="sxs-lookup"><span data-stu-id="3dc63-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="3dc63-136">3 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-136">3\.</span></span> <span data-ttu-id="3dc63-137">Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="3dc63-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="3dc63-138">Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="3dc63-138">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="3dc63-139">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3dc63-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="3dc63-140">4 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-140">4\.</span></span> <span data-ttu-id="3dc63-141">Visual Studio Code 'de *WebApplication1* klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="3dc63-141">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="3dc63-142">5 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-142">5\.</span></span> <span data-ttu-id="3dc63-143">Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister.</span><span class="sxs-lookup"><span data-stu-id="3dc63-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="3dc63-144">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-144">Select **Yes**.</span></span>

   <span data-ttu-id="3dc63-145">6 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-145">6\.</span></span> <span data-ttu-id="3dc63-146">Blazor sunucusu uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="3dc63-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="3dc63-147">Blazor WebAssembly uygulaması kullanıyorsanız, uygulamanın proje klasöründen `dotnet run` yürütün.</span><span class="sxs-lookup"><span data-stu-id="3dc63-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="3dc63-148">7 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-148">7\.</span></span> <span data-ttu-id="3dc63-149">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-149">In a browser, navigate to `https://localhost:5001`.</span></span>

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

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3dc63-150">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="3dc63-150">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="3dc63-151">Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="3dc63-151">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="3dc63-152">Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="3dc63-152">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="3dc63-153">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3dc63-153">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="3dc63-154">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="3dc63-155">En son [.NET Core 3,0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) sürümünü yükler.</span><span class="sxs-lookup"><span data-stu-id="3dc63-155">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="3dc63-156">İsteğe bağlı olarak, [.NET Core 3,1 PREVIEW SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) ' yı yükleyerek ve sonra aşağıdaki komutu bir komut kabuğunda çalıştırarak [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu yükleme:</span><span class="sxs-lookup"><span data-stu-id="3dc63-156">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview2.19528.8
   ```

1. <span data-ttu-id="3dc63-157">Araç seçiminiz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="3dc63-157">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3dc63-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3dc63-158">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="3dc63-159">1 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-159">1\.</span></span> <span data-ttu-id="3dc63-160">**ASP.net ve Web geliştirme** iş yüküyle en son [Visual Studio 'yu](https://visualstudio.com/vs/) yükler.</span><span class="sxs-lookup"><span data-stu-id="3dc63-160">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="3dc63-161">2 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-161">2\.</span></span> <span data-ttu-id="3dc63-162">İsteğe bağlı olarak [Visual Studio 16,4 Preview 2 veya üstünü](https://visualstudio.microsoft.com/vs/preview/) , **ASP.net ve Web geliştirme** Iş yüküyle Blazor webassembly uygulama geliştirmesi ile birlikte yükler.</span><span class="sxs-lookup"><span data-stu-id="3dc63-162">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="3dc63-163">3 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-163">3\.</span></span> <span data-ttu-id="3dc63-164">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3dc63-164">Create a new project.</span></span>

   <span data-ttu-id="3dc63-165">4 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-165">4\.</span></span> <span data-ttu-id="3dc63-166">**Blazor uygulama**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-166">Select **Blazor App**.</span></span> <span data-ttu-id="3dc63-167">**İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-167">Select **Next**.</span></span>

   <span data-ttu-id="3dc63-168">5 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-168">5\.</span></span> <span data-ttu-id="3dc63-169">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-169">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="3dc63-170">**Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-170">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="3dc63-171">**Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-171">Select **Create**.</span></span>

   <span data-ttu-id="3dc63-172">6 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-172">6\.</span></span> <span data-ttu-id="3dc63-173">Blazor Weelsembly deneyimi için, **Webassembly uygulama şablonunuBlazor** seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-173">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="3dc63-174">Blazor sunucusu deneyimi için **Blazor sunucusu uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-174">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="3dc63-175">**Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-175">Select **Create**.</span></span> <span data-ttu-id="3dc63-176">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3dc63-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="3dc63-177">7 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-177">7\.</span></span> <span data-ttu-id="3dc63-178">Uygulamayı çalıştırmak için **F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="3dc63-178">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3dc63-179">Blazor Visual Studio uzantısını ASP.NET Core Blazor önceki bir önizleme sürümü için yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3dc63-179">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="3dc63-180">Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzeye eklemek yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="3dc63-180">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3dc63-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3dc63-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="3dc63-182">1 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-182">1\.</span></span> <span data-ttu-id="3dc63-183">[Visual Studio Code](https://code.visualstudio.com/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="3dc63-183">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="3dc63-184">2 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-184">2\.</span></span> <span data-ttu-id="3dc63-185">En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.</span><span class="sxs-lookup"><span data-stu-id="3dc63-185">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="3dc63-186">3 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-186">3\.</span></span> <span data-ttu-id="3dc63-187">Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="3dc63-187">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="3dc63-188">Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="3dc63-188">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="3dc63-189">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3dc63-189">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="3dc63-190">4 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-190">4\.</span></span> <span data-ttu-id="3dc63-191">Visual Studio Code 'de *WebApplication1* klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="3dc63-191">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="3dc63-192">5 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-192">5\.</span></span> <span data-ttu-id="3dc63-193">Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister.</span><span class="sxs-lookup"><span data-stu-id="3dc63-193">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="3dc63-194">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-194">Select **Yes**.</span></span>

   <span data-ttu-id="3dc63-195">6 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-195">6\.</span></span> <span data-ttu-id="3dc63-196">Blazor sunucusu uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="3dc63-196">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="3dc63-197">Blazor WebAssembly uygulaması kullanıyorsanız, uygulamanın proje klasöründen `dotnet run` yürütün.</span><span class="sxs-lookup"><span data-stu-id="3dc63-197">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="3dc63-198">7 \.</span><span class="sxs-lookup"><span data-stu-id="3dc63-198">7\.</span></span> <span data-ttu-id="3dc63-199">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-199">In a browser, navigate to `https://localhost:5001`.</span></span>

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

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3dc63-200">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="3dc63-200">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="3dc63-201">Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="3dc63-201">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="3dc63-202">Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="3dc63-202">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="3dc63-203">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3dc63-203">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="3dc63-204">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-204">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="3dc63-205">Kenar çubuğu 'ndaki sekmelerde birden çok sayfa mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="3dc63-205">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="3dc63-206">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="3dc63-206">Home</span></span>
* <span data-ttu-id="3dc63-207">Sayaç</span><span class="sxs-lookup"><span data-stu-id="3dc63-207">Counter</span></span>
* <span data-ttu-id="3dc63-208">Verileri getir</span><span class="sxs-lookup"><span data-stu-id="3dc63-208">Fetch data</span></span>

<span data-ttu-id="3dc63-209">Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-209">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="3dc63-210">Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak kullanabilirsiniz C#Blazor.</span><span class="sxs-lookup"><span data-stu-id="3dc63-210">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="3dc63-211">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="3dc63-211">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="3dc63-212">Tarayıcıda `/counter` için bir istek, en üstteki `@page` yönergesi tarafından belirtilen şekilde `Counter` bileşeninin içeriğini işlemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="3dc63-212">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="3dc63-213">Bileşenler, daha sonra, Kullanıcı arabirimini esnek ve verimli bir şekilde güncelleştirmek için kullanılabilen işleme ağacının bellek içi gösterimine işlenir.</span><span class="sxs-lookup"><span data-stu-id="3dc63-213">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="3dc63-214">**Bana tıklama** düğmesi her seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="3dc63-214">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="3dc63-215">`onclick` olayı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="3dc63-215">The `onclick` event is fired.</span></span>
* <span data-ttu-id="3dc63-216">`IncrementCount` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3dc63-216">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="3dc63-217">`currentCount` artırılır.</span><span class="sxs-lookup"><span data-stu-id="3dc63-217">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="3dc63-218">Bileşen yeniden işlenir.</span><span class="sxs-lookup"><span data-stu-id="3dc63-218">The component is rendered again.</span></span>

<span data-ttu-id="3dc63-219">Çalışma zamanı, yeni içeriği önceki içerikle karşılaştırır ve yalnızca değiştirilen içeriği Belge Nesne Modeli (DOM) öğesine uygular.</span><span class="sxs-lookup"><span data-stu-id="3dc63-219">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="3dc63-220">HTML sözdizimini kullanarak başka bir bileşene bileşen ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-220">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="3dc63-221">Örneğin, `Index` bileşenine bir `<Counter />` öğesi ekleyerek `Counter` bileşenini uygulamanın giriş sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-221">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="3dc63-222">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="3dc63-222">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="3dc63-223">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="3dc63-223">Run the app.</span></span> <span data-ttu-id="3dc63-224">Giriş sayfasının `Counter` bileşeni tarafından kendi sayacı vardır.</span><span class="sxs-lookup"><span data-stu-id="3dc63-224">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="3dc63-225">Bileşen parametreleri, alt bileşende özellikler ayarlamanıza olanak tanıyan öznitelikler veya [alt içerik](xref:blazor/components#child-content)kullanılarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="3dc63-225">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="3dc63-226">`Counter` bileşenine bir parametre eklemek için, bileşenin `@code` bloğunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="3dc63-226">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="3dc63-227">`[Parameter]` özniteliğiyle `IncrementAmount` için ortak özellik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-227">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="3dc63-228">`currentCount`değerini artırdığınızda `IncrementAmount` kullanmak için `IncrementCount` yöntemini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-228">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="3dc63-229">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="3dc63-229">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="3dc63-230">`Index` bileşenin `<Counter>` öğesindeki bir özniteliği kullanarak `IncrementAmount` belirtin.</span><span class="sxs-lookup"><span data-stu-id="3dc63-230">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="3dc63-231">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="3dc63-231">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="3dc63-232">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="3dc63-232">Run the app.</span></span> <span data-ttu-id="3dc63-233">`Index` bileşeni, **bana tıklama** düğmesi seçildiğinde her seferinde on ile artan kendi sayacıdır.</span><span class="sxs-lookup"><span data-stu-id="3dc63-233">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="3dc63-234">`/counter` `Counter` bileşeni (*Counter. Razor*), bir tarafından arttırmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="3dc63-234">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3dc63-235">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="3dc63-235">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="3dc63-236">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3dc63-236">Additional resources</span></span>

* <xref:signalr/introduction>
