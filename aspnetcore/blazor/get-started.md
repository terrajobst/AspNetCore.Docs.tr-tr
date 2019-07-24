---
title: ASP.NET Core Blazor kullanmaya başlama
author: guardrex
description: Tercih ettiğiniz araç ile bir Blazor uygulaması oluşturarak Blazor kullanmaya başlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/23/2019
uid: blazor/get-started
ms.openlocfilehash: 9ebe1e2d36f96bfa214c2389535c28db5db5020a
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412409"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="d849d-103">ASP.NET Core Blazor kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="d849d-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="d849d-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="d849d-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d849d-105">Blazor kullanmaya başlama:</span><span class="sxs-lookup"><span data-stu-id="d849d-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="d849d-106">En son [.NET Core 3,0 PREVIEW SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) sürümünü yükler.</span><span class="sxs-lookup"><span data-stu-id="d849d-106">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="d849d-107">Komut kabuğu 'nda aşağıdaki komutu çalıştırarak Blazor şablonlarını yüklemelisiniz:</span><span class="sxs-lookup"><span data-stu-id="d849d-107">Install the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview7.19365.7
   ```

1. <span data-ttu-id="d849d-108">Araç seçiminiz için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="d849d-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d849d-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d849d-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="d849d-110">1 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-110">1\.</span></span> <span data-ttu-id="d849d-111">**ASP.net ve Web geliştirme** iş yüküyle en son [Visual Studio önizlemesini](https://visualstudio.com/vs/preview) yüklersiniz.</span><span class="sxs-lookup"><span data-stu-id="d849d-111">Install the latest [Visual Studio preview](https://visualstudio.com/vs/preview) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="d849d-112">2 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-112">2\.</span></span> <span data-ttu-id="d849d-113">Visual Studio Market en son [Blazor uzantısını](https://go.microsoft.com/fwlink/?linkid=870389) yükler.</span><span class="sxs-lookup"><span data-stu-id="d849d-113">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="d849d-114">Bu adım Blazor şablonlarının Visual Studio tarafından kullanılabilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d849d-114">This step makes Blazor templates available to Visual Studio.</span></span>

   <span data-ttu-id="d849d-115">3 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-115">3\.</span></span> <span data-ttu-id="d849d-116">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d849d-116">Create a new project.</span></span>

   <span data-ttu-id="d849d-117">4 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-117">4\.</span></span> <span data-ttu-id="d849d-118">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="d849d-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="d849d-119">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="d849d-119">Select **Next**.</span></span>

   <span data-ttu-id="d849d-120">5 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-120">5\.</span></span> <span data-ttu-id="d849d-121">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="d849d-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="d849d-122">**Konum** girişinin doğru olduğunu onaylayın veya proje için bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="d849d-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="d849d-123">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="d849d-123">Select **Create**.</span></span>

   <span data-ttu-id="d849d-124">6 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-124">6\.</span></span> <span data-ttu-id="d849d-125">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,0** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="d849d-125">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>

   <span data-ttu-id="d849d-126">7 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-126">7\.</span></span> <span data-ttu-id="d849d-127">Blazor istemci tarafı deneyimi için **Blazor WebAssembly uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="d849d-127">For a Blazor client-side experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="d849d-128">Blazor sunucu tarafı deneyimi için **Blazor Server uygulama** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="d849d-128">For a Blazor server-side experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="d849d-129">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="d849d-129">Select **Create**.</span></span> <span data-ttu-id="d849d-130">İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d849d-130">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d849d-131">8 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-131">8\.</span></span> <span data-ttu-id="d849d-132">Uygulamayı çalıştırmak için **F5** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="d849d-132">Press **F5** to run the app.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d849d-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d849d-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="d849d-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-134">1\.</span></span> <span data-ttu-id="d849d-135">[Visual Studio Code](https://code.visualstudio.com/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="d849d-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="d849d-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-136">2\.</span></span> <span data-ttu-id="d849d-137">En son [ C# Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)yükler.</span><span class="sxs-lookup"><span data-stu-id="d849d-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="d849d-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-138">3\.</span></span> <span data-ttu-id="d849d-139">Bir Blazor istemci tarafı deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="d849d-139">For a Blazor client-side experience, execute the following command in a command shell:</span></span>

      ```console
      dotnet new blazor -o WebApplication1
      ```

      <span data-ttu-id="d849d-140">Bir Blazor sunucu tarafı deneyimi için komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="d849d-140">For a Blazor server-side experience, execute the following command in a command shell:</span></span>

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      <span data-ttu-id="d849d-141">İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d849d-141">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d849d-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-142">4\.</span></span> <span data-ttu-id="d849d-143">Visual Studio Code 'de *WebApplication1* klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="d849d-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="d849d-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-144">5\.</span></span> <span data-ttu-id="d849d-145">Blazor sunucu tarafı bir proje için IDE, projeyi derlemek ve hatalarını ayıklamak için varlık eklemenizi ister.</span><span class="sxs-lookup"><span data-stu-id="d849d-145">For a Blazor server-side project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="d849d-146">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="d849d-146">Select **Yes**.</span></span>

   <span data-ttu-id="d849d-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-147">6\.</span></span> <span data-ttu-id="d849d-148">Blazor sunucu tarafı uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d849d-148">If using a Blazor server-side app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="d849d-149">Bir Blazor istemci tarafı uygulaması kullanılıyorsa, uygulamanın proje klasöründen `dotnet run` yürütün.</span><span class="sxs-lookup"><span data-stu-id="d849d-149">If using a Blazor client-side app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="d849d-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="d849d-150">7\.</span></span> <span data-ttu-id="d849d-151">Bir tarayıcıda öğesine `https://localhost:5001`gidin.</span><span class="sxs-lookup"><span data-stu-id="d849d-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor server-side experience, select the **ASP.NET Core Blazor Server App** template. For a Blazor client-side experience, select the **ASP.NET Core Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d849d-152">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d849d-152">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="d849d-153">Bir Blazor istemci tarafı deneyimi için komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="d849d-153">For a Blazor client-side experience, execute the following commands in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d849d-154">Bir Blazor sunucu tarafı deneyimi için komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="d849d-154">For a Blazor server-side experience, execute the following commands in a command shell:</span></span>

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d849d-155">İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d849d-155">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d849d-156">Bir tarayıcıda öğesine `https://localhost:5001`gidin.</span><span class="sxs-lookup"><span data-stu-id="d849d-156">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="d849d-157">Kenar çubuğu 'ndaki sekmelerde birden çok sayfa mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="d849d-157">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="d849d-158">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="d849d-158">Home</span></span>
* <span data-ttu-id="d849d-159">Sayaç</span><span class="sxs-lookup"><span data-stu-id="d849d-159">Counter</span></span>
* <span data-ttu-id="d849d-160">Verileri getir</span><span class="sxs-lookup"><span data-stu-id="d849d-160">Fetch data</span></span>

