---
title: ASP.NET Core Blazor kullanmaya başlama
author: guardrex
description: Tercih ettiğiniz araç ile bir Blazor uygulaması oluşturarak Blazor kullanmaya başlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: blazor/get-started
ms.openlocfilehash: b5043c7e4549800c1ab49bc37dd8f3568975d4aa
ms.sourcegitcommit: 67116718dc33a7a01696d41af38590fdbb58e014
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799235"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="8c9c4-103">ASP.NET Core Blazor kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="8c9c4-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="8c9c4-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="8c9c4-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="8c9c4-105">Blazor kullanmaya başlama:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="8c9c4-106">[.NET Core 3,1 Preview SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="8c9c4-107">Komut kabuğu 'nda aşağıdaki komutu çalıştırarak [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu yükler.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span></span> <span data-ttu-id="8c9c4-108">[Microsoft. AspNetCore. Blazor. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) paketinin önizleme sürümü vardır, ancak Blazor WebAssembly önizlemededir.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview2.19528.8
   ```

1. <span data-ttu-id="8c9c4-109">Araç seçiminiz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-109">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8c9c4-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8c9c4-110">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="8c9c4-111">1 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-111">1\.</span></span> <span data-ttu-id="8c9c4-112">**ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 16,4 Preview 2 veya üstünü yükledikten sonra](https://visualstudio.microsoft.com/vs/preview/) .</span><span class="sxs-lookup"><span data-stu-id="8c9c4-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="8c9c4-113">2 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-113">2\.</span></span> <span data-ttu-id="8c9c4-114">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-114">Create a new project.</span></span>

   <span data-ttu-id="8c9c4-115">3 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-115">3\.</span></span> <span data-ttu-id="8c9c4-116">**Blazor uygulamasını**seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-116">Select **Blazor App**.</span></span> <span data-ttu-id="8c9c4-117">**İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-117">Select **Next**.</span></span>

   <span data-ttu-id="8c9c4-118">4 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-118">4\.</span></span> <span data-ttu-id="8c9c4-119">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-119">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="8c9c4-120">**Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-120">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="8c9c4-121">**Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-121">Select **Create**.</span></span>

   <span data-ttu-id="8c9c4-122">5 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-122">5\.</span></span> <span data-ttu-id="8c9c4-123">Blazor WebAssembly deneyimi için **Blazor Webassembly uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="8c9c4-124">Blazor sunucu deneyimi için **Blazor Server uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="8c9c4-125">**Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-125">Select **Create**.</span></span> <span data-ttu-id="8c9c4-126">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="8c9c4-127">6 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-127">6\.</span></span> <span data-ttu-id="8c9c4-128">Uygulamayı çalıştırmak için **Ctrl** +**F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-128">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8c9c4-129">ASP.NET Core Blazor 'nin önceki bir önizleme sürümü için Blazor Visual Studio uzantısı 'nı yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="8c9c4-130">Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzey için yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8c9c4-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8c9c4-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="8c9c4-132">1 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-132">1\.</span></span> <span data-ttu-id="8c9c4-133">[Visual Studio Code](https://code.visualstudio.com/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-133">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="8c9c4-134">2 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-134">2\.</span></span> <span data-ttu-id="8c9c4-135">En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="8c9c4-136">3 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-136">3\.</span></span> <span data-ttu-id="8c9c4-137">Bir Blazor WebAssembly deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="8c9c4-138">Bir Blazor sunucu deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-138">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="8c9c4-139">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="8c9c4-140">4 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-140">4\.</span></span> <span data-ttu-id="8c9c4-141">Visual Studio Code 'de *WebApplication1* klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-141">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="8c9c4-142">5 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-142">5\.</span></span> <span data-ttu-id="8c9c4-143">Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="8c9c4-144">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-144">Select **Yes**.</span></span>

   <span data-ttu-id="8c9c4-145">6 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-145">6\.</span></span> <span data-ttu-id="8c9c4-146">Bir Blazor Server uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="8c9c4-147">Blazor WebAssembly uygulaması kullanılıyorsa, uygulamanın proje klasöründen `dotnet run` yürütün.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="8c9c4-148">7 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-148">7\.</span></span> <span data-ttu-id="8c9c4-149">Bir tarayıcıda `https://localhost:5001` ' a gidin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-149">In a browser, navigate to `https://localhost:5001`.</span></span>

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

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8c9c4-150">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8c9c4-150">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="8c9c4-151">Bir Blazor Weelsembly deneyimi için komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-151">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="8c9c4-152">Bir Blazor sunucu deneyimi için aşağıdaki komutları bir komut kabuğunda yürütün:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-152">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="8c9c4-153">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-153">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="8c9c4-154">Bir tarayıcıda `https://localhost:5001` ' a gidin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="8c9c4-155">En son [.NET Core 3,0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) sürümünü yükler.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-155">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="8c9c4-156">İsteğe bağlı olarak, [.NET Core 3,1 PREVIEW SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) ' yı yükleyerek ve sonra bir komut kabuğunda aşağıdaki komutu çalıştırarak [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu yükleme:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-156">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview2.19528.8
   ```

1. <span data-ttu-id="8c9c4-157">Araç seçiminiz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-157">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8c9c4-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8c9c4-158">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="8c9c4-159">1 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-159">1\.</span></span> <span data-ttu-id="8c9c4-160">**ASP.net ve Web geliştirme** iş yüküyle en son [Visual Studio 'yu](https://visualstudio.com/vs/) yükler.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-160">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="8c9c4-161">2 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-161">2\.</span></span> <span data-ttu-id="8c9c4-162">**ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 16,4 Preview 2 veya üstünü](https://visualstudio.microsoft.com/vs/preview/) , Blazor webassembly uygulama geliştirmesi için de yükler.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-162">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="8c9c4-163">3 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-163">3\.</span></span> <span data-ttu-id="8c9c4-164">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-164">Create a new project.</span></span>

   <span data-ttu-id="8c9c4-165">4 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-165">4\.</span></span> <span data-ttu-id="8c9c4-166">**Blazor uygulamasını**seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-166">Select **Blazor App**.</span></span> <span data-ttu-id="8c9c4-167">**İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-167">Select **Next**.</span></span>

   <span data-ttu-id="8c9c4-168">5 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-168">5\.</span></span> <span data-ttu-id="8c9c4-169">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-169">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="8c9c4-170">**Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-170">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="8c9c4-171">**Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-171">Select **Create**.</span></span>

   <span data-ttu-id="8c9c4-172">6 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-172">6\.</span></span> <span data-ttu-id="8c9c4-173">Blazor WebAssembly deneyimi için **Blazor Webassembly uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-173">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="8c9c4-174">Blazor sunucu deneyimi için **Blazor Server uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-174">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="8c9c4-175">**Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-175">Select **Create**.</span></span> <span data-ttu-id="8c9c4-176">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="8c9c4-177">7 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-177">7\.</span></span> <span data-ttu-id="8c9c4-178">Uygulamayı çalıştırmak için **F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-178">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8c9c4-179">ASP.NET Core Blazor 'nin önceki bir önizleme sürümü için Blazor Visual Studio uzantısı 'nı yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-179">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="8c9c4-180">Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzey için yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-180">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8c9c4-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8c9c4-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="8c9c4-182">1 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-182">1\.</span></span> <span data-ttu-id="8c9c4-183">[Visual Studio Code](https://code.visualstudio.com/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-183">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="8c9c4-184">2 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-184">2\.</span></span> <span data-ttu-id="8c9c4-185">En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-185">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="8c9c4-186">3 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-186">3\.</span></span> <span data-ttu-id="8c9c4-187">Bir Blazor WebAssembly deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-187">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="8c9c4-188">Bir Blazor sunucu deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-188">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="8c9c4-189">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-189">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="8c9c4-190">4 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-190">4\.</span></span> <span data-ttu-id="8c9c4-191">Visual Studio Code 'de *WebApplication1* klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-191">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="8c9c4-192">5 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-192">5\.</span></span> <span data-ttu-id="8c9c4-193">Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-193">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="8c9c4-194">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-194">Select **Yes**.</span></span>

   <span data-ttu-id="8c9c4-195">6 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-195">6\.</span></span> <span data-ttu-id="8c9c4-196">Bir Blazor Server uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-196">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="8c9c4-197">Blazor WebAssembly uygulaması kullanılıyorsa, uygulamanın proje klasöründen `dotnet run` yürütün.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-197">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="8c9c4-198">7 \.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-198">7\.</span></span> <span data-ttu-id="8c9c4-199">Bir tarayıcıda `https://localhost:5001` ' a gidin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-199">In a browser, navigate to `https://localhost:5001`.</span></span>

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

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8c9c4-200">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8c9c4-200">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="8c9c4-201">Bir Blazor Weelsembly deneyimi için komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-201">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="8c9c4-202">Bir Blazor sunucu deneyimi için aşağıdaki komutları bir komut kabuğunda yürütün:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-202">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="8c9c4-203">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-203">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="8c9c4-204">Bir tarayıcıda `https://localhost:5001` ' a gidin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-204">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="8c9c4-205">Kenar çubuğu 'ndaki sekmelerde birden çok sayfa mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-205">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="8c9c4-206">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="8c9c4-206">Home</span></span>
* <span data-ttu-id="8c9c4-207">Sayaç</span><span class="sxs-lookup"><span data-stu-id="8c9c4-207">Counter</span></span>
* <span data-ttu-id="8c9c4-208">Verileri getir</span><span class="sxs-lookup"><span data-stu-id="8c9c4-208">Fetch data</span></span>

<span data-ttu-id="8c9c4-209">Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-209">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="8c9c4-210">Bir Web sayfasındaki sayacı normal şekilde artırma, JavaScript yazma gerektirir, ancak Blazor ile kullanabilirsiniz C#.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-210">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="8c9c4-211">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-211">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="8c9c4-212">En üstteki `@page` yönergesinde belirtilen şekilde tarayıcıda `/counter` için bir istek, `Counter` bileşeninin içeriğini işlemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-212">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="8c9c4-213">Bileşenler, daha sonra, Kullanıcı arabirimini esnek ve verimli bir şekilde güncelleştirmek için kullanılabilen işleme ağacının bellek içi gösterimine işlenir.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-213">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="8c9c4-214">**Bana tıklama** düğmesi her seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-214">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="8c9c4-215">`onclick` olayı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-215">The `onclick` event is fired.</span></span>
* <span data-ttu-id="8c9c4-216">`IncrementCount` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-216">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="8c9c4-217">`currentCount` artırılır.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-217">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="8c9c4-218">Bileşen yeniden işlenir.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-218">The component is rendered again.</span></span>

<span data-ttu-id="8c9c4-219">Çalışma zamanı, yeni içeriği önceki içerikle karşılaştırır ve yalnızca değiştirilen içeriği Belge Nesne Modeli (DOM) öğesine uygular.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-219">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="8c9c4-220">HTML sözdizimini kullanarak başka bir bileşene bileşen ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-220">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="8c9c4-221">Örneğin, `Index` bileşenine bir `<Counter />` öğesi ekleyerek uygulamanın giriş sayfasına `Counter` bileşenini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-221">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="8c9c4-222">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-222">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="8c9c4-223">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-223">Run the app.</span></span> <span data-ttu-id="8c9c4-224">Giriş sayfasının `Counter` bileşeni tarafından sağlanmış kendi sayacı vardır.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-224">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="8c9c4-225">Bileşen parametreleri, alt bileşende özellikler ayarlamanıza olanak tanıyan öznitelikler veya [alt içerik](xref:blazor/components#child-content)kullanılarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-225">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="8c9c4-226">`Counter` bileşenine bir parametre eklemek için, bileşenin `@code` bloğunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-226">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="8c9c4-227">`[Parameter]` özniteliğiyle `IncrementAmount` için ortak özellik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-227">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="8c9c4-228">`currentCount`değerini artırdığınızda `IncrementAmount` kullanmak için `IncrementCount` yöntemini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-228">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="8c9c4-229">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-229">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="8c9c4-230">Bir özniteliği kullanarak `Index` bileşeninin `<Counter>` öğesinde `IncrementAmount` belirtin.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-230">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="8c9c4-231">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="8c9c4-231">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="8c9c4-232">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-232">Run the app.</span></span> <span data-ttu-id="8c9c4-233">`Index` bileşeni, **bana tıklama** düğmesi seçildiğinde her seferinde on ile artan kendi sayacıdır.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-233">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="8c9c4-234">`/counter` `Counter` bileşeni (*Counter. Razor*), bir tarafından arttırmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="8c9c4-234">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c9c4-235">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="8c9c4-235">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="8c9c4-236">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8c9c4-236">Additional resources</span></span>

* <xref:signalr/introduction>
