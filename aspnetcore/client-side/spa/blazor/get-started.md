---
title: Blazor ile çalışmaya başlama
author: guardrex
description: Oluşturma ve değiştirme Blazor proje Blazor ile çalışmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: spa/blazor/get-started
ms.openlocfilehash: 667c57d536450fa2f8ae1cabc7c5a76a16d38a55
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665593"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="79d38-103">Blazor ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="79d38-103">Get started with Blazor</span></span>

<span data-ttu-id="79d38-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="79d38-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79d38-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79d38-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="79d38-106">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="79d38-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="79d38-107">Visual Studio'da ilk Blazor projenizi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="79d38-107">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="79d38-108">Son yükleme [Blazor dil Hizmetleri Uzantısı](https://go.microsoft.com/fwlink/?linkid=870389) Visual Studio Market'ten.</span><span class="sxs-lookup"><span data-stu-id="79d38-108">Install the latest [Blazor Language Services extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="79d38-109">Bu adım Blazor şablonları Visual Studio için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="79d38-109">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="79d38-110">Bir komut kabuğu'nda aşağıdaki komutu çalıştırarak Blazor şablonları .NET Core CLI ile kullanılabilir duruma getir:</span><span class="sxs-lookup"><span data-stu-id="79d38-110">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="79d38-111">Seçin **dosya** > **yeni proje** > **Web** > **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="79d38-111">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="79d38-112">Emin **.NET Core** ve **ASP.NET Core 3.0** üstünde seçilir.</span><span class="sxs-lookup"><span data-stu-id="79d38-112">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="79d38-113">Seçin **Blazor** şablonu seçip alt **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="79d38-113">Choose the **Blazor** template and select **OK**.</span></span>
1. <span data-ttu-id="79d38-114">Tuşuna **F5** uygulamayı çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="79d38-114">Press **F5** to run the app.</span></span>

<span data-ttu-id="79d38-115">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="79d38-115">Congratulations!</span></span> <span data-ttu-id="79d38-116">Yalnızca ilk Blazor uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="79d38-116">You just ran your first Blazor app!</span></span>

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

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="79d38-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="79d38-117">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="79d38-118">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="79d38-118">Prerequisites:</span></span>

* [<span data-ttu-id="79d38-119">.NET core SDK 3.0 Önizleme</span><span class="sxs-lookup"><span data-stu-id="79d38-119">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="79d38-120">Bir komut kabuğu'nda aşağıdaki komutu çalıştırarak Blazor şablonları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="79d38-120">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="79d38-121">Bir komut kabuğu'nda ilk Blazor projenizi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="79d38-121">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="79d38-122">Bir tarayıcıda gidin `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="79d38-122">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="79d38-123">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="79d38-123">Congratulations!</span></span> <span data-ttu-id="79d38-124">Yalnızca ilk Blazor uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="79d38-124">You just ran your first Blazor app!</span></span>

---

## <a name="blazor-project"></a><span data-ttu-id="79d38-125">Blazor proje</span><span class="sxs-lookup"><span data-stu-id="79d38-125">Blazor project</span></span>

<span data-ttu-id="79d38-126">Uygulamayı çalıştırdığınızda, birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="79d38-126">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="79d38-127">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="79d38-127">Home</span></span>
* <span data-ttu-id="79d38-128">Sayaç</span><span class="sxs-lookup"><span data-stu-id="79d38-128">Counter</span></span>
* <span data-ttu-id="79d38-129">Veri getirme</span><span class="sxs-lookup"><span data-stu-id="79d38-129">Fetch data</span></span>

<span data-ttu-id="79d38-130">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="79d38-130">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="79d38-131">Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektirir, ancak Blazor sağlar daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="79d38-131">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="79d38-132">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="79d38-132">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="79d38-133">Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur.</span><span class="sxs-lookup"><span data-stu-id="79d38-133">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="79d38-134">Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.</span><span class="sxs-lookup"><span data-stu-id="79d38-134">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="79d38-135">Her zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="79d38-135">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="79d38-136">`onclick` Olay tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="79d38-136">The `onclick` event is fired.</span></span>
* <span data-ttu-id="79d38-137">`IncrementCount` Yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="79d38-137">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="79d38-138">`currentCount` Artırılır.</span><span class="sxs-lookup"><span data-stu-id="79d38-138">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="79d38-139">Bileşeni yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="79d38-139">The component is rendered again.</span></span>

<span data-ttu-id="79d38-140">Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="79d38-140">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="79d38-141">Bir bileşen başka bir bileşene bir HTML benzeri sözdizimi kullanarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="79d38-141">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="79d38-142">Bileşen parametreleri, öznitelikleri veya alt içeriğin kullanarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="79d38-142">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="79d38-143">Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="79d38-143">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="79d38-144">İçinde *Pages/Index.cshtml*, anket istemi bileşen bir sayaç bileşeni ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="79d38-144">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="79d38-145">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="79d38-145">Run the app.</span></span> <span data-ttu-id="79d38-146">Giriş sayfası, kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="79d38-146">The homepage has its own counter.</span></span>

<span data-ttu-id="79d38-147">Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:</span><span class="sxs-lookup"><span data-stu-id="79d38-147">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="79d38-148">Bir özelliği için ekleme `IncrementAmount` ile donatılmış `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="79d38-148">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="79d38-149">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="79d38-149">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="79d38-150">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="79d38-150">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="79d38-151">Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="79d38-151">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="79d38-152">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="79d38-152">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="79d38-153">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="79d38-153">Run the app.</span></span> <span data-ttu-id="79d38-154">Giriş sayfası on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="79d38-154">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="79d38-155">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="79d38-155">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
