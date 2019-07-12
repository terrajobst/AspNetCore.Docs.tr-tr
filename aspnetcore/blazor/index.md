---
title: ASP.NET core'da Blazor giriş
author: guardrex
description: ASP.NET Core Blazor, etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET, ASP.NET Core uygulaması oluşturmak için bir yöntem keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 07/01/2019
uid: blazor/index
ms.openlocfilehash: e0af0f27d79973f10493251c3f6c6daebe1b99a8
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855777"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="99942-103">Blazor giriş</span><span class="sxs-lookup"><span data-stu-id="99942-103">Introduction to Blazor</span></span>

<span data-ttu-id="99942-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="99942-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="99942-105">*Blazor hoşgeldiniz!*</span><span class="sxs-lookup"><span data-stu-id="99942-105">*Welcome to Blazor!*</span></span>

<span data-ttu-id="99942-106">Blazor etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET oluşturmaya yönelik bir yapıdır:</span><span class="sxs-lookup"><span data-stu-id="99942-106">Blazor is a framework for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="99942-107">Kullanarak zengin etkileşimli kullanıcı arabirimleri oluşturun C# JavaScript yerine.</span><span class="sxs-lookup"><span data-stu-id="99942-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="99942-108">.NET ile yazılan sunucu tarafı ve istemci tarafı uygulama mantığı paylaşın.</span><span class="sxs-lookup"><span data-stu-id="99942-108">Share server-side and client-side app logic written in .NET.</span></span>
* <span data-ttu-id="99942-109">HTML ve CSS olarak UI mobil tarayıcılar dahil olmak üzere geniş tarayıcı desteği işleyin.</span><span class="sxs-lookup"><span data-stu-id="99942-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="99942-110">.NET için istemci tarafı web dağıtımı kullanarak aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="99942-110">Using .NET for client-side web development offers the following advantages:</span></span>

* <span data-ttu-id="99942-111">Kod yazmaya C# JavaScript yerine.</span><span class="sxs-lookup"><span data-stu-id="99942-111">Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="99942-112">Mevcut .NET ekosisteminin .NET kitaplıklarının yararlanın.</span><span class="sxs-lookup"><span data-stu-id="99942-112">Leverage the existing .NET ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="99942-113">Uygulama mantığı, sunucu ve istemci arasında paylaşın.</span><span class="sxs-lookup"><span data-stu-id="99942-113">Share app logic across server and client.</span></span>
* <span data-ttu-id="99942-114">Yararlanın. NET performans, güvenilirlik ve güvenlik.</span><span class="sxs-lookup"><span data-stu-id="99942-114">Benefit from .NET's performance, reliability, and security.</span></span>
* <span data-ttu-id="99942-115">Windows, Linux ve macOS üzerinde Visual Studio üretken kalın.</span><span class="sxs-lookup"><span data-stu-id="99942-115">Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="99942-116">Dillerin, çerçevelerin ve tutarlı, zengin ve kullanımı kolay araçlar ortak kümesine göre oluşturun.</span><span class="sxs-lookup"><span data-stu-id="99942-116">Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

## <a name="components"></a><span data-ttu-id="99942-117">Bileşenler</span><span class="sxs-lookup"><span data-stu-id="99942-117">Components</span></span>

<span data-ttu-id="99942-118">Blazor uygulamaları temel *bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="99942-118">Blazor apps are based on *components*.</span></span> <span data-ttu-id="99942-119">Blazor bileşeninde bir kullanıcı Arabirimi, bir sayfa, iletişim kutusu veya veri girişi formuna gibi öğesidir.</span><span class="sxs-lookup"><span data-stu-id="99942-119">A component in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span>

<span data-ttu-id="99942-120">.NET sınıfları .NET bütünleştirilmiş kodlarının oluşturulan bileşenlerdir:</span><span class="sxs-lookup"><span data-stu-id="99942-120">Components are .NET classes built into .NET assemblies that:</span></span>

* <span data-ttu-id="99942-121">UI işleme mantığı esnek tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="99942-121">Define flexible UI rendering logic.</span></span>
* <span data-ttu-id="99942-122">Kullanıcı olayları işleyin.</span><span class="sxs-lookup"><span data-stu-id="99942-122">Handle user events.</span></span>
* <span data-ttu-id="99942-123">İç içe geçmiş ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="99942-123">Can be nested and reused.</span></span>
* <span data-ttu-id="99942-124">Paylaşılan ve olarak dağıtılmış [Razor sınıf kitaplıkları](xref:razor-pages/ui-class) veya [NuGet paketlerini](/nuget/what-is-nuget).</span><span class="sxs-lookup"><span data-stu-id="99942-124">Can be shared and distributed as [Razor class libraries](xref:razor-pages/ui-class) or [NuGet packages](/nuget/what-is-nuget).</span></span>

