---
title: ASP.NET core'da Blazor giriş
author: guardrex
description: ASP.NET Core Blazor, etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET, ASP.NET Core uygulaması oluşturmak için bir yöntem keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983003"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="b5f1c-103">Blazor giriş</span><span class="sxs-lookup"><span data-stu-id="b5f1c-103">Introduction to Blazor</span></span>

<span data-ttu-id="b5f1c-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b5f1c-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b5f1c-105">Blazor hoşgeldiniz!</span><span class="sxs-lookup"><span data-stu-id="b5f1c-105">Welcome to Blazor!</span></span>

<span data-ttu-id="b5f1c-106">Etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET derleme:</span><span class="sxs-lookup"><span data-stu-id="b5f1c-106">Build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="b5f1c-107">Kullanarak zengin etkileşimli kullanıcı arabirimleri oluşturun C# JavaScript yerine.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="b5f1c-108">.NET ile yazılmış sunucu tarafı ve istemci tarafı uygulama mantığı paylaşın.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-108">Share server-side and client-side app logic written with .NET.</span></span>
* <span data-ttu-id="b5f1c-109">HTML ve CSS olarak UI mobil tarayıcılar dahil olmak üzere geniş tarayıcı desteği işleyin.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="b5f1c-110">Blazor çoğu uygulamaların gerektirdiği temel senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="b5f1c-110">Blazor supports core scenarios required by most apps:</span></span>

* <span data-ttu-id="b5f1c-111">Parametreler</span><span class="sxs-lookup"><span data-stu-id="b5f1c-111">Parameters</span></span>
* <span data-ttu-id="b5f1c-112">Olay işleme</span><span class="sxs-lookup"><span data-stu-id="b5f1c-112">Event handling</span></span>
* <span data-ttu-id="b5f1c-113">Veri bağlama</span><span class="sxs-lookup"><span data-stu-id="b5f1c-113">Data binding</span></span>
* <span data-ttu-id="b5f1c-114">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="b5f1c-114">Routing</span></span>
* <span data-ttu-id="b5f1c-115">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="b5f1c-115">Dependency injection</span></span>
* <span data-ttu-id="b5f1c-116">Düzenler</span><span class="sxs-lookup"><span data-stu-id="b5f1c-116">Layouts</span></span>
* <span data-ttu-id="b5f1c-117">Şablonlar</span><span class="sxs-lookup"><span data-stu-id="b5f1c-117">Templates</span></span>
* <span data-ttu-id="b5f1c-118">Basamaklı değerler</span><span class="sxs-lookup"><span data-stu-id="b5f1c-118">Cascading values</span></span>

## <a name="components"></a><span data-ttu-id="b5f1c-119">Bileşenler</span><span class="sxs-lookup"><span data-stu-id="b5f1c-119">Components</span></span>

<span data-ttu-id="b5f1c-120">A *bileşen* Blazor içinde bir kullanıcı Arabirimi, bir sayfa, iletişim kutusu veya veri girişi formuna gibi öğesidir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-120">A *component* in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="b5f1c-121">Bileşenleri kullanıcı olayları işlemek ve esnek UI işleme mantığı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-121">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="b5f1c-122">Bileşenleri iç içe geçmiş ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-122">Components can be nested and reused.</span></span>

<span data-ttu-id="b5f1c-123">.NET sınıf paylaşılabilir ve NuGet paketleri olarak dağıtılmış .NET derlemelerini yerleşik bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-123">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="b5f1c-124">Bileşen sınıfı genellikle bir Razor biçimlendirme sayfayla biçiminde yazılmış bir *.razor* dosya uzantısı.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-124">The component class is usually written in the form of a Razor markup page with a *.razor* file extension.</span></span> <span data-ttu-id="b5f1c-125">Blazor bileşenlerde Razor bileşenleri olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-125">Components in Blazor are sometimes referred to as Razor components.</span></span>

