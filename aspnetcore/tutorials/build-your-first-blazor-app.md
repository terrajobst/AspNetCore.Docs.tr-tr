---
title: İlk Blazor uygulamanızı oluşturun
author: guardrex
description: Adım adım bir Blazor uygulaması derleme ve hızlı Razor bileşenleri framework'ün temel özellikleri öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 99396e200eb121524abccc20a7d461062c94ecc7
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668161"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="be451-103">İlk Blazor uygulamanızı oluşturun</span><span class="sxs-lookup"><span data-stu-id="be451-103">Build your first Blazor app</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="be451-104">Bu öğreticide, bir adım adım Blazor uygulaması derleme ve hızlı Razor bileşenleri framework'ün temel özellikleri öğrenin.</span><span class="sxs-lookup"><span data-stu-id="be451-104">In this tutorial, you build a Blazor app step-by-step and quickly learn the basic features of the Razor Components framework.</span></span>

<span data-ttu-id="be451-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="be451-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="be451-106">Bkz: [başlama](xref:razor-components/get-started) Önkoşullar için konu.</span><span class="sxs-lookup"><span data-stu-id="be451-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="be451-107">Visual Studio projesi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="be451-107">To create the project in Visual Studio:</span></span>

1. <span data-ttu-id="be451-108">**Dosya** > **Yeni** > **Proje**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="be451-108">Select **File** > **New** > **Project**.</span></span> <span data-ttu-id="be451-109">Seçin **Web** > **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="be451-109">Select **Web** > **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="be451-110">İçinde "BlazorApp1" proje adı **adı** alan.</span><span class="sxs-lookup"><span data-stu-id="be451-110">Name the project "BlazorApp1" in the **Name** field.</span></span> <span data-ttu-id="be451-111">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="be451-111">Select **OK**.</span></span>

    ![Yeni ASP.NET Core projesi](build-your-first-blazor-app/_static/new-aspnet-core-project.png)

1. <span data-ttu-id="be451-113">**Yeni ASP.NET Core Web uygulaması** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="be451-113">The **New ASP.NET Core Web Application** dialog appears.</span></span> <span data-ttu-id="be451-114">Emin **.NET Core** üstünde seçilir.</span><span class="sxs-lookup"><span data-stu-id="be451-114">Make sure **.NET Core** is selected at the top.</span></span> <span data-ttu-id="be451-115">Seçin **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="be451-115">Select **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="be451-116">Seçin **Blazor** şablonu seçip alt **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="be451-116">Choose the **Blazor** template and select **OK**.</span></span>

    ![Yeni Blazor uygulaması iletişim kutusu](build-your-first-blazor-app/_static/new-blazor-app-dialog.png)

1. <span data-ttu-id="be451-118">Proje oluşturulduktan sonra basın **Ctrl-F5** uygulamayı çalıştırmak için *hata ayıklayıcı olmadan*.</span><span class="sxs-lookup"><span data-stu-id="be451-118">Once the project is created, press **Ctrl-F5** to run the app *without the debugger*.</span></span> <span data-ttu-id="be451-119">Hata ayıklayıcısı ile çalıştırma (**F5**) şu anda desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="be451-119">Running with the debugger (**F5**) isn't supported at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="be451-120">Aksi durumda Visual Studio kullanarak oluşturduğunuz Blazor uygulamayı Windows, macOS veya Linux üzerinde bir komut isteminde:</span><span class="sxs-lookup"><span data-stu-id="be451-120">If not using Visual Studio, create the Blazor app at a command prompt on Windows, macOS, or Linux:</span></span>
>
> ```console
> dotnet new --install "Microsoft.AspNetCore.Blazor.Templates"
> dotnet new blazor -o BlazorApp1
> cd BlazorApp1
> dotnet run
> ```
>
> <span data-ttu-id="be451-121">Localhost adresi ve bağlantı noktası konsol penceresi çıktısı sonra sağlanan kullanarak uygulamaya gidin `dotnet run` yürütülür.</span><span class="sxs-lookup"><span data-stu-id="be451-121">Navigate to the app using the localhost address and port provided in the console window output after `dotnet run` is executed.</span></span> <span data-ttu-id="be451-122">Kullanım **Ctrl-C** konsol penceresinde uygulamayı kapatmak için.</span><span class="sxs-lookup"><span data-stu-id="be451-122">Use **Ctrl-C** in the console window to shutdown the app.</span></span>