<span data-ttu-id="99942-125">Bileşen sınıfı genellikle biçiminde yazılmış bir [Razor](xref:mvc/views/razor) biçimlendirme sayfasıyla bir *.razor* dosya uzantısı.</span><span class="sxs-lookup"><span data-stu-id="99942-125">The component class is usually written in the form of a [Razor](xref:mvc/views/razor) markup page with a *.razor* file extension.</span></span> <span data-ttu-id="99942-126">Blazor bileşenlerde izlerse denir *Razor bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="99942-126">Components in Blazor are formally referred to as *Razor components*.</span></span> <span data-ttu-id="99942-127">Razor HTML biçimlendirmesi ile birleştiren bir söz dizimi olan C# kod Geliştirici üretkenliğini için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="99942-127">Razor is a syntax for combining HTML markup with C# code designed for developer productivity.</span></span> <span data-ttu-id="99942-128">Razor HTML biçimlendirmeyi arasında geçiş yapmanıza izin verir ve C# ile aynı dosyada [IntelliSense](/visualstudio/ide/using-intellisense) destekler.</span><span class="sxs-lookup"><span data-stu-id="99942-128">Razor allows you to switch between HTML markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="99942-129">Razor sayfaları ve MVC Razor de kullanın.</span><span class="sxs-lookup"><span data-stu-id="99942-129">Razor Pages and MVC also use Razor.</span></span> <span data-ttu-id="99942-130">Razor sayfaları ve MVC, istek/yanıt model oluşturulmuştur, bileşenleri, özellikle istemci tarafı UI mantığı ve birleştirme için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="99942-130">Unlike Razor Pages and MVC, which are built around a request/response model, components are used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="99942-131">Bir bileşen aşağıdaki Razor işaretlemesi gösterir (*Dialog.razor*), iç içe geçirilemez başka bir bileşen içinde:</span><span class="sxs-lookup"><span data-stu-id="99942-131">The following Razor markup demonstrates a component (*Dialog.razor*), which can be nested within another component:</span></span>

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
        Console.WriteLine("Write to the console in C#! 'Yes' button was selected.");
    }
}
```

<span data-ttu-id="99942-132">İletişim kutusunun gövde içeriği (`ChildContent`) ve başlık (`Title`) Bu bileşen, kullanıcı Arabiriminde kullanan bileşen tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="99942-132">The dialog's body content (`ChildContent`) and title (`Title`) are provided by the component that uses this component in its UI.</span></span> <span data-ttu-id="99942-133">`OnYes` olan bir C# düğmenin tarafından tetiklenen yöntemi `onclick` olay.</span><span class="sxs-lookup"><span data-stu-id="99942-133">`OnYes` is a C# method triggered by the button's `onclick` event.</span></span>

<span data-ttu-id="99942-134">Blazor doğal HTML etiketleri için kullanıcı Arabirimi oluşturma kullanır.</span><span class="sxs-lookup"><span data-stu-id="99942-134">Blazor uses natural HTML tags for UI composition.</span></span> <span data-ttu-id="99942-135">HTML öğeleri bileşenleri belirtin ve bir etiketin öznitelikleri, bir bileşenin özelliklerine değerlerini geçirirsiniz.</span><span class="sxs-lookup"><span data-stu-id="99942-135">HTML elements specify components, and a tag's attributes pass values to a component's properties.</span></span>

<span data-ttu-id="99942-136">Aşağıdaki örnekte, `Index` bileşen `Dialog` bileşeni.</span><span class="sxs-lookup"><span data-stu-id="99942-136">In the following example, the `Index` component uses the `Dialog` component.</span></span> <span data-ttu-id="99942-137">`ChildContent` ve `Title` içeriğini ve öznitelikleri ayarlama `<Dialog>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="99942-137">`ChildContent` and `Title` are set by the attributes and content of the `<Dialog>` element.</span></span>

<span data-ttu-id="99942-138">*Index.Razor*:</span><span class="sxs-lookup"><span data-stu-id="99942-138">*Index.razor*:</span></span>

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

<span data-ttu-id="99942-139">İletişim zaman işlenir üst (*Index.razor*) bir tarayıcıdan erişildiğinde:</span><span class="sxs-lookup"><span data-stu-id="99942-139">The dialog is rendered when the parent (*Index.razor*) is accessed in a browser:</span></span>

![Tarayıcıda işlenen iletişim bileşeni](index/_static/dialog.png)

