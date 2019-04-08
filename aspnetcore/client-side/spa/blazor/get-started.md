---
title: Blazor ile çalışmaya başlama
author: guardrex
description: Oluşturma ve değiştirme Blazor proje Blazor ile çalışmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: spa/blazor/get-started
ms.openlocfilehash: b3928c2812be6f34cdf2f17295a1251106f651e5
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068241"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="92c7d-103">Blazor ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="92c7d-103">Get started with Blazor</span></span>

<span data-ttu-id="92c7d-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="92c7d-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# [<a name="visual-studio"></a><span data-ttu-id="92c7d-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="92c7d-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="92c7d-106">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="92c7d-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="92c7d-107">Visual Studio'da ilk Blazor projenizi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="92c7d-107">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="92c7d-108">Son yükleme [.NET Core 3.0 Önizleme SDK'sı](https://dotnet.microsoft.com/download/dotnet-core/3.0) bırakın.</span><span class="sxs-lookup"><span data-stu-id="92c7d-108">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="92c7d-109">Visual Studio Önizleme SDK'ları kullanmak etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="92c7d-109">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="92c7d-110">Açık **Araçları** > **seçenekleri** menü çubuğundaki.</span><span class="sxs-lookup"><span data-stu-id="92c7d-110">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="92c7d-111">Açık **projeler ve çözümler** düğümü.</span><span class="sxs-lookup"><span data-stu-id="92c7d-111">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="92c7d-112">Açık **.NET Core** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="92c7d-112">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="92c7d-113">İçin kutuyu **önizlemeleri .NET Core SDK'sını kullanma**.</span><span class="sxs-lookup"><span data-stu-id="92c7d-113">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="92c7d-114">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="92c7d-114">Select **OK**.</span></span>
1. <span data-ttu-id="92c7d-115">Son yükleme [Blazor uzantısı](https://go.microsoft.com/fwlink/?linkid=870389) Visual Studio Market'ten.</span><span class="sxs-lookup"><span data-stu-id="92c7d-115">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="92c7d-116">Bu adım Blazor şablonları Visual Studio için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="92c7d-116">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="92c7d-117">Bir komut kabuğu'nda aşağıdaki komutu çalıştırarak Blazor şablonları .NET Core CLI ile kullanılabilir duruma getir:</span><span class="sxs-lookup"><span data-stu-id="92c7d-117">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```
1. <span data-ttu-id="92c7d-118">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="92c7d-118">Create a new project.</span></span>
1. <span data-ttu-id="92c7d-119">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="92c7d-119">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="92c7d-120">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="92c7d-120">Select **Next**.</span></span>
1. <span data-ttu-id="92c7d-121">Bir ad sağlayın **proje adı** alan.</span><span class="sxs-lookup"><span data-stu-id="92c7d-121">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="92c7d-122">Onayla **konumu** giriş doğru olduğundan veya proje için bir konum sağlayın.</span><span class="sxs-lookup"><span data-stu-id="92c7d-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="92c7d-123">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="92c7d-123">Select **Create**.</span></span>
1. <span data-ttu-id="92c7d-124">Emin **.NET Core** ve **ASP.NET Core 3.0** üstünde seçilir.</span><span class="sxs-lookup"><span data-stu-id="92c7d-124">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="92c7d-125">Seçin **Blazor** şablonu seçip alt **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="92c7d-125">Select the **Blazor** template and select **Create**.</span></span>
1. <span data-ttu-id="92c7d-126">Tuşuna **F5** uygulamayı çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="92c7d-126">Press **F5** to run the app.</span></span>

<span data-ttu-id="92c7d-127">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="92c7d-127">Congratulations!</span></span> <span data-ttu-id="92c7d-128">Yalnızca ilk Blazor uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="92c7d-128">You just ran your first Blazor app!</span></span>

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

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

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

# [<a name="net-core-cli"></a><span data-ttu-id="92c7d-129">.NET core CLI</span><span class="sxs-lookup"><span data-stu-id="92c7d-129">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="92c7d-130">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="92c7d-130">Prerequisites:</span></span>

* [<span data-ttu-id="92c7d-131">.NET core SDK 3.0 Önizleme</span><span class="sxs-lookup"><span data-stu-id="92c7d-131">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="92c7d-132">Bir komut kabuğu'nda aşağıdaki komutu çalıştırarak Blazor şablonları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="92c7d-132">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="92c7d-133">Bir komut kabuğu'nda ilk Blazor projenizi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="92c7d-133">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="92c7d-134">Bir tarayıcıda gidin `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="92c7d-134">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="92c7d-135">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="92c7d-135">Congratulations!</span></span> <span data-ttu-id="92c7d-136">Yalnızca ilk Blazor uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="92c7d-136">You just ran your first Blazor app!</span></span>

---

## <a name="blazor-project"></a><span data-ttu-id="92c7d-137">Blazor proje</span><span class="sxs-lookup"><span data-stu-id="92c7d-137">Blazor project</span></span>

<span data-ttu-id="92c7d-138">Uygulamayı çalıştırdığınızda, birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="92c7d-138">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="92c7d-139">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="92c7d-139">Home</span></span>
* <span data-ttu-id="92c7d-140">Sayaç</span><span class="sxs-lookup"><span data-stu-id="92c7d-140">Counter</span></span>
* <span data-ttu-id="92c7d-141">Veri getirme</span><span class="sxs-lookup"><span data-stu-id="92c7d-141">Fetch data</span></span>

<span data-ttu-id="92c7d-142">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="92c7d-142">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="92c7d-143">Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektirir, ancak Blazor sağlar daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="92c7d-143">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="92c7d-144">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="92c7d-144">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="92c7d-145">Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur.</span><span class="sxs-lookup"><span data-stu-id="92c7d-145">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="92c7d-146">Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.</span><span class="sxs-lookup"><span data-stu-id="92c7d-146">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="92c7d-147">Her zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="92c7d-147">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="92c7d-148">`onclick` Olay tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="92c7d-148">The `onclick` event is fired.</span></span>
* <span data-ttu-id="92c7d-149">`IncrementCount` Yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="92c7d-149">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="92c7d-150">`currentCount` Artırılır.</span><span class="sxs-lookup"><span data-stu-id="92c7d-150">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="92c7d-151">Bileşeni yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="92c7d-151">The component is rendered again.</span></span>

<span data-ttu-id="92c7d-152">Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="92c7d-152">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="92c7d-153">Bir bileşen başka bir bileşene bir HTML benzeri sözdizimi kullanarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="92c7d-153">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="92c7d-154">Bileşen parametreleri, öznitelikleri veya alt içeriğin kullanarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="92c7d-154">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="92c7d-155">Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="92c7d-155">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="92c7d-156">İçinde *Pages/Index.cshtml*, anket istemi bileşen bir sayaç bileşeni ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="92c7d-156">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="92c7d-157">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="92c7d-157">Run the app.</span></span> <span data-ttu-id="92c7d-158">Giriş sayfası, kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="92c7d-158">The homepage has its own counter.</span></span>

<span data-ttu-id="92c7d-159">Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:</span><span class="sxs-lookup"><span data-stu-id="92c7d-159">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="92c7d-160">Bir özelliği için ekleme `IncrementAmount` ile donatılmış `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="92c7d-160">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="92c7d-161">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="92c7d-161">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="92c7d-162">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="92c7d-162">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="92c7d-163">Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="92c7d-163">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="92c7d-164">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="92c7d-164">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="92c7d-165">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="92c7d-165">Run the app.</span></span> <span data-ttu-id="92c7d-166">Giriş sayfası on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="92c7d-166">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="92c7d-167">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="92c7d-167">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
