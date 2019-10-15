---
title: İlk Blazor uygulamanızı oluşturma
author: guardrex
description: Adım adım Blazor uygulaması oluşturun.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 1c0492106b888665c23932f16cc83d22947956d2
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334044"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="9900a-103">İlk Blazor uygulamanızı oluşturma</span><span class="sxs-lookup"><span data-stu-id="9900a-103">Build your first Blazor app</span></span>

<span data-ttu-id="9900a-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="9900a-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="9900a-105">Bu öğreticide bir Blazor uygulamasının nasıl oluşturulacağı ve değiştirileceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9900a-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="9900a-106">Bu öğreticide bir Blazor projesi oluşturmak için <xref:blazor/get-started> makalesindeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="9900a-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="9900a-107">Projeyi *ToDoList*olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="9900a-107">Name the project *ToDoList*.</span></span>

## <a name="build-components"></a><span data-ttu-id="9900a-108">Derleme bileşenleri</span><span class="sxs-lookup"><span data-stu-id="9900a-108">Build components</span></span>

1. <span data-ttu-id="9900a-109">*Sayfalar* klasöründe uygulamanın üç sayfasının her birine gidin: giriş, sayaç ve veri getirme.</span><span class="sxs-lookup"><span data-stu-id="9900a-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="9900a-110">Bu sayfalar, Razor bileşen dosyaları *dizini. Razor*, *Counter. Razor*ve *fetchdata. Razor*tarafından uygulanır.</span><span class="sxs-lookup"><span data-stu-id="9900a-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="9900a-111">Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="9900a-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="9900a-112">Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir, ancak Blazor kullanarak C#daha iyi bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="9900a-112">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="9900a-113">*Counter. Razor* dosyasındaki `Counter` bileşeninin uygulamasını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="9900a-113">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="9900a-114">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9900a-114">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="9900a-115">@No__t-0 bileşeninin Kullanıcı arabirimi HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="9900a-115">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="9900a-116">Dinamik işleme mantığı (örneğin, döngüler, koşullar, ifadeler) C# [Razor](xref:mvc/views/razor)adlı gömülü bir sözdizimi kullanılarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="9900a-116">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="9900a-117">HTML biçimlendirme ve C# işleme mantığı, derleme zamanında bir bileşen sınıfına dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="9900a-117">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="9900a-118">Oluşturulan .NET sınıfının adı dosya adıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="9900a-118">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="9900a-119">Bileşen sınıfının üyeleri `@code` bloğunda tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="9900a-119">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="9900a-120">@No__t-0 bloğunda, bileşen durumu (özellikler, alanlar) ve yöntemler olay işleme için veya diğer bileşen mantığını tanımlamak için belirtilir.</span><span class="sxs-lookup"><span data-stu-id="9900a-120">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="9900a-121">Bu Üyeler daha sonra bileşenin işleme mantığının bir parçası olarak ve olayları işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9900a-121">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="9900a-122">**Bana tıklama** düğmesi seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="9900a-122">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="9900a-123">@No__t-0 bileşeninin kayıtlı `onclick` işleyicisi (`IncrementCount` yöntemi) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="9900a-123">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="9900a-124">@No__t-0 bileşeni, kendi işleme ağacını yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9900a-124">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="9900a-125">Yeni işleme ağacı öncekiyle karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="9900a-125">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="9900a-126">Yalnızca Belge Nesne Modeli (DOM) üzerinde yapılan değişiklikler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="9900a-126">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="9900a-127">Görünen sayı güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="9900a-127">The displayed count is updated.</span></span>

1. <span data-ttu-id="9900a-128">@No__t- C# 1 bileşeninin mantığını değiştirerek sayıyı bir tane yerine iki ile artırın.</span><span class="sxs-lookup"><span data-stu-id="9900a-128">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="9900a-129">Değişiklikleri görmek için uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9900a-129">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="9900a-130">**Ben tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="9900a-130">Select the **Click me** button.</span></span> <span data-ttu-id="9900a-131">Sayaç iki olarak artar.</span><span class="sxs-lookup"><span data-stu-id="9900a-131">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="9900a-132">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="9900a-132">Use components</span></span>

