---
title: İlk Blazor uygulamanızı oluşturun
author: guardrex
description: Adım adım bir Blazor uygulaması oluşturun.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: c52efcb1224e2bbea56fa55c70faf253ef96d433
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982765"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="84a4c-103">İlk Blazor uygulamanızı oluşturun</span><span class="sxs-lookup"><span data-stu-id="84a4c-103">Build your first Blazor app</span></span>

<span data-ttu-id="84a4c-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="84a4c-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="84a4c-105">Bu öğreticide oluşturun ve Blazor uygulamayı değiştirmek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="84a4c-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="84a4c-106">Sunulan yönergeleri <xref:blazor/get-started> makalenin Bu öğreticide bir Blazor projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="84a4c-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span>

## <a name="build-components"></a><span data-ttu-id="84a4c-107">Yapı bileşenleri</span><span class="sxs-lookup"><span data-stu-id="84a4c-107">Build components</span></span>

1. <span data-ttu-id="84a4c-108">Her uygulamanın üç sayfalarında Gözat *sayfaları* klasörü: Giriş, sayaç ve veri getirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="84a4c-108">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="84a4c-109">Bu sayfalar, Razor bileşen dosyaları tarafından uygulanır: *Index.Razor*, *Counter.razor*, ve *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="84a4c-109">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="84a4c-110">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="84a4c-110">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="84a4c-111">Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektirir, ancak Blazor sağlar daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="84a4c-111">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="84a4c-112">Uygulama sayacı bileşenin inceleyin *Counter.razor* dosya.</span><span class="sxs-lookup"><span data-stu-id="84a4c-112">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="84a4c-113">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="84a4c-113">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="84a4c-114">Kullanıcı arabirimini sayacı bileşenin HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="84a4c-114">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="84a4c-115">(Örneğin, döngü, koşullular, ifadeleri) dinamik işleme mantığı, katıştırılmış kullanarak eklenir C# adlı söz dizimi [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="84a4c-115">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="84a4c-116">HTML biçimlendirmesi ve C# işleme mantığı, oluşturma zamanında bir bileşen sınıfı dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="84a4c-116">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="84a4c-117">Oluşturulan .NET sınıf adı dosya adıyla eşleşen.</span><span class="sxs-lookup"><span data-stu-id="84a4c-117">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="84a4c-118">Bileşen sınıfı üyeleri tanımlanmış bir `@functions` blok.</span><span class="sxs-lookup"><span data-stu-id="84a4c-118">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="84a4c-119">İçinde `@functions` blok, bileşen durumu (Özellikler, alanlar) ve olay işleme için veya başka bir bileşen mantığı tanımlamak için yöntemleri belirtilir.</span><span class="sxs-lookup"><span data-stu-id="84a4c-119">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="84a4c-120">Bu üyeleri, ardından bileşenin işleme mantığı ve olayları işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="84a4c-120">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="84a4c-121">Zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="84a4c-121">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="84a4c-122">Sayaç bileşen kayıtlı `onclick` işleyici çağrılır ( `IncrementCount` yöntemi).</span><span class="sxs-lookup"><span data-stu-id="84a4c-122">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="84a4c-123">Sayaç bileşen kendi işleme ağacında yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="84a4c-123">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="84a4c-124">Yeni bir işleme ağacı Öncekine karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="84a4c-124">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="84a4c-125">Yalnızca belge nesne modeli (DOM) değişiklikler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="84a4c-125">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="84a4c-126">Görüntülenen sayısı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="84a4c-126">The displayed count is updated.</span></span>

1. <span data-ttu-id="84a4c-127">Değiştirme C# bir yerine ikiye sayısı artması yapmak için sayaç bileşeninin mantığı.</span><span class="sxs-lookup"><span data-stu-id="84a4c-127">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="84a4c-128">Yeniden oluşturun ve değişiklikleri görmek için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="84a4c-128">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="84a4c-129">Seçin **me tıklayın** düğmeyi ve iki sayacını artırır.</span><span class="sxs-lookup"><span data-stu-id="84a4c-129">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="84a4c-130">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="84a4c-130">Use components</span></span>

<span data-ttu-id="84a4c-131">Bir HTML benzeri sözdizimi kullanarak başka bir bileşene dönüştürerek bileşen içerir.</span><span class="sxs-lookup"><span data-stu-id="84a4c-131">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="84a4c-132">Sayaç bileşen uygulamanın dizini (giriş) bileşenine ekleyerek bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="84a4c-132">Add the Counter component to the app's Index (Home) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="84a4c-133">Bu deneyim için bir anket istemi bileşeni Blazor kullanıyorsanız (`<SurveyPrompt>` öğesi) dizin bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="84a4c-133">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="84a4c-134">Değiştirin `<SurveyPrompt>` öğeyle `<Counter>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="84a4c-134">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="84a4c-135">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="84a4c-135">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="84a4c-136">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="84a4c-136">Rebuild and run the app.</span></span> <span data-ttu-id="84a4c-137">Giriş sayfası, kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="84a4c-137">The Home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="84a4c-138">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="84a4c-138">Component parameters</span></span>

