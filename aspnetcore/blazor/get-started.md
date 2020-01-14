---
title: ASP.NET Core Blazor kullanmaya başlama
author: guardrex
description: Seçtiğiniz araç ile Blazor bir uygulama oluşturarak Blazor kullanmaya başlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/09/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: 554f4daff92a0839ee7679287a4618e9b51e0fe5
ms.sourcegitcommit: 925cdbd94613243f33bc7613a62ea34006219931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2020
ms.locfileid: "75921303"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="12bd0-103">ASP.NET Core Blazor kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="12bd0-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="12bd0-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="12bd0-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="12bd0-105">Blazorkullanmaya başlayın:</span><span class="sxs-lookup"><span data-stu-id="12bd0-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="12bd0-106">[.NET Core 3,1 SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.</span><span class="sxs-lookup"><span data-stu-id="12bd0-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="12bd0-107">İsteğe bağlı olarak [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu yükler:</span><span class="sxs-lookup"><span data-stu-id="12bd0-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="12bd0-108">[.NET Core 3,1 veya üzeri (Önizleme) SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.</span><span class="sxs-lookup"><span data-stu-id="12bd0-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="12bd0-109">Komut kabuğu 'nda aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="12bd0-109">Run the following command in a command shell.</span></span> <span data-ttu-id="12bd0-110">[Microsoft.AspNetCore.Blazor.Şablon](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) paketinin önizleme sürümü varsa Blazor WebAssembly önizleme aşamasındadır.</span><span class="sxs-lookup"><span data-stu-id="12bd0-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="12bd0-111">Araç seçiminiz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="12bd0-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="12bd0-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12bd0-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="12bd0-113">1 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-113">1\.</span></span> <span data-ttu-id="12bd0-114">**ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 16,4 veya üstünü yükledikten sonra](https://visualstudio.microsoft.com/vs/preview/) .</span><span class="sxs-lookup"><span data-stu-id="12bd0-114">Install [Visual Studio 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="12bd0-115">2 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-115">2\.</span></span> <span data-ttu-id="12bd0-116">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="12bd0-116">Create a new project.</span></span>

   <span data-ttu-id="12bd0-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-117">3\.</span></span> <span data-ttu-id="12bd0-118">**Blazor uygulama**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-118">Select **Blazor App**.</span></span> <span data-ttu-id="12bd0-119">**İleri**'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-119">Select **Next**.</span></span>

   <span data-ttu-id="12bd0-120">4 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-120">4\.</span></span> <span data-ttu-id="12bd0-121">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="12bd0-122">**Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="12bd0-123">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="12bd0-123">Select **Create**.</span></span>

   <span data-ttu-id="12bd0-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-124">5\.</span></span> <span data-ttu-id="12bd0-125">Blazor Weelsembly deneyimi için, **Webassembly uygulama şablonunuBlazor** seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="12bd0-126">Blazor sunucusu deneyimi için **Blazor sunucusu uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="12bd0-127">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="12bd0-127">Select **Create**.</span></span> <span data-ttu-id="12bd0-128">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="12bd0-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="12bd0-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-129">6\.</span></span> <span data-ttu-id="12bd0-130">Uygulamayı çalıştırmak için **Ctrl**+**F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="12bd0-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="12bd0-131">Blazor Visual Studio uzantısını ASP.NET Core Blazor önceki bir önizleme sürümü için yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="12bd0-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="12bd0-132">Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzeye eklemek yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="12bd0-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="12bd0-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="12bd0-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="12bd0-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-134">1\.</span></span> <span data-ttu-id="12bd0-135">[Visual Studio Code](https://code.visualstudio.com/)’u yükleyin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="12bd0-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-136">2\.</span></span> <span data-ttu-id="12bd0-137">En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.</span><span class="sxs-lookup"><span data-stu-id="12bd0-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="12bd0-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-138">3\.</span></span> <span data-ttu-id="12bd0-139">Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="12bd0-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="12bd0-140">Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="12bd0-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="12bd0-141">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="12bd0-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="12bd0-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-142">4\.</span></span> <span data-ttu-id="12bd0-143">Visual Studio Code 'de *WebApplication1* klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="12bd0-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="12bd0-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-144">5\.</span></span> <span data-ttu-id="12bd0-145">Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister.</span><span class="sxs-lookup"><span data-stu-id="12bd0-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="12bd0-146">**Evet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-146">Select **Yes**.</span></span>

   <span data-ttu-id="12bd0-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-147">6\.</span></span> <span data-ttu-id="12bd0-148">Blazor sunucusu uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="12bd0-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="12bd0-149">Blazor WebAssembly uygulaması kullanıyorsanız, uygulamanın proje klasöründen `dotnet run` yürütün.</span><span class="sxs-lookup"><span data-stu-id="12bd0-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="12bd0-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-150">7\.</span></span> <span data-ttu-id="12bd0-151">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="12bd0-152">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12bd0-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="12bd0-153">1 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-153">1\.</span></span> <span data-ttu-id="12bd0-154">[Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="12bd0-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="12bd0-155">2 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-155">2\.</span></span> <span data-ttu-id="12bd0-156">**Yeni çözüm** > **Dosya** seçin veya yeni bir **Proje**oluşturun.</span><span class="sxs-lookup"><span data-stu-id="12bd0-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="12bd0-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-157">3\.</span></span> <span data-ttu-id="12bd0-158">Kenar çubuğunda **.NET Core** > **uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="12bd0-159">4 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-159">4\.</span></span> <span data-ttu-id="12bd0-160">**Blazor sunucusu uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="12bd0-161">Şu anda yalnızca Blazor sunucusu şablonu Mac için Visual Studio kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="12bd0-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="12bd0-162">Blazor Weelsembly deneyimi için **.NET Core CLI** sekmesindeki yönergeleri izleyin. Blazor sunucusu şablonunu seçtikten sonra **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="12bd0-163">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="12bd0-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="12bd0-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-164">5\.</span></span> <span data-ttu-id="12bd0-165">**Hedef Framework 'ü** **.NET Core 3,1** olarak ayarlayın ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="12bd0-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-166">6\.</span></span> <span data-ttu-id="12bd0-167">**Proje adı** alanında, uygulamayı `WebApplication1`olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="12bd0-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="12bd0-168">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="12bd0-168">Select **Create**.</span></span>

   <span data-ttu-id="12bd0-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-169">7\.</span></span> <span data-ttu-id="12bd0-170">Uygulamayı *hata ayıklayıcısı olmadan*çalıştırmak Için **hata ayıklama olmadan** **Çalıştır > Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="12bd0-171">Uygulamayı hata *ayıklayıcıyla*çalıştırmak Için, **hata ayıklamayı Başlat** ile uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="12bd0-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="12bd0-172">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="12bd0-172">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="12bd0-173">Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="12bd0-173">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="12bd0-174">Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="12bd0-174">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="12bd0-175">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="12bd0-175">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="12bd0-176">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-176">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="12bd0-177">En son [.NET Core 3,0 SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.0)yükler.</span><span class="sxs-lookup"><span data-stu-id="12bd0-177">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

1. <span data-ttu-id="12bd0-178">İsteğe bağlı olarak [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu yükler:</span><span class="sxs-lookup"><span data-stu-id="12bd0-178">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="12bd0-179">[.NET Core 3,1 veya üzeri (Önizleme) SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.</span><span class="sxs-lookup"><span data-stu-id="12bd0-179">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="12bd0-180">Komut kabuğu 'nda aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="12bd0-180">Run the following command in a command shell.</span></span> <span data-ttu-id="12bd0-181">[Microsoft.AspNetCore.Blazor.Şablon](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) paketinin önizleme sürümü varsa Blazor WebAssembly önizleme aşamasındadır.</span><span class="sxs-lookup"><span data-stu-id="12bd0-181">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="12bd0-182">Araç seçiminiz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="12bd0-182">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="12bd0-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12bd0-183">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="12bd0-184">1 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-184">1\.</span></span> <span data-ttu-id="12bd0-185">**ASP.net ve Web geliştirme** iş yüküyle en son [Visual Studio 'yu](https://visualstudio.com/vs/) yükler.</span><span class="sxs-lookup"><span data-stu-id="12bd0-185">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="12bd0-186">2 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-186">2\.</span></span> <span data-ttu-id="12bd0-187">İsteğe bağlı olarak [Visual Studio 16,4 Preview 2 veya üstünü](https://visualstudio.microsoft.com/vs/preview/) , **ASP.net ve Web geliştirme** Iş yüküyle Blazor webassembly uygulama geliştirmesi ile birlikte yükler.</span><span class="sxs-lookup"><span data-stu-id="12bd0-187">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="12bd0-188">3 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-188">3\.</span></span> <span data-ttu-id="12bd0-189">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="12bd0-189">Create a new project.</span></span>

   <span data-ttu-id="12bd0-190">4 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-190">4\.</span></span> <span data-ttu-id="12bd0-191">**Blazor uygulama**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-191">Select **Blazor App**.</span></span> <span data-ttu-id="12bd0-192">**İleri**'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-192">Select **Next**.</span></span>

   <span data-ttu-id="12bd0-193">5 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-193">5\.</span></span> <span data-ttu-id="12bd0-194">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-194">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="12bd0-195">**Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-195">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="12bd0-196">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="12bd0-196">Select **Create**.</span></span>

   <span data-ttu-id="12bd0-197">6 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-197">6\.</span></span> <span data-ttu-id="12bd0-198">Blazor Weelsembly deneyimi için, **Webassembly uygulama şablonunuBlazor** seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-198">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="12bd0-199">Blazor sunucusu deneyimi için **Blazor sunucusu uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-199">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="12bd0-200">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="12bd0-200">Select **Create**.</span></span> <span data-ttu-id="12bd0-201">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="12bd0-201">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="12bd0-202">7 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-202">7\.</span></span> <span data-ttu-id="12bd0-203">Uygulamayı çalıştırmak için **F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="12bd0-203">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="12bd0-204">Blazor Visual Studio uzantısını ASP.NET Core Blazor önceki bir önizleme sürümü için yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="12bd0-204">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="12bd0-205">Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzeye eklemek yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="12bd0-205">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="12bd0-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="12bd0-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="12bd0-207">1 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-207">1\.</span></span> <span data-ttu-id="12bd0-208">[Visual Studio Code](https://code.visualstudio.com/)’u yükleyin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-208">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="12bd0-209">2 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-209">2\.</span></span> <span data-ttu-id="12bd0-210">En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.</span><span class="sxs-lookup"><span data-stu-id="12bd0-210">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="12bd0-211">3 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-211">3\.</span></span> <span data-ttu-id="12bd0-212">Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="12bd0-212">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="12bd0-213">Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="12bd0-213">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="12bd0-214">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="12bd0-214">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="12bd0-215">4 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-215">4\.</span></span> <span data-ttu-id="12bd0-216">Visual Studio Code 'de *WebApplication1* klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="12bd0-216">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="12bd0-217">5 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-217">5\.</span></span> <span data-ttu-id="12bd0-218">Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister.</span><span class="sxs-lookup"><span data-stu-id="12bd0-218">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="12bd0-219">**Evet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-219">Select **Yes**.</span></span>

   <span data-ttu-id="12bd0-220">6 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-220">6\.</span></span> <span data-ttu-id="12bd0-221">Blazor sunucusu uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="12bd0-221">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="12bd0-222">Blazor WebAssembly uygulaması kullanıyorsanız, uygulamanın proje klasöründen `dotnet run` yürütün.</span><span class="sxs-lookup"><span data-stu-id="12bd0-222">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="12bd0-223">7 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-223">7\.</span></span> <span data-ttu-id="12bd0-224">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-224">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="12bd0-225">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12bd0-225">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="12bd0-226">1 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-226">1\.</span></span> <span data-ttu-id="12bd0-227">[Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="12bd0-227">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="12bd0-228">[Güncelleştirme kanalını önizlemeye](/visualstudio/mac/install-preview)geçirin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-228">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="12bd0-229">2 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-229">2\.</span></span> <span data-ttu-id="12bd0-230">**Yeni çözüm** > **Dosya** seçin veya yeni bir **Proje**oluşturun.</span><span class="sxs-lookup"><span data-stu-id="12bd0-230">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="12bd0-231">3 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-231">3\.</span></span> <span data-ttu-id="12bd0-232">Kenar çubuğunda **.NET Core** > **uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-232">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="12bd0-233">4 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-233">4\.</span></span> <span data-ttu-id="12bd0-234">**Blazor sunucusu uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-234">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="12bd0-235">Şu anda yalnızca Blazor sunucusu şablonu Mac için Visual Studio kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="12bd0-235">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="12bd0-236">Blazor Weelsembly deneyimi için **.NET Core CLI** sekmesindeki yönergeleri izleyin. Blazor sunucusu şablonunu seçtikten sonra **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-236">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="12bd0-237">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="12bd0-237">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="12bd0-238">5 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-238">5\.</span></span> <span data-ttu-id="12bd0-239">**Hedef Framework 'ü** **.NET Core 3,0** olarak ayarlayın ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-239">Set the **Target Framework** to **.NET Core 3.0** and select **Next**.</span></span>

   <span data-ttu-id="12bd0-240">6 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-240">6\.</span></span> <span data-ttu-id="12bd0-241">**Proje adı** alanında, uygulamayı `WebApplication1`olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="12bd0-241">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="12bd0-242">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="12bd0-242">Select **Create**.</span></span>

   <span data-ttu-id="12bd0-243">7 \.</span><span class="sxs-lookup"><span data-stu-id="12bd0-243">7\.</span></span> <span data-ttu-id="12bd0-244">Uygulamayı *hata ayıklayıcısı olmadan*çalıştırmak Için **hata ayıklama olmadan** **Çalıştır > Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-244">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="12bd0-245">Uygulamayı hata *ayıklayıcıyla*çalıştırmak Için, **hata ayıklamayı Başlat** ile uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="12bd0-245">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="12bd0-246">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="12bd0-246">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="12bd0-247">Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="12bd0-247">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="12bd0-248">Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="12bd0-248">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="12bd0-249">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="12bd0-249">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="12bd0-250">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-250">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="12bd0-251">Kenar çubuğu 'ndaki sekmelerde birden çok sayfa mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="12bd0-251">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="12bd0-252">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="12bd0-252">Home</span></span>
* <span data-ttu-id="12bd0-253">Sayaç</span><span class="sxs-lookup"><span data-stu-id="12bd0-253">Counter</span></span>
* <span data-ttu-id="12bd0-254">Verileri getir</span><span class="sxs-lookup"><span data-stu-id="12bd0-254">Fetch data</span></span>

<span data-ttu-id="12bd0-255">Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-255">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="12bd0-256">Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak kullanabilirsiniz C#Blazor.</span><span class="sxs-lookup"><span data-stu-id="12bd0-256">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="12bd0-257">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="12bd0-257">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="12bd0-258">Tarayıcıda `/counter` için bir istek, en üstteki `@page` yönergesi tarafından belirtilen şekilde `Counter` bileşeninin içeriğini işlemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="12bd0-258">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="12bd0-259">Bileşenler, daha sonra, Kullanıcı arabirimini esnek ve verimli bir şekilde güncelleştirmek için kullanılabilen işleme ağacının bellek içi gösterimine işlenir.</span><span class="sxs-lookup"><span data-stu-id="12bd0-259">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="12bd0-260">**Bana tıklama** düğmesi her seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="12bd0-260">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="12bd0-261">`onclick` olayı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="12bd0-261">The `onclick` event is fired.</span></span>
* <span data-ttu-id="12bd0-262">`IncrementCount` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="12bd0-262">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="12bd0-263">`currentCount` artırılır.</span><span class="sxs-lookup"><span data-stu-id="12bd0-263">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="12bd0-264">Bileşen yeniden işlenir.</span><span class="sxs-lookup"><span data-stu-id="12bd0-264">The component is rendered again.</span></span>

<span data-ttu-id="12bd0-265">Çalışma zamanı, yeni içeriği önceki içerikle karşılaştırır ve yalnızca değiştirilen içeriği Belge Nesne Modeli (DOM) öğesine uygular.</span><span class="sxs-lookup"><span data-stu-id="12bd0-265">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="12bd0-266">HTML sözdizimini kullanarak başka bir bileşene bileşen ekleyin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-266">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="12bd0-267">Örneğin, `Index` bileşenine bir `<Counter />` öğesi ekleyerek `Counter` bileşenini uygulamanın giriş sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-267">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="12bd0-268">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="12bd0-268">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="12bd0-269">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="12bd0-269">Run the app.</span></span> <span data-ttu-id="12bd0-270">Giriş sayfasının `Counter` bileşeni tarafından kendi sayacı vardır.</span><span class="sxs-lookup"><span data-stu-id="12bd0-270">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="12bd0-271">Bileşen parametreleri, alt bileşende özellikler ayarlamanıza olanak tanıyan öznitelikler veya [alt içerik](xref:blazor/components#child-content)kullanılarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="12bd0-271">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="12bd0-272">`Counter` bileşenine bir parametre eklemek için, bileşenin `@code` bloğunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="12bd0-272">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="12bd0-273">`[Parameter]` özniteliğiyle `IncrementAmount` için ortak özellik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-273">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="12bd0-274">`currentCount`değerini artırdığınızda `IncrementAmount` kullanmak için `IncrementCount` yöntemini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-274">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="12bd0-275">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="12bd0-275">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="12bd0-276">`Index` bileşenin `<Counter>` öğesindeki bir özniteliği kullanarak `IncrementAmount` belirtin.</span><span class="sxs-lookup"><span data-stu-id="12bd0-276">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="12bd0-277">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="12bd0-277">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="12bd0-278">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="12bd0-278">Run the app.</span></span> <span data-ttu-id="12bd0-279">`Index` bileşeni, **bana tıklama** düğmesi seçildiğinde her seferinde on ile artan kendi sayacıdır.</span><span class="sxs-lookup"><span data-stu-id="12bd0-279">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="12bd0-280">`/counter` `Counter` bileşeni (*Counter. Razor*), bir tarafından arttırmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="12bd0-280">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12bd0-281">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="12bd0-281">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="12bd0-282">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="12bd0-282">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