<span data-ttu-id="9900a-133">Bir bileşeni, bir HTML söz dizimini kullanarak başka bir bileşene ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9900a-133">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="9900a-134">@No__t-3 bileşenine (*Index. Razor*) bir `<Counter />` öğesi ekleyerek uygulamanın `Index` bileşenine `Counter` bileşenini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9900a-134">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="9900a-135">Bu deneyim için Blazor WebAssembly kullanıyorsanız, `SurveyPrompt` bileşeni `Index` bileşeni tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9900a-135">If you're using Blazor WebAssembly for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="9900a-136">@No__t-0 öğesini bir `<Counter />` öğesiyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9900a-136">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="9900a-137">Bu deneyim için bir Blazor Server uygulaması kullanıyorsanız, `<Counter />` öğesini `Index` bileşenine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9900a-137">If you're using a Blazor Server app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="9900a-138">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9900a-138">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="9900a-139">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9900a-139">Rebuild and run the app.</span></span> <span data-ttu-id="9900a-140">@No__t-0 bileşeninin kendi sayacı vardır.</span><span class="sxs-lookup"><span data-stu-id="9900a-140">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="9900a-141">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="9900a-141">Component parameters</span></span>

<span data-ttu-id="9900a-142">Bileşenler de parametrelere sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="9900a-142">Components can also have parameters.</span></span> <span data-ttu-id="9900a-143">Bileşen parametreleri, bileşen sınıfında `[Parameter]` özniteliğiyle ortak özellikler kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="9900a-143">Component parameters are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="9900a-144">Biçimlendirme içindeki bir bileşenin bağımsız değişkenlerini belirtmek için öznitelikleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="9900a-144">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="9900a-145">Bileşenin `@code` C# kodunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="9900a-145">Update the component's `@code` C# code:</span></span>

   * <span data-ttu-id="9900a-146">@No__t-1 özniteliğiyle ortak bir `IncrementAmount` özelliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9900a-146">Add a public `IncrementAmount` property with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="9900a-147">@No__t-0 yöntemini `currentCount` değerini artırdığınızda `IncrementAmount` ' i kullanacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9900a-147">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="9900a-148">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9900a-148">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="9900a-149">Bir öznitelik kullanarak `Index` bileşeninin `<Counter>` öğesinde bir `IncrementAmount` parametresi belirtin.</span><span class="sxs-lookup"><span data-stu-id="9900a-149">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="9900a-150">Sayacı on olarak artırmak için değeri ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="9900a-150">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="9900a-151">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9900a-151">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="9900a-152">@No__t-0 bileşenini yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="9900a-152">Reload the `Index` component.</span></span> <span data-ttu-id="9900a-153">**Beni tıklama** düğmesi seçildiğinde sayaç on bir kez artar.</span><span class="sxs-lookup"><span data-stu-id="9900a-153">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="9900a-154">@No__t-0 bileşenindeki sayaç bir artış ile devam eder.</span><span class="sxs-lookup"><span data-stu-id="9900a-154">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="9900a-155">Bileşenlere yönlendir</span><span class="sxs-lookup"><span data-stu-id="9900a-155">Route to components</span></span>

<span data-ttu-id="9900a-156">*Counter. Razor* dosyasının en üstündeki `@page` yönergesi, `Counter` bileşeninin bir yönlendirme uç noktası olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="9900a-156">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="9900a-157">@No__t-0 bileşeni, `/counter` ' e gönderilen istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="9900a-157">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="9900a-158">@No__t-0 yönergesi olmadan bir bileşen yönlendirilmiş istekleri işlemez, ancak bileşen diğer bileşenler tarafından hala kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9900a-158">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="9900a-159">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="9900a-159">Dependency injection</span></span>