<span data-ttu-id="84a4c-139">Bileşenleri parametrelerini de sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="84a4c-139">Components can also have parameters.</span></span> <span data-ttu-id="84a4c-140">Bileşen parametreleri, genel olmayan özellikler ile donatılmış bileşen sınıfı kullanarak tanımlanan `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="84a4c-140">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="84a4c-141">Öznitelikleri bir bileşen için bağımsız değişken biçimlendirme içinde belirtmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="84a4c-141">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="84a4c-142">Bileşenin güncelleştirme `@functions` C# kod:</span><span class="sxs-lookup"><span data-stu-id="84a4c-142">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="84a4c-143">Ekleme bir `IncrementAmount` özelliği düzenlenmiş ile `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="84a4c-143">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="84a4c-144">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="84a4c-144">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="84a4c-145">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="84a4c-145">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="84a4c-146">Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="84a4c-146">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="84a4c-147">Sayaç tarafından on Artır bir değere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="84a4c-147">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="84a4c-148">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="84a4c-148">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="84a4c-149">Ana sayfayı yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="84a4c-149">Reload the Home page.</span></span> <span data-ttu-id="84a4c-150">Sayaç artırılır on tarafından her zaman **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="84a4c-150">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="84a4c-151">Bir sayacı sayfa artışlarla üzerinde sayacı.</span><span class="sxs-lookup"><span data-stu-id="84a4c-151">The counter on the Counter page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="84a4c-152">Bileşenleri için yol</span><span class="sxs-lookup"><span data-stu-id="84a4c-152">Route to components</span></span>

