---
title: ASP.NET core'da Blazor giriş
author: guardrex
description: ASP.NET Core Blazor, etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET, ASP.NET Core uygulaması oluşturmak için bir yöntem keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 05/01/2019
uid: blazor/index
ms.openlocfilehash: d58115b17536cad0b3927e6d32b7dbe8db8e4b0f
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034428"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="ebef6-103">Blazor giriş</span><span class="sxs-lookup"><span data-stu-id="ebef6-103">Introduction to Blazor</span></span>

<span data-ttu-id="ebef6-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ebef6-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ebef6-105">*Blazor hoşgeldiniz!*</span><span class="sxs-lookup"><span data-stu-id="ebef6-105">*Welcome to Blazor!*</span></span>

<span data-ttu-id="ebef6-106">Blazor etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET oluşturmaya yönelik bir yapıdır:</span><span class="sxs-lookup"><span data-stu-id="ebef6-106">Blazor is a framework for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="ebef6-107">Kullanarak zengin etkileşimli kullanıcı arabirimleri oluşturun C# JavaScript yerine.</span><span class="sxs-lookup"><span data-stu-id="ebef6-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="ebef6-108">.NET ile yazılmış sunucu tarafı ve istemci tarafı uygulama mantığı paylaşın.</span><span class="sxs-lookup"><span data-stu-id="ebef6-108">Share server-side and client-side app logic written with .NET.</span></span>
* <span data-ttu-id="ebef6-109">HTML ve CSS olarak UI mobil tarayıcılar dahil olmak üzere geniş tarayıcı desteği işleyin.</span><span class="sxs-lookup"><span data-stu-id="ebef6-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="ebef6-110">.NET için istemci tarafı web dağıtımı kullanarak aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="ebef6-110">Using .NET for client-side web development offers the following advantages:</span></span>

* <span data-ttu-id="ebef6-111">Kod yazmaya C# JavaScript yerine.</span><span class="sxs-lookup"><span data-stu-id="ebef6-111">Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="ebef6-112">Mevcut .NET ekosisteminin .NET kitaplıklarının yararlanın.</span><span class="sxs-lookup"><span data-stu-id="ebef6-112">Leverage the existing .NET ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="ebef6-113">Uygulama mantığı, sunucu ve istemci arasında paylaşın.</span><span class="sxs-lookup"><span data-stu-id="ebef6-113">Share app logic across the server and client.</span></span>
* <span data-ttu-id="ebef6-114">Yararlanın. NET performans, güvenilirlik ve güvenlik.</span><span class="sxs-lookup"><span data-stu-id="ebef6-114">Benefit from .NET's performance, reliability, and security.</span></span>
* <span data-ttu-id="ebef6-115">Windows, Linux ve macOS üzerinde Visual Studio üretken kalın.</span><span class="sxs-lookup"><span data-stu-id="ebef6-115">Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="ebef6-116">Dillerin, çerçevelerin ve tutarlı, zengin ve kullanımı kolay araçlar ortak kümesine göre oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ebef6-116">Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

## <a name="components"></a><span data-ttu-id="ebef6-117">Bileşenler</span><span class="sxs-lookup"><span data-stu-id="ebef6-117">Components</span></span>

<span data-ttu-id="ebef6-118">Blazor uygulamaları temel *bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="ebef6-118">Blazor apps are based on *components*.</span></span> <span data-ttu-id="ebef6-119">Blazor bileşeninde bir kullanıcı Arabirimi, bir sayfa, iletişim kutusu veya veri girişi formuna gibi öğesidir.</span><span class="sxs-lookup"><span data-stu-id="ebef6-119">A component in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="ebef6-120">Bileşenleri kullanıcı olayları işlemek ve esnek UI işleme mantığı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="ebef6-120">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="ebef6-121">Bileşenleri iç içe geçmiş ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ebef6-121">Components can be nested and reused.</span></span>

<span data-ttu-id="ebef6-122">.NET sınıf paylaşılabilir ve olarak dağıtılmış .NET derlemelerini yerleşik bileşenlerdir [NuGet paketlerini](/nuget/what-is-nuget).</span><span class="sxs-lookup"><span data-stu-id="ebef6-122">Components are .NET classes built into .NET assemblies that can be shared and distributed as [NuGet packages](/nuget/what-is-nuget).</span></span> <span data-ttu-id="ebef6-123">Bileşen sınıfı genellikle bir Razor biçimlendirme sayfayla biçiminde yazılmış bir *.razor* dosya uzantısı.</span><span class="sxs-lookup"><span data-stu-id="ebef6-123">The component class is usually written in the form of a Razor markup page with a *.razor* file extension.</span></span>

