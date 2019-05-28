---
title: Blazor ile çalışmaya başlama
author: guardrex
description: Blazor ile kendi tercih ettiğiniz araçları Blazor uygulamayla oluşturarak başlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/26/2019
uid: blazor/get-started
ms.openlocfilehash: a67f9742184716338bf6235c0b340900b17b19dc
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251193"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="045c2-103">Blazor ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="045c2-103">Get started with Blazor</span></span>

<span data-ttu-id="045c2-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="045c2-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="045c2-105">Blazor ile kullanmaya başlayın:</span><span class="sxs-lookup"><span data-stu-id="045c2-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="045c2-106">Son yükleme [.NET Core 3.0 Önizleme SDK'sı](https://dotnet.microsoft.com/download/dotnet-core/3.0) bırakın.</span><span class="sxs-lookup"><span data-stu-id="045c2-106">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="045c2-107">Blazor şablonları, bir komut kabuğu'nda aşağıdaki komutu çalıştırarak yükleyin:</span><span class="sxs-lookup"><span data-stu-id="045c2-107">Install the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview5-19227-01
   ```

1. <span data-ttu-id="045c2-108">Tercih ettiğiniz araçları için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="045c2-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="045c2-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="045c2-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="045c2-110">1.&nbsp;en son yükleme [Visual Studio Önizleme](https://visualstudio.com/preview) ile **ASP.NET ve web geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="045c2-110">1.&nbsp;Install the latest [Visual Studio preview](https://visualstudio.com/preview) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="045c2-111">2.&nbsp;en son yükleme [Blazor uzantısı](https://go.microsoft.com/fwlink/?linkid=870389) Visual Studio Market'ten.</span><span class="sxs-lookup"><span data-stu-id="045c2-111">2.&nbsp;Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="045c2-112">Bu adım Blazor şablonları Visual Studio için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="045c2-112">This step makes Blazor templates available to Visual Studio.</span></span>

   <span data-ttu-id="045c2-113">3.&nbsp;yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="045c2-113">3.&nbsp;Create a new project.</span></span>

   <span data-ttu-id="045c2-114">4.&nbsp;seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="045c2-114">4.&nbsp;Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="045c2-115">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="045c2-115">Select **Next**.</span></span>

   <span data-ttu-id="045c2-116">5.&nbsp;bir proje adı belirtin **proje adı** alan veya varsayılan proje adı kabul edin.</span><span class="sxs-lookup"><span data-stu-id="045c2-116">5.&nbsp;Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="045c2-117">Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="045c2-117">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="045c2-118">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="045c2-118">Select **Create**.</span></span>

   <span data-ttu-id="045c2-119">6.&nbsp;içinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda onaylayın **.NET Core** ve **ASP.NET Core 3.0** seçilir.</span><span class="sxs-lookup"><span data-stu-id="045c2-119">6.&nbsp;In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>

   <span data-ttu-id="045c2-120">7.&nbsp;Blazor istemci-tarafı deneyimi için seçin **Blazor (istemci-tarafı)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="045c2-120">7.&nbsp;For a Blazor client-side experience, choose the **Blazor (client-side)** template.</span></span> <span data-ttu-id="045c2-121">Blazor sunucu tarafı deneyimi için seçin **Blazor (sunucu tarafı)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="045c2-121">For a Blazor server-side experience, choose the **Blazor (server-side)** template.</span></span> <span data-ttu-id="045c2-122">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="045c2-122">Select **Create**.</span></span> <span data-ttu-id="045c2-123">İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz: <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="045c2-123">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="045c2-124">8.&nbsp;tuşuna **F5** uygulamayı çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="045c2-124">8.&nbsp;Press **F5** to run the app.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="045c2-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="045c2-125">Visual Studio Code</span></span>](#tab/visual-studio-code)
   
   <span data-ttu-id="045c2-126">1.&nbsp;yükleme [Visual Studio Code'u](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="045c2-126">1.&nbsp;Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="045c2-127">2.&nbsp;en son yükleme [ C# Visual Studio Code uzantısı için](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="045c2-127">2.&nbsp;Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="045c2-128">3.&nbsp;Blazor istemci-tarafı deneyimi için bir komut kabuğu'ndan aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="045c2-128">3.&nbsp;For a Blazor client-side experience, execute the following command from a command shell:</span></span>

      ```console
      dotnet new blazor -o WebApplication1
      ```

      <span data-ttu-id="045c2-129">Blazor sunucu tarafı deneyimi için bir komut kabuğu'ndan aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="045c2-129">For a Blazor server-side experience, execute the following command from a command shell:</span></span>

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      <span data-ttu-id="045c2-130">İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz: <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="045c2-130">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="045c2-131">4.&nbsp;açık *WebApplication1* Visual Studio code'da klasörü.</span><span class="sxs-lookup"><span data-stu-id="045c2-131">4.&nbsp;Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="045c2-132">5.&nbsp;bir Blazor için sunucu tarafı proje, IDE istekleri oluşturmak ve projede hata ayıklamak için varlıklar ekleyin.</span><span class="sxs-lookup"><span data-stu-id="045c2-132">5.&nbsp;For a Blazor server-side project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="045c2-133">Seçin **Evet**.</span><span class="sxs-lookup"><span data-stu-id="045c2-133">Select **Yes**.</span></span>

   <span data-ttu-id="045c2-134">6.&nbsp;Blazor sunucu-tarafı uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="045c2-134">6.&nbsp;If using a Blazor server-side app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="045c2-135">Blazor istemci-tarafı uygulaması kullanıyorsanız yürütme `dotnet run` uygulama proje klasöründen.</span><span class="sxs-lookup"><span data-stu-id="045c2-135">If using a Blazor client-side app, execute `dotnet run` from the app's project folder.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1.&nbsp;Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2.&nbsp;Select **File** > **New Solution** or **New Project**.

   3.&nbsp;In the sidebar, select **.NET Core** > **App**.

   4.&nbsp;For a Blazor server-side experience, select the **ASP.NET Core Blazor (server-side)** template. For a Blazor client-side experience, select the **ASP.NET Core Blazor (client-side)** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5.&nbsp;The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6.&nbsp;In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7.&nbsp;Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="045c2-136">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="045c2-136">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="045c2-137">Blazor istemci-tarafı deneyimi için bir komut kabuğu'ndan aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="045c2-137">For a Blazor client-side experience, execute the following commands from a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="045c2-138">Blazor sunucu tarafı deneyimi için bir komut kabuğu'ndan aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="045c2-138">For a Blazor server-side experience, execute the following commands from a command shell:</span></span>

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="045c2-139">İki Blazor barındırma modeli, sunucu tarafı ve istemci tarafı hakkında daha fazla bilgi için bkz: <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="045c2-139">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   ---

<span data-ttu-id="045c2-140">Bir tarayıcıda gidin `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="045c2-140">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="045c2-141">Birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="045c2-141">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="045c2-142">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="045c2-142">Home</span></span>
* <span data-ttu-id="045c2-143">Sayaç</span><span class="sxs-lookup"><span data-stu-id="045c2-143">Counter</span></span>
* <span data-ttu-id="045c2-144">Veri getirme</span><span class="sxs-lookup"><span data-stu-id="045c2-144">Fetch data</span></span>

