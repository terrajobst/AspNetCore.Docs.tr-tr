---
title: Razor bileşenleri ilk uygulamanızı oluşturun
author: guardrex
description: Adım adım bir Razor bileşenleri uygulaması derleme ve Razor bileşenleri temel kavramları öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 40bf5728193c854589e072730554b4f54086fb6a
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068134"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="d07f9-103">Razor bileşenleri ilk uygulamanızı oluşturun</span><span class="sxs-lookup"><span data-stu-id="d07f9-103">Build your first Razor Components app</span></span>

<span data-ttu-id="d07f9-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d07f9-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="d07f9-105">Bu öğretici, Razor bileşenleri ile uygulama oluşturma işlemini göstermektedir ve Razor bileşenleri temel kavramlarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="d07f9-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="d07f9-106">Bu öğretici (.NET Core 3.0 veya sonraki sürümlerde desteklenir) ya da bir Razor bileşenleri tabanlı proje veya (.NET Core gelecekteki bir sürümde desteklenir) Blazor tabanlı bir proje kullanarak keyfini çıkarabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d07f9-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="d07f9-107">ASP.NET Core Razor bileşenlerini kullanarak bir deneyim için (*önerilen*):</span><span class="sxs-lookup"><span data-stu-id="d07f9-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="d07f9-108">Sunulan yönergeleri <xref:razor-components/get-started> Razor bileşenleri tabanlı bir proje oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="d07f9-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="d07f9-109">Projeyi adlandırın `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="d07f9-109">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="d07f9-110">Blazor kullanarak bir deneyim için:</span><span class="sxs-lookup"><span data-stu-id="d07f9-110">For an experience using Blazor:</span></span>

* <span data-ttu-id="d07f9-111">Sunulan yönergeleri <xref:spa/blazor/get-started> Blazor tabanlı bir proje oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="d07f9-111">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="d07f9-112">Projeyi adlandırın `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="d07f9-112">Name the project `Blazor`.</span></span>

<span data-ttu-id="d07f9-113">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d07f9-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="d07f9-114">Önkoşullar için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="d07f9-114">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="d07f9-115">Yapı bileşenleri</span><span class="sxs-lookup"><span data-stu-id="d07f9-115">Build components</span></span>

1. <span data-ttu-id="d07f9-116">Her uygulamanın üç sayfalarında Gözat *bileşenleri/sayfaları* klasörü (*sayfaları* Blazor içinde): Giriş, sayaç ve veri getirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="d07f9-116">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="d07f9-117">Bu sayfalar, Razor bileşen dosyaları tarafından uygulanır: *Index.Razor*, *Counter.razor*, ve *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="d07f9-117">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="d07f9-118">(Blazor devam kullanılacak *.cshtml* dosya uzantısı: *Index.cshtml*, *Counter.cshtml*, ve *FetchData.cshtml*.)</span><span class="sxs-lookup"><span data-stu-id="d07f9-118">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="d07f9-119">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d07f9-119">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="d07f9-120">Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektiriyor, ancak Razor bileşenleri sağlayan daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="d07f9-120">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="d07f9-121">Uygulama sayacı bileşenin inceleyin *Counter.razor* dosya.</span><span class="sxs-lookup"><span data-stu-id="d07f9-121">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="d07f9-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* Blazor içinde):</span><span class="sxs-lookup"><span data-stu-id="d07f9-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="d07f9-123">Kullanıcı arabirimini sayacı bileşenin HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="d07f9-123">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="d07f9-124">(Örneğin, döngü, koşullular, ifadeleri) dinamik işleme mantığı, katıştırılmış kullanarak eklenir C# adlı söz dizimi [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="d07f9-124">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="d07f9-125">HTML biçimlendirmesi ve C# işleme mantığı, oluşturma zamanında bir bileşen sınıfı dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="d07f9-125">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="d07f9-126">Oluşturulan .NET sınıf adı dosya adıyla eşleşen.</span><span class="sxs-lookup"><span data-stu-id="d07f9-126">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="d07f9-127">Bileşen sınıfı üyeleri tanımlanmış bir `@functions` blok.</span><span class="sxs-lookup"><span data-stu-id="d07f9-127">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="d07f9-128">İçinde `@functions` blok, bileşen durumu (Özellikler, alanlar) ve olay işleme için veya başka bir bileşen mantığı tanımlamak için yöntemleri belirtilir.</span><span class="sxs-lookup"><span data-stu-id="d07f9-128">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="d07f9-129">Bu üyeleri, ardından bileşenin işleme mantığı ve olayları işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d07f9-129">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="d07f9-130">Zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="d07f9-130">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="d07f9-131">Sayaç bileşen kayıtlı `onclick` işleyici çağrılır ( `IncrementCount` yöntemi).</span><span class="sxs-lookup"><span data-stu-id="d07f9-131">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="d07f9-132">Sayaç bileşen kendi işleme ağacında yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d07f9-132">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="d07f9-133">Yeni bir işleme ağacı Öncekine karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="d07f9-133">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="d07f9-134">Yalnızca belge nesne modeli (DOM) değişiklikler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d07f9-134">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="d07f9-135">Görüntülenen sayısı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d07f9-135">The displayed count is updated.</span></span>

