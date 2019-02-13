---
title: Razor bileşenleri ile çalışmaya başlama
author: guardrex
description: Razor bileşenler oluşturma ve Razor bileşenleri projesini değiştirme kullanmaya nasıl başlayacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/03/2019
uid: razor-components/get-started
ms.openlocfilehash: a9ada603e5ed4e0e75c4aebc5105c331118666e6
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159323"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="10ff9-103">Razor bileşenleri ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="10ff9-103">Get started with Razor Components</span></span>

<span data-ttu-id="10ff9-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="10ff9-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="10ff9-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="10ff9-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="10ff9-106">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="10ff9-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="10ff9-107">Visual Studio'da ilk Razor bileşenleri projenizi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="10ff9-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="10ff9-108">Seçin **dosya** > **yeni proje** > **Web** > **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="10ff9-108">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="10ff9-109">Emin **.NET Core** ve **ASP.NET Core 3.0** üstünde seçilir.</span><span class="sxs-lookup"><span data-stu-id="10ff9-109">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="10ff9-110">Seçin **Razor bileşenleri** şablonu seçip alt **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="10ff9-110">Choose the **Razor Components** template and select **OK**.</span></span>

   ![Yeni uygulama iletişim kutusu](https://msdnshared.blob.core.windows.net/media/2019/01/razor-components-template.png)

1. <span data-ttu-id="10ff9-112">Tuşuna **F5** uygulamayı çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="10ff9-112">Press **F5** to run the app.</span></span>

<span data-ttu-id="10ff9-113">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="10ff9-113">Congratulations!</span></span> <span data-ttu-id="10ff9-114">Yalnızca ilk Razor bileşenleri uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="10ff9-114">You just ran your first Razor Components app!</span></span>

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

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="10ff9-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="10ff9-115">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="10ff9-116">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="10ff9-116">Prerequisites:</span></span>

* [<span data-ttu-id="10ff9-117">.NET core SDK 3.0 Önizleme</span><span class="sxs-lookup"><span data-stu-id="10ff9-117">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="10ff9-118">Bir komut kabuğu'ndan ilk Razor bileşenleri projenizi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="10ff9-118">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="10ff9-119">Bir tarayıcıda gidin `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="10ff9-119">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="10ff9-120">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="10ff9-120">Congratulations!</span></span> <span data-ttu-id="10ff9-121">Yalnızca ilk Razor bileşenleri uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="10ff9-121">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="10ff9-122">Razor bileşenleri proje</span><span class="sxs-lookup"><span data-stu-id="10ff9-122">Razor Components project</span></span>

<span data-ttu-id="10ff9-123">Razor bileşenleri şablon tarafından oluşturulan çözüm iki proje içerir:</span><span class="sxs-lookup"><span data-stu-id="10ff9-123">The solution created by the Razor Components template contains two projects:</span></span>

* <span data-ttu-id="10ff9-124">*WebApplication1.Server* &ndash; bir ASP.NET Core projesi Razor bileşenleri uygulamayı barındırmak için ayarlanmış sunucu projedir.</span><span class="sxs-lookup"><span data-stu-id="10ff9-124">*WebApplication1.Server* &ndash; The server project is an ASP.NET Core project set up to host the Razor Components app.</span></span>
* <span data-ttu-id="10ff9-125">*WebApplication1.App* &ndash; Razor bileşenleri kullanan istemci tarafı web kullanıcı Arabirimi proje.</span><span class="sxs-lookup"><span data-stu-id="10ff9-125">*WebApplication1.App* &ndash; The client-side web UI project that uses Razor Components.</span></span>

<span data-ttu-id="10ff9-126">UI mantığı *WebApplication1.App* proje ayrılmış uygulama ASP.NET Core 3.0 Preview 2 sürümündeki teknik bir sınırlama nedeniyle geri kalanından.</span><span class="sxs-lookup"><span data-stu-id="10ff9-126">The UI logic in the *WebApplication1.App* project is separated from the rest of the app due to a technical limitation in ASP.NET Core 3.0 Preview 2.</span></span> <span data-ttu-id="10ff9-127">Razor dosya uzantısı (*.cshtml*) kullanılan Razor bileşenleri için Razor sayfaları ve MVC görünümleri de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="10ff9-127">The Razor file extension (*.cshtml*) used for Razor Components is also used for Razor Pages and MVC views.</span></span> <span data-ttu-id="10ff9-128">Razor bileşenleri Razor dosyaları ayrı tutulur. Bu nedenle şu anda, Razor bileşenleri ve Razor sayfaları/MVC farklı derleme modelleri yok edin.</span><span class="sxs-lookup"><span data-stu-id="10ff9-128">Currently, Razor Components and Razor Pages/MVC have different compilation models, so the Razor Components Razor files are kept separate.</span></span> <span data-ttu-id="10ff9-129">Gelecekteki bir Önizleme'de, yeni bir dosya uzantısı tanıtan Razor bileşenleri için planlıyoruz (*.razor*).</span><span class="sxs-lookup"><span data-stu-id="10ff9-129">In a future preview, we plan to introduce a new file extension for Razor Components (*.razor*).</span></span> <span data-ttu-id="10ff9-130">Bileşenleri ve sayfa görünümleri barındırılacak *aynı projede*.</span><span class="sxs-lookup"><span data-stu-id="10ff9-130">Components, pages, and views will be hosted *in the same project*.</span></span>

<span data-ttu-id="10ff9-131">Uygulamayı çalıştırdığınızda, birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="10ff9-131">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="10ff9-132">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="10ff9-132">Home</span></span>
* <span data-ttu-id="10ff9-133">Sayaç</span><span class="sxs-lookup"><span data-stu-id="10ff9-133">Counter</span></span>
* <span data-ttu-id="10ff9-134">Veri getirme</span><span class="sxs-lookup"><span data-stu-id="10ff9-134">Fetch data</span></span>

<span data-ttu-id="10ff9-135">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="10ff9-135">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="10ff9-136">Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektiriyor, ancak Razor bileşenleri sağlayan daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="10ff9-136">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="10ff9-137">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="10ff9-137">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="10ff9-138">Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur.</span><span class="sxs-lookup"><span data-stu-id="10ff9-138">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="10ff9-139">Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.</span><span class="sxs-lookup"><span data-stu-id="10ff9-139">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="10ff9-140">Her zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="10ff9-140">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="10ff9-141">`onclick` Olay tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="10ff9-141">The `onclick` event is fired.</span></span>
* <span data-ttu-id="10ff9-142">`IncrementCount` Yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="10ff9-142">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="10ff9-143">`currentCount` Artırılır.</span><span class="sxs-lookup"><span data-stu-id="10ff9-143">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="10ff9-144">Bileşeni yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="10ff9-144">The component is rendered again.</span></span>

<span data-ttu-id="10ff9-145">Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="10ff9-145">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="10ff9-146">Bir bileşen başka bir bileşene bir HTML benzeri sözdizimi kullanarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="10ff9-146">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="10ff9-147">Bileşen parametreleri, öznitelikleri veya alt içeriğin kullanarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="10ff9-147">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="10ff9-148">Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="10ff9-148">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="10ff9-149">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="10ff9-149">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="10ff9-150">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="10ff9-150">Run the app.</span></span> <span data-ttu-id="10ff9-151">Giriş sayfası, kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="10ff9-151">The homepage has its own counter.</span></span>

<span data-ttu-id="10ff9-152">Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:</span><span class="sxs-lookup"><span data-stu-id="10ff9-152">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="10ff9-153">Bir özelliği için ekleme `IncrementAmount` ile donatılmış `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="10ff9-153">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="10ff9-154">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="10ff9-154">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="10ff9-155">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="10ff9-155">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="10ff9-156">Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="10ff9-156">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="10ff9-157">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="10ff9-157">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="10ff9-158">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="10ff9-158">Run the app.</span></span> <span data-ttu-id="10ff9-159">Giriş sayfası on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="10ff9-159">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="10ff9-160">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="10ff9-160">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