<span data-ttu-id="99942-141">Uygulamasında, IntelliSense içinde bu bileşen kullanıldığında [Visual Studio](/visualstudio/ide/using-intellisense) ve [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) söz dizimi ve parametre tamamlama ile geliştirme sürecini hızlandırdı.</span><span class="sxs-lookup"><span data-stu-id="99942-141">When this component is used in the app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) and [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="99942-142">Bileşenleri bir bellek içi gösterimine, tarayıcının belge nesne modeli (adlı DOM) işleme bir *ağaç işlemek*, esnek ve verimli bir şekilde kullanıcı arabirimini güncelleştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="99942-142">Components render into an in-memory representation of the browser's Document Object Model (DOM) called a *render tree*, which is used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="99942-143">Blazor istemci tarafı</span><span class="sxs-lookup"><span data-stu-id="99942-143">Blazor client-side</span></span>

<span data-ttu-id="99942-144">Blazor istemci tarafı .NET ile etkileşimli istemci tarafı web uygulamaları oluşturmak için bir tek sayfalı uygulama çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="99942-144">Blazor client-side is a single-page app framework for building interactive client-side web apps with .NET.</span></span> <span data-ttu-id="99942-145">Blazor istemci-tarafı açık web standartları transpilation eklentileri veya kod olmadan kullanır ve mobil tarayıcılar dahil tüm modern web tarayıcılarında çalışır.</span><span class="sxs-lookup"><span data-stu-id="99942-145">Blazor client-side uses open web standards without plugins or code transpilation and works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="99942-146">Çalışan tarayıcılar içinde .NET kod tarafından yapılır [WebAssembly](https://webassembly.org) (kısaltılmış *wasm*).</span><span class="sxs-lookup"><span data-stu-id="99942-146">Running .NET code inside web browsers is made possible by [WebAssembly](https://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="99942-147">WebAssembly hızlı indirme ve maksimum yürütme hızı için iyileştirilmiş bir compact bayt biçimidir.</span><span class="sxs-lookup"><span data-stu-id="99942-147">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span> <span data-ttu-id="99942-148">WebAssembly standart ve desteklenen web tarayıcılarından eklentileri olmadan'de açık bir web API'sidir.</span><span class="sxs-lookup"><span data-stu-id="99942-148">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span>

<span data-ttu-id="99942-149">WebAssembly kod tarayıcısı adlı JavaScript aracılığıyla tam işlevselliğini erişebilir *JavaScript birlikte çalışabilirlik* (veya *JavaScript birlikte çalışma*).</span><span class="sxs-lookup"><span data-stu-id="99942-149">WebAssembly code can access the full functionality of the browser via JavaScript, called *JavaScript interoperability* (or *JavaScript interop*).</span></span> <span data-ttu-id="99942-150">.NET kodu WebAssembly tarayıcıda çalıştırılan tarayıcı JavaScript sandbox istemci makineye kötü amaçlı Eylemler karşı korumalı alan sağlayan korumaları ile çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="99942-150">.NET code executed via WebAssembly in the browser runs in the browser's JavaScript sandbox with the protections that the sandbox provides against malicious actions on the client machine.</span></span>

![Blazor istemci tarafı .NET kodu tarayıcıda WebAssembly ile çalışır.](index/_static/blazor-client-side.png)

<span data-ttu-id="99942-152">Ne zaman bir Blazor istemci-tarafı uygulaması oluşturulur ve bir tarayıcıda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="99942-152">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="99942-153">C#kod dosyaları ve Razor dosyaları .NET bütünleştirilmiş kodlarının derlenir.</span><span class="sxs-lookup"><span data-stu-id="99942-153">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="99942-154">Derlemeler ve .NET çalışma zamanı tarayıcıya indirilir.</span><span class="sxs-lookup"><span data-stu-id="99942-154">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="99942-155">Blazor istemci-tarafı bootstraps .NET çalışma zamanı ve çalışma zamanı derlemeleri uygulama yüklemek için yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="99942-155">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="99942-156">Blazor istemci tarafı çalışma zamanı JavaScript birlikte çalışma DOM düzenleme ve tarayıcı API çağrıları işlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="99942-156">The Blazor client-side runtime uses JavaScript interop to handle DOM manipulation and browser API calls.</span></span>

<span data-ttu-id="99942-157">Yayımlanan bir uygulamanın boyutunu kendi *yükü boyutu*, uygulamanın uygun kritik performans faktördür.</span><span class="sxs-lookup"><span data-stu-id="99942-157">The size of the published app, its *payload size*, is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="99942-158">Çok sayıda uygulama, kullanıcı deneyimini azalır bir tarayıcıya indirmek için oldukça uzun bir zaman alır.</span><span class="sxs-lookup"><span data-stu-id="99942-158">A large app takes a relatively long time to download to a browser, which diminishes the user experience.</span></span> <span data-ttu-id="99942-159">Blazor istemci tarafı yük boyutundaki yükleme sürelerini kısaltmak için en iyi duruma getirir:</span><span class="sxs-lookup"><span data-stu-id="99942-159">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="99942-160">Kullanılmayan kod tarafından yayımlandığında uygulama oturumunu çıkartılır [Ara dil (IL) bağlayıcı](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="99942-160">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>
* <span data-ttu-id="99942-161">HTTP yanıtlarını sıkıştırılır.</span><span class="sxs-lookup"><span data-stu-id="99942-161">HTTP responses are compressed.</span></span>
* <span data-ttu-id="99942-162">.NET çalışma zamanı ve derlemeleri tarayıcıda önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="99942-162">The .NET runtime and assemblies are cached in the browser.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="99942-163">Blazor sunucu tarafı</span><span class="sxs-lookup"><span data-stu-id="99942-163">Blazor server-side</span></span>

<span data-ttu-id="99942-164">Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bileşeni işleme mantığı Blazor ayırır.</span><span class="sxs-lookup"><span data-stu-id="99942-164">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="99942-165">Sunucu tarafı Blazor Razor bileşenleri ASP.NET Core uygulaması sunucusunda barındırmak için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="99942-165">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="99942-166">Kullanıcı Arabirimi güncelleştirmeleri üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.</span><span class="sxs-lookup"><span data-stu-id="99942-166">UI updates are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="99942-167">Çalışma zamanı kullanıcı Arabirimi olayları tarayıcıdan sunucuya gönderme işler ve bileşenleri çalıştırdıktan sonra tarayıcıya sunucu tarafından gönderilen kullanıcı Arabirimi güncelleştirmeleri uygular.</span><span class="sxs-lookup"><span data-stu-id="99942-167">The runtime handles sending UI events from the browser to the server and applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="99942-168">Tarayıcı ile iletişim kurmak için Blazor sunucu-tarafı tarafından kullanılan bağlantı JavaScript birlikte çalışma çağrıları işlemek için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="99942-168">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Blazor sunucu tarafı .NET kod sunucu üzerinde çalışır ve belge nesne modeli istemcide bir SignalR bağlantısı üzerinden etkileşim](index/_static/blazor-server-side.png)

## <a name="javascript-interop"></a><span data-ttu-id="99942-170">JavaScript ile birlikte çalışma</span><span class="sxs-lookup"><span data-stu-id="99942-170">JavaScript interop</span></span>

<span data-ttu-id="99942-171">Üçüncü taraf JavaScript kitaplıklarını ve API'lerini tarayıcıya erişim gerektiren uygulamaları için bileşenler JavaScript ile çalışma.</span><span class="sxs-lookup"><span data-stu-id="99942-171">For apps that require third-party JavaScript libraries and access to browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="99942-172">Herhangi bir kitaplığı veya JavaScript kullanabilmek için API kullanma yeteneği bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="99942-172">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="99942-173">C#kod, JavaScript kodu çağırabilir ve JavaScript kod içine çağırabilir C# kod.</span><span class="sxs-lookup"><span data-stu-id="99942-173">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="99942-174">Daha fazla bilgi için bkz. <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="99942-174">For more information, see <xref:blazor/javascript-interop>.</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="99942-175">Kod paylaşımı ve .NET Standard</span><span class="sxs-lookup"><span data-stu-id="99942-175">Code sharing and .NET Standard</span></span>

<span data-ttu-id="99942-176">Blazor uygulayan [.NET Standard 2.0](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="99942-176">Blazor implements [.NET Standard 2.0](/dotnet/standard/net-standard).</span></span> <span data-ttu-id="99942-177">.NET standart, .NET API'leri, .NET uygulamaları arasında ortak olan bir resmi belirtimi olan.</span><span class="sxs-lookup"><span data-stu-id="99942-177">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="99942-178">.NET standart sınıf kitaplıkları Blazor, .NET Framework, .NET Core, Xamarin, Mono ve Unity gibi farklı .NET platformlar arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="99942-178">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

<span data-ttu-id="99942-179">Bir web tarayıcısı içinde (örneğin, dosya sistemine erişen bir yuva açma ve iş parçacığı) uygun olmayan API'leri throw bir <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="99942-179">APIs that aren't applicable inside of a web browser (for example, accessing the file system, opening a socket, and threading) throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="99942-180">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="99942-180">Additional resources</span></span>

* [<span data-ttu-id="99942-181">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="99942-181">WebAssembly</span></span>](https://webassembly.org/)
* <xref:blazor/hosting-models>
* [<span data-ttu-id="99942-182">C# Kılavuzu</span><span class="sxs-lookup"><span data-stu-id="99942-182">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="99942-183">HTML</span><span class="sxs-lookup"><span data-stu-id="99942-183">HTML</span></span>](https://www.w3.org/html/)
