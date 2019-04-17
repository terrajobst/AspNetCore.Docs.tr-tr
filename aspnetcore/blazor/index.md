---
title: ASP.NET core'da Blazor giriş
author: guardrex
description: ASP.NET Core Blazor, etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET, ASP.NET Core uygulaması oluşturmak için bir yöntem keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/15/2019
uid: blazor/index
ms.openlocfilehash: a5b12a5c5c10a74ab192d0d67a2ba9a5617232c7
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614918"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="da439-103">Blazor giriş</span><span class="sxs-lookup"><span data-stu-id="da439-103">Introduction to Blazor</span></span>

<span data-ttu-id="da439-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="da439-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="razor-components"></a><span data-ttu-id="da439-105">Razor Bileşenleri</span><span class="sxs-lookup"><span data-stu-id="da439-105">Razor Components</span></span>

<span data-ttu-id="da439-106">Blazor, sunucu tarafı barındırma modelini *Razor bileşenleri*, etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET derleme yolu:</span><span class="sxs-lookup"><span data-stu-id="da439-106">The server-side hosting model of Blazor, *Razor Components*, is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="da439-107">Kullanarak zengin etkileşimli kullanıcı arabirimleri oluşturun C# JavaScript yerine.</span><span class="sxs-lookup"><span data-stu-id="da439-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="da439-108">.NET ile yazılan tüm sunucu tarafı ve istemci tarafı uygulama mantığı paylaşın.</span><span class="sxs-lookup"><span data-stu-id="da439-108">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="da439-109">HTML ve CSS olarak UI mobil tarayıcılar dahil olmak üzere geniş tarayıcı desteği işleyin.</span><span class="sxs-lookup"><span data-stu-id="da439-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="da439-110">Razor bileşenleri dahil olmak üzere çoğu uygulama tarafından gereken çekirdek özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="da439-110">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="da439-111">Parametreler</span><span class="sxs-lookup"><span data-stu-id="da439-111">Parameters</span></span>
* <span data-ttu-id="da439-112">Olay işleme</span><span class="sxs-lookup"><span data-stu-id="da439-112">Event handling</span></span>
* <span data-ttu-id="da439-113">Veri bağlama</span><span class="sxs-lookup"><span data-stu-id="da439-113">Data binding</span></span>
* <span data-ttu-id="da439-114">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="da439-114">Routing</span></span>
* <span data-ttu-id="da439-115">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="da439-115">Dependency injection</span></span>
* <span data-ttu-id="da439-116">Düzenler</span><span class="sxs-lookup"><span data-stu-id="da439-116">Layouts</span></span>
* <span data-ttu-id="da439-117">Şablon oluşturma</span><span class="sxs-lookup"><span data-stu-id="da439-117">Templating</span></span>
* <span data-ttu-id="da439-118">Basamaklı değerler</span><span class="sxs-lookup"><span data-stu-id="da439-118">Cascading values</span></span>

<span data-ttu-id="da439-119">Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bileşeni işleme mantığı Razor bileşeni birbirinden ayırır.</span><span class="sxs-lookup"><span data-stu-id="da439-119">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="da439-120">ASP.NET Core Razor bileşenleri .NET Core 3. 0'ı ASP.NET Core uygulaması sunucusunda Razor bileşenlerini barındırma desteği ekler.</span><span class="sxs-lookup"><span data-stu-id="da439-120">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="da439-121">Kullanıcı Arabirimi güncelleştirmeleri SignalR bağlantısı üzerinden işlenir.</span><span class="sxs-lookup"><span data-stu-id="da439-121">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="da439-122">Çalışma zamanı:</span><span class="sxs-lookup"><span data-stu-id="da439-122">The runtime:</span></span>