<span data-ttu-id="be451-123">Blazor uygulama tarayıcıda çalışır:</span><span class="sxs-lookup"><span data-stu-id="be451-123">The Blazor app runs in the browser:</span></span>

![Blazor uygulama ana sayfası](https://user-images.githubusercontent.com/1874516/39509497-5515c3ea-4d9b-11e8-887f-019ea4fdb3ee.png)

## <a name="build-components"></a><span data-ttu-id="be451-125">Yapı bileşenleri</span><span class="sxs-lookup"><span data-stu-id="be451-125">Build components</span></span>

1. <span data-ttu-id="be451-126">Her bir uygulamanın üç göz atın: Giriş, sayaç ve veri getirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="be451-126">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span>

    <span data-ttu-id="be451-127">Bu üç sayfaları üç Razor dosyalarında tarafından uygulanan *sayfaları* klasörü: *Index.cshtml*, *Counter.cshtml*, ve *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="be451-127">These three pages are implemented by the three Razor files in the *Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span> <span data-ttu-id="be451-128">Bu dosyaların her biri, derlenmiş ve istemci tarafı tarayıcıda çalıştırılan bir bileşen uygular.</span><span class="sxs-lookup"><span data-stu-id="be451-128">Each of these files implements a component that's compiled and executed client-side in the browser.</span></span>

1. <span data-ttu-id="be451-129">Sayaç sayfasında düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="be451-129">Select the button on the Counter page.</span></span>

    ![Blazor uygulama ana sayfası](https://user-images.githubusercontent.com/1874516/39509525-6e367c66-4d9b-11e8-9978-e52a9750c34b.png)

    <span data-ttu-id="be451-131">Düğme seçildiğinde her zaman bir sayfa yenileme sayaç artırılır.</span><span class="sxs-lookup"><span data-stu-id="be451-131">Each time the button is selected, the counter is incremented without a page refresh.</span></span> <span data-ttu-id="be451-132">Normalde, bu tür bir istemci-tarafı davranışı JavaScript'te işlenir; ancak burada öğesinde uygulanır C# ve .NET tarafından `Counter` bileşeni.</span><span class="sxs-lookup"><span data-stu-id="be451-132">Normally, this kind of client-side behavior is handled in JavaScript; but here, it's implemented in C# and .NET by the `Counter` component.</span></span>

1. <span data-ttu-id="be451-133">Uygulamasını göz atın `Counter` bileşeninin *Counter.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="be451-133">Take a look at the implementation of the `Counter` component in the *Counter.cshtml* file:</span></span>

    ```cshtml
    @page "/counter"

    <h1>Counter</h1>

    <p>Current count: @currentCount</p>

    <button class="btn btn-primary" onclick="@IncrementCount">Click me</button>

    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
    ```

    <span data-ttu-id="be451-134">Kullanıcı Arabiriminde `Counter` bileşen normal HTML kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="be451-134">The UI for the `Counter` component is defined using normal HTML.</span></span> <span data-ttu-id="be451-135">(Örneğin, döngü, koşullular, ifadeleri) dinamik işleme mantığı, katıştırılmış kullanarak eklenir C# adlı söz dizimi [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="be451-135">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span></span> <span data-ttu-id="be451-136">HTML biçimlendirmesi ve C# işleme mantığı, oluşturma zamanında bir bileşen sınıfı dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="be451-136">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="be451-137">Oluşturulan .NET sınıf adı dosya adıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="be451-137">The name of the generated .NET class matches the name of the file.</span></span>

    <span data-ttu-id="be451-138">Bileşen sınıfı üyeleri tanımlanmış bir `@functions` blok.</span><span class="sxs-lookup"><span data-stu-id="be451-138">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="be451-139">İçinde `@functions` engelleme, bileşen durumu (Özellikler, alanlar) ve olay işleme için veya başka bir bileşen mantığı tanımlamak için yöntemleri belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="be451-139">In the `@functions` block, component state (properties, fields) and methods can be specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="be451-140">Bu üyeleri, ardından bileşenin işleme mantığı ve olayları işlemek için bir parçası olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="be451-140">These members can then be used as part of the component's rendering logic and for handling events.</span></span>

    <span data-ttu-id="be451-141">Düğme seçildiğinde `Counter` bileşeni kayıtlı `onclick` işleyici çağrılır ( `IncrementCount` yöntemi) ve `Counter` bileşen kendi işleme ağacında yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="be451-141">When the button is selected, the `Counter` component's registered `onclick` handler is called (the `IncrementCount` method) and the `Counter` component regenerates its render tree.</span></span> <span data-ttu-id="be451-142">Blazor, yeni bir işleme ağacı Öncekine karşı karşılaştırır ve tarayıcı belge nesne modeli (DOM) yapılan tüm değişiklikler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="be451-142">Blazor compares the new render tree against the previous one and applies any modifications to the browser Document Object Model (DOM).</span></span> <span data-ttu-id="be451-143">Görüntülenen sayısı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="be451-143">The displayed count is updated.</span></span>

1. <span data-ttu-id="be451-144">İşaretleme için güncelleştirme `Counter` daha üst düzey başlık olmak için bileşeni *heyecan verici*.</span><span class="sxs-lookup"><span data-stu-id="be451-144">Update the markup for the `Counter` component to make the top-level header more *exciting*.</span></span>

    ```cshtml
    @page "/counter"
    <h1><em>Counter!!</em></h1>
    ```

1. <span data-ttu-id="be451-145">Ayrıca değişiklik C# mantığını `Counter` bir yerine ikiye sayısı artması yapmak bileşeni.</span><span class="sxs-lookup"><span data-stu-id="be451-145">Also modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

    ```cshtml
    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount += 2;
        }
    }
    ```

1. <span data-ttu-id="be451-146">Sayaç sayfanın değişiklikleri görmek için tarayıcıyı yenileyin.</span><span class="sxs-lookup"><span data-stu-id="be451-146">Refresh the counter page in the browser to see the changes.</span></span>

    ![Heyecan verici sayacı](https://user-images.githubusercontent.com/1874516/39509668-e8949a92-4d9b-11e8-91e9-b6a494695d92.png)

## <a name="use-components"></a><span data-ttu-id="be451-148">Bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="be451-148">Use components</span></span>

<span data-ttu-id="be451-149">Bir bileşen tanımlandıktan sonra bileşenin diğer bileşenlere uygulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="be451-149">After a component is defined, the component can be used to implement other components.</span></span> <span data-ttu-id="be451-150">Bir bileşen kullanma için işaretleme, etiketin adını bileşen türü olduğu gibi HTML etiketleri arar.</span><span class="sxs-lookup"><span data-stu-id="be451-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

1. <span data-ttu-id="be451-151">Ekleme bir `Counter` ana sayfaya uygulaması bileşen (*Index.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="be451-151">Add a `Counter` component to the Home page of the app (*Index.cshtml*).</span></span>

    ```cshtml
    @page "/"

    <h1>Hello, world!</h1>

    Welcome to your new app.

    <SurveyPrompt Title="How is Blazor working for you?" />

    <Counter />
    ```

1. <span data-ttu-id="be451-152">Giriş sayfasını tarayıcıda yenileyin.</span><span class="sxs-lookup"><span data-stu-id="be451-152">Refresh the home page in the browser.</span></span> <span data-ttu-id="be451-153">Ayrı bir örneğini Not `Counter` giriş sayfasında bileşen.</span><span class="sxs-lookup"><span data-stu-id="be451-153">Note the separate instance of the `Counter` component on the Home page.</span></span>

    ![Sayaç ile Blazor giriş sayfası](https://user-images.githubusercontent.com/1874516/39509718-224483f6-4d9c-11e8-9030-b4c7228d669d.png)

## <a name="component-parameters"></a><span data-ttu-id="be451-155">Bileşen parametreleri</span><span class="sxs-lookup"><span data-stu-id="be451-155">Component parameters</span></span>

<span data-ttu-id="be451-156">Bileşenleri de özel özellikleri ile donatılmış bileşen sınıfı kullanarak tanımlanan parametreler sahip `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="be451-156">Components can also have parameters, which are defined using private properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="be451-157">Öznitelikleri bir bileşen için bağımsız değişken biçimlendirme içinde belirtmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="be451-157">Use attributes to specify arguments for a component in markup.</span></span> 

1. <span data-ttu-id="be451-158">Güncelleştirme `Counter` sahip bileşen bir `IncrementAmount` varsayılan olarak 1 parametresi.</span><span class="sxs-lookup"><span data-stu-id="be451-158">Update the `Counter` component to have an `IncrementAmount` parameter that defaults to 1.</span></span>

    ```cshtml
    @functions {
        int currentCount = 0;

        [Parameter]
        private int IncrementAmount { get; set; } = 1;

        void IncrementCount()
        {
            currentCount += IncrementAmount;
        }
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="be451-159">Visual Studio'dan kolayca bir bileşen parametresi kullanarak ekleyebilirsiniz `para` kod parçacığı.</span><span class="sxs-lookup"><span data-stu-id="be451-159">From Visual Studio, you can quickly add a component parameter by using the `para` snippet.</span></span> <span data-ttu-id="be451-160">Tür `para` ve tuşuna `Tab` tuşunu iki kez.</span><span class="sxs-lookup"><span data-stu-id="be451-160">Type `para` and then press the `Tab` key twice.</span></span>

1. <span data-ttu-id="be451-161">Giriş sayfasında (*Index.cshtml*), artış için değiştirebilirsiniz `Counter` bileşenin özelliği adıyla eşleşen bir öznitelik ayarlayarak 10 `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="be451-161">On the Home page (*Index.cshtml*), change the increment amount for the `Counter` to 10 by setting an attribute that matches the name of the component's property for `IncrementCount`.</span></span>

    ```cshtml
    <Counter IncrementAmount="10" />
    ```

1. <span data-ttu-id="be451-162">Sayfayı yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="be451-162">Reload the page.</span></span>

    <span data-ttu-id="be451-163">Sayacın sayaç sayfasında hala 1 artar ancak 10 giriş sayfası artık artışlarla üzerinde sayacı.</span><span class="sxs-lookup"><span data-stu-id="be451-163">The counter on the Home page now increments by 10, while the counter on the Counter page still increments by 1.</span></span>

    ![On göre Blazor sayısı](https://user-images.githubusercontent.com/1874516/39509798-618f0720-4d9c-11e8-9125-3d4c634dff46.png)

## <a name="route-to-components"></a><span data-ttu-id="be451-165">Bileşenleri için yol</span><span class="sxs-lookup"><span data-stu-id="be451-165">Route to components</span></span>

<span data-ttu-id="be451-166">`@page` En üstündeki yönerge *Counter.cshtml* dosya bu bileşen için istekleri yönlendirilir bir sayfa olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="be451-166">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a page to which requests can be routed.</span></span> <span data-ttu-id="be451-167">Özellikle, `Counter` bileşeni gönderilen istekleri işler `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="be451-167">Specifically, the `Counter` component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="be451-168">Olmadan `@page` yönergesi, bileşen yönlendirilmiş istekleri işleyen mıydı ancak bileşeni hala başka bileşenler tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="be451-168">Without the `@page` directive, the component wouldn't handle routed requests, but the component could still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="be451-169">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="be451-169">Dependency injection</span></span>

<span data-ttu-id="be451-170">Bileşenleri uygulamanın hizmet sağlayıcısına kayıtlı hizmetlerinin kullanılabilir [bağımlılık ekleme (dı)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="be451-170">Services registered with the app's service provider are available to components via [dependency injection (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span></span> <span data-ttu-id="be451-171">Hizmetler eklenen halinde bileşenini kullanarak `@inject` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="be451-171">Services can be injected into a component using the `@inject` directive.</span></span>

<span data-ttu-id="be451-172">Uygulamasını göz atın `FetchData` bileşeninin *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="be451-172">Take a look at the implementation of the `FetchData` component in *FetchData.cshtml*.</span></span> <span data-ttu-id="be451-173">`@inject` Yönergesi ekleme için kullanılan bir [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) örneğine bileşeni.</span><span class="sxs-lookup"><span data-stu-id="be451-173">The `@inject` directive is used to inject an [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) instance into the component.</span></span>

```cshtml
@page "/fetchdata"
@inject HttpClient Http
```

<span data-ttu-id="be451-174">`FetchData` Bileşen eklenen `HttpClient` bileşeni başlatılırken JSON verileri sunucudan almak için.</span><span class="sxs-lookup"><span data-stu-id="be451-174">The `FetchData` component uses the injected `HttpClient` to retrieve JSON data from the server when the component is initialized.</span></span> <span data-ttu-id="be451-175">Kapak altında `HttpClient` isteği göndermek için temel alınan tarayıcının Fetch API'sini çağırmak için JavaScript birlikte çalışması kullanarak çalışma zamanı uygulanan Blazor tarafından sağlanan (gelen C#, herhangi bir JavaScript kitaplığı veya tarayıcı API'yi çağırmak mümkündür).</span><span class="sxs-lookup"><span data-stu-id="be451-175">Under the covers, the `HttpClient` provided by the Blazor runtime is implemented using JavaScript interop to call the underlying browser's Fetch API to send the request (from C#, it's possible to call any JavaScript library or browser API).</span></span> <span data-ttu-id="be451-176">Alınan verileri seri durumdan `forecasts` C# bir dizi değişkenini `WeatherForecast` nesneleri.</span><span class="sxs-lookup"><span data-stu-id="be451-176">The retrieved data is deserialized into the `forecasts` C# variable as an array of `WeatherForecast` objects.</span></span>

```cshtml
@functions {
    WeatherForecast[] forecasts;

    protected override async Task OnInitAsync()
    {
        forecasts = await Http.GetJsonAsync<WeatherForecast[]>("/sample-data/weather.json");
    }

    class WeatherForecast
    {
        public DateTime Date { get; set; }
        public int TemperatureC { get; set; }
        public int TemperatureF { get; set; }
        public string Summary { get; set; }
    }
}
```

<span data-ttu-id="be451-177">A `@foreach` döngü, her bir tahmin örneği hava durumu tablosundaki bir satır olarak işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="be451-177">A `@foreach` loop is used to render each forecast instance as a row in the weather table.</span></span>

```cshtml
<table class="table">
    <thead>
        <tr>
            <th>Date</th>
            <th>Temp. (C)</th>
            <th>Temp. (F)</th>
            <th>Summary</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var forecast in forecasts)
        {
            <tr>
                <td>@forecast.Date.ToShortDateString()</td>
                <td>@forecast.TemperatureC</td>
                <td>@forecast.TemperatureF</td>
                <td>@forecast.Summary</td>
            </tr>
        }
    </tbody>
</table>
```

## <a name="build-a-todo-list"></a><span data-ttu-id="be451-178">Bir Yapılacaklar listesi oluşturun</span><span class="sxs-lookup"><span data-stu-id="be451-178">Build a todo list</span></span>

<span data-ttu-id="be451-179">Bir basit bir Yapılacaklar listesi uygulayan uygulamaya yeni bir sayfa ekleyin.</span><span class="sxs-lookup"><span data-stu-id="be451-179">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="be451-180">Boş bir metin dosyası için ekleme *sayfaları* adlı klasöre *Todo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="be451-180">Add an empty text file to the *Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="be451-181">Sayfa için ilk biçimlendirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="be451-181">Provide the initial markup for the page.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>
    ```

1. <span data-ttu-id="be451-182">Todo sayfası güncelleştirerek Gezinti çubuğuna ekleyin *Shared/NavMenu.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="be451-182">Add the Todo page to the navigation bar by updating *Shared/NavMenu.cshtml*.</span></span> <span data-ttu-id="be451-183">Ekleme bir `NavLink` aşağıdaki liste öğesi işaretleme varolan liste öğelerini aşağıda ekleyerek Todo sayfası.</span><span class="sxs-lookup"><span data-stu-id="be451-183">Add a `NavLink` for the Todo page by adding the following list item markup below the existing list items.</span></span>

    ```cshtml
    <li class="nav-item px-3">
        <NavLink class="nav-link" href="todo">
            <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
        </NavLink>
    </li>
    ```

1. <span data-ttu-id="be451-184">Uygulamayı tarayıcıda yenileyin.</span><span class="sxs-lookup"><span data-stu-id="be451-184">Refresh the app in the browser.</span></span> <span data-ttu-id="be451-185">Yeni Todo sayfasına bakın.</span><span class="sxs-lookup"><span data-stu-id="be451-185">See the new Todo page.</span></span>

    ![Blazor todo Başlat](https://user-images.githubusercontent.com/1874516/39509907-bb27e77a-4d9c-11e8-91e7-ea1e7c01729e.png)

1. <span data-ttu-id="be451-187">Ekleme bir *TodoItem.cs* todo öğelerini temsil etmek için bir sınıf tutmak için projenin kök dosya.</span><span class="sxs-lookup"><span data-stu-id="be451-187">Add a *TodoItem.cs* file to the root of the project to hold a class to represent the todo items.</span></span>

1. <span data-ttu-id="be451-188">Aşağıdaki C# için kod `TodoItem` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="be451-188">Use the following C# code for the `TodoItem` class.</span></span>

    ```csharp
    public class TodoItem
    {
        public string Title { get; set; }
        public bool IsDone { get; set; }
    }
    ```

1. <span data-ttu-id="be451-189">Geri Git `Todo` bileşeninin *Todo.cshtml* ve içinde açıklamada için bir alan eklemek bir `@functions` blok.</span><span class="sxs-lookup"><span data-stu-id="be451-189">Go back to the `Todo` component in *Todo.cshtml* and add a field for the todos in a `@functions` block.</span></span> <span data-ttu-id="be451-190">`Todo` Bileşen Yapılacaklar listesi durumunu korumak için bu alanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="be451-190">The `Todo` component uses this field to maintain the state of the todo list.</span></span>

    ```cshtml
    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. <span data-ttu-id="be451-191">Sırasız liste biçimlendirme eklemek ve `foreach` her bir todo öğesi bir liste öğesi olarak işlenecek döngü.</span><span class="sxs-lookup"><span data-stu-id="be451-191">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>
    ```

1. <span data-ttu-id="be451-192">Uygulama, açıklamada listeye eklemek için kullanıcı Arabirimi öğeleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="be451-192">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="be451-193">Bir metin girişi ve listenin altındaki bir düğme ekleyin.</span><span class="sxs-lookup"><span data-stu-id="be451-193">Add a text input and a button below the list.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" />
    <button>Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. <span data-ttu-id="be451-194">Tarayıcıyı yenileyin.</span><span class="sxs-lookup"><span data-stu-id="be451-194">Refresh the browser.</span></span>

    ![TODO düğmesi ekleme](https://user-images.githubusercontent.com/1874516/39512402-bc88ab46-4da5-11e8-9e3f-87b875b56383.png)

    <span data-ttu-id="be451-196">Hiçbir şey olmaz, **todo ekleyin** düğmesi seçili hiçbir olay işleyicisi, düğmenin kadar kablolu olduğundan.</span><span class="sxs-lookup"><span data-stu-id="be451-196">Nothing happens when the **Add todo** button is selected because no event handler is wired up to the button.</span></span>

1. <span data-ttu-id="be451-197">Ekleme bir `AddTodo` yönteme `Todo` bileşeni ve kayıt için düğmeyi tıkladığında kullanarak `onclick` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="be451-197">Add an `AddTodo` method to the `Todo` component and register it for button clicks using the `onclick` attribute.</span></span>

    ```cshtml
    <input placeholder="Something todo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();

        void AddTodo()
        {
            // Todo: Add the todo
        }
    }
    ```

    <span data-ttu-id="be451-198">`AddTodo` C# Düğme seçili her zaman yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="be451-198">The `AddTodo` C# method is called every time the button is selected.</span></span>

1. <span data-ttu-id="be451-199">Yeni todo öğesinin başlığını almak için ekleyin bir `newTodo` dize alanı ve değeri kullanarak metin olarak bağlama `bind` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="be451-199">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute.</span></span>

    ```csharp
    IList<TodoItem> todos = new List<TodoItem>();
    string newTodo;
    ```

    ```cshtml
    <input placeholder="Something todo" bind="@newTodo" />
    ```

1. <span data-ttu-id="be451-200">Güncelleştirme `AddTodo` ekleme yöntemi `TodoItem` listesine belirtilen başlığa sahip.</span><span class="sxs-lookup"><span data-stu-id="be451-200">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="be451-201">Metin girişi değerini ayarlayarak temizlemeyi unutmayın `newTodo` boş bir dize.</span><span class="sxs-lookup"><span data-stu-id="be451-201">Don't forget to clear the value of the text input by setting `newTodo` to an empty string.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

1. <span data-ttu-id="be451-202">Tarayıcıyı yenileyin.</span><span class="sxs-lookup"><span data-stu-id="be451-202">Refresh the browser.</span></span> <span data-ttu-id="be451-203">Bazı açıklamada yapılacaklar listesine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="be451-203">Add some todos to the todo list.</span></span>

    ![Açıklamada Ekle](https://user-images.githubusercontent.com/1874516/39512531-2d2ff62e-4da6-11e8-8b83-291b0efc821b.png)

1. <span data-ttu-id="be451-205">Son olarak, onay kutularını olmadan bir Yapılacaklar listesi nedir?</span><span class="sxs-lookup"><span data-stu-id="be451-205">Lastly, what's a todo list without check boxes?</span></span> <span data-ttu-id="be451-206">Her bir todo öğesi için başlık metni düzenlenebilir de yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="be451-206">The title text for each todo item can be made editable as well.</span></span> <span data-ttu-id="be451-207">Bir onay kutusu giriş ve her bir todo öğesi için metin girişi ekleme ve değerlerine bağlama `Title` ve `IsDone` özellikleri, sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="be451-207">Add a check box input and text input for each todo item and bind their values to the `Title` and `IsDone` properties, respectively.</span></span>

    ```cshtml
    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>
    ```

1. <span data-ttu-id="be451-208">Bu değerleri bağlı olduğunu doğrulamak için güncelleştirme `h1` henüz yapılmadı todo öğelerini sayısını göstermek için başlık (`IsDone` olan `false`).</span><span class="sxs-lookup"><span data-stu-id="be451-208">To verify that these values are bound, update the `h1` header to show a count of the number of todo items that are not yet done (`IsDone` is `false`).</span></span>

    ```cshtml
    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
    ```

1. <span data-ttu-id="be451-209">Tamamlanan `Todo` bileşeni şu şekilde görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="be451-209">The completed `Todo` component should look like this:</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

<span data-ttu-id="be451-210">Uygulamayı tarayıcıda yenileyin.</span><span class="sxs-lookup"><span data-stu-id="be451-210">Refresh the app in the browser.</span></span> <span data-ttu-id="be451-211">Birkaç Yapılacaklar öğesi eklemeyi deneyin.</span><span class="sxs-lookup"><span data-stu-id="be451-211">Try adding some todo items.</span></span>

![Tamamlanmış Blazor Yapılacaklar listesi](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

## <a name="publish-and-deploy"></a><span data-ttu-id="be451-213">Yayımlama ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="be451-213">Publish and deploy</span></span>

<span data-ttu-id="be451-214">Visual Studio kullanırken Todo Blazor uygulamasını Azure'da yayımlamak için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="be451-214">When using Visual Studio, perform the following steps to publish the Todo Blazor app to Azure:</span></span>

1. <span data-ttu-id="be451-215">Projeye sağ tıklayarak **Çözüm Gezgini** seçip **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="be451-215">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="be451-216">İçinde **yayımlama hedefi seçin** iletişim kutusunda **App Service** ve **Yeni Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="be451-216">In the **Pick a publish target** dialog, select **App Service** and **Create New**.</span></span> <span data-ttu-id="be451-217">**Yayımla**’yı seçin.</span><span class="sxs-lookup"><span data-stu-id="be451-217">Select **Publish**.</span></span>

    ![Yayımlama hedefi seçin](build-your-first-blazor-app/_static/blazor-publish-pick-target.png)

1. <span data-ttu-id="be451-219">İçinde **App Service Oluştur** iletişim kutusunda, uygulama için bir ad seçin ve abonelik, kaynak grubu ve barındırma planı seçin.</span><span class="sxs-lookup"><span data-stu-id="be451-219">In the **Create App Service** dialog, choose a name for the app and select the subscription, resource group, and hosting plan.</span></span> <span data-ttu-id="be451-220">Seçin **Oluştur** app service oluşturma ve uygulamayı yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="be451-220">Select **Create** to create the app service and publish the app.</span></span>

    ![App service oluştur](build-your-first-blazor-app/_static/blazor-publish-create-appservice2.png)

<span data-ttu-id="be451-222">Veya bunu dağıtılacak uygulama için bir dakika bekleyin.</span><span class="sxs-lookup"><span data-stu-id="be451-222">Wait a minute or so for the app to be deployed.</span></span>

<span data-ttu-id="be451-223">Uygulama artık Azure'da çalıştırıyor olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="be451-223">The app should now be running in Azure.</span></span> <span data-ttu-id="be451-224">Todo öğesini ilk Blazor uygulamanızı oluşturun işaretlemek *Bitti*.</span><span class="sxs-lookup"><span data-stu-id="be451-224">Mark the todo item to build your first Blazor app as *done*.</span></span> <span data-ttu-id="be451-225">İyi iş!</span><span class="sxs-lookup"><span data-stu-id="be451-225">Nice job!</span></span>

![Azure'da Blazor](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

> [!NOTE]
> <span data-ttu-id="be451-227">Aksi durumda Visual Studio'yu kullanarak Windows, macOS veya Linux üzerinde bir komut isteminde Blazor uygulama yayımlama:</span><span class="sxs-lookup"><span data-stu-id="be451-227">If not using Visual Studio, publish the Blazor app at a command prompt on Windows, macOS, or Linux:</span></span>
>
> ```console
> dotnet publish -c Release
> ```
>
> <span data-ttu-id="be451-228">Dağıtım oluşturulan */bin/yayın/\<hedef çerçeve > / publish* klasör.</span><span class="sxs-lookup"><span data-stu-id="be451-228">The deployment is created in the */bin/Release/\<target-framework>/publish* folder.</span></span> <span data-ttu-id="be451-229">İçeriği Taşı *yayımlama* klasörü sunucu ya da barındırma hizmeti.</span><span class="sxs-lookup"><span data-stu-id="be451-229">Move the contents of the *publish* folder to the server or hosting service.</span></span>
>
> <span data-ttu-id="be451-230">Daha fazla bilgi için [konak dağıtıp](xref:host-and-deploy/razor-components/index#publish-the-app) konu.</span><span class="sxs-lookup"><span data-stu-id="be451-230">For more information, see the [Host and deploy](xref:host-and-deploy/razor-components/index#publish-the-app) topic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be451-231">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="be451-231">Additional resources</span></span>

<span data-ttu-id="be451-232">Daha Blazor örnek uygulamasını dahil göz atın [uçuş Bulucu örnek](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) GitHub üzerinde.</span><span class="sxs-lookup"><span data-stu-id="be451-232">For a more involved Blazor sample app, check out the [Flight Finder sample](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) on GitHub.</span></span>

![Blazor uçuş Bulucu](https://msdnshared.blob.core.windows.net/media/2018/03/blazor-flight-finder.png)
