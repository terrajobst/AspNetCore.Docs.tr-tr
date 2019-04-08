---
title: Razor bileşenleri ile çalışmaya başlama
author: guardrex
description: Razor bileşenler oluşturma ve Razor bileşenleri projesini değiştirme kullanmaya nasıl başlayacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/get-started
ms.openlocfilehash: 151e58497b0f22fa7c5a9bde1f665eeb73fd5dc3
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068150"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="4c002-103">Razor bileşenleri ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="4c002-103">Get started with Razor Components</span></span>

<span data-ttu-id="4c002-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4c002-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# [<a name="visual-studio"></a><span data-ttu-id="4c002-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4c002-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4c002-106">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="4c002-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="4c002-107">Visual Studio'da ilk Razor bileşenleri projenizi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="4c002-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="4c002-108">Son yükleme [.NET Core 3.0 Önizleme SDK'sı](https://dotnet.microsoft.com/download/dotnet-core/3.0) bırakın.</span><span class="sxs-lookup"><span data-stu-id="4c002-108">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="4c002-109">Visual Studio Önizleme SDK'ları kullanmak etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="4c002-109">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="4c002-110">Açık **Araçları** > **seçenekleri** menü çubuğundaki.</span><span class="sxs-lookup"><span data-stu-id="4c002-110">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="4c002-111">Açık **projeler ve çözümler** düğümü.</span><span class="sxs-lookup"><span data-stu-id="4c002-111">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="4c002-112">Açık **.NET Core** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="4c002-112">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="4c002-113">İçin kutuyu **önizlemeleri .NET Core SDK'sını kullanma**.</span><span class="sxs-lookup"><span data-stu-id="4c002-113">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="4c002-114">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="4c002-114">Select **OK**.</span></span>
1. <span data-ttu-id="4c002-115">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4c002-115">Create a new project.</span></span>
1. <span data-ttu-id="4c002-116">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="4c002-116">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="4c002-117">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4c002-117">Select **Next**.</span></span>
1. <span data-ttu-id="4c002-118">Bir ad sağlayın **proje adı** alan.</span><span class="sxs-lookup"><span data-stu-id="4c002-118">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="4c002-119">Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="4c002-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="4c002-120">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="4c002-120">Select **Create**.</span></span>
1. <span data-ttu-id="4c002-121">Emin **.NET Core** ve **ASP.NET Core 3.0** üstünde seçilir.</span><span class="sxs-lookup"><span data-stu-id="4c002-121">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="4c002-122">Seçin **Razor bileşenleri** şablonu seçip alt **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="4c002-122">Choose the **Razor Components** template and select **Create**.</span></span>
1. <span data-ttu-id="4c002-123">Tuşuna **F5** uygulamayı çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="4c002-123">Press **F5** to run the app.</span></span>

<span data-ttu-id="4c002-124">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="4c002-124">Congratulations!</span></span> <span data-ttu-id="4c002-125">Yalnızca ilk Razor bileşenleri uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="4c002-125">You just ran your first Razor Components app!</span></span>

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

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# [<a name="net-core-cli"></a><span data-ttu-id="4c002-126">.NET core CLI</span><span class="sxs-lookup"><span data-stu-id="4c002-126">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="4c002-127">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="4c002-127">Prerequisites:</span></span>

* [<span data-ttu-id="4c002-128">.NET core SDK 3.0 Önizleme</span><span class="sxs-lookup"><span data-stu-id="4c002-128">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="4c002-129">Bir komut kabuğu'ndan ilk Razor bileşenleri projenizi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="4c002-129">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="4c002-130">Bir tarayıcıda gidin `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="4c002-130">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="4c002-131">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="4c002-131">Congratulations!</span></span> <span data-ttu-id="4c002-132">Yalnızca ilk Razor bileşenleri uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="4c002-132">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="4c002-133">Razor bileşenleri proje</span><span class="sxs-lookup"><span data-stu-id="4c002-133">Razor Components project</span></span>

<span data-ttu-id="4c002-134">Razor bileşenleri, Razor sözdizimi kullanılarak yazılan ancak Razor sayfaları ve MVC görünümleri farklı derlenir.</span><span class="sxs-lookup"><span data-stu-id="4c002-134">Razor Components are authored using Razor syntax but are compiled differently than Razor Pages and MVC views.</span></span> <span data-ttu-id="4c002-135">*.Razor* dosya uzantısı, Razor bileşen belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c002-135">The *.razor* file extension is used to specify a Razor Component.</span></span> <span data-ttu-id="4c002-136">Razor sayfaları ve MVC görünümleri kullanmaya devam *.cshtml* dosya uzantısı.</span><span class="sxs-lookup"><span data-stu-id="4c002-136">Razor Pages and MVC views continue to use the *.cshtml* file extension.</span></span>

> [!NOTE]
> <span data-ttu-id="4c002-137">Razor bileşenlerini kullanarak yazarı olduğu *.cshtml* dosyaları kullanarak Razor bileşen dosyaları tanımlanmış olduğu sürece dosya uzantısı `_RazorComponentInclude` MSBuild özelliği.</span><span class="sxs-lookup"><span data-stu-id="4c002-137">Razor Components can be authored using the *.cshtml* file extension as long as those files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="4c002-138">Örneğin, Razor bileşen şablonu kullanılarak oluşturulan bir uygulamayı belirtir tüm *.cshtml* altında dosyaları *bileşenleri* klasör Razor bileşenleri olarak kabul:</span><span class="sxs-lookup"><span data-stu-id="4c002-138">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components:</span></span>
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

<span data-ttu-id="4c002-139">Uygulamayı çalıştırdığınızda, birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="4c002-139">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="4c002-140">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="4c002-140">Home</span></span>
* <span data-ttu-id="4c002-141">Sayaç</span><span class="sxs-lookup"><span data-stu-id="4c002-141">Counter</span></span>
* <span data-ttu-id="4c002-142">Veri getirme</span><span class="sxs-lookup"><span data-stu-id="4c002-142">Fetch data</span></span>

<span data-ttu-id="4c002-143">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="4c002-143">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="4c002-144">Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektiriyor, ancak Razor bileşenleri sağlayan daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="4c002-144">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="4c002-145">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="4c002-145">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

<span data-ttu-id="4c002-146">Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur.</span><span class="sxs-lookup"><span data-stu-id="4c002-146">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="4c002-147">Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.</span><span class="sxs-lookup"><span data-stu-id="4c002-147">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="4c002-148">Her zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="4c002-148">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="4c002-149">`onclick` Olay tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="4c002-149">The `onclick` event is fired.</span></span>
* <span data-ttu-id="4c002-150">`IncrementCount` Yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4c002-150">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="4c002-151">`currentCount` Artırılır.</span><span class="sxs-lookup"><span data-stu-id="4c002-151">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="4c002-152">Bileşeni yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4c002-152">The component is rendered again.</span></span>

<span data-ttu-id="4c002-153">Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4c002-153">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="4c002-154">Bir bileşen başka bir bileşene bir HTML benzeri sözdizimi kullanarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4c002-154">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="4c002-155">Bileşen parametreleri, öznitelikleri veya alt içeriğin kullanarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="4c002-155">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="4c002-156">Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="4c002-156">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="4c002-157">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="4c002-157">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="4c002-158">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4c002-158">Run the app.</span></span> <span data-ttu-id="4c002-159">Giriş sayfası, kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="4c002-159">The homepage has its own counter.</span></span>

<span data-ttu-id="4c002-160">Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:</span><span class="sxs-lookup"><span data-stu-id="4c002-160">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="4c002-161">Bir özelliği için ekleme `IncrementAmount` ile donatılmış `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4c002-161">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="4c002-162">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="4c002-162">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="4c002-163">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="4c002-163">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4,8)]

<span data-ttu-id="4c002-164">Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="4c002-164">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="4c002-165">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="4c002-165">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

<span data-ttu-id="4c002-166">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4c002-166">Run the app.</span></span> <span data-ttu-id="4c002-167">Giriş sayfası on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="4c002-167">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c002-168">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="4c002-168">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
