---
title: İlk Blazor uygulamanızı oluşturma
author: guardrex
description: Blazor uygulaması oluşturun adım adım.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2019
no-loc:
- Blazor
uid: tutorials/first-blazor-app
ms.openlocfilehash: 07069410135e216ff5f4de94285a54be66d44615
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880376"
---
# <a name="build-your-first-opno-locblazor-app"></a><span data-ttu-id="7af46-103">İlk Blazor uygulamanızı oluşturma</span><span class="sxs-lookup"><span data-stu-id="7af46-103">Build your first Blazor app</span></span>

<span data-ttu-id="7af46-104">[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="7af46-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="7af46-105">Bu öğreticide bir Blazor uygulamasının nasıl oluşturulacağı ve değiştirileceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7af46-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="7af46-106">Bu öğreticide bir Blazor projesi oluşturmak için <xref:blazor/get-started> makalesindeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="7af46-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="7af46-107">Projeyi *ToDoList*olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="7af46-107">Name the project *ToDoList*.</span></span>

## <a name="build-components"></a><span data-ttu-id="7af46-108">Derleme bileşenleri</span><span class="sxs-lookup"><span data-stu-id="7af46-108">Build components</span></span>

1. <span data-ttu-id="7af46-109">*Sayfalar* klasöründe uygulamanın üç sayfasının her birine gidin: giriş, sayaç ve veri getirme.</span><span class="sxs-lookup"><span data-stu-id="7af46-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="7af46-110">Bu sayfalar, Razor bileşen dosyaları *dizini. Razor*, *Counter. Razor*ve *fetchdata. Razor*tarafından uygulanır.</span><span class="sxs-lookup"><span data-stu-id="7af46-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="7af46-111">Sayaç sayfasında, bir sayfa yenilemesi olmadan sayacı artırmak için **bana tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="7af46-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="7af46-112">Bir Web sayfasında normal olarak bir sayacı artırma, JavaScript yazmayı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="7af46-112">Incrementing a counter in a webpage normally requires writing JavaScript.</span></span> <span data-ttu-id="7af46-113">Blazor, bunun yerine yazabilirsiniz C# .</span><span class="sxs-lookup"><span data-stu-id="7af46-113">With Blazor, you can write C# instead.</span></span>

1. <span data-ttu-id="7af46-114">*Counter. Razor* dosyasındaki `Counter` bileşeninin uygulamasını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="7af46-114">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="7af46-115">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7af46-115">*Pages/Counter.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="7af46-116">`Counter` bileşenin kullanıcı arabirimi HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="7af46-116">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="7af46-117">Dinamik işleme mantığı (örneğin, döngüler, koşullar, ifadeler) C# [Razor](xref:mvc/views/razor)adlı gömülü bir sözdizimi kullanılarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="7af46-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="7af46-118">HTML biçimlendirme ve C# işleme mantığı, derleme zamanında bir bileşen sınıfına dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="7af46-118">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="7af46-119">Oluşturulan .NET sınıfının adı dosya adıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="7af46-119">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="7af46-120">Bileşen sınıfının üyeleri bir `@code` bloğunda tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="7af46-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="7af46-121">`@code` bloğunda, bileşen durumu (özellikler, alanlar) ve yöntemler olay işleme için veya diğer bileşen mantığını tanımlamak için belirtilir.</span><span class="sxs-lookup"><span data-stu-id="7af46-121">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="7af46-122">Bu Üyeler daha sonra bileşenin işleme mantığının bir parçası olarak ve olayları işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7af46-122">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="7af46-123">**Bana tıklama** düğmesi seçildiğinde:</span><span class="sxs-lookup"><span data-stu-id="7af46-123">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="7af46-124">`Counter` bileşenin kayıtlı `onclick` işleyicisine (`IncrementCount` yöntemi) denir.</span><span class="sxs-lookup"><span data-stu-id="7af46-124">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="7af46-125">`Counter` bileşeni, işleme ağacını yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7af46-125">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="7af46-126">Yeni işleme ağacı öncekiyle karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="7af46-126">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="7af46-127">Yalnızca Belge Nesne Modeli (DOM) üzerinde yapılan değişiklikler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="7af46-127">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="7af46-128">Görünen sayı güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="7af46-128">The displayed count is updated.</span></span>

1. <span data-ttu-id="7af46-129">Sayıyı bir C# yerine iki ile artırmak için `Counter` bileşenin mantığını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="7af46-129">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="7af46-130">Değişiklikleri görmek için uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7af46-130">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="7af46-131">**Ben tıklama** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="7af46-131">Select the **Click me** button.</span></span> <span data-ttu-id="7af46-132">Sayaç iki olarak artar.</span><span class="sxs-lookup"><span data-stu-id="7af46-132">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="7af46-133">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="7af46-133">Use components</span></span>

<span data-ttu-id="7af46-134">Bir bileşeni, bir HTML söz dizimini kullanarak başka bir bileşene ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7af46-134">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="7af46-135">`Index` bileşenine bir `<Counter />` öğesi ekleyerek uygulamanın `Index` bileşenine `Counter` bileşenini ekleyin (*Index. Razor*).</span><span class="sxs-lookup"><span data-stu-id="7af46-135">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="7af46-136">Bu deneyim için Blazor WebAssembly kullanıyorsanız, `Index` bileşeni tarafından `SurveyPrompt` bir bileşen kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7af46-136">If you're using Blazor WebAssembly for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="7af46-137">`<SurveyPrompt>` öğesini bir `<Counter />` öğesiyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="7af46-137">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="7af46-138">Bu deneyim için bir Blazor sunucusu uygulaması kullanıyorsanız, `Index` bileşenine `<Counter />` öğesini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7af46-138">If you're using a Blazor Server app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="7af46-139">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7af46-139">*Pages/Index.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="7af46-140">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7af46-140">Rebuild and run the app.</span></span> <span data-ttu-id="7af46-141">`Index` bileşeni kendi sayacıdır.</span><span class="sxs-lookup"><span data-stu-id="7af46-141">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="7af46-142">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="7af46-142">Component parameters</span></span>

<span data-ttu-id="7af46-143">Bileşenler de parametrelere sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="7af46-143">Components can also have parameters.</span></span> <span data-ttu-id="7af46-144">Bileşen parametreleri, bileşen sınıfında `[Parameter]` özniteliğiyle ortak özellikler kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="7af46-144">Component parameters are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="7af46-145">Biçimlendirme içindeki bir bileşenin bağımsız değişkenlerini belirtmek için öznitelikleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="7af46-145">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="7af46-146">Bileşenin `@code` C# kodunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="7af46-146">Update the component's `@code` C# code:</span></span>

   * <span data-ttu-id="7af46-147">`[Parameter]` özniteliğiyle ortak bir `IncrementAmount` özelliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7af46-147">Add a public `IncrementAmount` property with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="7af46-148">`currentCount`değerini artırdığınızda `IncrementAmount` kullanmak için `IncrementCount` yöntemini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="7af46-148">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="7af46-149">*Pages/Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7af46-149">*Pages/Counter.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="7af46-150">Özniteliği kullanarak `Index` bileşeninin `<Counter>` öğesinde bir `IncrementAmount` parametresi belirtin.</span><span class="sxs-lookup"><span data-stu-id="7af46-150">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="7af46-151">Sayacı on olarak artırmak için değeri ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7af46-151">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="7af46-152">*Pages/Index. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7af46-152">*Pages/Index.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="7af46-153">`Index` bileşenini yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="7af46-153">Reload the `Index` component.</span></span> <span data-ttu-id="7af46-154">**Beni tıklama** düğmesi seçildiğinde sayaç on bir kez artar.</span><span class="sxs-lookup"><span data-stu-id="7af46-154">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="7af46-155">`Counter` bileşenindeki sayaç bir artış ile devam eder.</span><span class="sxs-lookup"><span data-stu-id="7af46-155">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="7af46-156">Bileşenlere yönlendir</span><span class="sxs-lookup"><span data-stu-id="7af46-156">Route to components</span></span>

<span data-ttu-id="7af46-157">*Counter. Razor* dosyasının en üstündeki `@page` yönergesi, `Counter` bileşeninin bir yönlendirme uç noktası olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="7af46-157">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="7af46-158">`Counter` bileşeni `/counter`gönderilen istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="7af46-158">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="7af46-159">`@page` yönergesi olmadan, bileşen yönlendirilmiş istekleri işlemez, ancak bileşen diğer bileşenler tarafından hala kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7af46-159">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="7af46-160">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="7af46-160">Dependency injection</span></span>

### <a name="opno-locblazor-server-experience"></a>Blazor<span data-ttu-id="7af46-161"> sunucusu deneyimi</span><span class="sxs-lookup"><span data-stu-id="7af46-161"> Server experience</span></span>

<span data-ttu-id="7af46-162">Blazor sunucu uygulamasıyla çalışıyorsanız, `WeatherForecastService` hizmeti bir [tek](xref:fundamentals/dependency-injection#service-lifetimes) `Startup.ConfigureServices`olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="7af46-162">If working with a Blazor Server app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7af46-163">Uygulamanın tamamında [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)yoluyla hizmetin bir örneği mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="7af46-163">An instance of the service is available throughout the app via [dependency injection (DI)](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="7af46-164">`@inject` yönergesi, `WeatherForecastService` hizmetinin örneğini `FetchData` bileşenine eklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7af46-164">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the `FetchData` component.</span></span>

<span data-ttu-id="7af46-165">*Pages/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7af46-165">*Pages/FetchData.razor*:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="7af46-166">`FetchData` bileşeni, `WeatherForecast` nesnelerinin bir dizisini almak için, `ForecastService`olarak eklenen hizmeti kullanır:</span><span class="sxs-lookup"><span data-stu-id="7af46-166">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="opno-locblazor-webassembly-experience"></a>Blazor<span data-ttu-id="7af46-167"> Weelsembly deneyimi</span><span class="sxs-lookup"><span data-stu-id="7af46-167"> WebAssembly experience</span></span>

<span data-ttu-id="7af46-168">Blazor WebAssembly uygulamasıyla çalışıyorsanız, *Wwwroot/Sample-Data* klasöründeki *Hava durumu. JSON* dosyasından Hava durumu tahmin verileri almak için `HttpClient` eklenir.</span><span class="sxs-lookup"><span data-stu-id="7af46-168">If working with a Blazor WebAssembly app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder.</span></span>

<span data-ttu-id="7af46-169">*Pages/FetchData. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7af46-169">*Pages/FetchData.razor*:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="7af46-170">[`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) döngüsü, her tahmin örneğini Hava durumu verileri tablosunda bir satır olarak işlemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="7af46-170">An [`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="7af46-171">Yapılacaklar listesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="7af46-171">Build a todo list</span></span>

<span data-ttu-id="7af46-172">Uygulamaya basit bir yapılacaklar listesi uygulayan yeni bir bileşen ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7af46-172">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="7af46-173">Uygulamalar *klasörüne* *Todo. Razor* adlı boş bir dosya ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7af46-173">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="7af46-174">Bileşen için ilk biçimlendirmeyi belirtin:</span><span class="sxs-lookup"><span data-stu-id="7af46-174">Provide the initial markup for the component:</span></span>

   ```razor
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="7af46-175">Gezinti çubuğuna `Todo` bileşenini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7af46-175">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="7af46-176">`NavMenu` bileşeni (*Shared/NavMenu. Razor*) uygulamanın düzeninde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7af46-176">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="7af46-177">Düzenler, uygulamadaki içeriğin çoğaltılmasını önlemenize olanak sağlayan bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="7af46-177">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="7af46-178">Aşağıdaki liste öğesi işaretlemesini *paylaşılan/NavMenu. Razor* dosyasında var olan liste öğelerinin altına ekleyerek `Todo` bileşeni için bir `<NavLink>` öğesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7af46-178">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="7af46-179">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7af46-179">Rebuild and run the app.</span></span> <span data-ttu-id="7af46-180">`Todo` bileşeni bağlantısının çalıştığından emin olmak için yeni Todo sayfasını ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="7af46-180">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="7af46-181">Bir Todo öğesini temsil eden bir sınıfı tutmak için projenin köküne bir *TodoItem.cs* dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7af46-181">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="7af46-182">`TodoItem` sınıfı için C# aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="7af46-182">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="7af46-183">`Todo` bileşenine geri dönün (*Pages/Todo. Razor*):</span><span class="sxs-lookup"><span data-stu-id="7af46-183">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="7af46-184">`@code` bloğundaki Todo öğeleri için bir alan ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7af46-184">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="7af46-185">`Todo` bileşeni, Todo listesinin durumunu korumak için bu alanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="7af46-185">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="7af46-186">Her Todo öğesini bir liste öğesi (`<li>`) olarak işlemek için sıralanmamış liste işaretlemesi ve bir `foreach` döngüsü ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7af46-186">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="7af46-187">Uygulama, listeye Todo öğeleri eklemek için Kullanıcı arabirimi öğeleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="7af46-187">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="7af46-188">Sıralanmamış listenin (`<ul>...</ul>`) altına bir metin girişi (`<input>`) ve bir düğme (`<button>`) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7af46-188">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="7af46-189">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7af46-189">Rebuild and run the app.</span></span> <span data-ttu-id="7af46-190">**Todo Ekle** düğmesi seçildiğinde, bir olay işleyicisi düğmeye kablolu olmadığı için hiçbir şey olmaz.</span><span class="sxs-lookup"><span data-stu-id="7af46-190">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="7af46-191">`Todo` bileşenine bir `AddTodo` yöntemi ekleyin ve `@onclick` özniteliğini kullanarak düğme seçimleri için kaydedin.</span><span class="sxs-lookup"><span data-stu-id="7af46-191">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="7af46-192">Düğme seçildiğinde C# `AddTodo` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="7af46-192">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="7af46-193">Yeni Todo öğesinin başlığını almak için, `@code` bloğunun üst kısmına bir `newTodo` dize alanı ekleyin ve `<input>` öğesindeki `bind` özniteliğini kullanarak metin girişinin değerine bağlayın:</span><span class="sxs-lookup"><span data-stu-id="7af46-193">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="7af46-194">`AddTodo` yöntemini, belirtilen başlığa sahip `TodoItem` listeye eklemek için güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="7af46-194">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="7af46-195">`newTodo` boş bir dizeye ayarlayarak metin girişinin değerini temizleyin:</span><span class="sxs-lookup"><span data-stu-id="7af46-195">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="7af46-196">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7af46-196">Rebuild and run the app.</span></span> <span data-ttu-id="7af46-197">Yeni kodu test etmek için Todo listesine bazı Todo öğeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7af46-197">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="7af46-198">Her Todo öğesi için başlık metni düzenlenebilir hale getirilebilir ve bir onay kutusu kullanıcının tamamlanmış öğeleri izlemesine yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="7af46-198">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="7af46-199">Her Todo öğesi için bir onay kutusu girişi ekleyin ve değerini `IsDone` özelliğine bağlayın.</span><span class="sxs-lookup"><span data-stu-id="7af46-199">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="7af46-200">`@todo.Title`, `@todo.Title`bağlantılı `<input>` bir öğe olarak değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7af46-200">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="7af46-201">Bu değerlerin bağlandığını doğrulamak için `<h1>` üst bilgisini, tamamlanmamış olan Todo öğelerinin sayısının sayısını gösterecek şekilde güncelleştirin (`IsDone` `false`).</span><span class="sxs-lookup"><span data-stu-id="7af46-201">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```razor
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="7af46-202">Tamamlanan `Todo` bileşeni (*Sayfalar/Todo. Razor*):</span><span class="sxs-lookup"><span data-stu-id="7af46-202">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="7af46-203">Uygulamayı yeniden derleyin ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7af46-203">Rebuild and run the app.</span></span> <span data-ttu-id="7af46-204">Yeni kodu test etmek için Todo öğeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7af46-204">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
