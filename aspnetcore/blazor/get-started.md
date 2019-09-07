---
title: ASP.NET Core Blazor kullanmaya başlama
author: guardrex
description: Tercih ettiğiniz araç ile bir Blazor uygulaması oluşturarak Blazor kullanmaya başlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: blazor/get-started
ms.openlocfilehash: 7bff1d38de77adfa1835ee37f4e5f8912ba629cc
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800317"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="1e716-103">ASP.NET Core Blazor kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="1e716-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="1e716-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="1e716-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1e716-105">Blazor kullanmaya başlama:</span><span class="sxs-lookup"><span data-stu-id="1e716-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="1e716-106">En son [.NET Core 3,0 PREVIEW SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) sürümünü yükler.</span><span class="sxs-lookup"><span data-stu-id="1e716-106">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="1e716-107">Komut kabuğu 'nda aşağıdaki komutu çalıştırarak Blazor şablonlarını yüklemelisiniz:</span><span class="sxs-lookup"><span data-stu-id="1e716-107">Install the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview9.19424.4
   ```

1. <span data-ttu-id="1e716-108">Araç seçiminiz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="1e716-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1e716-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e716-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="1e716-110">1 \.</span><span class="sxs-lookup"><span data-stu-id="1e716-110">1\.</span></span> <span data-ttu-id="1e716-111">**ASP.net ve Web geliştirme** iş yüküyle en son [Visual Studio önizlemesini](https://visualstudio.com/vs/preview) yüklersiniz.</span><span class="sxs-lookup"><span data-stu-id="1e716-111">Install the latest [Visual Studio preview](https://visualstudio.com/vs/preview) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="1e716-112">2 \.</span><span class="sxs-lookup"><span data-stu-id="1e716-112">2\.</span></span> <span data-ttu-id="1e716-113">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1e716-113">Create a new project.</span></span>

   <span data-ttu-id="1e716-114">3 \.</span><span class="sxs-lookup"><span data-stu-id="1e716-114">3\.</span></span> <span data-ttu-id="1e716-115">**Blazor uygulamasını**seçin.</span><span class="sxs-lookup"><span data-stu-id="1e716-115">Select **Blazor App**.</span></span> <span data-ttu-id="1e716-116">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="1e716-116">Select **Next**.</span></span>

   <span data-ttu-id="1e716-117">4 \.</span><span class="sxs-lookup"><span data-stu-id="1e716-117">4\.</span></span> <span data-ttu-id="1e716-118">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="1e716-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="1e716-119">**Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="1e716-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="1e716-120">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="1e716-120">Select **Create**.</span></span>

   <span data-ttu-id="1e716-121">5 \.</span><span class="sxs-lookup"><span data-stu-id="1e716-121">5\.</span></span> <span data-ttu-id="1e716-122">Blazor istemci tarafı deneyimi için **Blazor WebAssembly uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="1e716-122">For a Blazor client-side experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="1e716-123">Blazor sunucu tarafı deneyimi için **Blazor Server uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="1e716-123">For a Blazor server-side experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="1e716-124">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="1e716-124">Select **Create**.</span></span> <span data-ttu-id="1e716-125">İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="1e716-125">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="1e716-126">6 \.</span><span class="sxs-lookup"><span data-stu-id="1e716-126">6\.</span></span> <span data-ttu-id="1e716-127">Uygulamayı çalıştırmak için **F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="1e716-127">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1e716-128">ASP.NET Core Blazor 'nin önceki bir önizleme sürümü için Blazor Visual Studio uzantısı 'nı yüklediyseniz (Preview 6 veya daha önceki bir sürümü), uzantıyı kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1e716-128">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="1e716-129">Blazor şablonlarının bir komut kabuğu 'na yüklenmesi artık Visual Studio 'daki şablonları yüzey için yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="1e716-129">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1e716-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e716-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="1e716-131">1 \.</span><span class="sxs-lookup"><span data-stu-id="1e716-131">1\.</span></span> <span data-ttu-id="1e716-132">[Visual Studio Code](https://code.visualstudio.com/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="1e716-132">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="1e716-133">2 \.</span><span class="sxs-lookup"><span data-stu-id="1e716-133">2\.</span></span> <span data-ttu-id="1e716-134">En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.</span><span class="sxs-lookup"><span data-stu-id="1e716-134">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="1e716-135">3 \.</span><span class="sxs-lookup"><span data-stu-id="1e716-135">3\.</span></span> <span data-ttu-id="1e716-136">Bir Blazor istemci tarafı deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="1e716-136">For a Blazor client-side experience, execute the following command in a command shell:</span></span>

      ```console
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="1e716-137">Bir Blazor sunucu tarafı deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="1e716-137">For a Blazor server-side experience, execute the following command in a command shell:</span></span>

      ```console
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="1e716-138">İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="1e716-138">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="1e716-139">4 \.</span><span class="sxs-lookup"><span data-stu-id="1e716-139">4\.</span></span> <span data-ttu-id="1e716-140">Visual Studio Code 'de *WebApplication1* klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="1e716-140">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="1e716-141">5 \.</span><span class="sxs-lookup"><span data-stu-id="1e716-141">5\.</span></span> <span data-ttu-id="1e716-142">Blazor sunucu tarafı bir proje için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister.</span><span class="sxs-lookup"><span data-stu-id="1e716-142">For a Blazor server-side project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="1e716-143">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="1e716-143">Select **Yes**.</span></span>

   <span data-ttu-id="1e716-144">6 \.</span><span class="sxs-lookup"><span data-stu-id="1e716-144">6\.</span></span> <span data-ttu-id="1e716-145">Blazor sunucu tarafı uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1e716-145">If using a Blazor server-side app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="1e716-146">Bir Blazor istemci tarafı uygulaması kullanılıyorsa, uygulamanın proje klasöründen `dotnet run` yürütün.</span><span class="sxs-lookup"><span data-stu-id="1e716-146">If using a Blazor client-side app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="1e716-147">7 \.</span><span class="sxs-lookup"><span data-stu-id="1e716-147">7\.</span></span> <span data-ttu-id="1e716-148">Bir tarayıcıda öğesine `https://localhost:5001`gidin.</span><span class="sxs-lookup"><span data-stu-id="1e716-148">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor server-side experience, select the **Blazor Server App** template. For a Blazor client-side experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1e716-149">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1e716-149">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="1e716-150">Bir Blazor istemci tarafı deneyimi için komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="1e716-150">For a Blazor client-side experience, execute the following commands in a command shell:</span></span>

   ```console
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="1e716-151">Bir Blazor sunucu tarafı deneyimi için komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="1e716-151">For a Blazor server-side experience, execute the following commands in a command shell:</span></span>

   ```console
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="1e716-152">İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="1e716-152">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="1e716-153">Bir tarayıcıda öğesine `https://localhost:5001`gidin.</span><span class="sxs-lookup"><span data-stu-id="1e716-153">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="1e716-154">Kenar çubuğu 'ndaki sekmelerde birden çok sayfa mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="1e716-154">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="1e716-155">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="1e716-155">Home</span></span>
* <span data-ttu-id="1e716-156">Sayaç</span><span class="sxs-lookup"><span data-stu-id="1e716-156">Counter</span></span>
* <span data-ttu-id="1e716-157">Verileri getir</span><span class="sxs-lookup"><span data-stu-id="1e716-157">Fetch data</span></span>

