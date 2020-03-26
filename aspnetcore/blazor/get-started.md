---
title: ASP.NET Core Blazor kullanmaya başlama
author: guardrex
description: Seçtiğiniz araç ile Blazor bir uygulama oluşturarak Blazor kullanmaya başlayın.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: abecb640930c1e5770c0fad45a1e9a6df31a20f4
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306451"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="24148-103">ASP.NET Core Blazor kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="24148-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="24148-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="24148-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="24148-105">Blazor kullanmaya başlamak için araç seçiminiz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="24148-105">To get started with Blazor, follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="24148-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24148-106">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="24148-107">Blazor Server uygulamaları oluşturmak için **ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019 sürüm 16,4 veya sonraki bir sürümünü](https://visualstudio.microsoft.com/vs/preview/) yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="24148-107">To create Blazor Server apps, install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="24148-108">Blazor Server ve Blazor WebAssembly uygulamaları oluşturmak için **ASP.net ve Web geliştirme** iş yüküyle Visual Studio 2019 16,6 Preview 2 veya sonraki bir sürümünü yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="24148-108">To create Blazor Server and Blazor WebAssembly apps, install Visual Studio 2019 16.6 Preview 2 or later with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="24148-109">İki Blazor barındırma modeli hakkında daha fazla bilgi için, *Blazor WebAssembly* ve *Blazor Server*<xref:blazor/hosting-models>bkz.</span><span class="sxs-lookup"><span data-stu-id="24148-109">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="24148-110">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="24148-110">Create a new project.</span></span>

1. <span data-ttu-id="24148-111">**Blazor uygulamasını**seçin.</span><span class="sxs-lookup"><span data-stu-id="24148-111">Select **Blazor App**.</span></span> <span data-ttu-id="24148-112">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="24148-112">Select **Next**.</span></span>

1. <span data-ttu-id="24148-113">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="24148-113">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="24148-114">**Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="24148-114">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="24148-115">**Oluştur**'u seçin.</span><span class="sxs-lookup"><span data-stu-id="24148-115">Select **Create**.</span></span>

1. <span data-ttu-id="24148-116">Blazor WebAssembly deneyimi için (Visual Studio 16,6 Preview 2 veya üzeri), **Blazor Webassembly uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="24148-116">For a Blazor WebAssembly experience (Visual Studio 16.6 Preview 2 or later), choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="24148-117">Bir Blazor Server deneyimi için (Visual Studio 16,4 veya üzeri) **Blazor Server uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="24148-117">For a Blazor Server experience (Visual Studio 16.4 or later), choose the **Blazor Server App** template.</span></span> <span data-ttu-id="24148-118">**Oluştur**'u seçin.</span><span class="sxs-lookup"><span data-stu-id="24148-118">Select **Create**.</span></span>

1. <span data-ttu-id="24148-119">Uygulamayı çalıştırmak için <kbd>Ctrl</kbd>+<kbd>F5</kbd> tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="24148-119">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="24148-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="24148-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="24148-121">[.NET Core 3,1 SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.</span><span class="sxs-lookup"><span data-stu-id="24148-121">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="24148-122">İsteğe bağlı olarak, aşağıdaki komutu çalıştırarak [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) önizleme şablonunu yükler:</span><span class="sxs-lookup"><span data-stu-id="24148-122">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > <span data-ttu-id="24148-123">3,2 Preview 3 Blazor WebAssembly şablonunu kullanmak için [.NET Core SDK Version 3.1.201 veya üzeri](https://dotnet.microsoft.com/download/dotnet-core/3.1) **gereklidir** .</span><span class="sxs-lookup"><span data-stu-id="24148-123">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview 3 Blazor WebAssembly template.</span></span> <span data-ttu-id="24148-124">Bir komut kabuğunda `dotnet --version` çalıştırarak yüklü .NET Core SDK sürümünü onaylayın.</span><span class="sxs-lookup"><span data-stu-id="24148-124">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="24148-125">[Visual Studio Code](https://code.visualstudio.com/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="24148-125">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="24148-126">En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) ve [JavaScript hata ayıklayıcı (gecelik)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) uzantısını `true``debug.javascript.usePreview` olarak kurun.</span><span class="sxs-lookup"><span data-stu-id="24148-126">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

1. <span data-ttu-id="24148-127">Bir Blazor sunucu deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="24148-127">For a Blazor Server experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   <span data-ttu-id="24148-128">Bir Blazor WebAssembly deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="24148-128">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   <span data-ttu-id="24148-129">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="24148-129">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="24148-130">Visual Studio Code 'de *WebApplication1* klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="24148-130">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="24148-131">IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister.</span><span class="sxs-lookup"><span data-stu-id="24148-131">The IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="24148-132">**Evet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="24148-132">Select **Yes**.</span></span>

1. <span data-ttu-id="24148-133">Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı tun.</span><span class="sxs-lookup"><span data-stu-id="24148-133">Tun the app using the Visual Studio Code debugger.</span></span>

1. <span data-ttu-id="24148-134">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="24148-134">In a browser, navigate to `https://localhost:5001`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="24148-135">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24148-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="24148-136">Blazor sunucusu Mac için Visual Studio desteklenir.</span><span class="sxs-lookup"><span data-stu-id="24148-136">Blazor Server is supported in Visual Studio for Mac.</span></span> <span data-ttu-id="24148-137">Blazor WebAssembly Şu anda desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="24148-137">Blazor WebAssembly isn't supported at this time.</span></span> <span data-ttu-id="24148-138">MacOS 'ta Blazor WebAssembly uygulamaları derlemek için **.NET Core CLI** sekmesindeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="24148-138">To build Blazor WebAssembly apps on macOS, follow the guidance on the **.NET Core CLI** tab.</span></span>

1. <span data-ttu-id="24148-139">[Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="24148-139">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

1. <span data-ttu-id="24148-140">**Yeni çözüm** > **Dosya** seçin veya yeni bir **Proje**oluşturun.</span><span class="sxs-lookup"><span data-stu-id="24148-140">Select **File** > **New Solution** or create a **New Project**.</span></span>

1. <span data-ttu-id="24148-141">Kenar çubuğunda **.NET Core** > **uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="24148-141">In the sidebar, select **.NET Core** > **App**.</span></span>

1. <span data-ttu-id="24148-142">**Blazor Server uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="24148-142">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="24148-143">**Oluştur**'u seçin.</span><span class="sxs-lookup"><span data-stu-id="24148-143">Select **Create**.</span></span>

   <span data-ttu-id="24148-144">Blazor sunucusu barındırma modeli hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="24148-144">For information on the Blazor Server hosting model, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="24148-145">**Hedef Framework 'ü** **.NET Core 3,1** olarak ayarlayın ve **İleri ' yi**seçin.</span><span class="sxs-lookup"><span data-stu-id="24148-145">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

1. <span data-ttu-id="24148-146">**Proje adı** alanında, uygulamayı `WebApplication1`olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="24148-146">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="24148-147">**Oluştur**'u seçin.</span><span class="sxs-lookup"><span data-stu-id="24148-147">Select **Create**.</span></span>

1. <span data-ttu-id="24148-148">Uygulamayı *hata ayıklayıcısı olmadan*çalıştırmak Için **hata ayıklama olmadan** **Çalıştır > Çalıştır** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="24148-148">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="24148-149">Uygulamayı hata *ayıklayıcıyla*çalıştırmak Için, **hata ayıklamayı Başlat** ile uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="24148-149">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

<span data-ttu-id="24148-150">Geliştirme sertifikasına güvenmek için bir istem görünürse, sertifikaya güvenin ve devam edin.</span><span class="sxs-lookup"><span data-stu-id="24148-150">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="24148-151">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="24148-151">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="24148-152">[.NET Core 3,1 SDK 'sını](https://dotnet.microsoft.com/download/dotnet-core/3.1)yükler.</span><span class="sxs-lookup"><span data-stu-id="24148-152">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="24148-153">İsteğe bağlı olarak, aşağıdaki komutu çalıştırarak [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) önizleme şablonunu yükler:</span><span class="sxs-lookup"><span data-stu-id="24148-153">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > <span data-ttu-id="24148-154">3,2 Preview 3 Blazor WebAssembly şablonunu kullanmak için [.NET Core SDK Version 3.1.201 veya üzeri](https://dotnet.microsoft.com/download/dotnet-core/3.1) **gereklidir** .</span><span class="sxs-lookup"><span data-stu-id="24148-154">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview 3 Blazor WebAssembly template.</span></span> <span data-ttu-id="24148-155">Bir komut kabuğunda `dotnet --version` çalıştırarak yüklü .NET Core SDK sürümünü onaylayın.</span><span class="sxs-lookup"><span data-stu-id="24148-155">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="24148-156">Bir Blazor sunucu deneyimi için aşağıdaki komutları bir komut kabuğunda yürütün:</span><span class="sxs-lookup"><span data-stu-id="24148-156">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="24148-157">Bir Blazor Weelsembly deneyimi için komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="24148-157">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="24148-158">İki Blazor barındırma modeli, *Blazor Server* ve *Blazor webassembly*hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="24148-158">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="24148-159">Bir tarayıcıda `https://localhost:5001`' a gidin.</span><span class="sxs-lookup"><span data-stu-id="24148-159">In a browser, navigate to `https://localhost:5001`.</span></span>

---

<span data-ttu-id="24148-160">Kenar çubuğu 'ndaki sekmelerde birden çok sayfa mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="24148-160">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="24148-161">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="24148-161">Home</span></span>
* <span data-ttu-id="24148-162">Sayaç</span><span class="sxs-lookup"><span data-stu-id="24148-162">Counter</span></span>
* <span data-ttu-id="24148-163">Verileri getir</span><span class="sxs-lookup"><span data-stu-id="24148-163">Fetch data</span></span>

<span data-ttu-id="24148-164">Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="24148-164">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="24148-165">Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak kullanabilirsiniz C#Blazor.</span><span class="sxs-lookup"><span data-stu-id="24148-165">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="24148-166">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="24148-166">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="24148-167">Tarayıcıda `/counter` için bir istek, en üstteki `@page` yönergesi tarafından belirtilen şekilde `Counter` bileşeninin içeriğini işlemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="24148-167">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="24148-168">Bileşenler, daha sonra, Kullanıcı arabirimini esnek ve verimli bir şekilde güncelleştirmek için kullanılabilen işleme ağacının bellek içi gösterimine işlenir.</span><span class="sxs-lookup"><span data-stu-id="24148-168">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="24148-169">**Bana tıklama** düğmesi her seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="24148-169">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="24148-170">`onclick` olayı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="24148-170">The `onclick` event is fired.</span></span>
* <span data-ttu-id="24148-171">`IncrementCount` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="24148-171">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="24148-172">`currentCount` artırılır.</span><span class="sxs-lookup"><span data-stu-id="24148-172">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="24148-173">Bileşen yeniden işlenir.</span><span class="sxs-lookup"><span data-stu-id="24148-173">The component is rendered again.</span></span>

<span data-ttu-id="24148-174">Çalışma zamanı, yeni içeriği önceki içerikle karşılaştırır ve yalnızca değiştirilen içeriği Belge Nesne Modeli (DOM) öğesine uygular.</span><span class="sxs-lookup"><span data-stu-id="24148-174">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="24148-175">HTML sözdizimini kullanarak başka bir bileşene bileşen ekleyin.</span><span class="sxs-lookup"><span data-stu-id="24148-175">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="24148-176">Örneğin, `Index` bileşenine bir `<Counter />` öğesi ekleyerek `Counter` bileşenini uygulamanın giriş sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="24148-176">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="24148-177">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="24148-177">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="24148-178">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="24148-178">Run the app.</span></span> <span data-ttu-id="24148-179">Giriş sayfasının `Counter` bileşeni tarafından kendi sayacı vardır.</span><span class="sxs-lookup"><span data-stu-id="24148-179">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="24148-180">Bileşen parametreleri, alt bileşende özellikler ayarlamanıza olanak tanıyan öznitelikler veya [alt içerik](xref:blazor/components#child-content)kullanılarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="24148-180">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="24148-181">`Counter` bileşenine bir parametre eklemek için, bileşenin `@code` bloğunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="24148-181">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="24148-182">`[Parameter]` özniteliğiyle `IncrementAmount` için ortak özellik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="24148-182">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="24148-183">`currentCount`değerini artırdığınızda `IncrementAmount` kullanmak için `IncrementCount` yöntemini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="24148-183">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="24148-184">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="24148-184">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="24148-185">`Index` bileşenin `<Counter>` öğesindeki bir özniteliği kullanarak `IncrementAmount` belirtin.</span><span class="sxs-lookup"><span data-stu-id="24148-185">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="24148-186">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="24148-186">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="24148-187">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="24148-187">Run the app.</span></span> <span data-ttu-id="24148-188">`Index` bileşeni, **bana tıklama** düğmesi seçildiğinde her seferinde on ile artan kendi sayacıdır.</span><span class="sxs-lookup"><span data-stu-id="24148-188">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="24148-189">`/counter` `Counter` bileşeni (*Counter. Razor*), bir tarafından arttırmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="24148-189">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24148-190">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="24148-190">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="24148-191">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="24148-191">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
