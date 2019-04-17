---
title: İlk Blazor uygulamanızı oluşturun
author: guardrex
description: Adım adım bir Blazor uygulaması oluşturun.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 310eb211f68076d6f52d6427940e07736d833e5b
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614913"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="d4c52-103">İlk Blazor uygulamanızı oluşturun</span><span class="sxs-lookup"><span data-stu-id="d4c52-103">Build your first Blazor app</span></span>

<span data-ttu-id="d4c52-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d4c52-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="d4c52-105">Bu öğreticide, sunucu tarafı Razor bileşenler veya istemci tarafı Blazor ile bir uygulama derleme işlemini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="d4c52-105">This tutorial shows you how to build an app with server-side Razor Components or client-side Blazor.</span></span>

<span data-ttu-id="d4c52-106">Sunucu tarafı ASP.NET Core Razor bileşenlerini kullanarak bir deneyim için (*önerilen*):</span><span class="sxs-lookup"><span data-stu-id="d4c52-106">For an experience using server-side ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="d4c52-107">İzleyin *Razor bileşenleri deneyimi* kılavuzunda <xref:blazor/get-started#server-side-razor-components-experience> Razor bileşenleri tabanlı bir proje oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="d4c52-107">Follow the *Razor Components experience* guidance in <xref:blazor/get-started#server-side-razor-components-experience> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="d4c52-108">Projeyi adlandırın `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="d4c52-108">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="d4c52-109">Blazor kullanarak bir deneyim için:</span><span class="sxs-lookup"><span data-stu-id="d4c52-109">For an experience using Blazor:</span></span>

* <span data-ttu-id="d4c52-110">İzleyin *Blazor deneyimi* kılavuzunda <xref:blazor/get-started#client-side-blazor-experience> Blazor tabanlı bir proje oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="d4c52-110">Follow the *Blazor experience* guidance in <xref:blazor/get-started#client-side-blazor-experience> to create a Blazor-based project.</span></span>
* <span data-ttu-id="d4c52-111">Projeyi adlandırın `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="d4c52-111">Name the project `Blazor`.</span></span>

<span data-ttu-id="d4c52-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d4c52-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="d4c52-113">Önkoşullar için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="d4c52-113">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="d4c52-114">Yapı bileşenleri</span><span class="sxs-lookup"><span data-stu-id="d4c52-114">Build components</span></span>

1. <span data-ttu-id="d4c52-115">Her uygulamanın üç sayfalarında Gözat *bileşenleri/sayfaları* klasörü (*sayfaları* Blazor içinde): Giriş, sayaç ve veri getirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="d4c52-115">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="d4c52-116">Bu sayfalar, Razor bileşen dosyaları tarafından uygulanır: *Index.Razor*, *Counter.razor*, ve *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="d4c52-116">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="d4c52-117">(Blazor devam kullanılacak *.cshtml* dosya uzantısı: *Index.cshtml*, *Counter.cshtml*, ve *FetchData.cshtml*.)</span><span class="sxs-lookup"><span data-stu-id="d4c52-117">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="d4c52-118">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d4c52-118">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="d4c52-119">Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektirir, ancak Blazor sağlar daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="d4c52-119">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="d4c52-120">Uygulama sayacı bileşenin inceleyin *Counter.razor* dosya.</span><span class="sxs-lookup"><span data-stu-id="d4c52-120">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="d4c52-121">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* Blazor içinde):</span><span class="sxs-lookup"><span data-stu-id="d4c52-121">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="d4c52-122">Kullanıcı arabirimini sayacı bileşenin HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="d4c52-122">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="d4c52-123">(Örneğin, döngü, koşullular, ifadeleri) dinamik işleme mantığı, katıştırılmış kullanarak eklenir C# adlı söz dizimi [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="d4c52-123">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="d4c52-124">HTML biçimlendirmesi ve C# işleme mantığı, oluşturma zamanında bir bileşen sınıfı dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="d4c52-124">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="d4c52-125">Oluşturulan .NET sınıf adı dosya adıyla eşleşen.</span><span class="sxs-lookup"><span data-stu-id="d4c52-125">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="d4c52-126">Bileşen sınıfı üyeleri tanımlanmış bir `@functions` blok.</span><span class="sxs-lookup"><span data-stu-id="d4c52-126">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="d4c52-127">İçinde `@functions` blok, bileşen durumu (Özellikler, alanlar) ve olay işleme için veya başka bir bileşen mantığı tanımlamak için yöntemleri belirtilir.</span><span class="sxs-lookup"><span data-stu-id="d4c52-127">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="d4c52-128">Bu üyeleri, ardından bileşenin işleme mantığı ve olayları işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d4c52-128">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="d4c52-129">Zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="d4c52-129">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="d4c52-130">Sayaç bileşen kayıtlı `onclick` işleyici çağrılır ( `IncrementCount` yöntemi).</span><span class="sxs-lookup"><span data-stu-id="d4c52-130">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="d4c52-131">Sayaç bileşen kendi işleme ağacında yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d4c52-131">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="d4c52-132">Yeni bir işleme ağacı Öncekine karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="d4c52-132">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="d4c52-133">Yalnızca belge nesne modeli (DOM) değişiklikler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d4c52-133">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="d4c52-134">Görüntülenen sayısı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d4c52-134">The displayed count is updated.</span></span>

1. <span data-ttu-id="d4c52-135">Değiştirme C# bir yerine ikiye sayısı artması yapmak için sayaç bileşeninin mantığı.</span><span class="sxs-lookup"><span data-stu-id="d4c52-135">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="d4c52-136">Yeniden oluşturun ve değişiklikleri görmek için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4c52-136">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="d4c52-137">Seçin **me tıklayın** düğmeyi ve iki sayacını artırır.</span><span class="sxs-lookup"><span data-stu-id="d4c52-137">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="d4c52-138">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="d4c52-138">Use components</span></span>

<span data-ttu-id="d4c52-139">Bir HTML benzeri sözdizimi kullanarak başka bir bileşene dönüştürerek bileşen içerir.</span><span class="sxs-lookup"><span data-stu-id="d4c52-139">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="d4c52-140">Sayaç bileşen uygulamanın dizini (giriş) bileşenine ekleyerek bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="d4c52-140">Add the Counter component to the app's Index (Home) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="d4c52-141">Bu deneyim için bir anket istemi bileşeni Blazor kullanıyorsanız (`<SurveyPrompt>` öğesi) dizin bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="d4c52-141">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="d4c52-142">Değiştirin `<SurveyPrompt>` öğeyle `<Counter>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="d4c52-142">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="d4c52-143">*Components/Pages/Index.razor* (*Pages/Index.cshtml* Blazor içinde):</span><span class="sxs-lookup"><span data-stu-id="d4c52-143">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="d4c52-144">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4c52-144">Rebuild and run the app.</span></span> <span data-ttu-id="d4c52-145">Giriş sayfası, kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="d4c52-145">The Home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="d4c52-146">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="d4c52-146">Component parameters</span></span>

