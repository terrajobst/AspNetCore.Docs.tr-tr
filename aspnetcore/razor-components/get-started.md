---
title: Razor bileşenleri ile çalışmaya başlama
author: guardrex
description: Razor bileşenleri framework ile çalışmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/03/2019
uid: razor-components/get-started
ms.openlocfilehash: 83f8bbcd415c6776ef14e1ce789f3f0a8cecc464
ms.sourcegitcommit: a91e8dd2f4b788114c8bc834507277f4b5e8d6c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55712230"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="9170a-103">Razor bileşenleri ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="9170a-103">Get started with Razor Components</span></span>

<span data-ttu-id="9170a-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9170a-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9170a-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9170a-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9170a-106">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="9170a-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="9170a-107">Visual Studio'da ilk Razor bileşenleri projenizi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="9170a-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="9170a-108">Seçin **dosya** > **yeni proje** > **Web** > **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="9170a-108">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="9170a-109">Emin **.NET Core** ve **ASP.NET Core 3.0** üstünde seçilir.</span><span class="sxs-lookup"><span data-stu-id="9170a-109">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="9170a-110">Seçin **Razor bileşenleri** şablonu seçip alt **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="9170a-110">Choose the **Razor Components** template and select **OK**.</span></span>

   ![Yeni uygulama iletişim kutusu](https://msdnshared.blob.core.windows.net/media/2019/01/razor-components-template.png)

1. <span data-ttu-id="9170a-112">Tuşuna **F5** uygulamayı çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="9170a-112">Press **F5** to run the app.</span></span>

<span data-ttu-id="9170a-113">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="9170a-113">Congratulations!</span></span> <span data-ttu-id="9170a-114">Yalnızca ilk Razor bileşenleri uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="9170a-114">You just ran your first Razor Components app!</span></span>

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

Congrats! You just ran your first Razor Components app!

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

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9170a-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9170a-115">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="9170a-116">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="9170a-116">Prerequisites:</span></span>

* [<span data-ttu-id="9170a-117">.NET core SDK 3.0 Önizleme</span><span class="sxs-lookup"><span data-stu-id="9170a-117">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

<span data-ttu-id="9170a-118">Bir komut kabuğu'ndan ilk Razor bileşenleri projenizi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="9170a-118">To create your first Razor Components project from a command shell:</span></span>

```console
dotnet new razorcomponents -o WebApplication1
cd WebApplication1
dotnet run
```

<span data-ttu-id="9170a-119">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="9170a-119">Congratulations!</span></span> <span data-ttu-id="9170a-120">Yalnızca ilk Razor bileşenleri uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="9170a-120">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="9170a-121">Razor bileşenleri proje</span><span class="sxs-lookup"><span data-stu-id="9170a-121">Razor Components project</span></span>

<span data-ttu-id="9170a-122">Razor bileşenleri şablon tarafından oluşturulan çözüm iki proje içerir:</span><span class="sxs-lookup"><span data-stu-id="9170a-122">The solution created by the Razor Components template contains two projects:</span></span>

* <span data-ttu-id="9170a-123">*WebApplication1.Server* &ndash; bir ASP.NET Core projesi Razor bileşenleri uygulamayı barındırmak için ayarlanmış sunucu projedir.</span><span class="sxs-lookup"><span data-stu-id="9170a-123">*WebApplication1.Server* &ndash; The server project is an ASP.NET Core project set up to host the Razor Components app.</span></span>
* <span data-ttu-id="9170a-124">*WebApplication1.App* &ndash; Razor bileşenleri kullanan istemci tarafı web kullanıcı Arabirimi proje.</span><span class="sxs-lookup"><span data-stu-id="9170a-124">*WebApplication1.App* &ndash; The client-side web UI project that uses Razor Components.</span></span>

<span data-ttu-id="9170a-125">UI mantığı *WebApplication1.App* proje ayrılmış uygulama ASP.NET Core 3.0 Preview 2 sürümündeki teknik bir sınırlama nedeniyle geri kalanından.</span><span class="sxs-lookup"><span data-stu-id="9170a-125">The UI logic in the *WebApplication1.App* project is separated from the rest of the app due to a technical limitation in ASP.NET Core 3.0 Preview 2.</span></span> <span data-ttu-id="9170a-126">Razor dosya uzantısı (*.cshtml*) kullanılan Razor bileşenleri için Razor sayfaları ve MVC görünümleri de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9170a-126">The Razor file extension (*.cshtml*) used for Razor Components is also used for Razor Pages and MVC views.</span></span> <span data-ttu-id="9170a-127">Razor bileşenleri Razor dosyaları ayrı tutulur. Bu nedenle şu anda, Razor bileşenleri ve Razor sayfaları/MVC farklı derleme modelleri yok edin.</span><span class="sxs-lookup"><span data-stu-id="9170a-127">Currently, Razor Components and Razor Pages/MVC have different compilation models, so the Razor Components Razor files are kept separate.</span></span> <span data-ttu-id="9170a-128">Gelecekteki bir Önizleme'de, yeni bir dosya uzantısı tanıtan Razor bileşenleri için planlıyoruz (*.razor*).</span><span class="sxs-lookup"><span data-stu-id="9170a-128">In a future preview, we plan to introduce a new file extension for Razor Components (*.razor*).</span></span> <span data-ttu-id="9170a-129">Bileşenleri ve sayfa görünümleri barındırılacak *aynı projede*.</span><span class="sxs-lookup"><span data-stu-id="9170a-129">Components, pages, and views will be hosted *in the same project*.</span></span>

<span data-ttu-id="9170a-130">Uygulamayı çalıştırdığınızda, birden çok sayfa sekmeleri Kenar çubuğunda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="9170a-130">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="9170a-131">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="9170a-131">Home</span></span>
* <span data-ttu-id="9170a-132">Sayaç</span><span class="sxs-lookup"><span data-stu-id="9170a-132">Counter</span></span>
* <span data-ttu-id="9170a-133">Veri getirme</span><span class="sxs-lookup"><span data-stu-id="9170a-133">Fetch data</span></span>

<span data-ttu-id="9170a-134">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="9170a-134">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="9170a-135">Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektiriyor, ancak Razor bileşenleri sağlayan daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="9170a-135">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="9170a-136">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9170a-136">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="9170a-137">Bir istek için `/counter` tarayıcıda tarafından belirtilen `@page` yönergesi üst içeriğini işlemek sayacı bileşen neden olur.</span><span class="sxs-lookup"><span data-stu-id="9170a-137">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="9170a-138">Bileşenleri UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir işleme ağacında bir bellek içi gösterimi halinde işler.</span><span class="sxs-lookup"><span data-stu-id="9170a-138">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="9170a-139">Her zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="9170a-139">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="9170a-140">`onclick` Olay tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="9170a-140">The `onclick` event is fired.</span></span>
* <span data-ttu-id="9170a-141">`IncrementCount` Yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="9170a-141">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="9170a-142">`currentCount` Artırılır.</span><span class="sxs-lookup"><span data-stu-id="9170a-142">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="9170a-143">Bileşeni yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9170a-143">The component is rendered again.</span></span>

<span data-ttu-id="9170a-144">Çalışma zamanı, önceki içeriği için yeni içerik karşılaştırır ve yalnızca değiştirilen içerik belge nesne modeli (DOM) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="9170a-144">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="9170a-145">Bir bileşen başka bir bileşene bir HTML benzeri sözdizimi kullanarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9170a-145">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="9170a-146">Bileşen parametreleri, öznitelikleri veya alt içeriğin kullanarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="9170a-146">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="9170a-147">Örneğin, bir sayaç bileşeni uygulamanın giriş sayfasına ekleyerek eklenebilir bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="9170a-147">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="9170a-148">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9170a-148">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="9170a-149">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9170a-149">Run the app.</span></span> <span data-ttu-id="9170a-150">Giriş sayfası, kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="9170a-150">The homepage has its own counter.</span></span>

<span data-ttu-id="9170a-151">Sayaç bileşenine parametre eklemek için bileşenin güncelleştirme `@functions` engelle:</span><span class="sxs-lookup"><span data-stu-id="9170a-151">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="9170a-152">Bir özelliği için ekleme `IncrementAmount` ile donatılmış `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="9170a-152">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="9170a-153">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="9170a-153">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="9170a-154">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9170a-154">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="9170a-155">Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="9170a-155">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="9170a-156">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9170a-156">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="9170a-157">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9170a-157">Run the app.</span></span> <span data-ttu-id="9170a-158">Giriş sayfası on tarafından her zaman artırır, kendi sayaç sahip **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="9170a-158">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9170a-159">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="9170a-159">Next steps</span></span>

<xref:tutorials/first-blazor-app>
