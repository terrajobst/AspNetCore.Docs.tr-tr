---
title: Blazor giriş
author: guardrex
description: 'ASP.NET Core Blazor, tarayıcı WebAssembly ile çalışan etkileşimli istemci tarafı .NET ile uygulama oluşturmak için yeni bir yöntem keşfedin.'
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: spa/blazor/index
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="9e9d3-103">Blazor giriş</span><span class="sxs-lookup"><span data-stu-id="9e9d3-103">Introduction to Blazor</span></span>

<span data-ttu-id="9e9d3-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9e9d3-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="9e9d3-105">Blazor .NET ile etkileşimli istemci tarafı Web uygulamaları oluşturmaya yönelik bir tek sayfalı uygulama çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-105">Blazor is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="9e9d3-106">Açık web standartları transpilation eklentileri veya kod olmadan Blazor kullanır.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-106">Blazor uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="9e9d3-107">Mobil tarayıcılar dahil tüm modern web tarayıcılarında Blazor çalışır.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-107">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="9e9d3-108">.NET istemci tarafı web geliştirme için tarayıcıda kullanarak birçok avantaj sunar:</span><span class="sxs-lookup"><span data-stu-id="9e9d3-108">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="9e9d3-109">**C#Dil**: Kod yazmaya C# JavaScript yerine.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-109">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="9e9d3-110">**.NET ekosisteminin**: .NET kitaplıkları mevcut ekosistemi yararlanın.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-110">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="9e9d3-111">**Tam yığın geliştirme**: Sunucu ve istemci tarafı mantığını paylaşın.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-111">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="9e9d3-112">**Hız ve ölçeklenebilirlik**: .NET, performans, güvenilirlik ve güvenlik derlendiği.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-112">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="9e9d3-113">**Sektör lideri Araçları**: Windows, Linux ve macOS üzerinde Visual Studio üretken kalın.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-113">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="9e9d3-114">**Kararlılık ve tutarlılık**:  Bir commonset dillerin, çerçevelerin ve tutarlı, zengin ve kullanımı kolay Araçlar'ın üzerinde oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-114">**Stability and consistency**:  Build on a commonset of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="9e9d3-115">Çalışan tarayıcılar içinde .NET kod tarafından yapılır [WebAssembly](http://webassembly.org) (kısaltılmış *wasm*).</span><span class="sxs-lookup"><span data-stu-id="9e9d3-115">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="9e9d3-116">WebAssembly standart ve desteklenen web tarayıcılarından eklentileri olmadan'de açık bir web API'sidir.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-116">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="9e9d3-117">WebAssembly hızlı indirme ve maksimum yürütme hızı için iyileştirilmiş bir compact bayt biçimidir.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-117">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="9e9d3-118">WebAssembly kod tarayıcı yoluyla JavaScript birlikte çalışma'nin tam işlevselliği erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-118">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="9e9d3-119">Aynı zamanda, kötü amaçlı Eylemler istemci makinede önlemek için JavaScript aynı güvenilir sanal WebAssembly yürütülen .NET kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-119">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![.NET kodu Blazor tarayıcı WebAssembly ile çalışır.](index/_static/blazor.png)

<span data-ttu-id="9e9d3-121">Ne zaman Blazor uygulama oluşturulur ve bir tarayıcıda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9e9d3-121">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="9e9d3-122">C#kod dosyaları ve Razor dosyaları .NET bütünleştirilmiş kodlarının derlenir.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-122">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="9e9d3-123">Derlemeler ve .NET çalışma zamanı tarayıcıya indirilir.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-123">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="9e9d3-124">Blazor bootstraps .NET çalışma zamanı ve çalışma zamanı derlemeleri uygulama yüklemek için yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-124">Blazor bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="9e9d3-125">Belge nesne modeli (DOM) işleme ve tarayıcı API çağrıları, JavaScript birlikte çalışma aracılığıyla Blazor çalışma zamanı tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-125">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="9e9d3-126">Çekirdek özellikleri dahil olmak üzere çoğu uygulama tarafından gerekli Blazor destekler:</span><span class="sxs-lookup"><span data-stu-id="9e9d3-126">Blazor supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="9e9d3-127">Parametreler</span><span class="sxs-lookup"><span data-stu-id="9e9d3-127">Parameters</span></span>
* <span data-ttu-id="9e9d3-128">Olay işleme</span><span class="sxs-lookup"><span data-stu-id="9e9d3-128">Event handling</span></span>
* <span data-ttu-id="9e9d3-129">Veri bağlama</span><span class="sxs-lookup"><span data-stu-id="9e9d3-129">Data binding</span></span>
* <span data-ttu-id="9e9d3-130">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="9e9d3-130">Routing</span></span>
* <span data-ttu-id="9e9d3-131">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="9e9d3-131">Dependency injection</span></span>
* <span data-ttu-id="9e9d3-132">Düzenler</span><span class="sxs-lookup"><span data-stu-id="9e9d3-132">Layouts</span></span>
* <span data-ttu-id="9e9d3-133">Şablon oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e9d3-133">Templating</span></span>
* <span data-ttu-id="9e9d3-134">Basamaklı değerler</span><span class="sxs-lookup"><span data-stu-id="9e9d3-134">Cascading values</span></span>

