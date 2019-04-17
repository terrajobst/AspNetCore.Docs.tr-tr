---
title: Blazor ile çalışmaya başlama
author: guardrex
description: Blazor ile çalışmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/get-started
ms.openlocfilehash: cc68e1c58af607f840b952f033d3f3b0e54e6cf4
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614912"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="f228e-103">Blazor ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="f228e-103">Get started with Blazor</span></span>

<span data-ttu-id="f228e-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f228e-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f228e-105">Aşağıdaki deneyimleri birini uygulayarak Blazor ile kullanmaya başlayın:</span><span class="sxs-lookup"><span data-stu-id="f228e-105">Get started with Blazor following either of the following experiences:</span></span>

* [<span data-ttu-id="f228e-106">Sunucu tarafı Razor bileşenleri</span><span class="sxs-lookup"><span data-stu-id="f228e-106">Server-side Razor Components</span></span>](#server-side-razor-components-experience)
* [<span data-ttu-id="f228e-107">İstemci tarafı Blazor</span><span class="sxs-lookup"><span data-stu-id="f228e-107">Client-side Blazor</span></span>](#client-side-blazor-experience)

## <a name="server-side-razor-components-experience"></a><span data-ttu-id="f228e-108">Sunucu tarafı Razor bileşenleri deneyimi</span><span class="sxs-lookup"><span data-stu-id="f228e-108">Server-side Razor Components experience</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f228e-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f228e-109">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f228e-110">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="f228e-110">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="f228e-111">Visual Studio'da ilk Razor bileşenleri projenizi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="f228e-111">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="f228e-112">Son yükleme [.NET Core 3.0 Önizleme SDK'sı](https://dotnet.microsoft.com/download/dotnet-core/3.0) bırakın.</span><span class="sxs-lookup"><span data-stu-id="f228e-112">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="f228e-113">Visual Studio Önizleme SDK'ları kullanmak etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="f228e-113">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="f228e-114">Açık **Araçları** > **seçenekleri** menü çubuğundaki.</span><span class="sxs-lookup"><span data-stu-id="f228e-114">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="f228e-115">Açık **projeler ve çözümler** düğümü.</span><span class="sxs-lookup"><span data-stu-id="f228e-115">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="f228e-116">Açık **.NET Core** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="f228e-116">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="f228e-117">İçin kutuyu **önizlemeleri .NET Core SDK'sını kullanma**.</span><span class="sxs-lookup"><span data-stu-id="f228e-117">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="f228e-118">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="f228e-118">Select **OK**.</span></span>
1. <span data-ttu-id="f228e-119">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f228e-119">Create a new project.</span></span>
1. <span data-ttu-id="f228e-120">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="f228e-120">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="f228e-121">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="f228e-121">Select **Next**.</span></span>
1. <span data-ttu-id="f228e-122">Bir ad sağlayın **proje adı** alan.</span><span class="sxs-lookup"><span data-stu-id="f228e-122">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="f228e-123">Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="f228e-123">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="f228e-124">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="f228e-124">Select **Create**.</span></span>
1. <span data-ttu-id="f228e-125">Emin **.NET Core** ve **ASP.NET Core 3.0** üstünde seçilir.</span><span class="sxs-lookup"><span data-stu-id="f228e-125">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="f228e-126">Seçin **Razor bileşenleri** şablonu seçip alt **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="f228e-126">Choose the **Razor Components** template and select **Create**.</span></span>
1. <span data-ttu-id="f228e-127">Tuşuna **F5** uygulamayı çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="f228e-127">Press **F5** to run the app.</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:blazor/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

-->

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f228e-128">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f228e-128">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="f228e-129">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="f228e-129">Prerequisites:</span></span>

* [<span data-ttu-id="f228e-130">.NET core SDK 3.0 Önizleme</span><span class="sxs-lookup"><span data-stu-id="f228e-130">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="f228e-131">Bir komut kabuğu'ndan ilk Razor bileşenleri projenizi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="f228e-131">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="f228e-132">Bir tarayıcıda gidin `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="f228e-132">In a browser, navigate to `https://localhost:5001`.</span></span>

---

<span data-ttu-id="f228e-133">Razor bileşenleri, Razor sözdizimi kullanılarak yazılan ancak Razor sayfaları ve MVC görünümleri farklı derlenir.</span><span class="sxs-lookup"><span data-stu-id="f228e-133">Razor Components are authored using Razor syntax but are compiled differently than Razor Pages and MVC views.</span></span> <span data-ttu-id="f228e-134">*.Razor* dosya uzantısı, Razor bileşen belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f228e-134">The *.razor* file extension is used to specify a Razor Component.</span></span> <span data-ttu-id="f228e-135">Razor sayfaları ve MVC görünümleri kullanmaya devam *.cshtml* dosya uzantısı.</span><span class="sxs-lookup"><span data-stu-id="f228e-135">Razor Pages and MVC views continue to use the *.cshtml* file extension.</span></span>

> [!NOTE]
> <span data-ttu-id="f228e-136">Razor bileşenlerini kullanarak yazarı olduğu *.cshtml* dosyaları kullanarak Razor bileşen dosyaları tanımlanmış olduğu sürece dosya uzantısı `_RazorComponentInclude` MSBuild özelliği.</span><span class="sxs-lookup"><span data-stu-id="f228e-136">Razor Components can be authored using the *.cshtml* file extension as long as those files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="f228e-137">Örneğin, Razor bileşen şablonu kullanılarak oluşturulan bir uygulamayı belirtir tüm *.cshtml* altında dosyaları *bileşenleri* klasör Razor bileşenleri olarak kabul:</span><span class="sxs-lookup"><span data-stu-id="f228e-137">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components:</span></span>
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

<span data-ttu-id="f228e-138">Uygulamayı çalıştırdığınızda, birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="f228e-138">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="f228e-139">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="f228e-139">Home</span></span>
* <span data-ttu-id="f228e-140">Sayaç</span><span class="sxs-lookup"><span data-stu-id="f228e-140">Counter</span></span>
* <span data-ttu-id="f228e-141">Veri getirme</span><span class="sxs-lookup"><span data-stu-id="f228e-141">Fetch data</span></span>

<span data-ttu-id="f228e-142">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="f228e-142">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="f228e-143">Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektiriyor, ancak Razor bileşenleri sağlayan daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="f228e-143">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="f228e-144">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="f228e-144">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

<span data-ttu-id="f228e-145">Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur.</span><span class="sxs-lookup"><span data-stu-id="f228e-145">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="f228e-146">Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.</span><span class="sxs-lookup"><span data-stu-id="f228e-146">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="f228e-147">Her zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="f228e-147">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="f228e-148">`onclick` Olay tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="f228e-148">The `onclick` event is fired.</span></span>
* <span data-ttu-id="f228e-149">`IncrementCount` Yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="f228e-149">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="f228e-150">`currentCount` Artırılır.</span><span class="sxs-lookup"><span data-stu-id="f228e-150">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="f228e-151">Bileşeni yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f228e-151">The component is rendered again.</span></span>

<span data-ttu-id="f228e-152">Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="f228e-152">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="f228e-153">Bir bileşen başka bir bileşene bir HTML benzeri sözdizimi kullanarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f228e-153">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="f228e-154">Bileşen parametreleri, öznitelikleri veya alt içeriğin kullanarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="f228e-154">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="f228e-155">Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="f228e-155">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="f228e-156">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="f228e-156">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="f228e-157">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f228e-157">Run the app.</span></span> <span data-ttu-id="f228e-158">Giriş sayfası, kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="f228e-158">The homepage has its own counter.</span></span>

<span data-ttu-id="f228e-159">Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:</span><span class="sxs-lookup"><span data-stu-id="f228e-159">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="f228e-160">Bir özelliği için ekleme `IncrementAmount` ile donatılmış `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f228e-160">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="f228e-161">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="f228e-161">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="f228e-162">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="f228e-162">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4-5,9)]

<span data-ttu-id="f228e-163">Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="f228e-163">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="f228e-164">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="f228e-164">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

<span data-ttu-id="f228e-165">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f228e-165">Run the app.</span></span> <span data-ttu-id="f228e-166">Giriş sayfası on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="f228e-166">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="client-side-blazor-experience"></a><span data-ttu-id="f228e-167">İstemci tarafı Blazor deneyimi</span><span class="sxs-lookup"><span data-stu-id="f228e-167">Client-side Blazor experience</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f228e-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f228e-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f228e-169">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="f228e-169">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="f228e-170">Visual Studio'da ilk Blazor projenizi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="f228e-170">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="f228e-171">Son yükleme [.NET Core 3.0 Önizleme SDK'sı](https://dotnet.microsoft.com/download/dotnet-core/3.0) bırakın.</span><span class="sxs-lookup"><span data-stu-id="f228e-171">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="f228e-172">Visual Studio Önizleme SDK'ları kullanmak etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="f228e-172">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="f228e-173">Açık **Araçları** > **seçenekleri** menü çubuğundaki.</span><span class="sxs-lookup"><span data-stu-id="f228e-173">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="f228e-174">Açık **projeler ve çözümler** düğümü.</span><span class="sxs-lookup"><span data-stu-id="f228e-174">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="f228e-175">Açık **.NET Core** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="f228e-175">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="f228e-176">İçin kutuyu **önizlemeleri .NET Core SDK'sını kullanma**.</span><span class="sxs-lookup"><span data-stu-id="f228e-176">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="f228e-177">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="f228e-177">Select **OK**.</span></span>
1. <span data-ttu-id="f228e-178">Son yükleme [Blazor uzantısı](https://go.microsoft.com/fwlink/?linkid=870389) Visual Studio Market'ten.</span><span class="sxs-lookup"><span data-stu-id="f228e-178">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="f228e-179">Bu adım Blazor şablonları Visual Studio için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="f228e-179">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="f228e-180">Bir komut kabuğu'nda aşağıdaki komutu çalıştırarak Blazor şablonları .NET Core CLI ile kullanılabilir duruma getir:</span><span class="sxs-lookup"><span data-stu-id="f228e-180">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="f228e-181">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f228e-181">Create a new project.</span></span>
1. <span data-ttu-id="f228e-182">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="f228e-182">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="f228e-183">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="f228e-183">Select **Next**.</span></span>
1. <span data-ttu-id="f228e-184">Bir ad sağlayın **proje adı** alan.</span><span class="sxs-lookup"><span data-stu-id="f228e-184">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="f228e-185">Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="f228e-185">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="f228e-186">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="f228e-186">Select **Create**.</span></span>
1. <span data-ttu-id="f228e-187">Emin **.NET Core** ve **ASP.NET Core 3.0** üstünde seçilir.</span><span class="sxs-lookup"><span data-stu-id="f228e-187">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="f228e-188">Seçin **Blazor** şablonu seçip alt **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="f228e-188">Select the **Blazor** template and select **Create**.</span></span>
1. <span data-ttu-id="f228e-189">Tuşuna **F5** uygulamayı çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="f228e-189">Press **F5** to run the app.</span></span>

<span data-ttu-id="f228e-190">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="f228e-190">Congratulations!</span></span> <span data-ttu-id="f228e-191">Yalnızca ilk Blazor uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="f228e-191">You just ran your first Blazor app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Blazor project in Visual Studio Code:

1. Execute the following command in a command shell:

   ```console
   dotnet new blazor -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Visual Studio code offers to create assets to build and debug the app, which includes the *tasks.json* and *launch.json* files. Select **Yes** to add the assets.

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Blazor app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:blazor/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Blazor project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **Blazor** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Blazor app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f228e-192">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f228e-192">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="f228e-193">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="f228e-193">Prerequisites:</span></span>

* [<span data-ttu-id="f228e-194">.NET core SDK 3.0 Önizleme</span><span class="sxs-lookup"><span data-stu-id="f228e-194">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="f228e-195">Bir komut kabuğu'nda aşağıdaki komutu çalıştırarak Blazor şablonları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f228e-195">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="f228e-196">Bir komut kabuğu'nda ilk Blazor projenizi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="f228e-196">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="f228e-197">Bir tarayıcıda gidin `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="f228e-197">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="f228e-198">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="f228e-198">Congratulations!</span></span> <span data-ttu-id="f228e-199">Yalnızca ilk Blazor uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="f228e-199">You just ran your first Blazor app!</span></span>

---

<span data-ttu-id="f228e-200">Uygulamayı çalıştırdığınızda, birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="f228e-200">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="f228e-201">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="f228e-201">Home</span></span>
* <span data-ttu-id="f228e-202">Sayaç</span><span class="sxs-lookup"><span data-stu-id="f228e-202">Counter</span></span>
* <span data-ttu-id="f228e-203">Veri getirme</span><span class="sxs-lookup"><span data-stu-id="f228e-203">Fetch data</span></span>

<span data-ttu-id="f228e-204">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="f228e-204">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="f228e-205">Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektirir, ancak Blazor sağlar daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="f228e-205">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="f228e-206">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f228e-206">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="f228e-207">Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur.</span><span class="sxs-lookup"><span data-stu-id="f228e-207">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="f228e-208">Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.</span><span class="sxs-lookup"><span data-stu-id="f228e-208">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="f228e-209">Her zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="f228e-209">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="f228e-210">`onclick` Olay tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="f228e-210">The `onclick` event is fired.</span></span>
* <span data-ttu-id="f228e-211">`IncrementCount` Yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="f228e-211">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="f228e-212">`currentCount` Artırılır.</span><span class="sxs-lookup"><span data-stu-id="f228e-212">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="f228e-213">Bileşeni yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f228e-213">The component is rendered again.</span></span>

<span data-ttu-id="f228e-214">Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="f228e-214">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="f228e-215">Bir bileşen başka bir bileşene bir HTML benzeri sözdizimi kullanarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f228e-215">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="f228e-216">Bileşen parametreleri, öznitelikleri veya alt içeriğin kullanarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="f228e-216">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="f228e-217">Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="f228e-217">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="f228e-218">İçinde *Pages/Index.cshtml*, anket istemi bileşen bir sayaç bileşeni ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="f228e-218">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="f228e-219">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f228e-219">Run the app.</span></span> <span data-ttu-id="f228e-220">Giriş sayfası, kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="f228e-220">The homepage has its own counter.</span></span>

<span data-ttu-id="f228e-221">Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:</span><span class="sxs-lookup"><span data-stu-id="f228e-221">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="f228e-222">Bir özelliği için ekleme `IncrementAmount` ile donatılmış `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f228e-222">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="f228e-223">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="f228e-223">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="f228e-224">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f228e-224">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4-5,9)]

<span data-ttu-id="f228e-225">Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="f228e-225">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="f228e-226">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f228e-226">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="f228e-227">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f228e-227">Run the app.</span></span> <span data-ttu-id="f228e-228">Giriş sayfası on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="f228e-228">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f228e-229">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="f228e-229">Next steps</span></span>

<xref:tutorials/first-blazor-app>
