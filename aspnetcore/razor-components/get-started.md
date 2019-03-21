---
title: Razor bileşenleri ile çalışmaya başlama
author: guardrex
description: Razor bileşenler oluşturma ve Razor bileşenleri projesini değiştirme kullanmaya nasıl başlayacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2019
uid: razor-components/get-started
ms.openlocfilehash: 026bc5b3222a8ffa35a064bef8bbf64834b67a90
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209698"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="16d38-103">Razor bileşenleri ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="16d38-103">Get started with Razor Components</span></span>

<span data-ttu-id="16d38-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="16d38-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="16d38-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16d38-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="16d38-106">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="16d38-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="16d38-107">Visual Studio'da ilk Razor bileşenleri projenizi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="16d38-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="16d38-108">Seçin **dosya** > **yeni proje** > **Web** > **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="16d38-108">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="16d38-109">Emin **.NET Core** ve **ASP.NET Core 3.0** üstünde seçilir.</span><span class="sxs-lookup"><span data-stu-id="16d38-109">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="16d38-110">Seçin **Razor bileşenleri** şablonu seçip alt **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="16d38-110">Choose the **Razor Components** template and select **OK**.</span></span>
1. <span data-ttu-id="16d38-111">Tuşuna **F5** uygulamayı çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="16d38-111">Press **F5** to run the app.</span></span>

<span data-ttu-id="16d38-112">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="16d38-112">Congratulations!</span></span> <span data-ttu-id="16d38-113">Yalnızca ilk Razor bileşenleri uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="16d38-113">You just ran your first Razor Components app!</span></span>

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

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="16d38-114">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="16d38-114">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="16d38-115">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="16d38-115">Prerequisites:</span></span>

* [<span data-ttu-id="16d38-116">.NET core SDK 3.0 Önizleme</span><span class="sxs-lookup"><span data-stu-id="16d38-116">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="16d38-117">Bir komut kabuğu'ndan ilk Razor bileşenleri projenizi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="16d38-117">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="16d38-118">Bir tarayıcıda gidin `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="16d38-118">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="16d38-119">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="16d38-119">Congratulations!</span></span> <span data-ttu-id="16d38-120">Yalnızca ilk Razor bileşenleri uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="16d38-120">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="16d38-121">Razor bileşenleri proje</span><span class="sxs-lookup"><span data-stu-id="16d38-121">Razor Components project</span></span>

<span data-ttu-id="16d38-122">Razor bileşenleri, Razor sözdizimi kullanılarak yazılan ancak Razor sayfaları ve MVC görünümleri farklı derlenir.</span><span class="sxs-lookup"><span data-stu-id="16d38-122">Razor Components are authored using Razor syntax but are compiled differently than Razor Pages and MVC views.</span></span> <span data-ttu-id="16d38-123">*.Razor* dosya uzantısı, Razor bileşen belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="16d38-123">The *.razor* file extension is used to specify a Razor Component.</span></span> <span data-ttu-id="16d38-124">Razor sayfaları ve MVC görünümleri kullanmaya devam *.cshtml* dosya uzantısı.</span><span class="sxs-lookup"><span data-stu-id="16d38-124">Razor Pages and MVC views continue to use the *.cshtml* file extension.</span></span>

> [!NOTE]
> <span data-ttu-id="16d38-125">Razor bileşenlerini kullanarak yazarı olduğu *.cshtml* dosyaları kullanarak Razor bileşen dosyaları tanımlanmış olduğu sürece dosya uzantısı `_RazorComponentInclude` MSBuild özelliği.</span><span class="sxs-lookup"><span data-stu-id="16d38-125">Razor Components can be authored using the *.cshtml* file extension as long as those files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="16d38-126">Örneğin, Razor bileşen şablonu kullanılarak oluşturulan bir uygulamayı belirtir tüm *.cshtml* altında dosyaları *bileşenleri* klasör Razor bileşenleri olarak kabul:</span><span class="sxs-lookup"><span data-stu-id="16d38-126">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components:</span></span>
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

<span data-ttu-id="16d38-127">Uygulamayı çalıştırdığınızda, birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="16d38-127">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="16d38-128">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="16d38-128">Home</span></span>
* <span data-ttu-id="16d38-129">Sayaç</span><span class="sxs-lookup"><span data-stu-id="16d38-129">Counter</span></span>
* <span data-ttu-id="16d38-130">Veri getirme</span><span class="sxs-lookup"><span data-stu-id="16d38-130">Fetch data</span></span>

<span data-ttu-id="16d38-131">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="16d38-131">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="16d38-132">Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektiriyor, ancak Razor bileşenleri sağlayan daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="16d38-132">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="16d38-133">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="16d38-133">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

<span data-ttu-id="16d38-134">Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur.</span><span class="sxs-lookup"><span data-stu-id="16d38-134">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="16d38-135">Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.</span><span class="sxs-lookup"><span data-stu-id="16d38-135">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="16d38-136">Her zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="16d38-136">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="16d38-137">`onclick` Olay tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="16d38-137">The `onclick` event is fired.</span></span>
* <span data-ttu-id="16d38-138">`IncrementCount` Yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="16d38-138">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="16d38-139">`currentCount` Artırılır.</span><span class="sxs-lookup"><span data-stu-id="16d38-139">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="16d38-140">Bileşeni yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="16d38-140">The component is rendered again.</span></span>

<span data-ttu-id="16d38-141">Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="16d38-141">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="16d38-142">Bir bileşen başka bir bileşene bir HTML benzeri sözdizimi kullanarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="16d38-142">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="16d38-143">Bileşen parametreleri, öznitelikleri veya alt içeriğin kullanarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="16d38-143">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="16d38-144">Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="16d38-144">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="16d38-145">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="16d38-145">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="16d38-146">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="16d38-146">Run the app.</span></span> <span data-ttu-id="16d38-147">Giriş sayfası, kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="16d38-147">The homepage has its own counter.</span></span>

<span data-ttu-id="16d38-148">Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:</span><span class="sxs-lookup"><span data-stu-id="16d38-148">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="16d38-149">Bir özelliği için ekleme `IncrementAmount` ile donatılmış `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="16d38-149">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="16d38-150">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="16d38-150">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="16d38-151">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="16d38-151">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4,8)]

<span data-ttu-id="16d38-152">Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="16d38-152">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="16d38-153">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="16d38-153">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

<span data-ttu-id="16d38-154">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="16d38-154">Run the app.</span></span> <span data-ttu-id="16d38-155">Giriş sayfası on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="16d38-155">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="16d38-156">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="16d38-156">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
