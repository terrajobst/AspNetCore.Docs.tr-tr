---
title: Razor bileşenlerine giriş
author: guardrex
description: ASP.NET Core Razor bileşenleri keşfedin bir .NET framework kullanarak web C#/Razor ve HTML.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/09/2019
uid: razor-components/index
ms.openlocfilehash: e8d75a0647704f1ff05e70a96abbb2e448868165
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56102935"
---
# <a name="introduction-to-razor-components"></a><span data-ttu-id="5025d-103">Razor bileşenlerine giriş</span><span class="sxs-lookup"><span data-stu-id="5025d-103">Introduction to Razor Components</span></span>

<span data-ttu-id="5025d-104">Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5025d-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="5025d-105">*Razor bileşenleri* etkileşimli istemci tarafı web .NET ile kullanıcı arabirimini derlemek için yepyeni bir yoludur:</span><span class="sxs-lookup"><span data-stu-id="5025d-105">*Razor Components* is a new way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="5025d-106">Kullanarak zengin etkileşimli kullanıcı arabirimleri oluşturun C# JavaScript yerine.</span><span class="sxs-lookup"><span data-stu-id="5025d-106">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="5025d-107">.NET ile yazılan tüm sunucu tarafı ve istemci tarafı uygulama mantığı paylaşın.</span><span class="sxs-lookup"><span data-stu-id="5025d-107">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="5025d-108">HTML ve CSS olarak UI mobil tarayıcılar dahil olmak üzere geniş tarayıcı desteği işleyin.</span><span class="sxs-lookup"><span data-stu-id="5025d-108">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

## <a name="components"></a><span data-ttu-id="5025d-109">Bileşenler</span><span class="sxs-lookup"><span data-stu-id="5025d-109">Components</span></span>

<span data-ttu-id="5025d-110">A *Razor bileşen* bir kullanıcı Arabirimi, bir sayfa, iletişim kutusu veya veri girişi formuna gibi parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="5025d-110">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="5025d-111">Bileşenleri kullanıcı olayları işlemek ve esnek UI işleme mantığı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="5025d-111">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="5025d-112">Bileşenleri iç içe geçmiş ve yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5025d-112">Components can be nested and reused.</span></span>

<span data-ttu-id="5025d-113">.NET sınıf paylaşılabilir ve NuGet paketleri olarak dağıtılmış .NET derlemelerini yerleşik bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="5025d-113">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="5025d-114">Sınıf ya da bir Razor biçimlendirme sayfası biçiminde yazılabilir (*.cshtml*) veya farklı bir C# sınıfı (*.cs*).</span><span class="sxs-lookup"><span data-stu-id="5025d-114">The class can either be written in the form of a Razor markup page (*.cshtml*) or as a C# class (*.cs*).</span></span>

<span data-ttu-id="5025d-115">[Razor](xref:mvc/views/razor) HTML biçimlendirmesi ile birleştiren bir söz dizimi C# kod.</span><span class="sxs-lookup"><span data-stu-id="5025d-115">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="5025d-116">Razor Geliştirici üretkenliği, biçimlendirme arasında geçiş yapmak Geliştirici izin vermek için tasarlanmıştır ve C# ile aynı dosyada [IntelliSense](/visualstudio/ide/using-intellisense) destekler.</span><span class="sxs-lookup"><span data-stu-id="5025d-116">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="5025d-117">Razor sayfaları ve MVC görünümleri de Razor kullanın.</span><span class="sxs-lookup"><span data-stu-id="5025d-117">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="5025d-118">Razor sayfaları ve istek/yanıt modeli çevresinde kurulan MVC görünümleri, bileşenleri özellikle kullanıcı Arabirimi oluşturma işlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5025d-118">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="5025d-119">Razor bileşenleri, özellikle istemci tarafı UI mantığı ve birleştirme için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5025d-119">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="5025d-120">Aşağıdaki biçimlendirme bir Razor dosyasında özel iletişim bileşeninin örneğidir (*DialogComponent.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="5025d-120">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.cshtml*):</span></span>

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

<span data-ttu-id="5025d-121">Bu bileşen, uygulamada başka bir yerde kullanıldığında, IntelliSense tamamlama söz dizimi ve parametre ile geliştirme hızlandırır.</span><span class="sxs-lookup"><span data-stu-id="5025d-121">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="5025d-122">Bileşenleri oluşturma DOM adlı tarayıcı bellek içi gösterimine bir *ağaç işleme* , ardından UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5025d-122">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="5025d-123">Razor bileşenleri dahil olmak üzere çoğu uygulama tarafından gereken çekirdek özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="5025d-123">Razor Components support core facilities required by most apps, including:</span></span>

