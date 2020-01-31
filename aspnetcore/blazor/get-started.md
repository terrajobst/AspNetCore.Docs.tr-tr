---
title: ASP.NET Core Blazor kullanmaya başlama
author: guardrex
description: Seçtiğiniz araç ile Blazor bir uygulama oluşturarak Blazor kullanmaya başlayın.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2019
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: bd33d874b3d6122f2ab820e9b147b0e62ba03a58
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869586"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="7788b-103">ASP.NET Core Blazor kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="7788b-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="7788b-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="7788b-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="7788b-105">Blazor kullanmaya başlama:</span><span class="sxs-lookup"><span data-stu-id="7788b-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="7788b-106">[.NET Core 3,1 SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.</span><span class="sxs-lookup"><span data-stu-id="7788b-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="7788b-107">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) şablonunu isteğe bağlı olarak yükler:</span><span class="sxs-lookup"><span data-stu-id="7788b-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="7788b-108">[.NET Core 3,1 veya üzeri (Önizleme) SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.</span><span class="sxs-lookup"><span data-stu-id="7788b-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="7788b-109">Komut kabuğu 'nda aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7788b-109">Run the following command in a command shell.</span></span> <span data-ttu-id="7788b-110">[Microsoft. AspNetCore. Blazor. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) paketinin önizleme sürümü vardır, ancak Blazor WebAssembly önizlemededir.</span><span class="sxs-lookup"><span data-stu-id="7788b-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.2.0-preview1.20073.1
   ```

1. <span data-ttu-id="7788b-111">Araç seçiminiz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="7788b-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7788b-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7788b-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="7788b-113">1 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-113">1\.</span></span> <span data-ttu-id="7788b-114">**ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019 sürüm 16,4 veya üstünü yükledikten sonra](https://visualstudio.microsoft.com/vs/preview/) .</span><span class="sxs-lookup"><span data-stu-id="7788b-114">Install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="7788b-115">2 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-115">2\.</span></span> <span data-ttu-id="7788b-116">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7788b-116">Create a new project.</span></span>

   <span data-ttu-id="7788b-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-117">3\.</span></span> <span data-ttu-id="7788b-118">**Blazor uygulamasını**seçin.</span><span class="sxs-lookup"><span data-stu-id="7788b-118">Select **Blazor App**.</span></span> <span data-ttu-id="7788b-119">**İleri**'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="7788b-119">Select **Next**.</span></span>

   <span data-ttu-id="7788b-120">4 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-120">4\.</span></span> <span data-ttu-id="7788b-121">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="7788b-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="7788b-122">**Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="7788b-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="7788b-123">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="7788b-123">Select **Create**.</span></span>

   <span data-ttu-id="7788b-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-124">5\.</span></span> <span data-ttu-id="7788b-125">Blazor WebAssembly deneyimi için **Blazor Webassembly uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="7788b-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="7788b-126">Blazor sunucu deneyimi için **Blazor Server uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="7788b-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="7788b-127">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="7788b-127">Select **Create**.</span></span> <span data-ttu-id="7788b-128">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="7788b-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="7788b-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-129">6\.</span></span> <span data-ttu-id="7788b-130">Uygulamayı çalıştırmak için **Ctrl**+**F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="7788b-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="7788b-131">ASP.NET Core Blazor 'nin önceki bir önizleme sürümü için Blazor Visual Studio uzantısı 'nı yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7788b-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="7788b-132">Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzey için yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="7788b-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7788b-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7788b-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="7788b-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-134">1\.</span></span> <span data-ttu-id="7788b-135">[Visual Studio Code](https://code.visualstudio.com/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="7788b-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="7788b-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-136">2\.</span></span> <span data-ttu-id="7788b-137">En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.</span><span class="sxs-lookup"><span data-stu-id="7788b-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="7788b-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-138">3\.</span></span> <span data-ttu-id="7788b-139">Bir Blazor WebAssembly deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="7788b-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="7788b-140">Bir Blazor sunucu deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="7788b-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="7788b-141">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="7788b-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="7788b-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-142">4\.</span></span> <span data-ttu-id="7788b-143">Visual Studio Code 'de *WebApplication1* klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="7788b-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="7788b-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-144">5\.</span></span> <span data-ttu-id="7788b-145">Bir Blazor Server projesi için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister.</span><span class="sxs-lookup"><span data-stu-id="7788b-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="7788b-146">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="7788b-146">Select **Yes**.</span></span>

   <span data-ttu-id="7788b-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-147">6\.</span></span> <span data-ttu-id="7788b-148">Bir Blazor Server uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7788b-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="7788b-149">Blazor WebAssembly uygulaması kullanılıyorsa, uygulamanın proje klasöründen `dotnet run` yürütün.</span><span class="sxs-lookup"><span data-stu-id="7788b-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="7788b-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-150">7\.</span></span> <span data-ttu-id="7788b-151">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="7788b-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7788b-152">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7788b-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="7788b-153">1 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-153">1\.</span></span> <span data-ttu-id="7788b-154">[Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="7788b-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="7788b-155">2 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-155">2\.</span></span> <span data-ttu-id="7788b-156">**Yeni çözüm** > **Dosya** seçin veya yeni bir **Proje**oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7788b-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="7788b-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-157">3\.</span></span> <span data-ttu-id="7788b-158">Kenar çubuğunda **.NET Core** > **uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="7788b-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="7788b-159">4 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-159">4\.</span></span> <span data-ttu-id="7788b-160">**Blazor Server uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="7788b-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="7788b-161">Şu anda yalnızca Blazor Server şablonu Mac için Visual Studio kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7788b-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="7788b-162">Blazor WebAssembly deneyimi için **.NET Core CLI** sekmesindeki yönergeleri izleyin. Blazor sunucu şablonunu seçtikten sonra **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="7788b-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="7788b-163">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="7788b-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="7788b-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-164">5\.</span></span> <span data-ttu-id="7788b-165">**Hedef Framework 'ü** **.NET Core 3,1** olarak ayarlayın ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="7788b-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="7788b-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-166">6\.</span></span> <span data-ttu-id="7788b-167">**Proje adı** alanında, uygulamayı `WebApplication1`olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="7788b-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="7788b-168">Seçin **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="7788b-168">Select **Create**.</span></span>

   <span data-ttu-id="7788b-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="7788b-169">7\.</span></span> <span data-ttu-id="7788b-170">Uygulamayı *hata ayıklayıcısı olmadan*çalıştırmak Için **hata ayıklama olmadan** **Çalıştır > Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="7788b-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="7788b-171">Uygulamayı hata *ayıklayıcıyla*çalıştırmak Için, **hata ayıklamayı Başlat** ile uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7788b-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="7788b-172">Geliştirme sertifikasına güvenmek için bir istem görünürse, sertifikaya güvenin ve devam edin.</span><span class="sxs-lookup"><span data-stu-id="7788b-172">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7788b-173">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7788b-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="7788b-174">Bir Blazor Weelsembly deneyimi için komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="7788b-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="7788b-175">Bir Blazor sunucu deneyimi için aşağıdaki komutları bir komut kabuğunda yürütün:</span><span class="sxs-lookup"><span data-stu-id="7788b-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="7788b-176">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="7788b-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="7788b-177">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="7788b-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="7788b-178">Kenar çubuğu 'ndaki sekmelerde birden çok sayfa mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="7788b-178">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="7788b-179">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="7788b-179">Home</span></span>
* <span data-ttu-id="7788b-180">Sayaç</span><span class="sxs-lookup"><span data-stu-id="7788b-180">Counter</span></span>
* <span data-ttu-id="7788b-181">Verileri getir</span><span class="sxs-lookup"><span data-stu-id="7788b-181">Fetch data</span></span>

<span data-ttu-id="7788b-182">Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="7788b-182">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="7788b-183">Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak kullanabilirsiniz C#Blazor.</span><span class="sxs-lookup"><span data-stu-id="7788b-183">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="7788b-184">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7788b-184">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="7788b-185">Tarayıcıda `/counter` için bir istek, en üstteki `@page` yönergesi tarafından belirtilen şekilde `Counter` bileşeninin içeriğini işlemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="7788b-185">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="7788b-186">Bileşenler, daha sonra, Kullanıcı arabirimini esnek ve verimli bir şekilde güncelleştirmek için kullanılabilen işleme ağacının bellek içi gösterimine işlenir.</span><span class="sxs-lookup"><span data-stu-id="7788b-186">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="7788b-187">**Bana tıklama** düğmesi her seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="7788b-187">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="7788b-188">`onclick` olayı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="7788b-188">The `onclick` event is fired.</span></span>
* <span data-ttu-id="7788b-189">`IncrementCount` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="7788b-189">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="7788b-190">`currentCount` artırılır.</span><span class="sxs-lookup"><span data-stu-id="7788b-190">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="7788b-191">Bileşen yeniden işlenir.</span><span class="sxs-lookup"><span data-stu-id="7788b-191">The component is rendered again.</span></span>

<span data-ttu-id="7788b-192">Çalışma zamanı, yeni içeriği önceki içerikle karşılaştırır ve yalnızca değiştirilen içeriği Belge Nesne Modeli (DOM) öğesine uygular.</span><span class="sxs-lookup"><span data-stu-id="7788b-192">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="7788b-193">HTML sözdizimini kullanarak başka bir bileşene bileşen ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7788b-193">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="7788b-194">Örneğin, `Index` bileşenine bir `<Counter />` öğesi ekleyerek `Counter` bileşenini uygulamanın giriş sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7788b-194">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="7788b-195">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7788b-195">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="7788b-196">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7788b-196">Run the app.</span></span> <span data-ttu-id="7788b-197">Giriş sayfasının `Counter` bileşeni tarafından kendi sayacı vardır.</span><span class="sxs-lookup"><span data-stu-id="7788b-197">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="7788b-198">Bileşen parametreleri, alt bileşende özellikler ayarlamanıza olanak tanıyan öznitelikler veya [alt içerik](xref:blazor/components#child-content)kullanılarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="7788b-198">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="7788b-199">`Counter` bileşenine bir parametre eklemek için, bileşenin `@code` bloğunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="7788b-199">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="7788b-200">`[Parameter]` özniteliğiyle `IncrementAmount` için ortak özellik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7788b-200">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="7788b-201">`currentCount`değerini artırdığınızda `IncrementAmount` kullanmak için `IncrementCount` yöntemini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="7788b-201">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="7788b-202">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7788b-202">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="7788b-203">`Index` bileşenin `<Counter>` öğesindeki bir özniteliği kullanarak `IncrementAmount` belirtin.</span><span class="sxs-lookup"><span data-stu-id="7788b-203">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="7788b-204">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7788b-204">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="7788b-205">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7788b-205">Run the app.</span></span> <span data-ttu-id="7788b-206">`Index` bileşeni, **bana tıklama** düğmesi seçildiğinde her seferinde on ile artan kendi sayacıdır.</span><span class="sxs-lookup"><span data-stu-id="7788b-206">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="7788b-207">`/counter` `Counter` bileşeni (*Counter. Razor*), bir tarafından arttırmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="7788b-207">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7788b-208">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="7788b-208">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="7788b-209">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7788b-209">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