<span data-ttu-id="84a4c-153">`@page` En üstündeki yönerge *Counter.razor* dosya bu bileşen yönlendirme bir uç nokta olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="84a4c-153">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="84a4c-154">Sayaç bileşeni için gönderilen istekleri işler `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="84a4c-154">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="84a4c-155">Olmadan `@page` yönergesi, bileşen yönlendirilen istekler da işlemiyor ancak bileşeni hala başka bileşenler tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="84a4c-155">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="84a4c-156">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="84a4c-156">Dependency injection</span></span>

<span data-ttu-id="84a4c-157">Bileşenleri uygulamanın service kapsayıcısında kayıtlı hizmetlerinin kullanılabilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="84a4c-157">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="84a4c-158">Halinde bileşenini kullanarak Hizmetleri ekleme `@inject` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="84a4c-158">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="84a4c-159">Parçalar bileşenin yönergeleri inceleyin.</span><span class="sxs-lookup"><span data-stu-id="84a4c-159">Examine the directives of the FetchData component.</span></span>

<span data-ttu-id="84a4c-160">Blazor sunucu tarafı uygulama ile çalışıyorsanız `WeatherForecastService` hizmet olarak kayıtlı bir [tekil](xref:fundamentals/dependency-injection#service-lifetimes), Hizmeti'nin bir örneğini uygulama boyunca kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="84a4c-160">If working with a Blazor server-side app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="84a4c-161">`@inject` Yönergesi örneğini ekleme için kullanılan `WeatherForecastService` halinde bileşenini hizmet.</span><span class="sxs-lookup"><span data-stu-id="84a4c-161">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="84a4c-162">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="84a4c-162">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="84a4c-163">Parçalar bileşeni olarak eklenen hizmet kullanan `ForecastService`, bir dizi alınacak `WeatherForecast` nesneler:</span><span class="sxs-lookup"><span data-stu-id="84a4c-163">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="84a4c-164">Bir Blazor istemci-tarafı uygulaması ile çalışıyorsanız `HttpClient` hava durumu tahminini verilerden elde etmek için eklenmiş *weather.json* dosyası *wwwroot/örnek-veri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="84a4c-164">If working with a Blazor client-side app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="84a4c-165">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="84a4c-165">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7)]

<span data-ttu-id="84a4c-166">A [ \@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) döngü, her bir tahmin örneği tablosunda bir satıra hava durumu verilerinin işlemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="84a4c-166">A [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a><span data-ttu-id="84a4c-167">Bir Yapılacaklar listesi oluşturun</span><span class="sxs-lookup"><span data-stu-id="84a4c-167">Build a todo list</span></span>

<span data-ttu-id="84a4c-168">Yeni bir bileşen, bir basit bir Yapılacaklar listesi uygulayan uygulamaya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="84a4c-168">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="84a4c-169">Adlı boş bir dosya ekleme *Todo.razor* uygulamada *sayfaları* klasörü:</span><span class="sxs-lookup"><span data-stu-id="84a4c-169">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="84a4c-170">Bileşen için ilk biçimlendirme sağlar:</span><span class="sxs-lookup"><span data-stu-id="84a4c-170">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="84a4c-171">Todo bileşeni Gezinti çubuğuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="84a4c-171">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="84a4c-172">NavMenu bileşeni (*Pages/Shared/NavMenu.razor*) uygulamanın düzende kullanılır.</span><span class="sxs-lookup"><span data-stu-id="84a4c-172">The NavMenu component (*Pages/Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="84a4c-173">Düzenleri, uygulama içeriği yinelenmesini önlemek izin bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="84a4c-173">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="84a4c-174">Daha fazla bilgi için bkz. <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="84a4c-174">For more information, see <xref:blazor/layouts>.</span></span>

   <span data-ttu-id="84a4c-175">Ekleme bir `<NavLink>` aşağıda bulunan mevcut liste öğelerini aşağıdaki liste öğesi işaretleme ekleyerek Todo bileşeni için *Pages/Shared/NavMenu.razor* dosyası:</span><span class="sxs-lookup"><span data-stu-id="84a4c-175">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Pages/Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="84a4c-176">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="84a4c-176">Rebuild and run the app.</span></span> <span data-ttu-id="84a4c-177">Todo bileşen bağlantısını çalıştığını onaylamak için yeni Todo sayfasını ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="84a4c-177">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="84a4c-178">Ekleme bir *TodoItem.cs* bir todo öğesini temsil eden bir sınıf tutmak için projenin kök dosya.</span><span class="sxs-lookup"><span data-stu-id="84a4c-178">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="84a4c-179">Aşağıdaki C# için kod `TodoItem` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="84a4c-179">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="84a4c-180">Todo bileşenine döndürür (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="84a4c-180">Return to the Todo component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="84a4c-181">Açıklamada içinde için bir alan eklemek bir `@functions` blok.</span><span class="sxs-lookup"><span data-stu-id="84a4c-181">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="84a4c-182">Todo bileşeni, yapılacaklar listesi durumunu korumak için bu alanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="84a4c-182">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="84a4c-183">Sırasız liste biçimlendirme eklemek ve `foreach` her bir todo öğesi bir liste öğesi olarak işlenecek döngü.</span><span class="sxs-lookup"><span data-stu-id="84a4c-183">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="84a4c-184">Uygulama, açıklamada listeye eklemek için kullanıcı Arabirimi öğeleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="84a4c-184">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="84a4c-185">Bir metin girişi ve listenin altındaki bir düğme ekleyin:</span><span class="sxs-lookup"><span data-stu-id="84a4c-185">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="84a4c-186">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="84a4c-186">Rebuild and run the app.</span></span> <span data-ttu-id="84a4c-187">Zaman **todo ekleyin** düğmesi seçilirse, hiçbir şey olmaz kadar düğmesi olay işleyicisi kablolu değildir çünkü.</span><span class="sxs-lookup"><span data-stu-id="84a4c-187">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="84a4c-188">Ekleme bir `AddTodo` Todo bileşen ve düğme için tıkladığında kullanarak kayıt yöntemi `onclick` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="84a4c-188">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="84a4c-189">`AddTodo` C# Yöntemi düğme seçildiğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="84a4c-189">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="84a4c-190">Yeni todo öğesinin başlığını almak için ekleyin bir `newTodo` dize alanı ve değeri kullanarak metin olarak bağlama `bind` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="84a4c-190">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. <span data-ttu-id="84a4c-191">Güncelleştirme `AddTodo` ekleme yöntemi `TodoItem` listesine belirtilen başlığa sahip.</span><span class="sxs-lookup"><span data-stu-id="84a4c-191">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="84a4c-192">Metin girişi değerini ayarlayarak Temizle `newTodo` boş bir dize:</span><span class="sxs-lookup"><span data-stu-id="84a4c-192">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="84a4c-193">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="84a4c-193">Rebuild and run the app.</span></span> <span data-ttu-id="84a4c-194">Bazı açıklamada yeni kodu test etmek için yapılacaklar listesine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="84a4c-194">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="84a4c-195">Her bir todo öğesi için başlık metnini düzenlenebilir hale getirilebilir ve onay kutusu tamamlanan öğeleri takip kullanıcı yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="84a4c-195">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="84a4c-196">Her bir todo öğesi için bir onay kutusu giriş ekleyin ve değerini bağlama `IsDone` özelliği.</span><span class="sxs-lookup"><span data-stu-id="84a4c-196">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="84a4c-197">Değişiklik `@todo.Title` için bir `<input>` bağlı öğe `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="84a4c-197">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="84a4c-198">Bu değerleri bağlı olduğunu doğrulamak için güncelleştirme `<h1>` tamamlanmamış todo öğelerini sayısını göstermek için başlık (`IsDone` olan `false`).</span><span class="sxs-lookup"><span data-stu-id="84a4c-198">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="84a4c-199">Tamamlanmış Todo bileşeni (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="84a4c-199">The completed Todo component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="84a4c-200">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="84a4c-200">Rebuild and run the app.</span></span> <span data-ttu-id="84a4c-201">Yeni kod test etmek için todo öğeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="84a4c-201">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="84a4c-202">Yayımlama ve uygulama dağıtma</span><span class="sxs-lookup"><span data-stu-id="84a4c-202">Publish and deploy the app</span></span>

<span data-ttu-id="84a4c-203">Uygulamayı yayımlamak için bkz: <xref:host-and-deploy/blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="84a4c-203">To publish the app, see <xref:host-and-deploy/blazor/index>.</span></span>
