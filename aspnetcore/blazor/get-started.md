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
ms.openlocfilehash: 2135c2a090d60ec7a46fa4f899f0f14987b6b4e0
ms.sourcegitcommit: 2388c2a7334ce66b6be3ffbab06dd7923df18f60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75951719"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="5b2c9-103">ASP.NET Core Blazor kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="5b2c9-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="5b2c9-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="5b2c9-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="5b2c9-105">Blazorkullanmaya başlayın:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="5b2c9-106">[.NET Core 3,1 SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="5b2c9-107">İsteğe bağlı olarak [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu yükler:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="5b2c9-108">[.NET Core 3,1 veya üzeri (Önizleme) SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="5b2c9-109">Komut kabuğu 'nda aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-109">Run the following command in a command shell.</span></span> <span data-ttu-id="5b2c9-110">[Microsoft.AspNetCore.Blazor.Şablon](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) paketinin önizleme sürümü varsa Blazor WebAssembly önizleme aşamasındadır.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="5b2c9-111">Araç seçiminiz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5b2c9-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b2c9-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="5b2c9-113">1 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-113">1\.</span></span> <span data-ttu-id="5b2c9-114">**ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 16,4 veya üstünü yükledikten sonra](https://visualstudio.microsoft.com/vs/preview/) .</span><span class="sxs-lookup"><span data-stu-id="5b2c9-114">Install [Visual Studio 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="5b2c9-115">2 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-115">2\.</span></span> <span data-ttu-id="5b2c9-116">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-116">Create a new project.</span></span>

   <span data-ttu-id="5b2c9-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-117">3\.</span></span> <span data-ttu-id="5b2c9-118">**Blazor uygulama**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-118">Select **Blazor App**.</span></span> <span data-ttu-id="5b2c9-119">**İleri**'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-119">Select **Next**.</span></span>

   <span data-ttu-id="5b2c9-120">4 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-120">4\.</span></span> <span data-ttu-id="5b2c9-121">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="5b2c9-122">**Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="5b2c9-123">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-123">Select **Create**.</span></span>

   <span data-ttu-id="5b2c9-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-124">5\.</span></span> <span data-ttu-id="5b2c9-125">Blazor Weelsembly deneyimi için, **Webassembly uygulama şablonunuBlazor** seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="5b2c9-126">Blazor sunucusu deneyimi için **Blazor sunucusu uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="5b2c9-127">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-127">Select **Create**.</span></span> <span data-ttu-id="5b2c9-128">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="5b2c9-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-129">6\.</span></span> <span data-ttu-id="5b2c9-130">Uygulamayı çalıştırmak için **Ctrl**+**F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5b2c9-131">Blazor Visual Studio uzantısını ASP.NET Core Blazor önceki bir önizleme sürümü için yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="5b2c9-132">Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzeye eklemek yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5b2c9-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5b2c9-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="5b2c9-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-134">1\.</span></span> <span data-ttu-id="5b2c9-135">[Visual Studio Code](https://code.visualstudio.com/)’u yükleyin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="5b2c9-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-136">2\.</span></span> <span data-ttu-id="5b2c9-137">En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="5b2c9-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-138">3\.</span></span> <span data-ttu-id="5b2c9-139">Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="5b2c9-140">Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="5b2c9-141">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="5b2c9-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-142">4\.</span></span> <span data-ttu-id="5b2c9-143">Visual Studio Code 'de *WebApplication1* klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="5b2c9-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-144">5\.</span></span> <span data-ttu-id="5b2c9-145">Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="5b2c9-146">**Evet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-146">Select **Yes**.</span></span>

   <span data-ttu-id="5b2c9-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-147">6\.</span></span> <span data-ttu-id="5b2c9-148">Blazor sunucusu uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="5b2c9-149">Blazor WebAssembly uygulaması kullanıyorsanız, uygulamanın proje klasöründen `dotnet run` yürütün.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="5b2c9-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-150">7\.</span></span> <span data-ttu-id="5b2c9-151">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5b2c9-152">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b2c9-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="5b2c9-153">1 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-153">1\.</span></span> <span data-ttu-id="5b2c9-154">[Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="5b2c9-155">2 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-155">2\.</span></span> <span data-ttu-id="5b2c9-156">**Yeni çözüm** > **Dosya** seçin veya yeni bir **Proje**oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="5b2c9-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-157">3\.</span></span> <span data-ttu-id="5b2c9-158">Kenar çubuğunda **.NET Core** > **uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="5b2c9-159">4 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-159">4\.</span></span> <span data-ttu-id="5b2c9-160">**Blazor sunucusu uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="5b2c9-161">Şu anda yalnızca Blazor sunucusu şablonu Mac için Visual Studio kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="5b2c9-162">Blazor Weelsembly deneyimi için **.NET Core CLI** sekmesindeki yönergeleri izleyin. Blazor sunucusu şablonunu seçtikten sonra **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="5b2c9-163">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="5b2c9-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-164">5\.</span></span> <span data-ttu-id="5b2c9-165">**Hedef Framework 'ü** **.NET Core 3,1** olarak ayarlayın ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="5b2c9-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-166">6\.</span></span> <span data-ttu-id="5b2c9-167">**Proje adı** alanında, uygulamayı `WebApplication1`olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="5b2c9-168">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-168">Select **Create**.</span></span>

   <span data-ttu-id="5b2c9-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-169">7\.</span></span> <span data-ttu-id="5b2c9-170">Uygulamayı *hata ayıklayıcısı olmadan*çalıştırmak Için **hata ayıklama olmadan** **Çalıştır > Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="5b2c9-171">Uygulamayı hata *ayıklayıcıyla*çalıştırmak Için, **hata ayıklamayı Başlat** ile uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="5b2c9-172">Geliştirme sertifikasına güvenmek için bir istem görünürse, sertifikaya güvenin ve devam edin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-172">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5b2c9-173">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5b2c9-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="5b2c9-174">Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="5b2c9-175">Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="5b2c9-176">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="5b2c9-177">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="5b2c9-178">En son [.NET Core 3,0 SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.0)yükler.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-178">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

1. <span data-ttu-id="5b2c9-179">İsteğe bağlı olarak [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu yükler:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-179">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="5b2c9-180">[.NET Core 3,1 veya üzeri (Önizleme) SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-180">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="5b2c9-181">Komut kabuğu 'nda aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-181">Run the following command in a command shell.</span></span> <span data-ttu-id="5b2c9-182">[Microsoft.AspNetCore.Blazor.Şablon](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) paketinin önizleme sürümü varsa Blazor WebAssembly önizleme aşamasındadır.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-182">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="5b2c9-183">Araç seçiminiz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-183">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5b2c9-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b2c9-184">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="5b2c9-185">1 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-185">1\.</span></span> <span data-ttu-id="5b2c9-186">**ASP.net ve Web geliştirme** iş yüküyle en son [Visual Studio 'yu](https://visualstudio.com/vs/) yükler.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-186">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="5b2c9-187">2 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-187">2\.</span></span> <span data-ttu-id="5b2c9-188">İsteğe bağlı olarak [Visual Studio 16,4 Preview 2 veya üstünü](https://visualstudio.microsoft.com/vs/preview/) , **ASP.net ve Web geliştirme** Iş yüküyle Blazor webassembly uygulama geliştirmesi ile birlikte yükler.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-188">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="5b2c9-189">3 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-189">3\.</span></span> <span data-ttu-id="5b2c9-190">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-190">Create a new project.</span></span>

   <span data-ttu-id="5b2c9-191">4 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-191">4\.</span></span> <span data-ttu-id="5b2c9-192">**Blazor uygulama**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-192">Select **Blazor App**.</span></span> <span data-ttu-id="5b2c9-193">**İleri**'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-193">Select **Next**.</span></span>

   <span data-ttu-id="5b2c9-194">5 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-194">5\.</span></span> <span data-ttu-id="5b2c9-195">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-195">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="5b2c9-196">**Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-196">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="5b2c9-197">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-197">Select **Create**.</span></span>

   <span data-ttu-id="5b2c9-198">6 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-198">6\.</span></span> <span data-ttu-id="5b2c9-199">Blazor Weelsembly deneyimi için, **Webassembly uygulama şablonunuBlazor** seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-199">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="5b2c9-200">Blazor sunucusu deneyimi için **Blazor sunucusu uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-200">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="5b2c9-201">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-201">Select **Create**.</span></span> <span data-ttu-id="5b2c9-202">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-202">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="5b2c9-203">7 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-203">7\.</span></span> <span data-ttu-id="5b2c9-204">Uygulamayı çalıştırmak için **F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-204">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5b2c9-205">Blazor Visual Studio uzantısını ASP.NET Core Blazor önceki bir önizleme sürümü için yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-205">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="5b2c9-206">Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzeye eklemek yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-206">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5b2c9-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5b2c9-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="5b2c9-208">1 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-208">1\.</span></span> <span data-ttu-id="5b2c9-209">[Visual Studio Code](https://code.visualstudio.com/)’u yükleyin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-209">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="5b2c9-210">2 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-210">2\.</span></span> <span data-ttu-id="5b2c9-211">En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-211">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="5b2c9-212">3 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-212">3\.</span></span> <span data-ttu-id="5b2c9-213">Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-213">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="5b2c9-214">Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-214">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="5b2c9-215">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-215">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="5b2c9-216">4 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-216">4\.</span></span> <span data-ttu-id="5b2c9-217">Visual Studio Code 'de *WebApplication1* klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-217">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="5b2c9-218">5 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-218">5\.</span></span> <span data-ttu-id="5b2c9-219">Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-219">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="5b2c9-220">**Evet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-220">Select **Yes**.</span></span>

   <span data-ttu-id="5b2c9-221">6 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-221">6\.</span></span> <span data-ttu-id="5b2c9-222">Blazor sunucusu uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-222">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="5b2c9-223">Blazor WebAssembly uygulaması kullanıyorsanız, uygulamanın proje klasöründen `dotnet run` yürütün.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-223">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="5b2c9-224">7 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-224">7\.</span></span> <span data-ttu-id="5b2c9-225">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-225">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5b2c9-226">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b2c9-226">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="5b2c9-227">1 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-227">1\.</span></span> <span data-ttu-id="5b2c9-228">[Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-228">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="5b2c9-229">[Güncelleştirme kanalını önizlemeye](/visualstudio/mac/install-preview)geçirin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-229">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="5b2c9-230">2 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-230">2\.</span></span> <span data-ttu-id="5b2c9-231">**Yeni çözüm** > **Dosya** seçin veya yeni bir **Proje**oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-231">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="5b2c9-232">3 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-232">3\.</span></span> <span data-ttu-id="5b2c9-233">Kenar çubuğunda **.NET Core** > **uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-233">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="5b2c9-234">4 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-234">4\.</span></span> <span data-ttu-id="5b2c9-235">**Blazor sunucusu uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-235">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="5b2c9-236">Şu anda yalnızca Blazor sunucusu şablonu Mac için Visual Studio kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-236">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="5b2c9-237">Blazor Weelsembly deneyimi için **.NET Core CLI** sekmesindeki yönergeleri izleyin. Blazor sunucusu şablonunu seçtikten sonra **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-237">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="5b2c9-238">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-238">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="5b2c9-239">5 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-239">5\.</span></span> <span data-ttu-id="5b2c9-240">**Hedef Framework 'ü** **.NET Core 3,0** olarak ayarlayın ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-240">Set the **Target Framework** to **.NET Core 3.0** and select **Next**.</span></span>

   <span data-ttu-id="5b2c9-241">6 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-241">6\.</span></span> <span data-ttu-id="5b2c9-242">**Proje adı** alanında, uygulamayı `WebApplication1`olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-242">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="5b2c9-243">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-243">Select **Create**.</span></span>

   <span data-ttu-id="5b2c9-244">7 \.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-244">7\.</span></span> <span data-ttu-id="5b2c9-245">Uygulamayı *hata ayıklayıcısı olmadan*çalıştırmak Için **hata ayıklama olmadan** **Çalıştır > Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-245">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="5b2c9-246">Uygulamayı hata *ayıklayıcıyla*çalıştırmak Için, **hata ayıklamayı Başlat** ile uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-246">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5b2c9-247">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5b2c9-247">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="5b2c9-248">Blazor Weelsembly deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-248">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="5b2c9-249">Blazor sunucusu deneyimi için, komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-249">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="5b2c9-250">Blazor, *Blazor sunucusu* ve *Blazor webassembly*'yi barındıran iki hakkında bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-250">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="5b2c9-251">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-251">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="5b2c9-252">Kenar çubuğu 'ndaki sekmelerde birden çok sayfa mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-252">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="5b2c9-253">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="5b2c9-253">Home</span></span>
* <span data-ttu-id="5b2c9-254">Sayaç</span><span class="sxs-lookup"><span data-stu-id="5b2c9-254">Counter</span></span>
* <span data-ttu-id="5b2c9-255">Verileri getir</span><span class="sxs-lookup"><span data-stu-id="5b2c9-255">Fetch data</span></span>

<span data-ttu-id="5b2c9-256">Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-256">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="5b2c9-257">Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak kullanabilirsiniz C#Blazor.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-257">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="5b2c9-258">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-258">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="5b2c9-259">Tarayıcıda `/counter` için bir istek, en üstteki `@page` yönergesi tarafından belirtilen şekilde `Counter` bileşeninin içeriğini işlemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-259">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="5b2c9-260">Bileşenler, daha sonra, Kullanıcı arabirimini esnek ve verimli bir şekilde güncelleştirmek için kullanılabilen işleme ağacının bellek içi gösterimine işlenir.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-260">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="5b2c9-261">**Bana tıklama** düğmesi her seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-261">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="5b2c9-262">`onclick` olayı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-262">The `onclick` event is fired.</span></span>
* <span data-ttu-id="5b2c9-263">`IncrementCount` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-263">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="5b2c9-264">`currentCount` artırılır.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-264">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="5b2c9-265">Bileşen yeniden işlenir.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-265">The component is rendered again.</span></span>

<span data-ttu-id="5b2c9-266">Çalışma zamanı, yeni içeriği önceki içerikle karşılaştırır ve yalnızca değiştirilen içeriği Belge Nesne Modeli (DOM) öğesine uygular.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-266">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="5b2c9-267">HTML sözdizimini kullanarak başka bir bileşene bileşen ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-267">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="5b2c9-268">Örneğin, `Index` bileşenine bir `<Counter />` öğesi ekleyerek `Counter` bileşenini uygulamanın giriş sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-268">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="5b2c9-269">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-269">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="5b2c9-270">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-270">Run the app.</span></span> <span data-ttu-id="5b2c9-271">Giriş sayfasının `Counter` bileşeni tarafından kendi sayacı vardır.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-271">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="5b2c9-272">Bileşen parametreleri, alt bileşende özellikler ayarlamanıza olanak tanıyan öznitelikler veya [alt içerik](xref:blazor/components#child-content)kullanılarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-272">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="5b2c9-273">`Counter` bileşenine bir parametre eklemek için, bileşenin `@code` bloğunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-273">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="5b2c9-274">`[Parameter]` özniteliğiyle `IncrementAmount` için ortak özellik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-274">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="5b2c9-275">`currentCount`değerini artırdığınızda `IncrementAmount` kullanmak için `IncrementCount` yöntemini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-275">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="5b2c9-276">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-276">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="5b2c9-277">`Index` bileşenin `<Counter>` öğesindeki bir özniteliği kullanarak `IncrementAmount` belirtin.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-277">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="5b2c9-278">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="5b2c9-278">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="5b2c9-279">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-279">Run the app.</span></span> <span data-ttu-id="5b2c9-280">`Index` bileşeni, **bana tıklama** düğmesi seçildiğinde her seferinde on ile artan kendi sayacıdır.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-280">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="5b2c9-281">`/counter` `Counter` bileşeni (*Counter. Razor*), bir tarafından arttırmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="5b2c9-281">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b2c9-282">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="5b2c9-282">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="5b2c9-283">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5b2c9-283">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
