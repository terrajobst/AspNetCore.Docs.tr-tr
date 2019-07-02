---
title: İlk Blazor uygulamanızı oluşturun
author: guardrex
description: Adım adım bir Blazor uygulaması oluşturun.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: d592c5bac1eb9822843a1ad1513a15fdfd6b1032
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500309"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="c1404-103">İlk Blazor uygulamanızı oluşturun</span><span class="sxs-lookup"><span data-stu-id="c1404-103">Build your first Blazor app</span></span>

<span data-ttu-id="c1404-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c1404-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c1404-105">Bu öğreticide oluşturun ve Blazor uygulamayı değiştirmek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="c1404-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="c1404-106">Sunulan yönergeleri <xref:blazor/get-started> makalenin Bu öğreticide bir Blazor projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c1404-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="c1404-107">Projeyi adlandırın *ToDoList*.</span><span class="sxs-lookup"><span data-stu-id="c1404-107">Name the project *ToDoList*.</span></span>

## <a name="build-components"></a><span data-ttu-id="c1404-108">Yapı bileşenleri</span><span class="sxs-lookup"><span data-stu-id="c1404-108">Build components</span></span>

1. <span data-ttu-id="c1404-109">Her uygulamanın üç sayfalarında Gözat *sayfaları* klasörü: Giriş, sayaç ve veri getirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="c1404-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="c1404-110">Bu sayfalar Razor bileşen dosyaları tarafından uygulanan *Index.razor*, *Counter.razor*, ve *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="c1404-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="c1404-111">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c1404-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="c1404-112">Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektirir, ancak Blazor sağlar daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="c1404-112">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="c1404-113">Uygulamasını inceleyin `Counter` bileşeninin *Counter.razor* dosya.</span><span class="sxs-lookup"><span data-stu-id="c1404-113">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="c1404-114">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="c1404-114">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="c1404-115">Kullanıcı Arabiriminde `Counter` bileşen HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="c1404-115">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="c1404-116">(Örneğin, döngü, koşullular, ifadeleri) dinamik işleme mantığı, katıştırılmış kullanarak eklenir C# adlı söz dizimi [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="c1404-116">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="c1404-117">HTML biçimlendirmesi ve C# işleme mantığı, oluşturma zamanında bir bileşen sınıfı dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="c1404-117">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="c1404-118">Oluşturulan .NET sınıf adı dosya adıyla eşleşen.</span><span class="sxs-lookup"><span data-stu-id="c1404-118">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="c1404-119">Bileşen sınıfı üyeleri tanımlanmış bir `@code` blok.</span><span class="sxs-lookup"><span data-stu-id="c1404-119">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="c1404-120">İçinde `@code` blok, bileşen durumu (Özellikler, alanlar) ve olay işleme için veya başka bir bileşen mantığı tanımlamak için yöntemleri belirtilir.</span><span class="sxs-lookup"><span data-stu-id="c1404-120">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="c1404-121">Bu üyeleri, ardından bileşenin işleme mantığı ve olayları işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c1404-121">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="c1404-122">Zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="c1404-122">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="c1404-123">`Counter` Bileşeni kayıtlı `onclick` işleyici çağrılır ( `IncrementCount` yöntemi).</span><span class="sxs-lookup"><span data-stu-id="c1404-123">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="c1404-124">`Counter` Bileşen kendi işleme ağacında yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c1404-124">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="c1404-125">Yeni bir işleme ağacı Öncekine karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="c1404-125">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="c1404-126">Yalnızca belge nesne modeli (DOM) değişiklikler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="c1404-126">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="c1404-127">Görüntülenen sayısı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c1404-127">The displayed count is updated.</span></span>