<span data-ttu-id="d4c52-147">Bileşenleri parametrelerini de sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d4c52-147">Components can also have parameters.</span></span> <span data-ttu-id="d4c52-148">Bileşen parametreleri, genel olmayan özellikler ile donatılmış bileşen sınıfı kullanarak tanımlanan `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="d4c52-148">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="d4c52-149">Öznitelikleri bir bileşen için bağımsız değişken biçimlendirme içinde belirtmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="d4c52-149">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="d4c52-150">Bileşenin güncelleştirme `@functions` C# kod:</span><span class="sxs-lookup"><span data-stu-id="d4c52-150">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="d4c52-151">Ekleme bir `IncrementAmount` özelliği düzenlenmiş ile `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="d4c52-151">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="d4c52-152">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="d4c52-152">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="d4c52-153">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* Blazor içinde):</span><span class="sxs-lookup"><span data-stu-id="d4c52-153">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="d4c52-154">Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="d4c52-154">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="d4c52-155">Sayaç tarafından on Artır bir değere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d4c52-155">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="d4c52-156">*Components/Pages/Index.razor* (*Pages/Index.cshtml* Blazor içinde):</span><span class="sxs-lookup"><span data-stu-id="d4c52-156">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="d4c52-157">Ana sayfayı yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="d4c52-157">Reload the Home page.</span></span> <span data-ttu-id="d4c52-158">Sayaç artırılır on tarafından her zaman **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="d4c52-158">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="d4c52-159">Bir sayacı sayfa artışlarla üzerinde sayacı.</span><span class="sxs-lookup"><span data-stu-id="d4c52-159">The counter on the Counter page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="d4c52-160">Bileşenleri için yol</span><span class="sxs-lookup"><span data-stu-id="d4c52-160">Route to components</span></span>