### <a name="blazor-server-experience"></a><span data-ttu-id="9900a-160">Blazor sunucusu deneyimi</span><span class="sxs-lookup"><span data-stu-id="9900a-160">Blazor Server experience</span></span>

<span data-ttu-id="9900a-161">Bir Blazor sunucu uygulamasıyla çalışıyorsanız, `WeatherForecastService` hizmeti `Startup.ConfigureServices` ' de [tek](xref:fundamentals/dependency-injection#service-lifetimes) bir kayıt olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="9900a-161">If working with a Blazor Server app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9900a-162">Uygulamanın tamamında [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)yoluyla hizmetin bir örneği mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="9900a-162">An instance of the service is available throughout the app via [dependency injection (DI)](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="9900a-163">@No__t-0 yönergesi, `WeatherForecastService` hizmetinin örneğini `FetchData` bileşenine eklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9900a-163">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the `FetchData` component.</span></span>

<span data-ttu-id="9900a-164">*Pages/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9900a-164">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="9900a-165">@No__t-0 bileşeni, `WeatherForecast` nesnelerinin bir dizisini almak için eklenen hizmeti `ForecastService` olarak kullanır:</span><span class="sxs-lookup"><span data-stu-id="9900a-165">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="blazor-webassembly-experience"></a><span data-ttu-id="9900a-166">Blazor WebAssembly deneyimi</span><span class="sxs-lookup"><span data-stu-id="9900a-166">Blazor WebAssembly experience</span></span>

<span data-ttu-id="9900a-167">Blazor WebAssembly uygulamasıyla çalışıyorsanız, *Wwwroot/Sample-Data* klasöründeki *Hava durumu. JSON* dosyasından Hava durumu tahmin verileri almak için `HttpClient` eklenir.</span><span class="sxs-lookup"><span data-stu-id="9900a-167">If working with a Blazor WebAssembly app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder.</span></span>

<span data-ttu-id="9900a-168">*Pages/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9900a-168">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="9900a-169">[@No__t-1](/dotnet/csharp/language-reference/keywords/foreach-in) döngüsü, her tahmin örneğini Hava durumu verileri tablosunda bir satır olarak işlemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="9900a-169">An [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="9900a-170">Yapılacaklar listesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="9900a-170">Build a todo list</span></span>