* <span data-ttu-id="da439-123">UI olayları tarayıcıdan sunucuya gönderme işler.</span><span class="sxs-lookup"><span data-stu-id="da439-123">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="da439-124">Geçerli kullanıcı Arabirimi güncelleştirmeleri sunucu tarafından gönderilen bileşenleri çalıştırdıktan sonra tarayıcıya geri.</span><span class="sxs-lookup"><span data-stu-id="da439-124">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="da439-125">Tarayıcı ile iletişim kurmak için Razor bileşenleri tarafından kullanılan bağlantı JavaScript birlikte çalışma çağrıları işlemek için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="da439-125">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Razor bileşenleri .NET kod sunucu üzerinde çalışır ve belge nesne modeli istemcide bir SignalR bağlantısı üzerinden etkileşim](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="da439-127">Daha fazla bilgi için bkz. <xref:blazor/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="da439-127">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="da439-128">Bileşenler</span><span class="sxs-lookup"><span data-stu-id="da439-128">Components</span></span>

<span data-ttu-id="da439-129">A *Razor bileşen* bir kullanıcı Arabirimi, bir sayfa, iletişim kutusu veya veri girişi formuna gibi parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="da439-129">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="da439-130">Bileşenleri kullanıcı olayları işlemek ve esnek UI işleme mantığı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="da439-130">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="da439-131">Bileşenleri iç içe geçmiş ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="da439-131">Components can be nested and reused.</span></span>

<span data-ttu-id="da439-132">.NET sınıf paylaşılabilir ve NuGet paketleri olarak dağıtılmış .NET derlemelerini yerleşik bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="da439-132">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="da439-133">Sınıf normalde bir Razor biçimlendirme sayfayla biçiminde yazılmış bir *.razor* dosya uzantısı (Razor bileşenleri) veya Razor biçimlendirme sayfasıyla bir *.cshtml* dosya uzantısı (Blazor).</span><span class="sxs-lookup"><span data-stu-id="da439-133">The class is normally written in the form of a Razor markup page with a *.razor* file extension (Razor Components) or Razor markup page with a *.cshtml* file extension (Blazor).</span></span>

<span data-ttu-id="da439-134">[Razor](xref:mvc/views/razor) HTML biçimlendirmesi ile birleştiren bir söz dizimi C# kod.</span><span class="sxs-lookup"><span data-stu-id="da439-134">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="da439-135">Razor Geliştirici üretkenliği, biçimlendirme arasında geçiş yapmak Geliştirici izin vermek için tasarlanmıştır ve C# ile aynı dosyada [IntelliSense](/visualstudio/ide/using-intellisense) destekler.</span><span class="sxs-lookup"><span data-stu-id="da439-135">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="da439-136">Razor sayfaları ve MVC görünümleri de Razor kullanın.</span><span class="sxs-lookup"><span data-stu-id="da439-136">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="da439-137">Razor sayfaları ve istek/yanıt modeli çevresinde kurulan MVC görünümleri, bileşenleri özellikle kullanıcı Arabirimi oluşturma işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="da439-137">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="da439-138">Razor bileşenleri, özellikle istemci tarafı UI mantığı ve birleştirme için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="da439-138">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="da439-139">Aşağıdaki biçimlendirmede özel iletişim bileşen örneğidir:</span><span class="sxs-lookup"><span data-stu-id="da439-139">The following markup is an example of a custom dialog component:</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="da439-140">Bu bileşen, uygulamada başka bir yerde kullanıldığında, IntelliSense tamamlama söz dizimi ve parametre ile geliştirme hızlandırır.</span><span class="sxs-lookup"><span data-stu-id="da439-140">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="da439-141">Bileşenleri oluşturma DOM adlı tarayıcı bellek içi gösterimine bir *ağaç işleme* , ardından UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="da439-141">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="da439-142">JavaScript ile birlikte çalışma</span><span class="sxs-lookup"><span data-stu-id="da439-142">JavaScript interop</span></span>

<span data-ttu-id="da439-143">Üçüncü taraf JavaScript kitaplıklarını ve API'lerini tarayıcı gerektiren uygulamalar için bileşenleri JavaScript ile çalışma.</span><span class="sxs-lookup"><span data-stu-id="da439-143">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="da439-144">Herhangi bir kitaplığı veya JavaScript kullanabilmek için API kullanma yeteneği bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="da439-144">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="da439-145">C#kod, JavaScript kodu çağırabilir ve JavaScript kod içine çağırabilir C# kod.</span><span class="sxs-lookup"><span data-stu-id="da439-145">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="da439-146">Daha fazla bilgi için [JavaScript birlikte çalışma](xref:blazor/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="da439-146">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="da439-147">Kod paylaşımı ve .NET Standard</span><span class="sxs-lookup"><span data-stu-id="da439-147">Code sharing and .NET Standard</span></span>

<span data-ttu-id="da439-148">Uygulamaları başvurmak ve mevcut olanı kullan [.NET Standard](/dotnet/standard/net-standard) kitaplıkları.</span><span class="sxs-lookup"><span data-stu-id="da439-148">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="da439-149">.NET standart, .NET API'leri, .NET uygulamaları arasında ortak olan bir resmi belirtimi olan.</span><span class="sxs-lookup"><span data-stu-id="da439-149">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="da439-150">.NET Standard 2.0 Blazor uygular.</span><span class="sxs-lookup"><span data-stu-id="da439-150">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="da439-151">Bir web tarayıcısı içinde (örneğin, dosya sistemine erişen bir yuva açma, iş parçacığı oluşturma ve diğer özellikleri) uygun olmayan API'leri throw <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="da439-151">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="da439-152">.NET standart sınıf kitaplıkları Blazor, .NET Framework, .NET Core, Xamarin, Mono ve Unity gibi farklı .NET platformlar arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="da439-152">.NET Standard class libraries can be shared across different .NET platforms, like Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="blazor"></a><span data-ttu-id="da439-153">Blazor</span><span class="sxs-lookup"><span data-stu-id="da439-153">Blazor</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="da439-154">Blazor .NET ile etkileşimli istemci tarafı Web uygulamaları oluşturmaya yönelik bir tek sayfalı uygulama çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="da439-154">Blazor is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="da439-155">Açık web standartları transpilation eklentileri veya kod olmadan Blazor kullanır.</span><span class="sxs-lookup"><span data-stu-id="da439-155">Blazor uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="da439-156">Mobil tarayıcılar dahil tüm modern web tarayıcılarında Blazor çalışır.</span><span class="sxs-lookup"><span data-stu-id="da439-156">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="da439-157">.NET istemci tarafı web geliştirme için tarayıcıda kullanarak birçok avantaj sunar:</span><span class="sxs-lookup"><span data-stu-id="da439-157">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="da439-158">**C#Dil**: Kod yazmaya C# JavaScript yerine.</span><span class="sxs-lookup"><span data-stu-id="da439-158">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="da439-159">**.NET ekosisteminin**: .NET kitaplıkları mevcut ekosistemi yararlanın.</span><span class="sxs-lookup"><span data-stu-id="da439-159">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="da439-160">**Tam yığın geliştirme**: Sunucu ve istemci tarafı mantığını paylaşın.</span><span class="sxs-lookup"><span data-stu-id="da439-160">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="da439-161">**Hız ve ölçeklenebilirlik**: .NET, performans, güvenilirlik ve güvenlik derlendiği.</span><span class="sxs-lookup"><span data-stu-id="da439-161">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="da439-162">**Sektör lideri Araçları**: Windows, Linux ve macOS üzerinde Visual Studio üretken kalın.</span><span class="sxs-lookup"><span data-stu-id="da439-162">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="da439-163">**Kararlılık ve tutarlılık**:  Bir commonset dillerin, çerçevelerin ve tutarlı, zengin ve kullanımı kolay Araçlar'ın üzerinde oluşturun.</span><span class="sxs-lookup"><span data-stu-id="da439-163">**Stability and consistency**:  Build on a commonset of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="da439-164">Çalışan tarayıcılar içinde .NET kod tarafından yapılır [WebAssembly](http://webassembly.org) (kısaltılmış *wasm*).</span><span class="sxs-lookup"><span data-stu-id="da439-164">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="da439-165">WebAssembly standart ve desteklenen web tarayıcılarından eklentileri olmadan'de açık bir web API'sidir.</span><span class="sxs-lookup"><span data-stu-id="da439-165">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="da439-166">WebAssembly hızlı indirme ve maksimum yürütme hızı için iyileştirilmiş bir compact bayt biçimidir.</span><span class="sxs-lookup"><span data-stu-id="da439-166">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="da439-167">WebAssembly kod tarayıcı yoluyla JavaScript birlikte çalışma'nin tam işlevselliği erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da439-167">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="da439-168">Aynı zamanda, kötü amaçlı Eylemler istemci makinede önlemek için JavaScript aynı güvenilir sanal WebAssembly yürütülen .NET kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="da439-168">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![.NET kodu Blazor tarayıcı WebAssembly ile çalışır.](index/_static/blazor.png)

<span data-ttu-id="da439-170">Ne zaman Blazor uygulama oluşturulur ve bir tarayıcıda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="da439-170">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="da439-171">C#kod dosyaları ve Razor dosyaları .NET bütünleştirilmiş kodlarının derlenir.</span><span class="sxs-lookup"><span data-stu-id="da439-171">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="da439-172">Derlemeler ve .NET çalışma zamanı tarayıcıya indirilir.</span><span class="sxs-lookup"><span data-stu-id="da439-172">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="da439-173">Blazor bootstraps .NET çalışma zamanı ve çalışma zamanı derlemeleri uygulama yüklemek için yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="da439-173">Blazor bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="da439-174">Belge nesne modeli (DOM) işleme ve tarayıcı API çağrıları, JavaScript birlikte çalışma aracılığıyla Blazor çalışma zamanı tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="da439-174">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="da439-175">Çekirdek özellikleri dahil olmak üzere çoğu uygulama tarafından gerekli Blazor destekler:</span><span class="sxs-lookup"><span data-stu-id="da439-175">Blazor supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="da439-176">Parametreler</span><span class="sxs-lookup"><span data-stu-id="da439-176">Parameters</span></span>
* <span data-ttu-id="da439-177">Olay işleme</span><span class="sxs-lookup"><span data-stu-id="da439-177">Event handling</span></span>
* <span data-ttu-id="da439-178">Veri bağlama</span><span class="sxs-lookup"><span data-stu-id="da439-178">Data binding</span></span>
* <span data-ttu-id="da439-179">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="da439-179">Routing</span></span>
* <span data-ttu-id="da439-180">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="da439-180">Dependency injection</span></span>
* <span data-ttu-id="da439-181">Düzenler</span><span class="sxs-lookup"><span data-stu-id="da439-181">Layouts</span></span>
* <span data-ttu-id="da439-182">Şablon oluşturma</span><span class="sxs-lookup"><span data-stu-id="da439-182">Templating</span></span>
* <span data-ttu-id="da439-183">Basamaklı değerler</span><span class="sxs-lookup"><span data-stu-id="da439-183">Cascading values</span></span>

<span data-ttu-id="da439-184">Tarafından yayımlandığında indirilen uygulama boyutunu azaltmak için kullanılmayan kod uygulama oturumunu yapılandırıldıktan [Ara dil (IL) bağlayıcı](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="da439-184">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="da439-185">İstemci tarafı Blazor bir istemci-tarafı barındırma modelidir.</span><span class="sxs-lookup"><span data-stu-id="da439-185">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="da439-186">Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bir bileşenin işleme mantığı Blazor ayırır olduğundan esneklik nasıl Blazor barındırılabilir içinde.</span><span class="sxs-lookup"><span data-stu-id="da439-186">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="da439-187">ASP.NET Core kullanılmalı [Razor bileşenleri](#razor-components) Razor bileşenleri konakta sunucu kullanıcı Arabirimi güncelleştirmeleri SignalR bağlantısı üzerinden işlenmesini burada ASP.NET Core uygulaması için.</span><span class="sxs-lookup"><span data-stu-id="da439-187">Use ASP.NET Core [Razor Components](#razor-components) to host Razor Components on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="da439-188">Daha fazla bilgi için bkz. <xref:blazor/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="da439-188">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span> 

<span data-ttu-id="da439-189">Yükü boyutu, bir uygulamanın uygun için önemli performans faktördür.</span><span class="sxs-lookup"><span data-stu-id="da439-189">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="da439-190">Blazor yükü boyutu yükleme sürelerini kısaltmak için en iyi duruma getirir:</span><span class="sxs-lookup"><span data-stu-id="da439-190">Blazor optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="da439-191">.NET derlemeleri kullanılmayan bölümlerini derleme işlemi sırasında kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="da439-191">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="da439-192">HTTP yanıtlarını sıkıştırılır.</span><span class="sxs-lookup"><span data-stu-id="da439-192">HTTP responses are compressed.</span></span>
* <span data-ttu-id="da439-193">.NET çalışma zamanı ve derlemeleri tarayıcıda önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="da439-193">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="da439-194">[Razor bileşenleri](#razor-components) .NET derlemeleri, uygulama derleme ve çalışma zamanı sunucu tarafı Blazor değerinden daha küçük bir yükü boyutu sağlar.</span><span class="sxs-lookup"><span data-stu-id="da439-194">[Razor Components](#razor-components) provides a smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="da439-195">Razor bileşenleri uygulamalar yalnızca işaretleme dosyaları ve statik varlıklar istemcilere hizmet.</span><span class="sxs-lookup"><span data-stu-id="da439-195">Razor Components apps only serve markup files and static assets to clients.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="da439-196">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="da439-196">Additional resources</span></span>

* [<span data-ttu-id="da439-197">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="da439-197">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="da439-198">C# Kılavuzu</span><span class="sxs-lookup"><span data-stu-id="da439-198">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="da439-199">HTML</span><span class="sxs-lookup"><span data-stu-id="da439-199">HTML</span></span>](https://www.w3.org/html/)