<span data-ttu-id="9e9d3-135">İndirilen uygulama boyutunu azaltmak için kullanılmayan kod çıkartılır, uygulamadaki oturumunu tarafından yayımlandığında [Ara dil (IL) bağlayıcı](xref:host-and-deploy/razor-components/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="9e9d3-135">To reduce the size of the downloaded app unused code stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/razor-components/configure-linker).</span></span>

<span data-ttu-id="9e9d3-136">Blazor Razor bileşenler için istemci tarafı barındırma modelidir.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-136">Blazor is the client-side hosting model for Razor Components.</span></span> <span data-ttu-id="9e9d3-137">Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bir bileşenin işleme mantığı Razor bileşenleri ayırın olduğundan esneklik nasıl Razor bileşenleri barındırılabilir içinde.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-137">Because Razor Components decouple a component's rendering logic from how UI updates are applied, there's flexibility in how Razor Components can be hosted.</span></span> <span data-ttu-id="9e9d3-138">Bir SignalR bağlantısı üzerinden kullanıcı Arabirimi güncelleştirmeleri nerede işlenir Razor bileşenleri konak sunucusunda bir ASP.NET Core uygulaması için ASP.NET Core Razor bileşenleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-138">Use ASP.NET Core Razor Components to host Razor Components on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="9e9d3-139">Daha fazla bilgi için bkz. <xref:razor-components/index> ve <xref:razor-components/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-139">For more information, see <xref:razor-components/index> and <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span> 

## <a name="components"></a><span data-ttu-id="9e9d3-140">Bileşenler</span><span class="sxs-lookup"><span data-stu-id="9e9d3-140">Components</span></span>

<span data-ttu-id="9e9d3-141">A *Razor bileşen* bir kullanıcı Arabirimi, bir sayfa, iletişim kutusu veya veri girişi formuna gibi parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-141">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="9e9d3-142">Bileşenleri kullanıcı olayları işlemek ve esnek UI işleme mantığı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-142">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="9e9d3-143">Bileşenleri iç içe geçmiş ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-143">Components can be nested and reused.</span></span>

<span data-ttu-id="9e9d3-144">.NET sınıf paylaşılabilir ve NuGet paketleri olarak dağıtılmış .NET derlemelerini yerleşik bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-144">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="9e9d3-145">Sınıf ya da bir Razor biçimlendirme sayfası biçiminde yazılabilir (*.cshtml*) veya farklı bir C# sınıfı (*.cs*).</span><span class="sxs-lookup"><span data-stu-id="9e9d3-145">The class can either be written in the form of a Razor markup page (*.cshtml*) or as a C# class (*.cs*).</span></span>

<span data-ttu-id="9e9d3-146">[Razor](xref:mvc/views/razor) HTML biçimlendirmesi ile birleştiren bir söz dizimi C# kod.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-146">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="9e9d3-147">Razor Geliştirici üretkenliği, biçimlendirme arasında geçiş yapmak Geliştirici izin vermek için tasarlanmıştır ve C# ile aynı dosyada [IntelliSense](/visualstudio/ide/using-intellisense) destekler.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-147">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="9e9d3-148">Razor sayfaları ve MVC görünümleri de Razor kullanın.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-148">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="9e9d3-149">Razor sayfaları ve istek/yanıt modeli çevresinde kurulan MVC görünümleri, bileşenleri özellikle kullanıcı Arabirimi oluşturma işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-149">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="9e9d3-150">Razor bileşenleri, özellikle istemci tarafı UI mantığı ve birleştirme için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-150">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="9e9d3-151">Aşağıdaki biçimlendirme bir Razor dosyasında özel iletişim bileşeninin örneğidir (*DialogComponent.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="9e9d3-151">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.cshtml*):</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="9e9d3-152">Bu bileşen, uygulamada başka bir yerde kullanıldığında, IntelliSense tamamlama söz dizimi ve parametre ile geliştirme hızlandırır.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-152">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="9e9d3-153">Bileşenleri oluşturma DOM adlı tarayıcı bellek içi gösterimine bir *ağaç işleme* , ardından UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-153">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="9e9d3-154">JavaScript ile birlikte çalışma</span><span class="sxs-lookup"><span data-stu-id="9e9d3-154">JavaScript interop</span></span>