1. <span data-ttu-id="d07f9-136">Değiştirme C# bir yerine ikiye sayısı artması yapmak için sayaç bileşeninin mantığı.</span><span class="sxs-lookup"><span data-stu-id="d07f9-136">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="d07f9-137">Yeniden oluşturun ve değişiklikleri görmek için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d07f9-137">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="d07f9-138">Seçin **me tıklayın** düğmeyi ve iki sayacını artırır.</span><span class="sxs-lookup"><span data-stu-id="d07f9-138">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="d07f9-139">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="d07f9-139">Use components</span></span>

<span data-ttu-id="d07f9-140">Bir HTML benzeri sözdizimi kullanarak başka bir bileşene dönüştürerek bileşen içerir.</span><span class="sxs-lookup"><span data-stu-id="d07f9-140">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="d07f9-141">Sayaç bileşen uygulamanın dizini (giriş) bileşenine ekleyerek bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="d07f9-141">Add the Counter component to the app's Index (Home) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="d07f9-142">Bu deneyim için bir anket istemi bileşeni Blazor kullanıyorsanız (`<SurveyPrompt>` öğesi) dizin bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="d07f9-142">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="d07f9-143">Değiştirin `<SurveyPrompt>` öğeyle `<Counter>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="d07f9-143">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="d07f9-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* Blazor içinde):</span><span class="sxs-lookup"><span data-stu-id="d07f9-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="d07f9-145">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d07f9-145">Rebuild and run the app.</span></span> <span data-ttu-id="d07f9-146">Giriş sayfası, kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="d07f9-146">The Home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="d07f9-147">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="d07f9-147">Component parameters</span></span>

<span data-ttu-id="d07f9-148">Bileşenleri parametrelerini de sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d07f9-148">Components can also have parameters.</span></span> <span data-ttu-id="d07f9-149">Bileşen parametreleri, genel olmayan özellikler ile donatılmış bileşen sınıfı kullanarak tanımlanan `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="d07f9-149">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="d07f9-150">Öznitelikleri bir bileşen için bağımsız değişken biçimlendirme içinde belirtmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="d07f9-150">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="d07f9-151">Bileşenin güncelleştirme `@functions` C# kod:</span><span class="sxs-lookup"><span data-stu-id="d07f9-151">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="d07f9-152">Ekleme bir `IncrementAmount` özelliği düzenlenmiş ile `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="d07f9-152">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="d07f9-153">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="d07f9-153">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="d07f9-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* Blazor içinde):</span><span class="sxs-lookup"><span data-stu-id="d07f9-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="d07f9-155">Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="d07f9-155">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="d07f9-156">Sayaç tarafından on Artır bir değere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d07f9-156">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="d07f9-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* Blazor içinde):</span><span class="sxs-lookup"><span data-stu-id="d07f9-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="d07f9-158">Ana sayfayı yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="d07f9-158">Reload the Home page.</span></span> <span data-ttu-id="d07f9-159">Sayaç artırılır on tarafından her zaman **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="d07f9-159">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="d07f9-160">Bir sayacı sayfa artışlarla üzerinde sayacı.</span><span class="sxs-lookup"><span data-stu-id="d07f9-160">The counter on the Counter page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="d07f9-161">Bileşenleri için yol</span><span class="sxs-lookup"><span data-stu-id="d07f9-161">Route to components</span></span>

