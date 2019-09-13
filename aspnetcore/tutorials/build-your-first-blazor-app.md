---
title: İlk Blazor uygulamanızı oluşturma
author: guardrex
description: Adım adım Blazor uygulaması oluşturun.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: ffbdf6991830d554fc508d1d2fe8e4b9586210df
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70964185"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="95ea4-103">İlk Blazor uygulamanızı oluşturma</span><span class="sxs-lookup"><span data-stu-id="95ea4-103">Build your first Blazor app</span></span>

<span data-ttu-id="95ea4-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="95ea4-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="95ea4-105">Bu öğreticide bir Blazor uygulamasının nasıl oluşturulacağı ve değiştirileceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="95ea4-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="95ea4-106">Bu öğreticide bir Blazor <xref:blazor/get-started> projesi oluşturmak için makalesindeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="95ea4-107">Projeyi *ToDoList*olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="95ea4-107">Name the project *ToDoList*.</span></span>

## <a name="build-components"></a><span data-ttu-id="95ea4-108">Derleme bileşenleri</span><span class="sxs-lookup"><span data-stu-id="95ea4-108">Build components</span></span>

1. <span data-ttu-id="95ea4-109">*Sayfalar* klasöründe uygulamanın üç sayfasının her birine gidin: Giriş, sayaç ve veri getirme.</span><span class="sxs-lookup"><span data-stu-id="95ea4-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="95ea4-110">Bu sayfalar, Razor bileşen dosyaları *dizini. Razor*, *Counter. Razor*ve *fetchdata. Razor*tarafından uygulanır.</span><span class="sxs-lookup"><span data-stu-id="95ea4-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="95ea4-111">Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="95ea4-112">Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak Blazor kullanarak C#daha iyi bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="95ea4-112">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="95ea4-113">`Counter` *Counter. Razor* dosyasındaki bileşenin uygulamasını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-113">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="95ea4-114">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="95ea4-114">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="95ea4-115">`Counter` Bileşenin kullanıcı arabirimi HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="95ea4-115">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="95ea4-116">Dinamik işleme mantığı (örneğin, döngüler, koşullar, ifadeler) C# [Razor](xref:mvc/views/razor)adlı gömülü bir sözdizimi kullanılarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="95ea4-116">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="95ea4-117">HTML biçimlendirme ve C# işleme mantığı, derleme zamanında bir bileşen sınıfına dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="95ea4-117">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="95ea4-118">Oluşturulan .NET sınıfının adı dosya adıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="95ea4-118">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="95ea4-119">Bileşen sınıfının üyeleri bir `@code` blokta tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="95ea4-119">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="95ea4-120">`@code` Bloğunda, bileşen durumu (özellikler, alanlar) ve yöntemler olay işleme için veya diğer bileşen mantığını tanımlamak için belirtilir.</span><span class="sxs-lookup"><span data-stu-id="95ea4-120">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="95ea4-121">Bu Üyeler daha sonra bileşenin işleme mantığının bir parçası olarak ve olayları işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="95ea4-121">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="95ea4-122">**Bana tıklama** düğmesi seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="95ea4-122">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="95ea4-123">`Counter` Bileşenin kayıtlı `onclick` işleyicisine(`IncrementCount` yöntemi) denir.</span><span class="sxs-lookup"><span data-stu-id="95ea4-123">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="95ea4-124">`Counter` Bileşen, işleme ağacını yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="95ea4-124">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="95ea4-125">Yeni işleme ağacı öncekiyle karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="95ea4-125">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="95ea4-126">Yalnızca Belge Nesne Modeli (DOM) üzerinde yapılan değişiklikler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="95ea4-126">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="95ea4-127">Görünen sayı güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="95ea4-127">The displayed count is updated.</span></span>

1. <span data-ttu-id="95ea4-128">Bir sayının C# yerine iki tane `Counter` olmak üzere, bileşenin mantığını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-128">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="95ea4-129">Değişiklikleri görmek için uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="95ea4-129">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="95ea4-130">**Ben tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-130">Select the **Click me** button.</span></span> <span data-ttu-id="95ea4-131">Sayaç iki olarak artar.</span><span class="sxs-lookup"><span data-stu-id="95ea4-131">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="95ea4-132">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="95ea4-132">Use components</span></span>