<span data-ttu-id="9e9d3-155">Üçüncü taraf JavaScript kitaplıklarını ve API'lerini tarayıcı gerektiren uygulamalar için Blazor JavaScript ile birlikte çalışır.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-155">For apps that require third-party JavaScript libraries and browser APIs, Blazor interoperates with JavaScript.</span></span> <span data-ttu-id="9e9d3-156">Herhangi bir kitaplığı veya JavaScript kullanabilmek için API kullanma yeteneği bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-156">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="9e9d3-157">C#kod, JavaScript kodu çağırabilir ve JavaScript kod içine çağırabilir C# kod.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-157">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="9e9d3-158">Daha fazla bilgi için [JavaScript birlikte çalışma](xref:razor-components/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="9e9d3-158">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="9e9d3-159">Kod paylaşımı ve .NET Standard</span><span class="sxs-lookup"><span data-stu-id="9e9d3-159">Code sharing and .NET Standard</span></span>

<span data-ttu-id="9e9d3-160">Uygulamaları başvurmak ve mevcut olanı kullan [.NET Standard](/dotnet/standard/net-standard) kitaplıkları.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-160">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="9e9d3-161">.NET standart, .NET API'leri, .NET uygulamaları arasında ortak olan bir resmi belirtimi olan.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-161">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="9e9d3-162">.NET standard 2.0 veya üzeri desteklenir.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-162">.NET Standard 2.0 or higher is supported.</span></span> <span data-ttu-id="9e9d3-163">Bir web tarayıcısı içinde (örneğin, dosya sistemine erişen bir yuva açma, iş parçacığı oluşturma ve diğer özellikleri) uygun olmayan API'leri throw <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-163">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="9e9d3-164">.NET standart sınıf kitaplıkları, tarayıcı tabanlı uygulamalar ve sunucu kodu arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-164">.NET Standard class libraries can be shared across server code and in browser-based apps.</span></span>

## <a name="optimization"></a><span data-ttu-id="9e9d3-165">İyileştirme</span><span class="sxs-lookup"><span data-stu-id="9e9d3-165">Optimization</span></span>

<span data-ttu-id="9e9d3-166">Yükü boyutu, bir uygulamanın uygun için önemli performans faktördür.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-166">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="9e9d3-167">Blazor yükü boyutu yükleme sürelerini kısaltmak için en iyi duruma getirir:</span><span class="sxs-lookup"><span data-stu-id="9e9d3-167">Blazor optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="9e9d3-168">.NET derlemeleri kullanılmayan bölümlerini derleme işlemi sırasında kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-168">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="9e9d3-169">HTTP yanıtlarını sıkıştırılır.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-169">HTTP responses are compressed.</span></span>
* <span data-ttu-id="9e9d3-170">.NET çalışma zamanı ve derlemeleri tarayıcıda önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-170">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="9e9d3-171">[Sunucu tarafı Razor bileşenlerini](xref:razor-components/index) Blazor bile küçük bir yük boyutundan .NET derlemeleri, uygulama derleme ve çalışma zamanı sunucu tarafı sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-171">[Server-side Razor Components](xref:razor-components/index) provides an even smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="9e9d3-172">Razor bileşenleri uygulamalar yalnızca işaretleme dosyaları ve statik varlıklar istemcilere hizmet.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-172">Razor Components apps only serve markup files and static assets to clients.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9e9d3-173">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9e9d3-173">Additional resources</span></span>

* <xref:razor-components/index>
* [<span data-ttu-id="9e9d3-174">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="9e9d3-174">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="9e9d3-175">C# Kılavuzu</span><span class="sxs-lookup"><span data-stu-id="9e9d3-175">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="9e9d3-176">HTML</span><span class="sxs-lookup"><span data-stu-id="9e9d3-176">HTML</span></span>](https://www.w3.org/html/)