<span data-ttu-id="d07f9-162">`@page` En üstündeki yönerge *Counter.razor* dosya bu bileşen yönlendirme bir uç nokta olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="d07f9-162">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="d07f9-163">Sayaç bileşeni için gönderilen istekleri işler `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="d07f9-163">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="d07f9-164">Olmadan `@page` yönergesi, bileşen yönlendirilen istekler da işlemiyor ancak bileşeni hala başka bileşenler tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d07f9-164">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="d07f9-165">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="d07f9-165">Dependency injection</span></span>

<span data-ttu-id="d07f9-166">Bileşenleri uygulamanın service kapsayıcısında kayıtlı hizmetlerinin kullanılabilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d07f9-166">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d07f9-167">Halinde bileşenini kullanarak Hizmetleri ekleme `@inject` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="d07f9-167">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="d07f9-168">Örnek uygulamada parçalar bileşeninin yönergeleri inceleyin.</span><span class="sxs-lookup"><span data-stu-id="d07f9-168">Examine the directives of the FetchData component in the sample app.</span></span>

<span data-ttu-id="d07f9-169">Razor bileşenleri örnek uygulamada `WeatherForecastService` hizmet olarak kayıtlı bir [tekil](xref:fundamentals/dependency-injection#service-lifetimes), Hizmeti'nin bir örneğini uygulama boyunca kullanılabilir olması.</span><span class="sxs-lookup"><span data-stu-id="d07f9-169">In the Razor Components sample app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="d07f9-170">`@inject` Yönergesi örneğini ekleme için kullanılan `WeatherForecastService` halinde bileşenini hizmet.</span><span class="sxs-lookup"><span data-stu-id="d07f9-170">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="d07f9-171">*Components/Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="d07f9-171">*Components/Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="d07f9-172">Parçalar bileşeni olarak eklenen hizmet kullanan `ForecastService`, bir dizi alınacak `WeatherForecast` nesneler:</span><span class="sxs-lookup"><span data-stu-id="d07f9-172">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="d07f9-173">Örnek uygulama Blazor sürümünde `HttpClient` hava durumu tahminini verilerden elde etmek için eklenmiş *weather.json* dosyası *wwwroot/örnek-veri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="d07f9-173">In the Blazor version of the sample app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="d07f9-174">*Pages/FetchData.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d07f9-174">*Pages/FetchData.cshtml*:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

<span data-ttu-id="d07f9-175">Her iki örnek uygulamalarda bir [ @foreach ](/dotnet/csharp/language-reference/keywords/foreach-in) döngü, her bir tahmin örneği tablosunda bir satıra hava durumu verilerinin işlemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="d07f9-175">In both sample apps, a [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="d07f9-176">Bir Yapılacaklar listesi oluşturun</span><span class="sxs-lookup"><span data-stu-id="d07f9-176">Build a todo list</span></span>

<span data-ttu-id="d07f9-177">Yeni bir bileşen, bir basit bir Yapılacaklar listesi uygulayan uygulamaya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d07f9-177">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="d07f9-178">Örnek uygulamaya boş bir dosya ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d07f9-178">Add an empty file to the sample app:</span></span>

   * <span data-ttu-id="d07f9-179">Razor bileşenleri deneyimi için ekleme bir *Todo.razor* dosyasını *bileşenleri/sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="d07f9-179">For the Razor Components experience, add a *Todo.razor* file to the *Components/Pages* folder.</span></span>
   * <span data-ttu-id="d07f9-180">Blazor deneyimi için ekleme bir *Todo.cshtml* dosyasını *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="d07f9-180">For the Blazor experience, add a *Todo.cshtml* file to the *Pages* folder.</span></span>

1. <span data-ttu-id="d07f9-181">Bileşen için ilk biçimlendirme sağlar:</span><span class="sxs-lookup"><span data-stu-id="d07f9-181">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="d07f9-182">Todo bileşeni Gezinti çubuğuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d07f9-182">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="d07f9-183">NavMenu bileşeni (*Components/Shared/NavMenu.razor* veya *Shared/NavMenu.cshtml* Blazor içinde) uygulamanın düzende kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d07f9-183">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="d07f9-184">Düzenleri, uygulama içeriği yinelenmesini önlemek izin bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="d07f9-184">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="d07f9-185">Daha fazla bilgi için bkz. <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="d07f9-185">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="d07f9-186">Ekleme bir `<NavLink>` aşağıda bulunan mevcut liste öğelerini aşağıdaki liste öğesi işaretleme ekleyerek Todo bileşeni için *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* Blazor içinde) dosyası:</span><span class="sxs-lookup"><span data-stu-id="d07f9-186">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="d07f9-187">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d07f9-187">Rebuild and run the app.</span></span> <span data-ttu-id="d07f9-188">Todo bileşen bağlantısını çalıştığını onaylamak için yeni Todo sayfasını ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="d07f9-188">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="d07f9-189">Ekleme bir *TodoItem.cs* bir todo öğesini temsil eden bir sınıf tutmak için projenin kök dosya.</span><span class="sxs-lookup"><span data-stu-id="d07f9-189">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="d07f9-190">Aşağıdaki C# için kod `TodoItem` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="d07f9-190">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="d07f9-191">Todo bileşenine döndürür (*Components/Pages/Todo.razor* veya *Pages/Todo.cshtml* Blazor içinde):</span><span class="sxs-lookup"><span data-stu-id="d07f9-191">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="d07f9-192">Açıklamada içinde için bir alan eklemek bir `@functions` blok.</span><span class="sxs-lookup"><span data-stu-id="d07f9-192">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="d07f9-193">Todo bileşeni, yapılacaklar listesi durumunu korumak için bu alanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="d07f9-193">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="d07f9-194">Sırasız liste biçimlendirme eklemek ve `foreach` her bir todo öğesi bir liste öğesi olarak işlenecek döngü.</span><span class="sxs-lookup"><span data-stu-id="d07f9-194">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="d07f9-195">Uygulama, açıklamada listeye eklemek için kullanıcı Arabirimi öğeleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d07f9-195">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="d07f9-196">Bir metin girişi ve listenin altındaki bir düğme ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d07f9-196">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="d07f9-197">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d07f9-197">Rebuild and run the app.</span></span> <span data-ttu-id="d07f9-198">Zaman **todo ekleyin** düğmesi seçilirse, hiçbir şey olmaz kadar düğmesi olay işleyicisi kablolu değildir çünkü.</span><span class="sxs-lookup"><span data-stu-id="d07f9-198">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="d07f9-199">Ekleme bir `AddTodo` Todo bileşen ve düğme için tıkladığında kullanarak kayıt yöntemi `onclick` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="d07f9-199">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="d07f9-200">`AddTodo` C# Yöntemi düğme seçildiğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d07f9-200">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="d07f9-201">Yeni todo öğesinin başlığını almak için ekleyin bir `newTodo` dize alanı ve değeri kullanarak metin olarak bağlama `bind` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="d07f9-201">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="d07f9-202">Güncelleştirme `AddTodo` ekleme yöntemi `TodoItem` listesine belirtilen başlığa sahip.</span><span class="sxs-lookup"><span data-stu-id="d07f9-202">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="d07f9-203">Metin girişi değerini ayarlayarak Temizle `newTodo` boş bir dize:</span><span class="sxs-lookup"><span data-stu-id="d07f9-203">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="d07f9-204">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d07f9-204">Rebuild and run the app.</span></span> <span data-ttu-id="d07f9-205">Bazı açıklamada yeni kodu test etmek için yapılacaklar listesine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d07f9-205">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="d07f9-206">Her bir todo öğesi için başlık metnini düzenlenebilir hale getirilebilir ve onay kutusu tamamlanan öğeleri takip kullanıcı yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="d07f9-206">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="d07f9-207">Her bir todo öğesi için bir onay kutusu giriş ekleyin ve değerini bağlama `IsDone` özelliği.</span><span class="sxs-lookup"><span data-stu-id="d07f9-207">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="d07f9-208">Değişiklik `@todo.Title` için bir `<input>` bağlı öğe `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="d07f9-208">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="d07f9-209">Bu değerleri bağlı olduğunu doğrulamak için güncelleştirme `<h1>` tamamlanmamış todo öğelerini sayısını göstermek için başlık (`IsDone` olan `false`).</span><span class="sxs-lookup"><span data-stu-id="d07f9-209">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="d07f9-210">Tamamlanmış Todo bileşeni (*Components/Pages/Todo.razor* veya *Pages/Todo.cshtml* Blazor içinde):</span><span class="sxs-lookup"><span data-stu-id="d07f9-210">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="d07f9-211">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d07f9-211">Rebuild and run the app.</span></span> <span data-ttu-id="d07f9-212">Yeni kod test etmek için todo öğeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d07f9-212">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="d07f9-213">Yayımlama ve uygulama dağıtma</span><span class="sxs-lookup"><span data-stu-id="d07f9-213">Publish and deploy the app</span></span>

<span data-ttu-id="d07f9-214">Uygulamayı yayımlamak için bkz: <xref:host-and-deploy/razor-components-blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="d07f9-214">To publish the app, see <xref:host-and-deploy/razor-components-blazor/index>.</span></span>