<span data-ttu-id="1e716-158">Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="1e716-158">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="1e716-159">Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak Razor bileşenleri kullanarak C#daha iyi bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="1e716-159">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor components provide a better approach using C#.</span></span>

<span data-ttu-id="1e716-160">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1e716-160">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="1e716-161">En üstteki `/counter` `@page` yönergeyle belirtilen şekilde tarayıcıda için bir istek, `Counter` bileşenin içeriğini işlemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="1e716-161">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="1e716-162">Bileşenler, daha sonra, Kullanıcı arabirimini esnek ve verimli bir şekilde güncelleştirmek için kullanılabilen işleme ağacının bellek içi gösterimine işlenir.</span><span class="sxs-lookup"><span data-stu-id="1e716-162">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="1e716-163">**Bana tıklama** düğmesi her seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="1e716-163">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="1e716-164">`onclick` Olay tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="1e716-164">The `onclick` event is fired.</span></span>
* <span data-ttu-id="1e716-165">`IncrementCount` Yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1e716-165">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="1e716-166">`currentCount` Artırılır.</span><span class="sxs-lookup"><span data-stu-id="1e716-166">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="1e716-167">Bileşen yeniden işlenir.</span><span class="sxs-lookup"><span data-stu-id="1e716-167">The component is rendered again.</span></span>

<span data-ttu-id="1e716-168">Çalışma zamanı, yeni içeriği önceki içerikle karşılaştırır ve yalnızca değiştirilen içeriği Belge Nesne Modeli (DOM) öğesine uygular.</span><span class="sxs-lookup"><span data-stu-id="1e716-168">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="1e716-169">HTML sözdizimini kullanarak başka bir bileşene bileşen ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1e716-169">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="1e716-170">Örneğin, bileşene bir `Counter` `<Counter />` öğe `Index` ekleyerek bileşeni uygulamanın giriş sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1e716-170">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="1e716-171">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1e716-171">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="1e716-172">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1e716-172">Run the app.</span></span> <span data-ttu-id="1e716-173">Giriş sayfası, `Counter` bileşen tarafından sağlanmış kendi sayacıdır.</span><span class="sxs-lookup"><span data-stu-id="1e716-173">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="1e716-174">Bileşen parametreleri, alt bileşende özellikler ayarlamanıza olanak tanıyan öznitelikler veya [alt içerik](xref:blazor/components#child-content)kullanılarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="1e716-174">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="1e716-175">`Counter` Bileşene bir parametre eklemek için `@code` bileşenin bloğunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="1e716-175">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="1e716-176">Özniteliği ile için `IncrementAmount` bir public özelliği ekleyin. `[Parameter]`</span><span class="sxs-lookup"><span data-stu-id="1e716-176">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="1e716-177">Değerini değerini `IncrementAmount`artırdığınızda kullanmak için `IncrementCount` yöntemini değiştirin `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="1e716-177">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="1e716-178">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1e716-178">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="1e716-179">Özniteliği kullanarak bileşenin`<Counter>`öğesindeöğesinibelirtin. `Index` `IncrementAmount`</span><span class="sxs-lookup"><span data-stu-id="1e716-179">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="1e716-180">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="1e716-180">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="1e716-181">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1e716-181">Run the app.</span></span> <span data-ttu-id="1e716-182">Bileşenin, bana tıklama düğmesi seçildiğinde her seferinde on ile artan kendi sayacı vardır. `Index`</span><span class="sxs-lookup"><span data-stu-id="1e716-182">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="1e716-183">' `Counter` De`/counter` bileşen (*Counter. Razor*), bir tane tarafından arttırmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="1e716-183">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e716-184">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="1e716-184">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="1e716-185">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1e716-185">Additional resources</span></span>

* <xref:signalr/introduction>
