---
title: ASP.NET Core Blazor kullanmaya başlama
author: guardrex
description: Seçtiğiniz araç ile Blazor bir uygulama oluşturarak Blazor kullanmaya başlayın.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 89c7529d2b8ec97db731f7c7268e19937c398115
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083245"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="17c0a-103">ASP.NET Core Blazor kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="17c0a-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="17c0a-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="17c0a-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="17c0a-105">Blazor kullanmaya başlama:</span><span class="sxs-lookup"><span data-stu-id="17c0a-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="17c0a-106">[.NET Core 3,1 SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.</span><span class="sxs-lookup"><span data-stu-id="17c0a-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="17c0a-107">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu isteğe bağlı olarak yükler:</span><span class="sxs-lookup"><span data-stu-id="17c0a-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="17c0a-108">[.NET Core 3.1.102 veya üzeri (Önizleme) SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.</span><span class="sxs-lookup"><span data-stu-id="17c0a-108">Install the [.NET Core 3.1.102 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="17c0a-109">Komut kabuğu 'nda aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="17c0a-109">Run the following command in a command shell.</span></span> <span data-ttu-id="17c0a-110">[Microsoft. AspNetCore. components. WebAssembly. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) paketinin önizleme sürümü vardır, ancak Blazor WebAssembly önizlemededir.</span><span class="sxs-lookup"><span data-stu-id="17c0a-110">The [Microsoft.AspNetCore.Components.WebAssembly.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview2.20160.5
   ```

   > [!NOTE]
   > <span data-ttu-id="17c0a-111">3,2 Preview 2 Blazor WebAssembly şablonunu kullanmak için .NET Core SDK Version 3.1.102 veya üzeri **gereklidir** .</span><span class="sxs-lookup"><span data-stu-id="17c0a-111">.NET Core SDK version 3.1.102 or later is **required** to use the 3.2 Preview 2 Blazor WebAssembly template.</span></span> <span data-ttu-id="17c0a-112">Bir komut kabuğunda `dotnet --version` çalıştırarak yüklü .NET Core SDK sürümünü onaylayın.</span><span class="sxs-lookup"><span data-stu-id="17c0a-112">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="17c0a-113">Araç seçiminiz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="17c0a-113">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studio"></a>[<span data-ttu-id="17c0a-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="17c0a-114">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="17c0a-115">1 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-115">1\.</span></span> <span data-ttu-id="17c0a-116">**ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019 sürüm 16,4 veya üstünü yükledikten sonra](https://visualstudio.microsoft.com/vs/preview/) .</span><span class="sxs-lookup"><span data-stu-id="17c0a-116">Install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="17c0a-117">2 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-117">2\.</span></span> <span data-ttu-id="17c0a-118">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="17c0a-118">Create a new project.</span></span>

   <span data-ttu-id="17c0a-119">3 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-119">3\.</span></span> <span data-ttu-id="17c0a-120">**Blazor uygulamasını**seçin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-120">Select **Blazor App**.</span></span> <span data-ttu-id="17c0a-121">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-121">Select **Next**.</span></span>

   <span data-ttu-id="17c0a-122">4 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-122">4\.</span></span> <span data-ttu-id="17c0a-123">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-123">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="17c0a-124">**Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-124">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="17c0a-125">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-125">Select **Create**.</span></span>

   <span data-ttu-id="17c0a-126">5 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-126">5\.</span></span> <span data-ttu-id="17c0a-127">Blazor WebAssembly deneyimi için **Blazor Webassembly uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-127">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="17c0a-128">Blazor sunucu deneyimi için **Blazor Server uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-128">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="17c0a-129">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-129">Select **Create**.</span></span> <span data-ttu-id="17c0a-130">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="17c0a-130">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span> <span data-ttu-id="17c0a-131">Blazor WebAssembly şablonu yoksa, önceki adıma dönün ve şablonu yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-131">If the Blazor WebAssembly template isn't present, return to the previous step and reinstall the template.</span></span>

   <span data-ttu-id="17c0a-132">6 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-132">6\.</span></span> <span data-ttu-id="17c0a-133">Uygulamayı çalıştırmak için **Ctrl**+**F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="17c0a-133">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="17c0a-134">ASP.NET Core Blazor 'nin önceki bir önizleme sürümü için Blazor Visual Studio uzantısı 'nı yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="17c0a-134">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="17c0a-135">Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzey için yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="17c0a-135">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-code"></a>[<span data-ttu-id="17c0a-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="17c0a-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="17c0a-137">1 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-137">1\.</span></span> <span data-ttu-id="17c0a-138">[Visual Studio Code](https://code.visualstudio.com/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="17c0a-138">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="17c0a-139">2 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-139">2\.</span></span> <span data-ttu-id="17c0a-140">En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)yükler.</span><span class="sxs-lookup"><span data-stu-id="17c0a-140">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span></span>

   <span data-ttu-id="17c0a-141">3 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-141">3\.</span></span> <span data-ttu-id="17c0a-142">Bir Blazor WebAssembly deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="17c0a-142">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="17c0a-143">Bir Blazor sunucu deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="17c0a-143">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="17c0a-144">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="17c0a-144">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="17c0a-145">4 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-145">4\.</span></span> <span data-ttu-id="17c0a-146">Visual Studio Code 'de *WebApplication1* klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="17c0a-146">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="17c0a-147">5 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-147">5\.</span></span> <span data-ttu-id="17c0a-148">Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister.</span><span class="sxs-lookup"><span data-stu-id="17c0a-148">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="17c0a-149">**Evet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-149">Select **Yes**.</span></span>

   <span data-ttu-id="17c0a-150">6 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-150">6\.</span></span> <span data-ttu-id="17c0a-151">Bir Blazor Server uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="17c0a-151">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="17c0a-152">Blazor WebAssembly uygulaması kullanılıyorsa, uygulamanın proje klasöründen `dotnet run` yürütün.</span><span class="sxs-lookup"><span data-stu-id="17c0a-152">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="17c0a-153">7 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-153">7\.</span></span> <span data-ttu-id="17c0a-154">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mac"></a>[<span data-ttu-id="17c0a-155">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="17c0a-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="17c0a-156">1 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-156">1\.</span></span> <span data-ttu-id="17c0a-157">[Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="17c0a-157">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="17c0a-158">2 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-158">2\.</span></span> <span data-ttu-id="17c0a-159">**Yeni çözüm** > **Dosya** seçin veya yeni bir **Proje**oluşturun.</span><span class="sxs-lookup"><span data-stu-id="17c0a-159">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="17c0a-160">3 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-160">3\.</span></span> <span data-ttu-id="17c0a-161">Kenar çubuğunda **.NET Core** > **uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-161">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="17c0a-162">4 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-162">4\.</span></span> <span data-ttu-id="17c0a-163">**Blazor Server uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-163">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="17c0a-164">Şu anda yalnızca Blazor Server şablonu Mac için Visual Studio kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="17c0a-164">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="17c0a-165">Blazor WebAssembly deneyimi için **.NET Core CLI** sekmesindeki yönergeleri izleyin. Blazor sunucu şablonunu seçtikten sonra **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-165">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="17c0a-166">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="17c0a-166">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="17c0a-167">5 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-167">5\.</span></span> <span data-ttu-id="17c0a-168">**Hedef Framework 'ü** **.NET Core 3,1** olarak ayarlayın ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-168">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="17c0a-169">6 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-169">6\.</span></span> <span data-ttu-id="17c0a-170">**Proje adı** alanında, uygulamayı `WebApplication1`olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="17c0a-170">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="17c0a-171">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-171">Select **Create**.</span></span>

   <span data-ttu-id="17c0a-172">7 \.</span><span class="sxs-lookup"><span data-stu-id="17c0a-172">7\.</span></span> <span data-ttu-id="17c0a-173">Uygulamayı *hata ayıklayıcısı olmadan*çalıştırmak Için **hata ayıklama olmadan** **Çalıştır > Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-173">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="17c0a-174">Uygulamayı hata *ayıklayıcıyla*çalıştırmak Için, **hata ayıklamayı Başlat** ile uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="17c0a-174">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="17c0a-175">Geliştirme sertifikasına güvenmek için bir istem görünürse, sertifikaya güvenin ve devam edin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-175">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-cli"></a>[<span data-ttu-id="17c0a-176">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="17c0a-176">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="17c0a-177">Bir Blazor Weelsembly deneyimi için komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="17c0a-177">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="17c0a-178">Bir Blazor sunucu deneyimi için aşağıdaki komutları bir komut kabuğunda yürütün:</span><span class="sxs-lookup"><span data-stu-id="17c0a-178">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="17c0a-179">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="17c0a-179">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="17c0a-180">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-180">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="17c0a-181">Kenar çubuğu 'ndaki sekmelerde birden çok sayfa mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="17c0a-181">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="17c0a-182">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="17c0a-182">Home</span></span>
* <span data-ttu-id="17c0a-183">Sayaç</span><span class="sxs-lookup"><span data-stu-id="17c0a-183">Counter</span></span>
* <span data-ttu-id="17c0a-184">Verileri getir</span><span class="sxs-lookup"><span data-stu-id="17c0a-184">Fetch data</span></span>

<span data-ttu-id="17c0a-185">Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-185">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="17c0a-186">Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak kullanabilirsiniz C#Blazor.</span><span class="sxs-lookup"><span data-stu-id="17c0a-186">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="17c0a-187">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="17c0a-187">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="17c0a-188">Tarayıcıda `/counter` için bir istek, en üstteki `@page` yönergesi tarafından belirtilen şekilde `Counter` bileşeninin içeriğini işlemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="17c0a-188">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="17c0a-189">Bileşenler, daha sonra, Kullanıcı arabirimini esnek ve verimli bir şekilde güncelleştirmek için kullanılabilen işleme ağacının bellek içi gösterimine işlenir.</span><span class="sxs-lookup"><span data-stu-id="17c0a-189">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="17c0a-190">**Bana tıklama** düğmesi her seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="17c0a-190">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="17c0a-191">`onclick` olayı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="17c0a-191">The `onclick` event is fired.</span></span>
* <span data-ttu-id="17c0a-192">`IncrementCount` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="17c0a-192">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="17c0a-193">`currentCount` artırılır.</span><span class="sxs-lookup"><span data-stu-id="17c0a-193">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="17c0a-194">Bileşen yeniden işlenir.</span><span class="sxs-lookup"><span data-stu-id="17c0a-194">The component is rendered again.</span></span>

<span data-ttu-id="17c0a-195">Çalışma zamanı, yeni içeriği önceki içerikle karşılaştırır ve yalnızca değiştirilen içeriği Belge Nesne Modeli (DOM) öğesine uygular.</span><span class="sxs-lookup"><span data-stu-id="17c0a-195">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="17c0a-196">HTML sözdizimini kullanarak başka bir bileşene bileşen ekleyin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-196">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="17c0a-197">Örneğin, `Index` bileşenine bir `<Counter />` öğesi ekleyerek `Counter` bileşenini uygulamanın giriş sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-197">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="17c0a-198">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="17c0a-198">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="17c0a-199">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="17c0a-199">Run the app.</span></span> <span data-ttu-id="17c0a-200">Giriş sayfasının `Counter` bileşeni tarafından kendi sayacı vardır.</span><span class="sxs-lookup"><span data-stu-id="17c0a-200">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="17c0a-201">Bileşen parametreleri, alt bileşende özellikler ayarlamanıza olanak tanıyan öznitelikler veya [alt içerik](xref:blazor/components#child-content)kullanılarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="17c0a-201">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="17c0a-202">`Counter` bileşenine bir parametre eklemek için, bileşenin `@code` bloğunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="17c0a-202">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="17c0a-203">`[Parameter]` özniteliğiyle `IncrementAmount` için ortak özellik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-203">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="17c0a-204">`currentCount`değerini artırdığınızda `IncrementAmount` kullanmak için `IncrementCount` yöntemini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-204">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="17c0a-205">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="17c0a-205">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="17c0a-206">`Index` bileşenin `<Counter>` öğesindeki bir özniteliği kullanarak `IncrementAmount` belirtin.</span><span class="sxs-lookup"><span data-stu-id="17c0a-206">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="17c0a-207">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="17c0a-207">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="17c0a-208">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="17c0a-208">Run the app.</span></span> <span data-ttu-id="17c0a-209">`Index` bileşeni, **bana tıklama** düğmesi seçildiğinde her seferinde on ile artan kendi sayacıdır.</span><span class="sxs-lookup"><span data-stu-id="17c0a-209">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="17c0a-210">`/counter` `Counter` bileşeni (*Counter. Razor*), bir tarafından arttırmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="17c0a-210">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17c0a-211">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="17c0a-211">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="17c0a-212">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="17c0a-212">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