* <span data-ttu-id="5025d-124">Parametreler</span><span class="sxs-lookup"><span data-stu-id="5025d-124">Parameters</span></span>
* <span data-ttu-id="5025d-125">Olay işleme</span><span class="sxs-lookup"><span data-stu-id="5025d-125">Event handling</span></span>
* <span data-ttu-id="5025d-126">Veri bağlama</span><span class="sxs-lookup"><span data-stu-id="5025d-126">Data binding</span></span>
* <span data-ttu-id="5025d-127">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="5025d-127">Routing</span></span>
* <span data-ttu-id="5025d-128">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="5025d-128">Dependency injection</span></span>
* <span data-ttu-id="5025d-129">Düzenler</span><span class="sxs-lookup"><span data-stu-id="5025d-129">Layouts</span></span>
* <span data-ttu-id="5025d-130">Şablon oluşturma</span><span class="sxs-lookup"><span data-stu-id="5025d-130">Templating</span></span>
* <span data-ttu-id="5025d-131">Basamaklı değerler</span><span class="sxs-lookup"><span data-stu-id="5025d-131">Cascading values</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="5025d-132">JavaScript ile birlikte çalışma</span><span class="sxs-lookup"><span data-stu-id="5025d-132">JavaScript interop</span></span>

<span data-ttu-id="5025d-133">Üçüncü taraf JavaScript kitaplıklarını ve API'lerini tarayıcı gerektiren uygulamalar için Razor bileşenleri JavaScript ile çalışma.</span><span class="sxs-lookup"><span data-stu-id="5025d-133">For apps that require third-party JavaScript libraries and browser APIs, Razor Components interoperate with JavaScript.</span></span> <span data-ttu-id="5025d-134">Razor bileşenleri herhangi bir kitaplığı veya JavaScript kullanabilmek için API kullanma yeteneği.</span><span class="sxs-lookup"><span data-stu-id="5025d-134">Razor Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="5025d-135">C#kod, JavaScript kodu çağırabilir ve JavaScript kod içine çağırabilir C# kod.</span><span class="sxs-lookup"><span data-stu-id="5025d-135">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="5025d-136">Daha fazla bilgi için [JavaScript birlikte çalışma](xref:razor-components/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="5025d-136">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="5025d-137">Barındırma modelleri</span><span class="sxs-lookup"><span data-stu-id="5025d-137">Hosting models</span></span>

### <a name="server-side-hosting-model"></a><span data-ttu-id="5025d-138">Sunucu tarafı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="5025d-138">Server-side hosting model</span></span>

<span data-ttu-id="5025d-139">Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bir bileşenin işleme mantığı Razor bileşenleri ayırın olduğundan esneklik nasıl Razor bileşenleri barındırılabilir içinde.</span><span class="sxs-lookup"><span data-stu-id="5025d-139">Because Razor Components decouple a component's rendering logic from how UI updates are applied, there's flexibility in how Razor Components can be hosted.</span></span> <span data-ttu-id="5025d-140">ASP.NET Core Razor bileşenleri .NET Core 3. 0'ı ASP.NET Core uygulaması sunucusunda Razor bileşenlerini barındırma desteği ekler.</span><span class="sxs-lookup"><span data-stu-id="5025d-140">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="5025d-141">Kullanıcı Arabirimi güncelleştirmeleri SignalR bağlantısı üzerinden işlenir.</span><span class="sxs-lookup"><span data-stu-id="5025d-141">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="5025d-142">Çalışma zamanı:</span><span class="sxs-lookup"><span data-stu-id="5025d-142">The runtime:</span></span>

* <span data-ttu-id="5025d-143">UI olayları tarayıcıdan sunucuya gönderme işler.</span><span class="sxs-lookup"><span data-stu-id="5025d-143">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="5025d-144">Geçerli kullanıcı Arabirimi güncelleştirmeleri sunucu tarafından gönderilen bileşenleri çalıştırdıktan sonra tarayıcıya geri.</span><span class="sxs-lookup"><span data-stu-id="5025d-144">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="5025d-145">Tarayıcı ile iletişim kurmak için Razor bileşenleri tarafından kullanılan bağlantı JavaScript birlikte çalışma çağrıları işlemek için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5025d-145">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Razor bileşenleri .NET kod sunucu üzerinde çalışır ve belge nesne modeli istemcide bir SignalR bağlantısı üzerinden etkileşim](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="5025d-147">Daha fazla bilgi için bkz. <xref:razor-components/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="5025d-147">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span>