<span data-ttu-id="b5f1c-126">[Razor](xref:mvc/views/razor) HTML biçimlendirmesi ile birleştiren bir söz dizimi C# kod.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-126">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="b5f1c-127">Razor Geliştirici üretkenliği, biçimlendirme arasında geçiş yapmak Geliştirici izin vermek için tasarlanmıştır ve C# ile aynı dosyada [IntelliSense](/visualstudio/ide/using-intellisense) destekler.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-127">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="b5f1c-128">Razor sayfaları ve MVC görünümleri de Razor kullanın.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-128">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="b5f1c-129">Razor sayfaları ve istek/yanıt modeli çevresinde kurulan MVC görünümleri, bileşenleri özellikle kullanıcı Arabirimi oluşturma işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-129">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="b5f1c-130">Razor bileşenleri, özellikle istemci tarafı UI mantığı ve birleştirme için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-130">Razor components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="b5f1c-131">Aşağıdaki biçimlendirmede özel iletişim bileşen örneğidir:</span><span class="sxs-lookup"><span data-stu-id="b5f1c-131">The following markup is an example of a custom dialog component:</span></span>

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

<span data-ttu-id="b5f1c-132">Bu bileşen kullanıldığında başka bir uygulamada, Intellisense'te [Visual Studio](https://visualstudio.microsoft.com/vs/) söz dizimi ve parametre tamamlama ile geliştirme sürecini hızlandırdı.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-132">When this component is used elsewhere in the app, IntelliSense in [Visual Studio](https://visualstudio.microsoft.com/vs/) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="b5f1c-133">Bileşenleri oluşturma DOM adlı tarayıcı bellek içi gösterimine bir *ağaç işleme* , ardından UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-133">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="b5f1c-134">Blazor sunucu tarafı</span><span class="sxs-lookup"><span data-stu-id="b5f1c-134">Blazor server-side</span></span>

<span data-ttu-id="b5f1c-135">Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bileşeni işleme mantığı Blazor ayırır.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-135">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="b5f1c-136">Sunucu tarafı Blazor Razor bileşenleri ASP.NET Core uygulaması sunucusunda barındırmak için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-136">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="b5f1c-137">Kullanıcı Arabirimi güncelleştirmeleri SignalR bağlantısı üzerinden işlenir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-137">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="b5f1c-138">Çalışma zamanı:</span><span class="sxs-lookup"><span data-stu-id="b5f1c-138">The runtime:</span></span>

* <span data-ttu-id="b5f1c-139">UI olayları tarayıcıdan sunucuya gönderme işler.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-139">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="b5f1c-140">Geçerli kullanıcı Arabirimi güncelleştirmeleri sunucu tarafından gönderilen bileşenleri çalıştırdıktan sonra tarayıcıya geri.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-140">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="b5f1c-141">Tarayıcı ile iletişim kurmak için Blazor sunucu-tarafı tarafından kullanılan bağlantı JavaScript birlikte çalışma çağrıları işlemek için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-141">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Blazor sunucu tarafı .NET kod sunucu üzerinde çalışır ve belge nesne modeli istemcide bir SignalR bağlantısı üzerinden etkileşim](index/_static/blazor-server-side.png)

<span data-ttu-id="b5f1c-143">Daha fazla bilgi için bkz. <xref:blazor/hosting-models#server-side>.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-143">For more information, see <xref:blazor/hosting-models#server-side>.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="b5f1c-144">Blazor istemci tarafı</span><span class="sxs-lookup"><span data-stu-id="b5f1c-144">Blazor client-side</span></span>