<span data-ttu-id="9900a-171">Uygulamaya basit bir yapılacaklar listesi uygulayan yeni bir bileşen ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9900a-171">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="9900a-172">Uygulamalar *klasörüne* *Todo. Razor* adlı boş bir dosya ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9900a-172">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="9900a-173">Bileşen için ilk biçimlendirmeyi belirtin:</span><span class="sxs-lookup"><span data-stu-id="9900a-173">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="9900a-174">@No__t-0 bileşenini gezinti çubuğuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9900a-174">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="9900a-175">@No__t-0 bileşeni (*Shared/NavMenu. Razor*) uygulamanın düzeninde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9900a-175">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="9900a-176">Düzenler, uygulamadaki içeriğin çoğaltılmasını önlemenize olanak sağlayan bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="9900a-176">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="9900a-177">Aşağıdaki liste öğesi işaretlemesini *paylaşılan/NavMenu. Razor* dosyasında var olan liste öğelerinin altına ekleyerek `Todo` bileşeni için `<NavLink>` öğesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9900a-177">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="9900a-178">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9900a-178">Rebuild and run the app.</span></span> <span data-ttu-id="9900a-179">@No__t-0 bileşeni bağlantısının çalıştığından emin olmak için yeni Todo sayfasını ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="9900a-179">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="9900a-180">Bir Todo öğesini temsil eden bir sınıfı tutmak için projenin köküne bir *TodoItem.cs* dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9900a-180">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="9900a-181">@No__t-1 C# sınıfı için aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="9900a-181">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="9900a-182">@No__t-0 bileşenine geri dönün (*Pages/Todo. Razor*):</span><span class="sxs-lookup"><span data-stu-id="9900a-182">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="9900a-183">@No__t-0 bloğunda Todo öğeleri için bir alan ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9900a-183">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="9900a-184">@No__t-0 bileşeni, Todo listesinin durumunu korumak için bu alanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="9900a-184">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="9900a-185">Her Todo öğesini bir liste öğesi (`<li>`) olarak işlemek için sıralanmamış liste işaretlemesi ve bir `foreach` döngüsü ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9900a-185">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="9900a-186">Uygulama, listeye Todo öğeleri eklemek için Kullanıcı arabirimi öğeleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="9900a-186">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="9900a-187">Sıralanmamış listenin altına bir metin girişi (`<input>`) ve bir düğme (`<button>`) ekleyin (`<ul>...</ul>`):</span><span class="sxs-lookup"><span data-stu-id="9900a-187">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="9900a-188">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9900a-188">Rebuild and run the app.</span></span> <span data-ttu-id="9900a-189">**Todo Ekle** düğmesi seçildiğinde, bir olay işleyicisi düğmeye kablolu olmadığı için hiçbir şey olmaz.</span><span class="sxs-lookup"><span data-stu-id="9900a-189">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="9900a-190">@No__t-1 bileşenine `AddTodo` yöntemi ekleyin ve `@onclick` özniteliğini kullanarak düğme seçimleri için kaydedin.</span><span class="sxs-lookup"><span data-stu-id="9900a-190">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="9900a-191">@No__t-0 C# yöntemi düğme seçildiğinde çağrılır:</span><span class="sxs-lookup"><span data-stu-id="9900a-191">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="9900a-192">Yeni Todo öğesinin başlığını almak için, `@code` bloğunun üst kısmına bir `newTodo` dize alanı ekleyin ve `<input>` öğesindeki `bind` özniteliğini kullanarak metin girişinin değerine bağlayın:</span><span class="sxs-lookup"><span data-stu-id="9900a-192">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="9900a-193">@No__t-0 yöntemini, belirtilen başlığa sahip `TodoItem` ' i listeye eklemek için güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="9900a-193">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="9900a-194">@No__t-0 ' i boş bir dizeye ayarlayarak metin girişinin değerini temizleyin:</span><span class="sxs-lookup"><span data-stu-id="9900a-194">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="9900a-195">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9900a-195">Rebuild and run the app.</span></span> <span data-ttu-id="9900a-196">Yeni kodu test etmek için Todo listesine bazı Todo öğeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9900a-196">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="9900a-197">Her Todo öğesi için başlık metni düzenlenebilir hale getirilebilir ve bir onay kutusu kullanıcının tamamlanmış öğeleri izlemesine yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="9900a-197">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="9900a-198">Her Todo öğesi için bir onay kutusu girişi ekleyin ve değerini `IsDone` özelliğine bağlayın.</span><span class="sxs-lookup"><span data-stu-id="9900a-198">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="9900a-199">@No__t-0 ' yı `@todo.Title` ' e bağlanacak bir `<input>` öğesine değiştirin:</span><span class="sxs-lookup"><span data-stu-id="9900a-199">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="9900a-200">Bu değerlerin bağlandığını doğrulamak için `<h1>` üst bilgisini, tamamlanmamış olan Todo öğelerinin sayısını gösterecek şekilde güncelleştirin (`IsDone` `false` ' dir).</span><span class="sxs-lookup"><span data-stu-id="9900a-200">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="9900a-201">Tamamlanan `Todo` bileşeni (*Sayfalar/Todo. Razor*):</span><span class="sxs-lookup"><span data-stu-id="9900a-201">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="9900a-202">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9900a-202">Rebuild and run the app.</span></span> <span data-ttu-id="9900a-203">Yeni kodu test etmek için Todo öğeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9900a-203">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