<span data-ttu-id="045c2-145">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="045c2-145">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="045c2-146">Normal olarak artan bir Web sayfasındaki bir sayaç gerekli JavaScript Yazma, ancak Razor bileşenleri sağlamak daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="045c2-146">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor components provide a better approach using C#.</span></span>

<span data-ttu-id="045c2-147">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="045c2-147">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="045c2-148">Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur.</span><span class="sxs-lookup"><span data-stu-id="045c2-148">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="045c2-149">Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.</span><span class="sxs-lookup"><span data-stu-id="045c2-149">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="045c2-150">Her zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="045c2-150">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="045c2-151">`onclick` Olay tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="045c2-151">The `onclick` event is fired.</span></span>
* <span data-ttu-id="045c2-152">`IncrementCount` Yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="045c2-152">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="045c2-153">`currentCount` Artırılır.</span><span class="sxs-lookup"><span data-stu-id="045c2-153">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="045c2-154">Bileşeni yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="045c2-154">The component is rendered again.</span></span>

<span data-ttu-id="045c2-155">Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="045c2-155">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="045c2-156">Bir bileşen başka bir bileşene HTML söz dizimi kullanılarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="045c2-156">Add a component to another component using an HTML syntax.</span></span> <span data-ttu-id="045c2-157">Bileşen parametreleri, öznitelikleri veya alt içeriğin kullanarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="045c2-157">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="045c2-158">Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="045c2-158">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="045c2-159">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="045c2-159">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="045c2-160">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="045c2-160">Run the app.</span></span> <span data-ttu-id="045c2-161">Giriş sayfası sayacı bileşen tarafından sağlanan kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="045c2-161">The homepage has its own counter provided by the Counter component.</span></span>

<span data-ttu-id="045c2-162">Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:</span><span class="sxs-lookup"><span data-stu-id="045c2-162">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="045c2-163">Bir özelliği için ekleme `IncrementAmount` ile bir `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="045c2-163">Add a property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="045c2-164">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="045c2-164">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="045c2-165">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="045c2-165">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="045c2-166">Belirtin `IncrementAmount` dizin bileşenin içinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="045c2-166">Specify the `IncrementAmount` in the Index component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="045c2-167">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="045c2-167">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="045c2-168">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="045c2-168">Run the app.</span></span> <span data-ttu-id="045c2-169">Dizin bileşenini on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="045c2-169">The Index component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="045c2-170">Sayaç bileşeni (*Counter.razor*) adresindeki `/counter` bir ilerlemeye devam eder.</span><span class="sxs-lookup"><span data-stu-id="045c2-170">The Counter component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="045c2-171">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="045c2-171">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="045c2-172">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="045c2-172">Additional resources</span></span>

* <xref:signalr/introduction>
