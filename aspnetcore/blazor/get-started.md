---
title: Blazor ile çalışmaya başlama
author: guardrex
description: Blazor ile kendi tercih ettiğiniz araçları Blazor uygulamayla oluşturarak başlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/05/2019
uid: blazor/get-started
ms.openlocfilehash: 9df18b25f6d82d655c093a17fe498615c0ff535e
ms.sourcegitcommit: c716ea9155a6b404c1f3d3d34e2388454cd276d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66716286"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="874d0-103">Blazor ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="874d0-103">Get started with Blazor</span></span>

<span data-ttu-id="874d0-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="874d0-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="874d0-105">Blazor ile kullanmaya başlayın:</span><span class="sxs-lookup"><span data-stu-id="874d0-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="874d0-106">Son yükleme [.NET Core 3.0 Önizleme SDK'sı](https://dotnet.microsoft.com/download/dotnet-core/3.0) bırakın.</span><span class="sxs-lookup"><span data-stu-id="874d0-106">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="874d0-107">Blazor şablonları, bir komut kabuğu'nda aşağıdaki komutu çalıştırarak yükleyin:</span><span class="sxs-lookup"><span data-stu-id="874d0-107">Install the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview5-19227-01
   ```

1. <span data-ttu-id="874d0-108">Tercih ettiğiniz araçları için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="874d0-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="874d0-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="874d0-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="874d0-110">1\.</span><span class="sxs-lookup"><span data-stu-id="874d0-110">1\.</span></span> <span data-ttu-id="874d0-111">Son yükleme [Visual Studio Önizleme](https://visualstudio.com/preview) ile **ASP.NET ve web geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="874d0-111">Install the latest [Visual Studio preview](https://visualstudio.com/preview) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="874d0-112">2\.</span><span class="sxs-lookup"><span data-stu-id="874d0-112">2\.</span></span> <span data-ttu-id="874d0-113">Son yükleme [Blazor uzantısı](https://go.microsoft.com/fwlink/?linkid=870389) Visual Studio Market'ten.</span><span class="sxs-lookup"><span data-stu-id="874d0-113">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="874d0-114">Bu adım Blazor şablonları Visual Studio için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="874d0-114">This step makes Blazor templates available to Visual Studio.</span></span>

   <span data-ttu-id="874d0-115">3\.</span><span class="sxs-lookup"><span data-stu-id="874d0-115">3\.</span></span> <span data-ttu-id="874d0-116">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="874d0-116">Create a new project.</span></span>

   <span data-ttu-id="874d0-117">4\.</span><span class="sxs-lookup"><span data-stu-id="874d0-117">4\.</span></span> <span data-ttu-id="874d0-118">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="874d0-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="874d0-119">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="874d0-119">Select **Next**.</span></span>

   <span data-ttu-id="874d0-120">5\.</span><span class="sxs-lookup"><span data-stu-id="874d0-120">5\.</span></span> <span data-ttu-id="874d0-121">Bir proje adı belirtin **proje adı** alan veya varsayılan proje adı kabul edin.</span><span class="sxs-lookup"><span data-stu-id="874d0-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="874d0-122">Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="874d0-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="874d0-123">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="874d0-123">Select **Create**.</span></span>

   <span data-ttu-id="874d0-124">6\.</span><span class="sxs-lookup"><span data-stu-id="874d0-124">6\.</span></span> <span data-ttu-id="874d0-125">İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda onaylayın **.NET Core** ve **ASP.NET Core 3.0** seçilir.</span><span class="sxs-lookup"><span data-stu-id="874d0-125">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>

   <span data-ttu-id="874d0-126">7\.</span><span class="sxs-lookup"><span data-stu-id="874d0-126">7\.</span></span> <span data-ttu-id="874d0-127">Blazor istemci-tarafı deneyimi için seçin **Blazor (istemci-tarafı)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="874d0-127">For a Blazor client-side experience, choose the **Blazor (client-side)** template.</span></span> <span data-ttu-id="874d0-128">Blazor sunucu tarafı deneyimi için seçin **Blazor (sunucu tarafı)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="874d0-128">For a Blazor server-side experience, choose the **Blazor (server-side)** template.</span></span> <span data-ttu-id="874d0-129">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="874d0-129">Select **Create**.</span></span> <span data-ttu-id="874d0-130">İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz: <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="874d0-130">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="874d0-131">8\.</span><span class="sxs-lookup"><span data-stu-id="874d0-131">8\.</span></span> <span data-ttu-id="874d0-132">Tuşuna **F5** uygulamayı çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="874d0-132">Press **F5** to run the app.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="874d0-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="874d0-133">Visual Studio Code</span></span>](#tab/visual-studio-code)
   
   <span data-ttu-id="874d0-134">1\.</span><span class="sxs-lookup"><span data-stu-id="874d0-134">1\.</span></span> <span data-ttu-id="874d0-135">Yükleme [Visual Studio Code'u](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="874d0-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="874d0-136">2\.</span><span class="sxs-lookup"><span data-stu-id="874d0-136">2\.</span></span> <span data-ttu-id="874d0-137">Son yükleme [ C# Visual Studio Code uzantısı için](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="874d0-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="874d0-138">3\.</span><span class="sxs-lookup"><span data-stu-id="874d0-138">3\.</span></span> <span data-ttu-id="874d0-139">Blazor istemci-tarafı deneyimi için bir komut kabuğu'ndan aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="874d0-139">For a Blazor client-side experience, execute the following command from a command shell:</span></span>

      ```console
      dotnet new blazor -o WebApplication1
      ```

      <span data-ttu-id="874d0-140">Blazor sunucu tarafı deneyimi için bir komut kabuğu'ndan aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="874d0-140">For a Blazor server-side experience, execute the following command from a command shell:</span></span>

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      <span data-ttu-id="874d0-141">İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz: <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="874d0-141">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="874d0-142">4\.</span><span class="sxs-lookup"><span data-stu-id="874d0-142">4\.</span></span> <span data-ttu-id="874d0-143">Açık *WebApplication1* Visual Studio code'da klasörü.</span><span class="sxs-lookup"><span data-stu-id="874d0-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="874d0-144">5\.</span><span class="sxs-lookup"><span data-stu-id="874d0-144">5\.</span></span> <span data-ttu-id="874d0-145">Blazor sunucu tarafı proje için yapı ve proje hatalarını ayıklamaya varlıklar ekleme IDE ister.</span><span class="sxs-lookup"><span data-stu-id="874d0-145">For a Blazor server-side project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="874d0-146">Seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="874d0-146">Select **Yes**.</span></span>

   <span data-ttu-id="874d0-147">6\.</span><span class="sxs-lookup"><span data-stu-id="874d0-147">6\.</span></span> <span data-ttu-id="874d0-148">Blazor sunucu-tarafı uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="874d0-148">If using a Blazor server-side app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="874d0-149">Blazor istemci-tarafı uygulaması kullanıyorsanız yürütme `dotnet run` uygulama proje klasöründen.</span><span class="sxs-lookup"><span data-stu-id="874d0-149">If using a Blazor client-side app, execute `dotnet run` from the app's project folder.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor server-side experience, select the **ASP.NET Core Blazor (server-side)** template. For a Blazor client-side experience, select the **ASP.NET Core Blazor (client-side)** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="874d0-150">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="874d0-150">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="874d0-151">Blazor istemci-tarafı deneyimi için bir komut kabuğu'ndan aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="874d0-151">For a Blazor client-side experience, execute the following commands from a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="874d0-152">Blazor sunucu tarafı deneyimi için bir komut kabuğu'ndan aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="874d0-152">For a Blazor server-side experience, execute the following commands from a command shell:</span></span>

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="874d0-153">İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz: <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="874d0-153">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   ---

<span data-ttu-id="874d0-154">Bir tarayıcıda gidin `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="874d0-154">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="874d0-155">Birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="874d0-155">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="874d0-156">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="874d0-156">Home</span></span>
* <span data-ttu-id="874d0-157">Sayaç</span><span class="sxs-lookup"><span data-stu-id="874d0-157">Counter</span></span>
* <span data-ttu-id="874d0-158">Veri getirme</span><span class="sxs-lookup"><span data-stu-id="874d0-158">Fetch data</span></span>

