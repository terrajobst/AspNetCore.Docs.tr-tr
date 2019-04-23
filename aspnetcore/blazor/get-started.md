---
title: Blazor ile çalışmaya başlama
author: guardrex
description: Blazor ile çalışmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/19/2019
uid: blazor/get-started
ms.openlocfilehash: 45ae0acc6aaee433cce4eddb2fe9c59c306581d7
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982671"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="b8540-103">Blazor ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="b8540-103">Get started with Blazor</span></span>

<span data-ttu-id="b8540-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b8540-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b8540-105">Birkaç adımda Blazor ile kullanmaya başlayın:</span><span class="sxs-lookup"><span data-stu-id="b8540-105">In a few steps, get started with Blazor:</span></span>

1. <span data-ttu-id="b8540-106">Son yükleme [.NET Core 3.0 Önizleme SDK'sı](https://dotnet.microsoft.com/download/dotnet-core/3.0) bırakın.</span><span class="sxs-lookup"><span data-stu-id="b8540-106">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="b8540-107">Blazor şablonları, bir komut kabuğu'nda aşağıdaki komutu çalıştırarak yükleyin:</span><span class="sxs-lookup"><span data-stu-id="b8540-107">Install the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview4-19216-03
   ```

1. <span data-ttu-id="b8540-108">Tercih ettiğiniz araçları için yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="b8540-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b8540-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8540-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="b8540-110">1.&nbsp;en son önizlemesi yükleme [Visual Studio 2019](https://visualstudio.com/preview) ile **ASP.NET ve web geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="b8540-110">1.&nbsp;Install the latest preview of [Visual Studio 2019](https://visualstudio.com/preview) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="b8540-111">2.&nbsp;en son yükleme [Blazor uzantısı](https://go.microsoft.com/fwlink/?linkid=870389) Visual Studio Market'ten.</span><span class="sxs-lookup"><span data-stu-id="b8540-111">2.&nbsp;Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="b8540-112">Bu adım Blazor şablonları Visual Studio için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="b8540-112">This step makes Blazor templates available to Visual Studio.</span></span>

   <span data-ttu-id="b8540-113">3.&nbsp;Önizleme SDK'ları kullanmak için Visual Studio etkinleştir: Açık **Araçları** > **seçenekleri** menü çubuğundaki.</span><span class="sxs-lookup"><span data-stu-id="b8540-113">3.&nbsp;Enable Visual Studio to use preview SDKs: Open **Tools** > **Options** in the menu bar.</span></span> <span data-ttu-id="b8540-114">Açık **projeler ve çözümler** düğümü.</span><span class="sxs-lookup"><span data-stu-id="b8540-114">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="b8540-115">Açık **.NET Core** sekmesi. İçin kutuyu **önizlemeleri .NET Core SDK'sını kullanma**.</span><span class="sxs-lookup"><span data-stu-id="b8540-115">Open the **.NET Core** tab. Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="b8540-116">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="b8540-116">Select **OK**.</span></span>

   <span data-ttu-id="b8540-117">4.&nbsp;yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b8540-117">4.&nbsp;Create a new project.</span></span>

   <span data-ttu-id="b8540-118">5.&nbsp;seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="b8540-118">5.&nbsp;Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="b8540-119">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b8540-119">Select **Next**.</span></span>

   <span data-ttu-id="b8540-120">6.&nbsp;bir adı belirtin **proje adı** alan.</span><span class="sxs-lookup"><span data-stu-id="b8540-120">6.&nbsp;Provide a name in the **Project name** field.</span></span> <span data-ttu-id="b8540-121">Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="b8540-121">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="b8540-122">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="b8540-122">Select **Create**.</span></span>

   <span data-ttu-id="b8540-123">7.&nbsp;emin **.NET Core** ve **ASP.NET Core 3.0** üstünde seçilir.</span><span class="sxs-lookup"><span data-stu-id="b8540-123">7.&nbsp;Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>

   <span data-ttu-id="b8540-124">8.&nbsp;Blazor istemci-tarafı bir deneyim için seçin **Blazor (istemci-tarafı)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="b8540-124">8.&nbsp;For an experience with Blazor client-side, choose the **Blazor (client-side)** template.</span></span> <span data-ttu-id="b8540-125">Sunucu tarafı Blazor bir deneyim için seçin **Blazor (sunucu tarafı)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="b8540-125">For an experience with Blazor server-side, choose the **Blazor (server-side)** template.</span></span> <span data-ttu-id="b8540-126">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="b8540-126">Select **Create**.</span></span>

   <span data-ttu-id="b8540-127">9.&nbsp;tuşuna **F5** uygulamayı çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="b8540-127">9.&nbsp;Press **F5** to run the app.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b8540-128">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8540-128">Visual Studio Code</span></span>](#tab/visual-studio-code)
   
   <span data-ttu-id="b8540-129">1.&nbsp;yükleme [Visual Studio Code'u](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="b8540-129">1.&nbsp;Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="b8540-130">2.&nbsp;en son yükleme [ C# Visual Studio Code uzantısı için](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="b8540-130">2.&nbsp;Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="b8540-131">3.&nbsp;Blazor istemci-tarafı bir deneyim için bir komut kabuğu'ndan aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="b8540-131">3.&nbsp;For an experience with Blazor client-side, execute the following command from a command shell:</span></span>

      ```console
      dotnet new blazor -o WebApplication1
      ```

      <span data-ttu-id="b8540-132">Sunucu tarafı Blazor bir deneyim için bir komut kabuğu'ndan aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="b8540-132">For an experience with Blazor server-side, execute the following command from a command shell:</span></span>

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      > [!NOTE]
      > <span data-ttu-id="b8540-133">Yalnızca Blazor istemci-tarafı macOS ASP.NET Core 3.0 Preview 4'te desteklenir.</span><span class="sxs-lookup"><span data-stu-id="b8540-133">Only Blazor client-side is supported on macOS in ASP.NET Core 3.0 Preview 4.</span></span> <span data-ttu-id="b8540-134">Daha fazla bilgi için [Blazor sunucu tarafı: çalıştırma dotnet macos'ta InvalidOperationException ile başarısız oluyor](https://github.com/aspnet/AspNetCore/issues/9402).</span><span class="sxs-lookup"><span data-stu-id="b8540-134">For more information, see [Blazor server side: dotnet run fails with InvalidOperationException on MacOS](https://github.com/aspnet/AspNetCore/issues/9402).</span></span>

   <span data-ttu-id="b8540-135">4.&nbsp;açık *WebApplication1* Visual Studio code'da klasörü.</span><span class="sxs-lookup"><span data-stu-id="b8540-135">4.&nbsp;Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="b8540-136">5.&nbsp;Visual Studio yapı ve proje hatalarını ayıklamaya, seçmek için gerekli varlıkları eklemek için kod tarafından Blazor sunucu tarafı projesi için istendiğinde **Evet**.</span><span class="sxs-lookup"><span data-stu-id="b8540-136">5.&nbsp;When prompted by Visual Studio Code for a Blazor server-side project to add required assets to build and debug the project, select **Yes**.</span></span>

   <span data-ttu-id="b8540-137">6.&nbsp;Blazor sunucu-tarafı uygulaması kullanıyorsanız, Visual Studio Code hata ayıklayıcıyı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b8540-137">6.&nbsp;If using a Blazor server-side app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="b8540-138">Blazor istemci-tarafı uygulaması kullanıyorsanız yürütme `dotnet run` uygulama proje klasöründen.</span><span class="sxs-lookup"><span data-stu-id="b8540-138">If using a Blazor client-side app, execute `dotnet run` from the app's project folder.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1.&nbsp;Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2.&nbsp;Select **File** > **New Solution** or **New Project**.

   3.&nbsp;In the sidebar, select **.NET Core** > **App**.

   4.&nbsp;For an experience with Blazor server-side, select the **ASP.NET Core Blazor (server-side)** template. For an experience with Blazor server-side, select the **ASP.NET Core Blazor (client-side)** template. Select **Next**.

   5.&nbsp;The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6.&nbsp;In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7.&nbsp;Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b8540-139">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b8540-139">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="b8540-140">İstemci tarafı Blazor bir deneyim için bir komut kabuğu'ndan aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="b8540-140">For an experience with Blazor client-side, execute the following commands from a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="b8540-141">Sunucu tarafı Blazor bir deneyim için bir komut kabuğu'ndan aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="b8540-141">For an experience with Blazor server-side, execute the following commands from a command shell:</span></span>

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   > [!NOTE]
   > <span data-ttu-id="b8540-142">MacOS üzerinde bir Blazor istemci-tarafı uygulaması kullanın.</span><span class="sxs-lookup"><span data-stu-id="b8540-142">On macOS, use a Blazor client-side app.</span></span> <span data-ttu-id="b8540-143">Sunucu tarafı Blazor macOS üzerinde ASP.NET Core 3.0 Preview 4 için desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="b8540-143">Blazor server-side isn't supported for macOS on ASP.NET Core 3.0 Preview 4.</span></span> <span data-ttu-id="b8540-144">Daha fazla bilgi için [Blazor sunucu tarafı: çalıştırma dotnet macos'ta InvalidOperationException ile başarısız oluyor](https://github.com/aspnet/AspNetCore/issues/9402).</span><span class="sxs-lookup"><span data-stu-id="b8540-144">For more information, see [Blazor server side: dotnet run fails with InvalidOperationException on MacOS](https://github.com/aspnet/AspNetCore/issues/9402).</span></span>

   ---

<span data-ttu-id="b8540-145">Bir tarayıcıda gidin `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="b8540-145">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="b8540-146">Birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="b8540-146">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="b8540-147">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="b8540-147">Home</span></span>
* <span data-ttu-id="b8540-148">Sayaç</span><span class="sxs-lookup"><span data-stu-id="b8540-148">Counter</span></span>
* <span data-ttu-id="b8540-149">Veri getirme</span><span class="sxs-lookup"><span data-stu-id="b8540-149">Fetch data</span></span>

<span data-ttu-id="b8540-150">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="b8540-150">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="b8540-151">Normal olarak artan bir Web sayfasındaki bir sayaç gerekli JavaScript Yazma, ancak Razor bileşenleri sağlamak daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="b8540-151">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor components provide a better approach using C#.</span></span>

<span data-ttu-id="b8540-152">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="b8540-152">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

<span data-ttu-id="b8540-153">Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur.</span><span class="sxs-lookup"><span data-stu-id="b8540-153">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="b8540-154">Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.</span><span class="sxs-lookup"><span data-stu-id="b8540-154">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="b8540-155">Her zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="b8540-155">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="b8540-156">`onclick` Olay tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="b8540-156">The `onclick` event is fired.</span></span>
* <span data-ttu-id="b8540-157">`IncrementCount` Yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b8540-157">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="b8540-158">`currentCount` Artırılır.</span><span class="sxs-lookup"><span data-stu-id="b8540-158">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="b8540-159">Bileşeni yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b8540-159">The component is rendered again.</span></span>

<span data-ttu-id="b8540-160">Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b8540-160">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="b8540-161">Bir bileşen başka bir bileşene bir HTML benzeri sözdizimi kullanarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b8540-161">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="b8540-162">Bileşen parametreleri, öznitelikleri veya alt içeriğin kullanarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="b8540-162">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="b8540-163">Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="b8540-163">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="b8540-164">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="b8540-164">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="b8540-165">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b8540-165">Run the app.</span></span> <span data-ttu-id="b8540-166">Giriş sayfası, kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="b8540-166">The homepage has its own counter.</span></span>

<span data-ttu-id="b8540-167">Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:</span><span class="sxs-lookup"><span data-stu-id="b8540-167">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="b8540-168">Bir özelliği için ekleme `IncrementAmount` ile donatılmış `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="b8540-168">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="b8540-169">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="b8540-169">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="b8540-170">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="b8540-170">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4-5,9)]

<span data-ttu-id="b8540-171">Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="b8540-171">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="b8540-172">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="b8540-172">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

<span data-ttu-id="b8540-173">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b8540-173">Run the app.</span></span> <span data-ttu-id="b8540-174">Giriş sayfası on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="b8540-174">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8540-175">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="b8540-175">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="b8540-176">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b8540-176">Additional resources</span></span>

* <xref:signalr/introduction>