1. <span data-ttu-id="c1404-128">Değiştirme C# mantığını `Counter` bir yerine ikiye sayısı artması yapmak bileşeni.</span><span class="sxs-lookup"><span data-stu-id="c1404-128">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="c1404-129">Yeniden oluşturun ve değişiklikleri görmek için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c1404-129">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="c1404-130">Seçin **me tıklayın** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c1404-130">Select the **Click me** button.</span></span> <span data-ttu-id="c1404-131">İki sayacını artırır.</span><span class="sxs-lookup"><span data-stu-id="c1404-131">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="c1404-132">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="c1404-132">Use components</span></span>

<span data-ttu-id="c1404-133">Bir bileşen HTML söz dizimi kullanılarak başka bir bileşeni içerir.</span><span class="sxs-lookup"><span data-stu-id="c1404-133">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="c1404-134">Ekleme `Counter` uygulamanın bileşeni `Index` bileşen ekleyerek bir `<Counter />` öğesine `Index` bileşeni (*Index.razor*).</span><span class="sxs-lookup"><span data-stu-id="c1404-134">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="c1404-135">Bu deneyim için Blazor istemci-tarafı kullanıyorsanız, bir `SurveyPrompt` bileşeni tarafından kullanılan `Index` bileşeni.</span><span class="sxs-lookup"><span data-stu-id="c1404-135">If you're using Blazor client-side for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="c1404-136">Değiştirin `<SurveyPrompt>` öğesi ile bir `<Counter />` öğesi.</span><span class="sxs-lookup"><span data-stu-id="c1404-136">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="c1404-137">Bu deneyim için Blazor sunucu-tarafı uygulaması kullanıyorsanız, ekleyin `<Counter />` öğesine `Index` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="c1404-137">If you're using a Blazor server-side app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="c1404-138">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="c1404-138">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="c1404-139">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c1404-139">Rebuild and run the app.</span></span> <span data-ttu-id="c1404-140">`Index` Bileşen kendi sayaç sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c1404-140">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="c1404-141">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="c1404-141">Component parameters</span></span>

<span data-ttu-id="c1404-142">Bileşenleri parametrelerini de sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c1404-142">Components can also have parameters.</span></span> <span data-ttu-id="c1404-143">Bileşen parametreleri, genel olmayan özellikler ile donatılmış bileşen sınıfı kullanarak tanımlanan `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="c1404-143">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="c1404-144">Öznitelikleri bir bileşen için bağımsız değişken biçimlendirme içinde belirtmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="c1404-144">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="c1404-145">Bileşenin güncelleştirme `@code` C# kod:</span><span class="sxs-lookup"><span data-stu-id="c1404-145">Update the component's `@code` C# code:</span></span>

   * <span data-ttu-id="c1404-146">Ekleme bir `IncrementAmount` özelliği düzenlenmiş ile `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="c1404-146">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="c1404-147">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="c1404-147">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="c1404-148">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="c1404-148">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="c1404-149">Belirtin bir `IncrementAmount` parametresinde `Index` bileşenin `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="c1404-149">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="c1404-150">Sayaç tarafından on Artır bir değere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="c1404-150">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="c1404-151">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="c1404-151">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="c1404-152">Reload `Index` bileşeni.</span><span class="sxs-lookup"><span data-stu-id="c1404-152">Reload the `Index` component.</span></span> <span data-ttu-id="c1404-153">Sayaç artırılır on tarafından her zaman **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="c1404-153">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="c1404-154">Sayaç `Counter` bileşen bir artırmasına devam eder.</span><span class="sxs-lookup"><span data-stu-id="c1404-154">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="c1404-155">Bileşenleri için yol</span><span class="sxs-lookup"><span data-stu-id="c1404-155">Route to components</span></span>

<span data-ttu-id="c1404-156">`@page` En üstündeki yönerge *Counter.razor* dosyasını belirtir `Counter` yönlendirme bir uç nokta bileşendir.</span><span class="sxs-lookup"><span data-stu-id="c1404-156">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="c1404-157">`Counter` Bileşeni gönderilen istekleri işler `/counter`.</span><span class="sxs-lookup"><span data-stu-id="c1404-157">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="c1404-158">Olmadan `@page` yönergesi, bir bileşen yönlendirilen istekler da işlemiyor ancak bileşeni hala başka bileşenler tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c1404-158">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="c1404-159">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="c1404-159">Dependency injection</span></span>

