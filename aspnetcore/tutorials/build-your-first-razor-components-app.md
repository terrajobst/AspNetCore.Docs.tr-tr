---
title: Razor bileşenleri ilk uygulamanızı oluşturun
author: guardrex
description: Adım adım bir Razor bileşenleri uygulaması derleme ve temel Razor bileşenleri framework kavramlarını öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 4bf3884d5d9575ebf2a09237e364b37fa1b35246
ms.sourcegitcommit: 3c2ba9a0d833d2a096d9d800ba67a1a7f9491af0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55854611"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="e2a71-103">Razor bileşenleri ilk uygulamanızı oluşturun</span><span class="sxs-lookup"><span data-stu-id="e2a71-103">Build your first Razor Components app</span></span>

<span data-ttu-id="e2a71-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e2a71-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="e2a71-105">Bu öğretici, bir Razor bileşenleri uygulamasının nasıl oluşturulacağını gösterir ve temel Razor bileşenleri framework kavramlarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="e2a71-105">This tutorial shows you how to build a Razor Components app and demonstrates basic Razor Components framework concepts.</span></span>

<span data-ttu-id="e2a71-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e2a71-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="e2a71-107">Bkz: [başlama](xref:razor-components/get-started) Önkoşullar için konu.</span><span class="sxs-lookup"><span data-stu-id="e2a71-107">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

## <a name="create-an-app-from-the-razor-components-template"></a><span data-ttu-id="e2a71-108">Razor bileşenleri şablondan uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="e2a71-108">Create an app from the Razor Components template</span></span>

<span data-ttu-id="e2a71-109">Sunulan yönergeleri [başlama](xref:razor-components/get-started) Razor bileşenleri şablondan bir Razor bileşenleri projesi oluşturmak için konu.</span><span class="sxs-lookup"><span data-stu-id="e2a71-109">Follow the guidance in the [Get started](xref:razor-components/get-started) topic to create a Razor Components project from the Razor Components template.</span></span> <span data-ttu-id="e2a71-110">Çözüm adı *WebApplication1*.</span><span class="sxs-lookup"><span data-stu-id="e2a71-110">Name the solution *WebApplication1*.</span></span> <span data-ttu-id="e2a71-111">.NET Core CLI komutları ile Visual Studio ya da komut kabuğunu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e2a71-111">You can use Visual Studio or a command shell with .NET Core CLI commands.</span></span>

## <a name="build-components"></a><span data-ttu-id="e2a71-112">Yapı bileşenleri</span><span class="sxs-lookup"><span data-stu-id="e2a71-112">Build components</span></span>

1. <span data-ttu-id="e2a71-113">Her bir uygulamanın üç göz atın: Giriş, sayaç ve veri getirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="e2a71-113">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="e2a71-114">Bu sayfalar, Razor dosyalarında tarafından uygulanan *WebApplication1.App/Pages* klasörü: *Index.cshtml*, *Counter.cshtml*, ve *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e2a71-114">These pages are implemented by Razor files in the *WebApplication1.App/Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span>

1. <span data-ttu-id="e2a71-115">Sayaç sayfasında **me tıklayın** sayfa yenileme olmadan sayaç artmaya düğmesi.</span><span class="sxs-lookup"><span data-stu-id="e2a71-115">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="e2a71-116">Normal olarak artan bir Web sayfasındaki bir sayaç JavaScript Yazma gerektiriyor, ancak Razor bileşenleri sağlayan daha iyi bir yaklaşım kullanarak C#.</span><span class="sxs-lookup"><span data-stu-id="e2a71-116">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="e2a71-117">Uygulama sayacı bileşenin inceleyin *Counter.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="e2a71-117">Examine the implementation of the Counter component in the *Counter.cshtml* file.</span></span>

   <span data-ttu-id="e2a71-118">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e2a71-118">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.cshtml)]

   <span data-ttu-id="e2a71-119">Kullanıcı arabirimini sayacı bileşenin HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="e2a71-119">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="e2a71-120">(Örneğin, döngü, koşullular, ifadeleri) dinamik işleme mantığı, katıştırılmış kullanarak eklenir C# adlı söz dizimi [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="e2a71-120">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="e2a71-121">HTML biçimlendirmesi ve C# işleme mantığı, oluşturma zamanında bir bileşen sınıfı dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="e2a71-121">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="e2a71-122">Oluşturulan .NET sınıf adı dosya adıyla eşleşen.</span><span class="sxs-lookup"><span data-stu-id="e2a71-122">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="e2a71-123">Bileşen sınıfı üyeleri tanımlanmış bir `@functions` blok.</span><span class="sxs-lookup"><span data-stu-id="e2a71-123">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="e2a71-124">İçinde `@functions` blok, bileşen durumu (Özellikler, alanlar) ve olay işleme için veya başka bir bileşen mantığı tanımlamak için yöntemleri belirtilir.</span><span class="sxs-lookup"><span data-stu-id="e2a71-124">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="e2a71-125">Bu üyeleri, ardından bileşenin işleme mantığı ve olayları işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e2a71-125">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="e2a71-126">Zaman **me tıklayın** düğmesi seçili:</span><span class="sxs-lookup"><span data-stu-id="e2a71-126">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="e2a71-127">Sayaç bileşen kayıtlı `onclick` işleyici çağrılır ( `IncrementCount` yöntemi).</span><span class="sxs-lookup"><span data-stu-id="e2a71-127">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="e2a71-128">Sayaç bileşen kendi işleme ağacında yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e2a71-128">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="e2a71-129">Yeni bir işleme ağacı Öncekine karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="e2a71-129">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="e2a71-130">Yalnızca belge nesne modeli (DOM) değişiklikler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e2a71-130">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="e2a71-131">Görüntülenen sayısı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e2a71-131">The displayed count is updated.</span></span>