<span data-ttu-id="874d0-159">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="874d0-159">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="874d0-160">Normal olarak artan bir Web sayfasındaki bir sayaç gerekli JavaScript Yazma, ancak Razor bileşenleri sağlamak daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="874d0-160">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor components provide a better approach using C#.</span></span>

<span data-ttu-id="874d0-161">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="874d0-161">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="874d0-162">Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur.</span><span class="sxs-lookup"><span data-stu-id="874d0-162">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="874d0-163">Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.</span><span class="sxs-lookup"><span data-stu-id="874d0-163">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="874d0-164">Her zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="874d0-164">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="874d0-165">`onclick` Olay tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="874d0-165">The `onclick` event is fired.</span></span>
* <span data-ttu-id="874d0-166">`IncrementCount` Yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="874d0-166">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="874d0-167">`currentCount` Artırılır.</span><span class="sxs-lookup"><span data-stu-id="874d0-167">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="874d0-168">Bileşeni yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="874d0-168">The component is rendered again.</span></span>

<span data-ttu-id="874d0-169">Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="874d0-169">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="874d0-170">Bir bileşen başka bir bileşene HTML sözdizimini kullanarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="874d0-170">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="874d0-171">Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="874d0-171">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="874d0-172">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="874d0-172">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="874d0-173">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="874d0-173">Run the app.</span></span> <span data-ttu-id="874d0-174">Giriş sayfası sayacı bileşen tarafından sağlanan kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="874d0-174">The homepage has its own counter provided by the Counter component.</span></span>

<span data-ttu-id="874d0-175">Bileşen parametreleri, öznitelikleri kullanarak belirtilir veya [alt içeriğin](xref:blazor/components#child-content), en alt bileşen özelliklerini ayarlama sağlar.</span><span class="sxs-lookup"><span data-stu-id="874d0-175">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="874d0-176">Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:</span><span class="sxs-lookup"><span data-stu-id="874d0-176">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="874d0-177">Bir özelliği için ekleme `IncrementAmount` ile bir `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="874d0-177">Add a property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="874d0-178">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="874d0-178">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="874d0-179">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="874d0-179">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="874d0-180">Belirtin `IncrementAmount` dizin bileşenin içinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="874d0-180">Specify the `IncrementAmount` in the Index component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="874d0-181">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="874d0-181">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="874d0-182">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="874d0-182">Run the app.</span></span> <span data-ttu-id="874d0-183">Dizin bileşenini on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="874d0-183">The Index component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="874d0-184">Sayaç bileşeni (*Counter.razor*) adresindeki `/counter` bir ilerlemeye devam eder.</span><span class="sxs-lookup"><span data-stu-id="874d0-184">The Counter component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="874d0-185">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="874d0-185">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="874d0-186">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="874d0-186">Additional resources</span></span>

* <xref:signalr/introduction>