<span data-ttu-id="d4c52-161">`@page` En üstündeki yönerge *Counter.razor* dosya bu bileşen yönlendirme bir uç nokta olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="d4c52-161">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="d4c52-162">Sayaç bileşeni için gönderilen istekleri işler `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="d4c52-162">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="d4c52-163">Olmadan `@page` yönergesi, bileşen yönlendirilen istekler da işlemiyor ancak bileşeni hala başka bileşenler tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d4c52-163">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="d4c52-164">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="d4c52-164">Dependency injection</span></span>

<span data-ttu-id="d4c52-165">Bileşenleri uygulamanın service kapsayıcısında kayıtlı hizmetlerinin kullanılabilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d4c52-165">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d4c52-166">Halinde bileşenini kullanarak Hizmetleri ekleme `@inject` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="d4c52-166">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="d4c52-167">Örnek uygulamada parçalar bileşeninin yönergeleri inceleyin.</span><span class="sxs-lookup"><span data-stu-id="d4c52-167">Examine the directives of the FetchData component in the sample app.</span></span>

<span data-ttu-id="d4c52-168">Razor bileşenleri örnek uygulamada `WeatherForecastService` hizmet olarak kayıtlı bir [tekil](xref:fundamentals/dependency-injection#service-lifetimes), Hizmeti'nin bir örneğini uygulama boyunca kullanılabilir olması.</span><span class="sxs-lookup"><span data-stu-id="d4c52-168">In the Razor Components sample app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="d4c52-169">`@inject` Yönergesi örneğini ekleme için kullanılan `WeatherForecastService` halinde bileşenini hizmet.</span><span class="sxs-lookup"><span data-stu-id="d4c52-169">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="d4c52-170">*Components/Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="d4c52-170">*Components/Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="d4c52-171">Parçalar bileşeni olarak eklenen hizmet kullanan `ForecastService`, bir dizi alınacak `WeatherForecast` nesneler:</span><span class="sxs-lookup"><span data-stu-id="d4c52-171">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="d4c52-172">Örnek uygulama Blazor sürümünde `HttpClient` hava durumu tahminini verilerden elde etmek için eklenmiş *weather.json* dosyası *wwwroot/örnek-veri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="d4c52-172">In the Blazor version of the sample app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="d4c52-173">*Pages/FetchData.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d4c52-173">*Pages/FetchData.cshtml*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

<span data-ttu-id="d4c52-174">Her iki örnek uygulamalarda bir [ @foreach ](/dotnet/csharp/language-reference/keywords/foreach-in) döngü, her bir tahmin örneği tablosunda bir satıra hava durumu verilerinin işlemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="d4c52-174">In both sample apps, a [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="d4c52-175">Bir Yapılacaklar listesi oluşturun</span><span class="sxs-lookup"><span data-stu-id="d4c52-175">Build a todo list</span></span>

<span data-ttu-id="d4c52-176">Yeni bir bileşen, bir basit bir Yapılacaklar listesi uygulayan uygulamaya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d4c52-176">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="d4c52-177">Örnek uygulamaya boş bir dosya ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d4c52-177">Add an empty file to the sample app:</span></span>

   * <span data-ttu-id="d4c52-178">Razor bileşenleri deneyimi için ekleme bir *Todo.razor* dosyasını *bileşenleri/sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="d4c52-178">For the Razor Components experience, add a *Todo.razor* file to the *Components/Pages* folder.</span></span>
   * <span data-ttu-id="d4c52-179">Blazor deneyimi için ekleme bir *Todo.cshtml* dosyasını *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="d4c52-179">For the Blazor experience, add a *Todo.cshtml* file to the *Pages* folder.</span></span>

1. <span data-ttu-id="d4c52-180">Bileşen için ilk biçimlendirme sağlar:</span><span class="sxs-lookup"><span data-stu-id="d4c52-180">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="d4c52-181">Todo bileşeni Gezinti çubuğuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d4c52-181">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="d4c52-182">NavMenu bileşeni (*Components/Shared/NavMenu.razor* veya *Shared/NavMenu.cshtml* Blazor içinde) uygulamanın düzende kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d4c52-182">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="d4c52-183">Düzenleri, uygulama içeriği yinelenmesini önlemek izin bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="d4c52-183">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="d4c52-184">Daha fazla bilgi için bkz. <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="d4c52-184">For more information, see <xref:blazor/layouts>.</span></span>

   <span data-ttu-id="d4c52-185">Ekleme bir `<NavLink>` aşağıda bulunan mevcut liste öğelerini aşağıdaki liste öğesi işaretleme ekleyerek Todo bileşeni için *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* Blazor içinde) dosyası:</span><span class="sxs-lookup"><span data-stu-id="d4c52-185">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="d4c52-186">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4c52-186">Rebuild and run the app.</span></span> <span data-ttu-id="d4c52-187">Todo bileşen bağlantısını çalıştığını onaylamak için yeni Todo sayfasını ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="d4c52-187">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="d4c52-188">Ekleme bir *TodoItem.cs* bir todo öğesini temsil eden bir sınıf tutmak için projenin kök dosya.</span><span class="sxs-lookup"><span data-stu-id="d4c52-188">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="d4c52-189">Aşağıdaki C# için kod `TodoItem` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="d4c52-189">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="d4c52-190">Todo bileşenine döndürür (*Components/Pages/Todo.razor* veya *Pages/Todo.cshtml* Blazor içinde):</span><span class="sxs-lookup"><span data-stu-id="d4c52-190">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="d4c52-191">Açıklamada içinde için bir alan eklemek bir `@functions` blok.</span><span class="sxs-lookup"><span data-stu-id="d4c52-191">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="d4c52-192">Todo bileşeni, yapılacaklar listesi durumunu korumak için bu alanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="d4c52-192">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="d4c52-193">Sırasız liste biçimlendirme eklemek ve `foreach` her bir todo öğesi bir liste öğesi olarak işlenecek döngü.</span><span class="sxs-lookup"><span data-stu-id="d4c52-193">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="d4c52-194">Uygulama, açıklamada listeye eklemek için kullanıcı Arabirimi öğeleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d4c52-194">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="d4c52-195">Bir metin girişi ve listenin altındaki bir düğme ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d4c52-195">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="d4c52-196">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4c52-196">Rebuild and run the app.</span></span> <span data-ttu-id="d4c52-197">Zaman **todo ekleyin** düğmesi seçilirse, hiçbir şey olmaz kadar düğmesi olay işleyicisi kablolu değildir çünkü.</span><span class="sxs-lookup"><span data-stu-id="d4c52-197">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="d4c52-198">Ekleme bir `AddTodo` Todo bileşen ve düğme için tıkladığında kullanarak kayıt yöntemi `onclick` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="d4c52-198">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="d4c52-199">`AddTodo` C# Yöntemi düğme seçildiğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d4c52-199">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="d4c52-200">Yeni todo öğesinin başlığını almak için ekleyin bir `newTodo` dize alanı ve değeri kullanarak metin olarak bağlama `bind` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="d4c52-200">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. <span data-ttu-id="d4c52-201">Güncelleştirme `AddTodo` ekleme yöntemi `TodoItem` listesine belirtilen başlığa sahip.</span><span class="sxs-lookup"><span data-stu-id="d4c52-201">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="d4c52-202">Metin girişi değerini ayarlayarak Temizle `newTodo` boş bir dize:</span><span class="sxs-lookup"><span data-stu-id="d4c52-202">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="d4c52-203">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4c52-203">Rebuild and run the app.</span></span> <span data-ttu-id="d4c52-204">Bazı açıklamada yeni kodu test etmek için yapılacaklar listesine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d4c52-204">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="d4c52-205">Her bir todo öğesi için başlık metnini düzenlenebilir hale getirilebilir ve onay kutusu tamamlanan öğeleri takip kullanıcı yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="d4c52-205">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="d4c52-206">Her bir todo öğesi için bir onay kutusu giriş ekleyin ve değerini bağlama `IsDone` özelliği.</span><span class="sxs-lookup"><span data-stu-id="d4c52-206">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="d4c52-207">Değişiklik `@todo.Title` için bir `<input>` bağlı öğe `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="d4c52-207">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="d4c52-208">Bu değerleri bağlı olduğunu doğrulamak için güncelleştirme `<h1>` tamamlanmamış todo öğelerini sayısını göstermek için başlık (`IsDone` olan `false`).</span><span class="sxs-lookup"><span data-stu-id="d4c52-208">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="d4c52-209">Tamamlanmış Todo bileşeni (*Components/Pages/Todo.razor* veya *Pages/Todo.cshtml* Blazor içinde):</span><span class="sxs-lookup"><span data-stu-id="d4c52-209">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="d4c52-210">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d4c52-210">Rebuild and run the app.</span></span> <span data-ttu-id="d4c52-211">Yeni kod test etmek için todo öğeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d4c52-211">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="d4c52-212">Yayımlama ve uygulama dağıtma</span><span class="sxs-lookup"><span data-stu-id="d4c52-212">Publish and deploy the app</span></span>

<span data-ttu-id="d4c52-213">Uygulamayı yayımlamak için bkz: <xref:host-and-deploy/blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="d4c52-213">To publish the app, see <xref:host-and-deploy/blazor/index>.</span></span>