1. <span data-ttu-id="e2a71-132">Değiştirme C# bir yerine ikiye sayısı artması yapmak için sayaç bileşeninin mantığı.</span><span class="sxs-lookup"><span data-stu-id="e2a71-132">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.cshtml?highlight=14)]

1. <span data-ttu-id="e2a71-133">Yeniden oluşturun ve değişiklikleri görmek için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e2a71-133">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="e2a71-134">Seçin **me tıklayın** düğmeyi ve iki sayacını artırır.</span><span class="sxs-lookup"><span data-stu-id="e2a71-134">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="e2a71-135">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="e2a71-135">Use components</span></span>

<span data-ttu-id="e2a71-136">Bir HTML benzeri sözdizimi kullanarak başka bir bileşene dönüştürerek bileşen içerir.</span><span class="sxs-lookup"><span data-stu-id="e2a71-136">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="e2a71-137">Sayaç bileşen uygulamanın dizini (giriş sayfası) bileşenine ekleyerek bir `<Counter />` dizin bileşeni öğesi.</span><span class="sxs-lookup"><span data-stu-id="e2a71-137">Add the Counter component to the app's Index (home page) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="e2a71-138">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e2a71-138">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.cshtml?highlight=7)]

1. <span data-ttu-id="e2a71-139">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e2a71-139">Rebuild and run the app.</span></span> <span data-ttu-id="e2a71-140">Giriş sayfası, kendi sayaç vardır.</span><span class="sxs-lookup"><span data-stu-id="e2a71-140">The home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="e2a71-141">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="e2a71-141">Component parameters</span></span>

<span data-ttu-id="e2a71-142">Bileşenleri parametrelerini de sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e2a71-142">Components can also have parameters.</span></span> <span data-ttu-id="e2a71-143">Bileşen parametreleri, genel olmayan özellikler ile donatılmış bileşen sınıfı kullanarak tanımlanan `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="e2a71-143">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="e2a71-144">Öznitelikleri bir bileşen için bağımsız değişken biçimlendirme içinde belirtmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="e2a71-144">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="e2a71-145">Bileşenin güncelleştirme `@functions` C# kod:</span><span class="sxs-lookup"><span data-stu-id="e2a71-145">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="e2a71-146">Ekleme bir `IncrementAmount` özelliği düzenlenmiş ile `[Parameter]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e2a71-146">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="e2a71-147">Değişiklik `IncrementCount` yönteminin kullanılacağını `IncrementAmount` değerini artırmayı olduğunda `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="e2a71-147">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="e2a71-148">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e2a71-148">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/WebApplication1/WebApplication1.App/Pages/Counter.cshtml?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="e2a71-149">Belirtin bir `IncrementAmount` ana bileşenin parametresinde `<Counter>` öğesini kullanarak bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="e2a71-149">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="e2a71-150">Sayaç tarafından on Artır bir değere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="e2a71-150">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="e2a71-151">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e2a71-151">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/WebApplication1/WebApplication1.App/Pages/Index.cshtml?highlight=7)]

1. <span data-ttu-id="e2a71-152">Sayfayı yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="e2a71-152">Reload the page.</span></span> <span data-ttu-id="e2a71-153">Giriş sayfası sayaç artırılır on tarafından her zaman **me tıklayın** düğmesi seçili.</span><span class="sxs-lookup"><span data-stu-id="e2a71-153">The home page counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="e2a71-154">Sayaca sağ *sayacı* sayfasında artışlarla bir.</span><span class="sxs-lookup"><span data-stu-id="e2a71-154">The counter on the *Counter* page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="e2a71-155">Bileşenleri için yol</span><span class="sxs-lookup"><span data-stu-id="e2a71-155">Route to components</span></span>

<span data-ttu-id="e2a71-156">`@page` En üstündeki yönerge *Counter.cshtml* dosya bu bileşen yönlendirme bir uç nokta olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="e2a71-156">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="e2a71-157">Sayaç bileşeni için gönderilen istekleri işler `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="e2a71-157">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="e2a71-158">Olmadan `@page` yönergesi, bileşen yönlendirilen istekler da işlemiyor ancak bileşeni hala başka bileşenler tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e2a71-158">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="e2a71-159">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="e2a71-159">Dependency injection</span></span>