<span data-ttu-id="95ea4-133">Bir bileşeni, bir HTML söz dizimini kullanarak başka bir bileşene ekleyin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-133">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="95ea4-134">`Index`Bileşenebir öğe`Index`ekleyerek bileşeni uygulamanın bileşenine ekleyin (*Index. Razor*). `Counter` `<Counter />`</span><span class="sxs-lookup"><span data-stu-id="95ea4-134">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="95ea4-135">Bu deneyim için Blazor webassembly kullanıyorsanız bileşen `SurveyPrompt` `Index` tarafından bir bileşen kullanılır.</span><span class="sxs-lookup"><span data-stu-id="95ea4-135">If you're using Blazor WebAssembly for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="95ea4-136">`<SurveyPrompt>` Öğesini bir`<Counter />` öğesiyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-136">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="95ea4-137">Bu deneyim için bir Blazor Server uygulaması kullanıyorsanız, `<Counter />` öğesini `Index` bileşene ekleyin:</span><span class="sxs-lookup"><span data-stu-id="95ea4-137">If you're using a Blazor Server app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="95ea4-138">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="95ea4-138">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="95ea4-139">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="95ea4-139">Rebuild and run the app.</span></span> <span data-ttu-id="95ea4-140">`Index` Bileşenin kendi sayacı vardır.</span><span class="sxs-lookup"><span data-stu-id="95ea4-140">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="95ea4-141">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="95ea4-141">Component parameters</span></span>