<span data-ttu-id="d849d-161">Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="d849d-161">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="d849d-162">Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak Razor bileşenleri kullanarak C#daha iyi bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="d849d-162">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor components provide a better approach using C#.</span></span>

<span data-ttu-id="d849d-163">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d849d-163">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="d849d-164">En üstteki `/counter` `@page` yönergeyle belirtilen şekilde tarayıcıda için bir istek, `Counter` bileşenin içeriğini işlemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="d849d-164">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="d849d-165">Bileşenler, daha sonra, Kullanıcı arabirimini esnek ve verimli bir şekilde güncelleştirmek için kullanılabilen işleme ağacının bellek içi gösterimine işlenir.</span><span class="sxs-lookup"><span data-stu-id="d849d-165">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="d849d-166">**Bana tıklama** düğmesi her seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="d849d-166">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="d849d-167">`onclick` Olay tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="d849d-167">The `onclick` event is fired.</span></span>
* <span data-ttu-id="d849d-168">`IncrementCount` Yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d849d-168">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="d849d-169">`currentCount` Artırılır.</span><span class="sxs-lookup"><span data-stu-id="d849d-169">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="d849d-170">Bileşen yeniden işlenir.</span><span class="sxs-lookup"><span data-stu-id="d849d-170">The component is rendered again.</span></span>

<span data-ttu-id="d849d-171">Çalışma zamanı, yeni içeriği önceki içerikle karşılaştırır ve yalnızca değiştirilen içeriği Belge Nesne Modeli (DOM) öğesine uygular.</span><span class="sxs-lookup"><span data-stu-id="d849d-171">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="d849d-172">HTML sözdizimini kullanarak başka bir bileşene bileşen ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d849d-172">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="d849d-173">Örneğin, bileşene bir `Counter` `<Counter />` öğe `Index` ekleyerek bileşeni uygulamanın giriş sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d849d-173">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="d849d-174">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d849d-174">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="d849d-175">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d849d-175">Run the app.</span></span> <span data-ttu-id="d849d-176">Giriş sayfası, `Counter` bileşen tarafından sağlanmış kendi sayacıdır.</span><span class="sxs-lookup"><span data-stu-id="d849d-176">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="d849d-177">Bileşen parametreleri, alt bileşende özellikler ayarlamanıza olanak tanıyan öznitelikler veya [alt içerik](xref:blazor/components#child-content)kullanılarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="d849d-177">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="d849d-178">`Counter` Bileşene bir parametre eklemek için `@code` bileşenin bloğunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d849d-178">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="d849d-179">Özniteliği ile için `IncrementAmount` bir özelliği ekleyin. `[Parameter]`</span><span class="sxs-lookup"><span data-stu-id="d849d-179">Add a property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="d849d-180">Değerini değerini `IncrementAmount`artırdığınızda kullanmak için `IncrementCount` yöntemini değiştirin `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="d849d-180">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="d849d-181">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d849d-181">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="d849d-182">Özniteliği kullanarak bileşenin`<Counter>`öğesindeöğesinibelirtin. `Index` `IncrementAmount`</span><span class="sxs-lookup"><span data-stu-id="d849d-182">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="d849d-183">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="d849d-183">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="d849d-184">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d849d-184">Run the app.</span></span> <span data-ttu-id="d849d-185">Bileşenin, bana tıklama düğmesi seçildiğinde her seferinde on ile artan kendi sayacı vardır.  `Index`</span><span class="sxs-lookup"><span data-stu-id="d849d-185">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="d849d-186">' `Counter` De`/counter` bileşen (*Counter. Razor*), bir tane tarafından arttırmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="d849d-186">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d849d-187">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="d849d-187">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="d849d-188">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d849d-188">Additional resources</span></span>

* <xref:signalr/introduction>