<span data-ttu-id="e2a71-160">Bileşenleri uygulamanın service kapsayıcısında kayıtlı hizmetlerinin kullanılabilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e2a71-160">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e2a71-161">Halinde bileşenini kullanarak Hizmetleri ekleme `@inject` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="e2a71-161">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="e2a71-162">Parçalar bileşenin yönergeleri inceleyin (*WebApplication1.App/Pages/FetchData.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="e2a71-162">Examine the directives of the FetchData component (*WebApplication1.App/Pages/FetchData.cshtml*).</span></span> <span data-ttu-id="e2a71-163">`@inject` Yönergesi örneğini ekleme için kullanılan `WeatherForecastService` hizmet bileşeni içinde:</span><span class="sxs-lookup"><span data-stu-id="e2a71-163">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=3)]

<span data-ttu-id="e2a71-164">`WeatherForecastService` Hizmet olarak kayıtlı bir [singleton](xref:fundamentals/dependency-injection#service-lifetimes), Hizmeti'nin bir örneğini uygulama boyunca kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e2a71-164">The `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span>

<span data-ttu-id="e2a71-165">Parçalar bileşeni olarak eklenen hizmet kullanan `ForecastService`, bir dizi alınacak `WeatherForecast` nesneler:</span><span class="sxs-lookup"><span data-stu-id="e2a71-165">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.cshtml?highlight=6)]

<span data-ttu-id="e2a71-166">A [ @foreach ](/dotnet/csharp/language-reference/keywords/foreach-in) döngü, her bir tahmin örneği tablosunda bir satıra hava durumu verilerinin işlemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="e2a71-166">A [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.cshtml?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="e2a71-167">Bir Yapılacaklar listesi oluşturun</span><span class="sxs-lookup"><span data-stu-id="e2a71-167">Build a todo list</span></span>

<span data-ttu-id="e2a71-168">Bir basit bir Yapılacaklar listesi uygulayan uygulamaya yeni bir sayfa ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e2a71-168">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="e2a71-169">Boş bir dosyaya eklemek *WebApplication1.App/Pages* adlı klasöre *Todo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e2a71-169">Add an empty file to the *WebApplication1.App/Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="e2a71-170">Sayfa için ilk biçimlendirme sağlar:</span><span class="sxs-lookup"><span data-stu-id="e2a71-170">Provide the initial markup for the page:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="e2a71-171">Todo sayfa gezinti çubuğuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e2a71-171">Add the Todo page to the navigation bar.</span></span>

   <span data-ttu-id="e2a71-172">NavMenu bileşeni (*WebApplication1/Shared/NavMenu.csthml*) uygulamanın düzende kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e2a71-172">The NavMenu component (*WebApplication1/Shared/NavMenu.csthml*) is used in the app's layout.</span></span> <span data-ttu-id="e2a71-173">Düzenleri, uygulama içeriği yinelenmesini önlemek izin bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="e2a71-173">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="e2a71-174">Daha fazla bilgi için bkz. <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="e2a71-174">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="e2a71-175">Ekleme bir `<NavLink>` için aşağıda bulunan mevcut liste öğelerini aşağıdaki liste öğesi işaretleme ekleyerek Todo sayfası *WebApplication1/Shared/NavMenu.csthml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="e2a71-175">Add a `<NavLink>` for the Todo page by adding the following list item markup below the existing list items in the *WebApplication1/Shared/NavMenu.csthml* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="e2a71-176">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e2a71-176">Rebuild and run the app.</span></span> <span data-ttu-id="e2a71-177">Todo sayfanın bağlantısını çalıştığını onaylamak için yeni Todo sayfasını ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="e2a71-177">Visit the new Todo page to confirm that the link to the Todo page works.</span></span>

1. <span data-ttu-id="e2a71-178">Ekleme bir *TodoItem.cs* bir todo öğesini temsil eden bir sınıf tutmak için projenin kök dosya.</span><span class="sxs-lookup"><span data-stu-id="e2a71-178">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="e2a71-179">Aşağıdaki C# için kod `TodoItem` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="e2a71-179">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/WebApplication1/WebApplication1.App/TodoItem.cs)]

1. <span data-ttu-id="e2a71-180">Todo bileşenine döndürür (*Todo.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="e2a71-180">Return to the Todo component (*Todo.cshtml*):</span></span>

   * <span data-ttu-id="e2a71-181">Açıklamada içinde için bir alan eklemek bir `@functions` blok.</span><span class="sxs-lookup"><span data-stu-id="e2a71-181">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="e2a71-182">Todo bileşeni, yapılacaklar listesi durumunu korumak için bu alanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="e2a71-182">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="e2a71-183">Sırasız liste biçimlendirme eklemek ve `foreach` her bir todo öğesi bir liste öğesi olarak işlenecek döngü.</span><span class="sxs-lookup"><span data-stu-id="e2a71-183">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData4.cshtml?highlight=5-10,12-14)]