<span data-ttu-id="ebef6-124">Blazor bileşenlerde bazen denir *Razor bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="ebef6-124">Components in Blazor are sometimes referred to as *Razor components*.</span></span> <span data-ttu-id="ebef6-125">[Razor](xref:mvc/views/razor) HTML biçimlendirmesi ile birleştiren bir söz dizimi C# kod Geliştirici üretkenliğini için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="ebef6-125">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code designed for developer productivity.</span></span> <span data-ttu-id="ebef6-126">Razor HTML biçimlendirmeyi arasında geçiş yapmanıza izin verir ve C# ile aynı dosyada [IntelliSense](/visualstudio/ide/using-intellisense) destekler.</span><span class="sxs-lookup"><span data-stu-id="ebef6-126">Razor allows you to switch between HTML markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="ebef6-127">Razor sayfaları ve MVC Razor de kullanın.</span><span class="sxs-lookup"><span data-stu-id="ebef6-127">Razor Pages and MVC also use Razor.</span></span> <span data-ttu-id="ebef6-128">Razor sayfaları ve MVC, istek/yanıt model oluşturulmuştur, bileşenleri, özellikle istemci tarafı UI mantığı ve birleştirme için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ebef6-128">Unlike Razor Pages and MVC, which are built around a request/response model, components are used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="ebef6-129">Bir bileşen aşağıdaki Razor işaretlemesi gösterir (*Dialog.razor*), iç içe geçirilemez başka bir bileşen içinde:</span><span class="sxs-lookup"><span data-stu-id="ebef6-129">The following Razor markup demonstrates a component (*Dialog.razor*), which can be nested within another component:</span></span>

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="@OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    private string Title { get; set; }

    [Parameter]
    private RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#!");
    }
}
```

<span data-ttu-id="ebef6-130">İletişim kutusunun gövde içeriği (`ChildContent`) ve başlık (`Title`) Bu bileşen, kullanıcı Arabiriminde kullanan bileşen tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ebef6-130">The dialog's body content (`ChildContent`) and title (`Title`) are provided by the component that uses this component in its UI.</span></span> <span data-ttu-id="ebef6-131">`OnYes` olan bir C# düğmenin tarafından tetiklenen yöntemi `onclick` olay.</span><span class="sxs-lookup"><span data-stu-id="ebef6-131">`OnYes` is a C# method triggered by the button's `onclick` event.</span></span>

<span data-ttu-id="ebef6-132">Blazor doğal HTML etiketleri için kullanıcı Arabirimi oluşturma kullanır.</span><span class="sxs-lookup"><span data-stu-id="ebef6-132">Blazor uses natural HTML tags for UI composition.</span></span> <span data-ttu-id="ebef6-133">HTML öğeleri bileşenleri belirtin ve bir etiketin öznitelikleri, bir bileşenin özelliklerine değerlerini geçirirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ebef6-133">HTML elements specify components, and a tag's attributes pass values to a component's properties.</span></span> <span data-ttu-id="ebef6-134">`ChildContent` ve `Title` iletişim bileşen, bileşen tarafından ayarlanır (*Index.razor*):</span><span class="sxs-lookup"><span data-stu-id="ebef6-134">`ChildContent` and `Title` are set by the component that uses the Dialog component (*Index.razor*):</span></span>

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

<span data-ttu-id="ebef6-135">İletişim zaman işlenir üst (*Index.razor*) bir tarayıcıdan erişildiğinde:</span><span class="sxs-lookup"><span data-stu-id="ebef6-135">The dialog is rendered when the parent (*Index.razor*) is accessed in a browser:</span></span>

![Tarayıcıda işlenen iletişim bileşeni](index/_static/dialog.png)

<span data-ttu-id="ebef6-137">Uygulamasında, IntelliSense içinde bu bileşen kullanıldığında [Visual Studio](/visualstudio/ide/using-intellisense) ve [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) söz dizimi ve parametre tamamlama ile geliştirme sürecini hızlandırdı.</span><span class="sxs-lookup"><span data-stu-id="ebef6-137">When this component is used in the app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) and [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="ebef6-138">Bileşenleri oluşturma DOM adlı tarayıcı bellek içi gösterimine bir *ağaç işleme* UI esnek ve verimli bir şekilde güncelleştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ebef6-138">Components render into an in-memory representation of the browser DOM called a *render tree* that's used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="ebef6-139">Blazor istemci tarafı</span><span class="sxs-lookup"><span data-stu-id="ebef6-139">Blazor client-side</span></span>

<span data-ttu-id="ebef6-140">Blazor istemci tarafı .NET ile etkileşimli istemci tarafı web uygulamaları oluşturmak için bir tek sayfalı uygulama çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="ebef6-140">Blazor client-side is a single-page app framework for building interactive client-side web apps with .NET.</span></span> <span data-ttu-id="ebef6-141">Blazor istemci-tarafı açık web standartları transpilation eklentileri veya kod olmadan kullanır ve mobil tarayıcılar dahil tüm modern web tarayıcılarında çalışır.</span><span class="sxs-lookup"><span data-stu-id="ebef6-141">Blazor client-side uses open web standards without plugins or code transpilation and works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="ebef6-142">Çalışan tarayıcılar içinde .NET kod tarafından yapılır [WebAssembly](http://webassembly.org) (kısaltılmış *wasm*).</span><span class="sxs-lookup"><span data-stu-id="ebef6-142">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="ebef6-143">WebAssembly standart ve desteklenen web tarayıcılarından eklentileri olmadan'de açık bir web API'sidir.</span><span class="sxs-lookup"><span data-stu-id="ebef6-143">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="ebef6-144">WebAssembly hızlı indirme ve maksimum yürütme hızı için iyileştirilmiş bir compact bayt biçimidir.</span><span class="sxs-lookup"><span data-stu-id="ebef6-144">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="ebef6-145">WebAssembly kod tarayıcısı adlı JavaScript aracılığıyla tam işlevselliğini erişebilir *JavaScript birlikte çalışabilirlik* (veya *JavaScript birlikte çalışma*).</span><span class="sxs-lookup"><span data-stu-id="ebef6-145">WebAssembly code can access the full functionality of the browser via JavaScript, called *JavaScript interoperability* (or *JavaScript interop*).</span></span> <span data-ttu-id="ebef6-146">.NET kodu WebAssembly tarayıcıda çalıştırılan sanal bir uygulama istemci makinede kötü amaçlı eylemler gerçekleştirme fırsatı ortadan kaldırır ve JavaScript aynı güvenilir sanal çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="ebef6-146">.NET code executed via WebAssembly in the browser runs in the same trusted sandbox as JavaScript, which virtually eliminates the opportunity for an app to perform malicious actions on the client machine.</span></span>

![Blazor istemci tarafı .NET kodu tarayıcıda WebAssembly ile çalışır.](index/_static/blazor-client-side.png)

<span data-ttu-id="ebef6-148">Ne zaman bir Blazor istemci-tarafı uygulaması oluşturulur ve bir tarayıcıda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ebef6-148">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="ebef6-149">C#kod dosyaları ve Razor dosyaları .NET bütünleştirilmiş kodlarının derlenir.</span><span class="sxs-lookup"><span data-stu-id="ebef6-149">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="ebef6-150">Derlemeler ve .NET çalışma zamanı tarayıcıya indirilir.</span><span class="sxs-lookup"><span data-stu-id="ebef6-150">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="ebef6-151">Blazor istemci-tarafı bootstraps .NET çalışma zamanı ve çalışma zamanı derlemeleri uygulama yüklemek için yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ebef6-151">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="ebef6-152">Blazor istemci tarafı çalışma zamanı JavaScript birlikte çalışma belge nesne modeli (DOM) işleme ve tarayıcı API çağrıları işlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="ebef6-152">The Blazor client-side runtime uses JavaScript interop to handle Document Object Model (DOM) manipulation and browser API calls.</span></span>

<span data-ttu-id="ebef6-153">Yayımlanan bir uygulamanın boyutunu kendi *yükü boyutu*, uygulamanın uygun kritik performans faktördür.</span><span class="sxs-lookup"><span data-stu-id="ebef6-153">The size of the published app, its *payload size*, is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="ebef6-154">Çok sayıda uygulama, kullanıcı deneyimini gelmez bir tarayıcıya indirmek için oldukça uzun bir zaman alır.</span><span class="sxs-lookup"><span data-stu-id="ebef6-154">A large app takes a relatively long time to download to a browser, which hurts the user experience.</span></span> <span data-ttu-id="ebef6-155">Blazor istemci tarafı yük boyutundaki yükleme sürelerini kısaltmak için en iyi duruma getirir:</span><span class="sxs-lookup"><span data-stu-id="ebef6-155">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="ebef6-156">Kullanılmayan kod tarafından yayımlandığında uygulama oturumunu çıkartılır [Ara dil (IL) bağlayıcı](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="ebef6-156">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>
* <span data-ttu-id="ebef6-157">HTTP yanıtlarını sıkıştırılır.</span><span class="sxs-lookup"><span data-stu-id="ebef6-157">HTTP responses are compressed.</span></span>
* <span data-ttu-id="ebef6-158">.NET çalışma zamanı ve derlemeleri tarayıcıda önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="ebef6-158">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="ebef6-159">Daha fazla bilgi ve barındırma modeli seçme konusunda yönergeler için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="ebef6-159">For more information and guidance on choosing a hosting model, see <xref:blazor/hosting-models>.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="ebef6-160">Blazor sunucu tarafı</span><span class="sxs-lookup"><span data-stu-id="ebef6-160">Blazor server-side</span></span>

<span data-ttu-id="ebef6-161">Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bileşeni işleme mantığı Blazor ayırır.</span><span class="sxs-lookup"><span data-stu-id="ebef6-161">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="ebef6-162">Sunucu tarafı Blazor Razor bileşenleri ASP.NET Core uygulaması sunucusunda barındırmak için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="ebef6-162">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="ebef6-163">Kullanıcı Arabirimi güncelleştirmeleri üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.</span><span class="sxs-lookup"><span data-stu-id="ebef6-163">UI updates are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="ebef6-164">Çalışma zamanı kullanıcı Arabirimi olayları tarayıcıdan sunucuya gönderme işler ve bileşenleri çalıştırdıktan sonra tarayıcıya sunucu tarafından gönderilen kullanıcı Arabirimi güncelleştirmeleri uygular.</span><span class="sxs-lookup"><span data-stu-id="ebef6-164">The runtime handles sending UI events from the browser to the server and applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="ebef6-165">Tarayıcı ile iletişim kurmak için Blazor sunucu-tarafı tarafından kullanılan bağlantı JavaScript birlikte çalışma çağrıları işlemek için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ebef6-165">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Blazor sunucu tarafı .NET kod sunucu üzerinde çalışır ve belge nesne modeli istemcide bir SignalR bağlantısı üzerinden etkileşim](index/_static/blazor-server-side.png)

<span data-ttu-id="ebef6-167">Daha fazla bilgi ve barındırma modeli seçme konusunda yönergeler için bkz. <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="ebef6-167">For more information and guidance on choosing a hosting model, see <xref:blazor/hosting-models>.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="ebef6-168">JavaScript ile birlikte çalışma</span><span class="sxs-lookup"><span data-stu-id="ebef6-168">JavaScript interop</span></span>

<span data-ttu-id="ebef6-169">Üçüncü taraf JavaScript kitaplıklarını ve API'lerini tarayıcı gerektiren uygulamalar için bileşenleri JavaScript ile çalışma.</span><span class="sxs-lookup"><span data-stu-id="ebef6-169">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="ebef6-170">Herhangi bir kitaplığı veya JavaScript kullanabilmek için API kullanma yeteneği bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="ebef6-170">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="ebef6-171">C#kod, JavaScript kodu çağırabilir ve JavaScript kod içine çağırabilir C# kod.</span><span class="sxs-lookup"><span data-stu-id="ebef6-171">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="ebef6-172">Daha fazla bilgi için bkz. <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="ebef6-172">For more information, see <xref:blazor/javascript-interop>.</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="ebef6-173">Kod paylaşımı ve .NET Standard</span><span class="sxs-lookup"><span data-stu-id="ebef6-173">Code sharing and .NET Standard</span></span>

<span data-ttu-id="ebef6-174">Blazor uygulayan [.NET Standard 2.0](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="ebef6-174">Blazor implements [.NET Standard 2.0](/dotnet/standard/net-standard).</span></span> <span data-ttu-id="ebef6-175">.NET standart, .NET API'leri, .NET uygulamaları arasında ortak olan bir resmi belirtimi olan.</span><span class="sxs-lookup"><span data-stu-id="ebef6-175">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="ebef6-176">.NET standart sınıf kitaplıkları Blazor, .NET Framework, .NET Core, Xamarin, Mono ve Unity gibi farklı .NET platformlar arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="ebef6-176">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

<span data-ttu-id="ebef6-177">Bir web tarayıcısı içinde (örneğin, dosya sistemine erişen bir yuva açma ve iş parçacığı) uygun olmayan API'leri throw bir <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="ebef6-177">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, and threading) throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ebef6-178">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ebef6-178">Additional resources</span></span>

* [<span data-ttu-id="ebef6-179">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="ebef6-179">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="ebef6-180">C# Kılavuzu</span><span class="sxs-lookup"><span data-stu-id="ebef6-180">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="ebef6-181">HTML</span><span class="sxs-lookup"><span data-stu-id="ebef6-181">HTML</span></span>](https://www.w3.org/html/)