### <a name="client-side-hosting-model"></a><span data-ttu-id="5025d-148">İstemci tarafı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="5025d-148">Client-side hosting model</span></span>

<span data-ttu-id="5025d-149">*Blazor* Razor bileşenlerinin Deneysel istemci-tarafı barındırma modeli.</span><span class="sxs-lookup"><span data-stu-id="5025d-149">*Blazor* is the experimental client-side hosting model of Razor Components.</span></span> <span data-ttu-id="5025d-150">Blazor transpilation eklentileri veya kod olmadan açık web standartları kullanarak tarayıcıda .NET üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="5025d-150">Blazor runs on .NET in the browser using open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="5025d-151">Mobil tarayıcılar dahil tüm modern web tarayıcılarında Blazor çalışır.</span><span class="sxs-lookup"><span data-stu-id="5025d-151">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="5025d-152">Çalışan tarayıcılar içinde .NET kod tarafından yapılır [WebAssembly](http://webassembly.org) (kısaltılmış *wasm*).</span><span class="sxs-lookup"><span data-stu-id="5025d-152">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="5025d-153">WebAssembly standart ve desteklenen web tarayıcılarından eklentileri olmadan'de açık bir web API'sidir.</span><span class="sxs-lookup"><span data-stu-id="5025d-153">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="5025d-154">WebAssembly hızlı indirme ve maksimum yürütme hızı için iyileştirilmiş bir compact bayt biçimidir.</span><span class="sxs-lookup"><span data-stu-id="5025d-154">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="5025d-155">WebAssembly kod tarayıcı yoluyla JavaScript birlikte çalışma'nin tam işlevselliği erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5025d-155">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="5025d-156">Aynı zamanda, kötü amaçlı Eylemler istemci makinede önlemek için JavaScript aynı güvenilir sanal WebAssembly kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="5025d-156">At the same time, WebAssembly code runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![.NET kodu Blazor tarayıcı WebAssembly ile çalışır.](index/_static/blazor.png)

<span data-ttu-id="5025d-158">Ne zaman Blazor uygulama oluşturulur ve bir tarayıcıda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="5025d-158">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="5025d-159">C#kod dosyaları ve Razor dosyaları .NET bütünleştirilmiş kodlarının derlenir.</span><span class="sxs-lookup"><span data-stu-id="5025d-159">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="5025d-160">Derlemeler ve .NET çalışma zamanı tarayıcıya indirilir.</span><span class="sxs-lookup"><span data-stu-id="5025d-160">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="5025d-161">Blazor .NET çalışma zamanı bootstrap için JavaScript kullanır ve çalışma zamanı derlemeleri yüklemek için yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="5025d-161">Blazor uses JavaScript to bootstrap the .NET runtime and configures the runtime to load assemblies.</span></span> <span data-ttu-id="5025d-162">Belge nesne modeli (DOM) işleme ve tarayıcı API çağrıları, JavaScript birlikte çalışma aracılığıyla Blazor çalışma zamanı tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="5025d-162">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="5025d-163">WebAssembly desteklemeyen eski tarayıcılar desteklemek için kullanma [sunucu tarafı barındırma modeli](#server-side-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="5025d-163">To support older browsers that don't support WebAssembly, use the [server-side hosting model](#server-side-hosting-model).</span></span>

<span data-ttu-id="5025d-164">Daha fazla bilgi için bkz. <xref:razor-components/hosting-models#client-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="5025d-164">For more information, see <xref:razor-components/hosting-models#client-side-hosting-model>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5025d-165">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5025d-165">Additional resources</span></span>

* [<span data-ttu-id="5025d-166">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="5025d-166">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="5025d-167">C# Kılavuzu</span><span class="sxs-lookup"><span data-stu-id="5025d-167">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="5025d-168">HTML</span><span class="sxs-lookup"><span data-stu-id="5025d-168">HTML</span></span>](https://www.w3.org/html/)