1. <span data-ttu-id="e2a71-184">Uygulama, açıklamada listeye eklemek için kullanıcı Arabirimi öğeleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e2a71-184">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="e2a71-185">Bir metin girişi ve listenin altındaki bir düğme ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e2a71-185">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData5.cshtml?highlight=12-13)]

1. <span data-ttu-id="e2a71-186">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e2a71-186">Rebuild and run the app.</span></span> <span data-ttu-id="e2a71-187">Zaman **todo ekleyin** düğmesi seçilirse, hiçbir şey olmaz kadar düğmesi olay işleyicisi kablolu değildir çünkü.</span><span class="sxs-lookup"><span data-stu-id="e2a71-187">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="e2a71-188">Ekleme bir `AddTodo` Todo bileşen ve düğme için tıkladığında kullanarak kayıt yöntemi `onclick` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="e2a71-188">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData6.cshtml?highlight=2,7-10)]

   <span data-ttu-id="e2a71-189">`AddTodo` C# Yöntemi düğme seçildiğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="e2a71-189">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="e2a71-190">Yeni todo öğesinin başlığını almak için ekleyin bir `newTodo` dize alanı ve değeri kullanarak metin olarak bağlama `bind` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="e2a71-190">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData7.cshtml?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="e2a71-191">Güncelleştirme `AddTodo` ekleme yöntemi `TodoItem` listesine belirtilen başlığa sahip.</span><span class="sxs-lookup"><span data-stu-id="e2a71-191">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="e2a71-192">Metin girişi değerini ayarlayarak Temizle `newTodo` boş bir dize:</span><span class="sxs-lookup"><span data-stu-id="e2a71-192">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData8.cshtml?highlight=19-26)]

1. <span data-ttu-id="e2a71-193">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e2a71-193">Rebuild and run the app.</span></span> <span data-ttu-id="e2a71-194">Bazı açıklamada yeni kodu test etmek için yapılacaklar listesine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e2a71-194">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="e2a71-195">Her bir todo öğesi için başlık metnini düzenlenebilir hale getirilebilir ve onay kutusu tamamlanan öğeleri takip kullanıcı yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="e2a71-195">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="e2a71-196">Her bir todo öğesi için bir onay kutusu giriş ekleyin ve değerini bağlama `IsDone` özelliği.</span><span class="sxs-lookup"><span data-stu-id="e2a71-196">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="e2a71-197">Değişiklik `@todo.Title` için bir `<input>` bağlı öğe `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="e2a71-197">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData9.cshtml?highlight=5-6)]

1. <span data-ttu-id="e2a71-198">Bu değerleri bağlı olduğunu doğrulamak için güncelleştirme `<h1>` tamamlanmamış todo öğelerini sayısını göstermek için başlık (`IsDone` olan `false`).</span><span class="sxs-lookup"><span data-stu-id="e2a71-198">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="e2a71-199">Tamamlanmış Todo bileşeni (*Todo.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="e2a71-199">The completed Todo component (*Todo.cshtml*):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/WebApplication1/WebApplication1.App/Pages/Todo.cshtml)]

1. <span data-ttu-id="e2a71-200">Yeniden oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e2a71-200">Rebuild and run the app.</span></span> <span data-ttu-id="e2a71-201">Yeni kod test etmek için todo öğeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e2a71-201">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="e2a71-202">Yayımlama ve uygulama dağıtma</span><span class="sxs-lookup"><span data-stu-id="e2a71-202">Publish and deploy the app</span></span>

<span data-ttu-id="e2a71-203">Uygulamayı yayımlamak için bkz: <xref:host-and-deploy/razor-components/index#publish-the-app>.</span><span class="sxs-lookup"><span data-stu-id="e2a71-203">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