<span data-ttu-id="95ea4-142">Bileşenler de parametrelere sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="95ea4-142">Components can also have parameters.</span></span> <span data-ttu-id="95ea4-143">Bileşen parametreleri, bileşen sınıfında `[Parameter]` özniteliğiyle birlikte ortak özellikler kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="95ea4-143">Component parameters are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="95ea4-144">Biçimlendirme içindeki bir bileşenin bağımsız değişkenlerini belirtmek için öznitelikleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="95ea4-144">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="95ea4-145">Bileşenin `@code` C# kodunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="95ea4-145">Update the component's `@code` C# code:</span></span>

   * <span data-ttu-id="95ea4-146">Özniteliği ile ortak `IncrementAmount` bir özellik ekleyin. `[Parameter]`</span><span class="sxs-lookup"><span data-stu-id="95ea4-146">Add a public `IncrementAmount` property with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="95ea4-147">Değerini değerini `IncrementAmount`artırdığınızda kullanmak için `IncrementCount` yöntemini değiştirin `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="95ea4-147">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="95ea4-148">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="95ea4-148">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="95ea4-149">Öznitelik kullanarak `IncrementAmount` `Index` bileşenin`<Counter>` öğesinde bir parametre belirtin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-149">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="95ea4-150">Sayacı on olarak artırmak için değeri ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="95ea4-150">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="95ea4-151">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="95ea4-151">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="95ea4-152">`Index` Bileşeni yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-152">Reload the `Index` component.</span></span> <span data-ttu-id="95ea4-153">**Beni tıklama** düğmesi seçildiğinde sayaç on bir kez artar.</span><span class="sxs-lookup"><span data-stu-id="95ea4-153">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="95ea4-154">`Counter` Bileşendeki sayaç bir artırmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="95ea4-154">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="95ea4-155">Bileşenlere yönlendir</span><span class="sxs-lookup"><span data-stu-id="95ea4-155">Route to components</span></span>

<span data-ttu-id="95ea4-156">*Counter. Razor* dosyasının en üstündeki `Counter` yönerge,bileşeninbiryönlendirmeuçnoktasıolduğunubelirtir.`@page`</span><span class="sxs-lookup"><span data-stu-id="95ea4-156">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="95ea4-157">`Counter` Bileşen öğesine`/counter`gönderilen istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="95ea4-157">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="95ea4-158">`@page` Yönerge olmadan, bileşen yönlendirilmiş istekleri işlemez, ancak bileşen diğer bileşenler tarafından hala kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="95ea4-158">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="95ea4-159">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="95ea4-159">Dependency injection</span></span>

<span data-ttu-id="95ea4-160">Uygulamanın hizmet kapsayıcısına kayıtlı hizmetler, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)yoluyla bileşenler için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="95ea4-160">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="95ea4-161">`@inject` Yönergesi kullanarak bir bileşene hizmet ekleme.</span><span class="sxs-lookup"><span data-stu-id="95ea4-161">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="95ea4-162">`FetchData` Bileşenin yönergelerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-162">Examine the directives of the `FetchData` component.</span></span>

<span data-ttu-id="95ea4-163">Bir Blazor sunucu uygulamasıyla çalışıyorsanız, `WeatherForecastService` hizmet [tek](xref:fundamentals/dependency-injection#service-lifetimes)bir olarak kaydedilir, bu yüzden hizmetin bir örneği uygulama genelinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="95ea4-163">If working with a Blazor Server app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="95ea4-164">Yönergesi, `WeatherForecastService` hizmet örneğini bileşene eklemek için kullanılır. `@inject`</span><span class="sxs-lookup"><span data-stu-id="95ea4-164">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="95ea4-165">*Pages/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="95ea4-165">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="95ea4-166">Bileşen, `WeatherForecast` nesne dizisini almak için olarak `ForecastService`eklenen hizmeti kullanır: `FetchData`</span><span class="sxs-lookup"><span data-stu-id="95ea4-166">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="95ea4-167">Bir Blazor webassembly uygulamasıyla çalışıyorsanız, `HttpClient` *Wwwroot/Sample-Data* klasöründeki *Hava durumu. JSON* dosyasından Hava durumu tahmin verileri almak için eklenen:</span><span class="sxs-lookup"><span data-stu-id="95ea4-167">If working with a Blazor WebAssembly app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="95ea4-168">*Pages/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="95ea4-168">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="95ea4-169">Bir foreach döngüsü, her tahmin örneğini Hava durumu verileri tablosunda bir satır olarak işlemek için kullanılır: [ \@](/dotnet/csharp/language-reference/keywords/foreach-in)</span><span class="sxs-lookup"><span data-stu-id="95ea4-169">A [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a><span data-ttu-id="95ea4-170">Yapılacaklar listesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="95ea4-170">Build a todo list</span></span>

<span data-ttu-id="95ea4-171">Uygulamaya basit bir yapılacaklar listesi uygulayan yeni bir bileşen ekleyin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-171">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="95ea4-172">Uygulamalar *klasörüne* *Todo. Razor* adlı boş bir dosya ekleyin:</span><span class="sxs-lookup"><span data-stu-id="95ea4-172">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="95ea4-173">Bileşen için ilk biçimlendirmeyi belirtin:</span><span class="sxs-lookup"><span data-stu-id="95ea4-173">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="95ea4-174">`Todo` Bileşeni gezinti çubuğuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-174">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="95ea4-175">Bileşen (*Shared/navmenu. Razor*) uygulamanın düzeninde kullanılır. `NavMenu`</span><span class="sxs-lookup"><span data-stu-id="95ea4-175">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="95ea4-176">Düzenler, uygulamadaki içeriğin çoğaltılmasını önlemenize olanak sağlayan bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="95ea4-176">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="95ea4-177">`<NavLink>` *Paylaşılan/navmenu. Razor* dosyasındaki mevcut liste öğelerinin altına aşağıdaki liste öğesi işaretlemesini ekleyerek `Todo` bileşen için bir öğe ekleyin:</span><span class="sxs-lookup"><span data-stu-id="95ea4-177">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="95ea4-178">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="95ea4-178">Rebuild and run the app.</span></span> <span data-ttu-id="95ea4-179">`Todo` Bileşen bağlantısının çalıştığından emin olmak için yeni Todo sayfasını ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-179">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="95ea4-180">Bir Todo öğesini temsil eden bir sınıfı tutmak için projenin köküne bir *TodoItem.cs* dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-180">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="95ea4-181">`TodoItem` Sınıfı için aşağıdaki C# kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="95ea4-181">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="95ea4-182">Bileşene geri dön (*Pages/Todo. Razor*): `Todo`</span><span class="sxs-lookup"><span data-stu-id="95ea4-182">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="95ea4-183">Bir `@code` bloktaki Todo öğeleri için bir alan ekleyin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-183">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="95ea4-184">`Todo` Bileşen, Todo listesinin durumunu korumak için bu alanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="95ea4-184">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="95ea4-185">Her Todo öğesini bir liste öğesi `foreach` (`<li>`) olarak işlemek için sıralanmamış liste işaretlemesi ve bir döngüsü ekleyin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-185">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="95ea4-186">Uygulama, listeye Todo öğeleri eklemek için Kullanıcı arabirimi öğeleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="95ea4-186">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="95ea4-187">Sıralanmamış listenin`<input>`(`<ul>...</ul>`) altına bir metin girişi ()`<button>`ve bir düğme () ekleyin:</span><span class="sxs-lookup"><span data-stu-id="95ea4-187">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="95ea4-188">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="95ea4-188">Rebuild and run the app.</span></span> <span data-ttu-id="95ea4-189">**Todo Ekle** düğmesi seçildiğinde, bir olay işleyicisi düğmeye kablolu olmadığı için hiçbir şey olmaz.</span><span class="sxs-lookup"><span data-stu-id="95ea4-189">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="95ea4-190">`Todo` Bileşene bir `AddTodo` yöntem ekleyin ve `@onclick` özniteliği kullanarak düğme seçimleri için kaydedin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-190">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="95ea4-191">`AddTodo` Düğme seçildiğinde C# yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="95ea4-191">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="95ea4-192">Yeni Todo öğesinin `newTodo` başlığını almak için, `@code` bloğunun üst kısmına bir dize alanı ekleyin ve bunu, `<input>` öğesindeki `bind` özniteliği kullanarak metin girişinin değerine bağlayın:</span><span class="sxs-lookup"><span data-stu-id="95ea4-192">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="95ea4-193">Belirtilen başlığa `TodoItem` sahip öğesini listeye eklemek için yönteminigüncelleştirin.`AddTodo`</span><span class="sxs-lookup"><span data-stu-id="95ea4-193">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="95ea4-194">Metin girişinin değerini boş bir dizeye ayarlayarak `newTodo` temizleyin:</span><span class="sxs-lookup"><span data-stu-id="95ea4-194">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="95ea4-195">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="95ea4-195">Rebuild and run the app.</span></span> <span data-ttu-id="95ea4-196">Yeni kodu test etmek için Todo listesine bazı Todo öğeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-196">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="95ea4-197">Her Todo öğesi için başlık metni düzenlenebilir hale getirilebilir ve bir onay kutusu kullanıcının tamamlanmış öğeleri izlemesine yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="95ea4-197">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="95ea4-198">Her Todo öğesi için bir onay kutusu girişi ekleyin ve değerini `IsDone` özelliğine bağlayın.</span><span class="sxs-lookup"><span data-stu-id="95ea4-198">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="95ea4-199">Öğesine göre bir `<input>` öğeye`@todo.Title`geç: `@todo.Title`</span><span class="sxs-lookup"><span data-stu-id="95ea4-199">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="95ea4-200">Bu değerlerin bağlandığını doğrulamak için `<h1>` üst bilgiyi, tamamlanmamış olan Todo öğelerinin sayısının sayısını gösterecek şekilde güncelleştirin (`IsDone` yani `false`).</span><span class="sxs-lookup"><span data-stu-id="95ea4-200">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="95ea4-201">Tamamlanan `Todo` bileşen (*sayfa/Todo. Razor*):</span><span class="sxs-lookup"><span data-stu-id="95ea4-201">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="95ea4-202">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="95ea4-202">Rebuild and run the app.</span></span> <span data-ttu-id="95ea4-203">Yeni kodu test etmek için Todo öğeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="95ea4-203">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