<span data-ttu-id="b5f1c-145">Blazor istemci tarafı .NET ile etkileşimli istemci tarafı Web uygulamaları oluşturmak için bir tek sayfalı uygulama çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-145">Blazor client-side is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="b5f1c-146">Blazor istemci-tarafı eklentileri veya kod transpilation olmadan açık web standartları kullanır.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-146">Blazor client-side uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="b5f1c-147">Mobil tarayıcılar dahil tüm modern web tarayıcılarında Blazor istemci-tarafı çalışır.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-147">Blazor client-side works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="b5f1c-148">.NET istemci tarafı web geliştirme için tarayıcıda kullanarak birçok avantaj sunar:</span><span class="sxs-lookup"><span data-stu-id="b5f1c-148">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="b5f1c-149">**C#Dil**: Kod yazmaya C# JavaScript yerine.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-149">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="b5f1c-150">**.NET ekosisteminin**: .NET kitaplıkları mevcut ekosistemi yararlanın.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-150">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="b5f1c-151">**Tam yığın geliştirme**: Sunucu ve istemci tarafı mantığını paylaşın.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-151">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="b5f1c-152">**Hız ve ölçeklenebilirlik**: .NET, performans, güvenilirlik ve güvenlik derlendiği.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-152">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="b5f1c-153">**Sektör lideri Araçları**: Windows, Linux ve macOS üzerinde Visual Studio üretken kalın.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-153">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="b5f1c-154">**Kararlılık ve tutarlılık**:  Dillerin, çerçevelerin ve tutarlı, zengin ve kullanımı kolay araçlar ortak kümesine göre oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-154">**Stability and consistency**:  Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="b5f1c-155">Çalışan tarayıcılar içinde .NET kod tarafından yapılır [WebAssembly](http://webassembly.org) (kısaltılmış *wasm*).</span><span class="sxs-lookup"><span data-stu-id="b5f1c-155">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="b5f1c-156">WebAssembly standart ve desteklenen web tarayıcılarından eklentileri olmadan'de açık bir web API'sidir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-156">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="b5f1c-157">WebAssembly hızlı indirme ve maksimum yürütme hızı için iyileştirilmiş bir compact bayt biçimidir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-157">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="b5f1c-158">WebAssembly kod tarayıcı yoluyla JavaScript birlikte çalışma'nin tam işlevselliği erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-158">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="b5f1c-159">Aynı zamanda, kötü amaçlı Eylemler istemci makinede önlemek için JavaScript aynı güvenilir sanal WebAssembly yürütülen .NET kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-159">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor istemci tarafı .NET kodu tarayıcıda WebAssembly ile çalışır.](index/_static/blazor-client-side.png)

<span data-ttu-id="b5f1c-161">Ne zaman bir Blazor istemci-tarafı uygulaması oluşturulur ve bir tarayıcıda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b5f1c-161">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="b5f1c-162">C#kod dosyaları ve Razor dosyaları .NET bütünleştirilmiş kodlarının derlenir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-162">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="b5f1c-163">Derlemeler ve .NET çalışma zamanı tarayıcıya indirilir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-163">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="b5f1c-164">Blazor istemci-tarafı bootstraps .NET çalışma zamanı ve çalışma zamanı derlemeleri uygulama yüklemek için yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-164">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="b5f1c-165">Belge nesne modeli (DOM) işleme ve tarayıcı API çağrıları, JavaScript birlikte çalışma aracılığıyla Blazor istemci tarafı çalışma zamanı tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-165">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor client-side runtime via JavaScript interop.</span></span>