<span data-ttu-id="c1404-160">Bileşenleri uygulamanın service kapsayıcısında kayıtlı hizmetlerinin kullanılabilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c1404-160">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c1404-161">Halinde bileşenini kullanarak Hizmetleri ekleme `@inject` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="c1404-161">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="c1404-162">Yönergeleri inceleyin `FetchData` bileşeni.</span><span class="sxs-lookup"><span data-stu-id="c1404-162">Examine the directives of the `FetchData` component.</span></span>

<span data-ttu-id="c1404-163">Blazor sunucu tarafı uygulama ile çalışıyorsanız `WeatherForecastService` hizmet olarak kayıtlı bir [tekil](xref:fundamentals/dependency-injection#service-lifetimes), Hizmeti'nin bir örneğini uygulama boyunca kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c1404-163">If working with a Blazor server-side app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="c1404-164">`@inject` Yönergesi örneğini ekleme için kullanılan `WeatherForecastService` halinde bileşenini hizmet.</span><span class="sxs-lookup"><span data-stu-id="c1404-164">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="c1404-165">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="c1404-165">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="c1404-166">`FetchData` Bileşeni olarak eklenen hizmet kullanan `ForecastService`, bir dizi alınacak `WeatherForecast` nesneler:</span><span class="sxs-lookup"><span data-stu-id="c1404-166">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="c1404-167">Bir Blazor istemci-tarafı uygulaması ile çalışıyorsanız `HttpClient` hava durumu tahminini verilerden elde etmek için eklenmiş *weather.json* dosyası *wwwroot/örnek-veri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="c1404-167">If working with a Blazor client-side app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="c1404-168">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="c1404-168">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="c1404-169">A [ \@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) döngü, her bir tahmin örneği tablosunda bir satıra hava durumu verilerinin işlemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="c1404-169">A [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a><span data-ttu-id="c1404-170">Bir Yapılacaklar listesi oluşturun</span><span class="sxs-lookup"><span data-stu-id="c1404-170">Build a todo list</span></span>

<span data-ttu-id="c1404-171">Yeni bir bileşen, bir basit bir Yapılacaklar listesi uygulayan uygulamaya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c1404-171">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="c1404-172">Adlı boş bir dosya ekleme *Todo.razor* uygulamada *sayfaları* klasörü:</span><span class="sxs-lookup"><span data-stu-id="c1404-172">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="c1404-173">Bileşen için ilk biçimlendirme sağlar:</span><span class="sxs-lookup"><span data-stu-id="c1404-173">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="c1404-174">Ekleme `Todo` gezinti çubuğunu bileşeni.</span><span class="sxs-lookup"><span data-stu-id="c1404-174">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="c1404-175">`NavMenu` Bileşeni (*Shared/NavMenu.razor*) uygulamanın düzende kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c1404-175">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="c1404-176">Düzenleri, uygulama içeriği yinelenmesini önlemek izin bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="c1404-176">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="c1404-177">Ekleme bir `<NavLink>` öğesi için `Todo` aşağıdaki liste öğesi işaretleme mevcut listenin altındaki ekleyerek bileşen öğeleri *Shared/NavMenu.razor* dosyası:</span><span class="sxs-lookup"><span data-stu-id="c1404-177">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="c1404-178">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c1404-178">Rebuild and run the app.</span></span> <span data-ttu-id="c1404-179">Bağlantıyı doğrulamak için yeni Todo sayfasını ziyaret edin `Todo` bileşen çalışır.</span><span class="sxs-lookup"><span data-stu-id="c1404-179">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="c1404-180">Ekleme bir *TodoItem.cs* bir todo öğesini temsil eden bir sınıf tutmak için projenin kök dosya.</span><span class="sxs-lookup"><span data-stu-id="c1404-180">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="c1404-181">Aşağıdaki C# için kod `TodoItem` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="c1404-181">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="c1404-182">Geri dönüp `Todo` bileşeni (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="c1404-182">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="c1404-183">Todo öğeleri için bir alan eklemek bir `@code` blok.</span><span class="sxs-lookup"><span data-stu-id="c1404-183">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="c1404-184">`Todo` Bileşen Yapılacaklar listesi durumunu korumak için bu alanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="c1404-184">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="c1404-185">Sırasız liste biçimlendirme eklemek ve `foreach` her bir todo öğesi bir liste öğesi olarak işlenecek döngü (`<li>`).</span><span class="sxs-lookup"><span data-stu-id="c1404-185">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="c1404-186">Uygulama yapılacaklar listesine eklemek için kullanıcı Arabirimi öğeleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="c1404-186">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="c1404-187">Bir metin girişi ekleyin (`<input>`) ve bir düğmeyi (`<button>`) sırasız liste aşağıda (`<ul>...</ul>`):</span><span class="sxs-lookup"><span data-stu-id="c1404-187">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="c1404-188">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c1404-188">Rebuild and run the app.</span></span> <span data-ttu-id="c1404-189">Zaman **todo ekleyin** düğmesi seçilirse, hiçbir şey olmaz kadar düğmesi olay işleyicisi kablolu değildir çünkü.</span><span class="sxs-lookup"><span data-stu-id="c1404-189">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="c1404-190">Ekleme bir `AddTodo` yönteme `Todo` bileşeni ve kullanarak düğmesi seçimlerini kaydetmek `@onclick` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="c1404-190">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="c1404-191">`AddTodo` C# Yöntemi düğme seçildiğinde çağrılır:</span><span class="sxs-lookup"><span data-stu-id="c1404-191">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="c1404-192">Yeni todo öğesinin başlığını almak için ekleyin bir `newTodo` dize alanı en üstündeki `@code` engelleme ve değeri kullanarak metin olarak bağlama `bind` özniteliğini `<input>` öğesi:</span><span class="sxs-lookup"><span data-stu-id="c1404-192">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="@newTodo" />
   ```

1. <span data-ttu-id="c1404-193">Güncelleştirme `AddTodo` ekleme yöntemi `TodoItem` listesine belirtilen başlığa sahip.</span><span class="sxs-lookup"><span data-stu-id="c1404-193">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="c1404-194">Metin girişi değerini ayarlayarak Temizle `newTodo` boş bir dize:</span><span class="sxs-lookup"><span data-stu-id="c1404-194">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="c1404-195">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c1404-195">Rebuild and run the app.</span></span> <span data-ttu-id="c1404-196">Yeni kod test etmek için yapılacaklar listesi birkaç Yapılacaklar öğesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c1404-196">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="c1404-197">Her bir todo öğesi için başlık metnini düzenlenebilir hale getirilebilir ve onay kutusu tamamlanan öğeleri takip kullanıcı yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="c1404-197">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="c1404-198">Her bir todo öğesi için bir onay kutusu giriş ekleyin ve değerini bağlama `IsDone` özelliği.</span><span class="sxs-lookup"><span data-stu-id="c1404-198">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="c1404-199">Değişiklik `@todo.Title` için bir `<input>` bağlı öğe `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="c1404-199">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="c1404-200">Bu değerleri bağlı olduğunu doğrulamak için güncelleştirme `<h1>` tamamlanmamış todo öğelerini sayısını göstermek için başlık (`IsDone` olan `false`).</span><span class="sxs-lookup"><span data-stu-id="c1404-200">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="c1404-201">Tamamlanan `Todo` bileşeni (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="c1404-201">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="c1404-202">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c1404-202">Rebuild and run the app.</span></span> <span data-ttu-id="c1404-203">Yeni kod test etmek için todo öğeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c1404-203">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