<span data-ttu-id="b5f1c-166">Tarafından yayımlandığında indirilen uygulama boyutunu azaltmak için kullanılmayan kod uygulama oturumunu yapılandırıldıktan [Ara dil (IL) bağlayıcı](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="b5f1c-166">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="b5f1c-167">İstemci tarafı Blazor bir istemci-tarafı barındırma modelidir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-167">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="b5f1c-168">Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bir bileşenin işleme mantığı Blazor ayırır olduğundan esneklik nasıl Blazor barındırılabilir içinde.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-168">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="b5f1c-169">Kullanım [Blazor sunucu tarafı](#blazor-server-side) konağa Blazor kullanıcı Arabirimi güncelleştirmeleri SignalR bağlantısı üzerinden işlenmesini burada ASP.NET Core uygulaması sunucusunda.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-169">Use [Blazor server-side](#blazor-server-side) to host Blazor on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="b5f1c-170">Daha fazla bilgi için bkz. <xref:blazor/hosting-models#server-side>.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-170">For more information, see <xref:blazor/hosting-models#server-side>.</span></span> 

<span data-ttu-id="b5f1c-171">Yükü boyutu, bir uygulamanın uygun için önemli performans faktördür.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-171">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="b5f1c-172">Blazor istemci tarafı yük boyutundaki yükleme sürelerini kısaltmak için en iyi duruma getirir:</span><span class="sxs-lookup"><span data-stu-id="b5f1c-172">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="b5f1c-173">.NET derlemeleri kullanılmayan bölümlerini derleme işlemi sırasında kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-173">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="b5f1c-174">HTTP yanıtlarını sıkıştırılır.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-174">HTTP responses are compressed.</span></span>
* <span data-ttu-id="b5f1c-175">.NET çalışma zamanı ve derlemeleri tarayıcıda önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-175">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="b5f1c-176">[Sunucu tarafı Blazor](#blazor-server-side) daha küçük bir yük boyutundan Blazor istemci tarafı .NET derlemeleri, uygulama derleme ve çalışma zamanı sunucu tarafı sağlar.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-176">[Blazor server-side](#blazor-server-side) provides a smaller payload size than Blazor client-side by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="b5f1c-177">Blazor sunucu tarafı uygulamalar yalnızca işaretleme dosyaları ve statik varlıklar istemcilere hizmet.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-177">Blazor server-side apps only serve markup files and static assets to clients.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="b5f1c-178">JavaScript ile birlikte çalışma</span><span class="sxs-lookup"><span data-stu-id="b5f1c-178">JavaScript interop</span></span>

<span data-ttu-id="b5f1c-179">Üçüncü taraf JavaScript kitaplıklarını ve API'lerini tarayıcı gerektiren uygulamalar için bileşenleri JavaScript ile çalışma.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-179">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="b5f1c-180">Herhangi bir kitaplığı veya JavaScript kullanabilmek için API kullanma yeteneği bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-180">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="b5f1c-181">C#kod, JavaScript kodu çağırabilir ve JavaScript kod içine çağırabilir C# kod.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-181">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="b5f1c-182">Daha fazla bilgi için [JavaScript birlikte çalışma](xref:blazor/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="b5f1c-182">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="b5f1c-183">Kod paylaşımı ve .NET Standard</span><span class="sxs-lookup"><span data-stu-id="b5f1c-183">Code sharing and .NET Standard</span></span>

<span data-ttu-id="b5f1c-184">Uygulamaları başvurmak ve mevcut olanı kullan [.NET Standard](/dotnet/standard/net-standard) kitaplıkları.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-184">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="b5f1c-185">.NET standart, .NET API'leri, .NET uygulamaları arasında ortak olan bir resmi belirtimi olan.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-185">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="b5f1c-186">.NET Standard 2.0 Blazor uygular.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-186">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="b5f1c-187">Bir web tarayıcısı içinde (örneğin, dosya sistemine erişen bir yuva açma, iş parçacığı oluşturma ve diğer özellikleri) uygun olmayan API'leri throw <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-187">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="b5f1c-188">.NET standart sınıf kitaplıkları Blazor, .NET Framework, .NET Core, Xamarin, Mono ve Unity gibi farklı .NET platformlar arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="b5f1c-188">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5f1c-189">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b5f1c-189">Additional resources</span></span>

* [<span data-ttu-id="b5f1c-190">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="b5f1c-190">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="b5f1c-191">C# Kılavuzu</span><span class="sxs-lookup"><span data-stu-id="b5f1c-191">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="b5f1c-192">HTML</span><span class="sxs-lookup"><span data-stu-id="b5f1c-192">HTML</span></span>](https://www.w3.org/html/)
