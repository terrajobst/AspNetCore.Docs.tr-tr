---
title: ASP.NET Core Blazor barındırma modelleri
author: guardrex
description: İstemci tarafı ve sunucu tarafı Blazor modelleri barındırma anlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: blazor/hosting-models
ms.openlocfilehash: c794daf6f33138c57500a04116a3d172f0201bd5
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152796"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="cd883-103">ASP.NET Core Blazor barındırma modelleri</span><span class="sxs-lookup"><span data-stu-id="cd883-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="cd883-104">Tarafından [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="cd883-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="cd883-105">Blazor istemci-tarafı çalışmak üzere tasarlanmış bir web çerçevesi olan tarayıcıda bir [WebAssembly](http://webassembly.org/)-.NET çalışma zamanı tabanlı (*Blazor istemci-tarafı*) veya sunucu tarafı ASP.NET Core (*Blazor sunucu tarafı* ).</span><span class="sxs-lookup"><span data-stu-id="cd883-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](http://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="cd883-106">Barındırma modeli, uygulama ve bileşen modelleri bakılmaksızın *aynı*.</span><span class="sxs-lookup"><span data-stu-id="cd883-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="cd883-107">Bu makalede açıklanan barındırma modellerine yönelik bir proje oluşturmak için bkz: <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="cd883-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="client-side"></a><span data-ttu-id="cd883-108">İstemci tarafı</span><span class="sxs-lookup"><span data-stu-id="cd883-108">Client-side</span></span>

<span data-ttu-id="cd883-109">Asıl barındırma için Blazor WebAssembly tarayıcıda çalışan istemci-tarafı modelidir.</span><span class="sxs-lookup"><span data-stu-id="cd883-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="cd883-110">Tarayıcıya .NET çalışma zamanı Blazor uygulamayı ve bağımlılıkları indirilir.</span><span class="sxs-lookup"><span data-stu-id="cd883-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="cd883-111">Uygulamayı doğrudan tarayıcıda kullanıcı Arabirimi iş parçacığında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="cd883-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="cd883-112">Kullanıcı Arabirimi güncelleştirmeleri ve olay işleme, aynı işlem içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="cd883-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="cd883-113">Statik dosya olarak bir web sunucusu veya hizmeti statik içeriği istemcilere hizmet uygulamasının varlıklarını dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="cd883-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor istemci-tarafı: Tarayıcı içinde bir kullanıcı Arabirimi iş parçacığında Blazor uygulama çalışır.](hosting-models/_static/client-side.png)

<span data-ttu-id="cd883-115">İstemci tarafı barındırma modeli kullanarak bir Blazor uygulaması oluşturmak için aşağıdaki şablonlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="cd883-115">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="cd883-116">**(İstemci-tarafı) Blazor** ([dotnet yeni blazor](/dotnet/core/tools/dotnet-new)) &ndash; statik dosyalar bir dizi dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="cd883-116">**Blazor (client-side)** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="cd883-117">**Blazor (ASP.NET Core barındırılan)** ([dotnet yeni blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; bir ASP.NET Core sunucusu tarafından barındırılan.</span><span class="sxs-lookup"><span data-stu-id="cd883-117">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="cd883-118">ASP.NET Core uygulaması Blazor uygulama istemcilere hizmet.</span><span class="sxs-lookup"><span data-stu-id="cd883-118">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="cd883-119">Web API çağrıları kullanarak ağ üzerinden istemci-tarafı Blazor uygulama sunucusu ile etkileşim kurabilir veya [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="cd883-119">The client-side Blazor app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="cd883-120">Şablonları içerir *blazor.webassembly.js* işleyen betik:</span><span class="sxs-lookup"><span data-stu-id="cd883-120">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="cd883-121">.NET çalışma zamanı, uygulama ve uygulamanın bağımlılıklarını karşıdan yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="cd883-121">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="cd883-122">Uygulamayı çalıştırmak için çalışma zamanı başlatma.</span><span class="sxs-lookup"><span data-stu-id="cd883-122">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="cd883-123">İstemci tarafı barındırma modeli, çeşitli avantajlar sunar.</span><span class="sxs-lookup"><span data-stu-id="cd883-123">The client-side hosting model offers several benefits.</span></span> <span data-ttu-id="cd883-124">İstemci tarafı Blazor:</span><span class="sxs-lookup"><span data-stu-id="cd883-124">Client-side Blazor:</span></span>

* <span data-ttu-id="cd883-125">.NET sunucu tarafı bağımlılığı yoktur.</span><span class="sxs-lookup"><span data-stu-id="cd883-125">Has no .NET server-side dependency.</span></span>
* <span data-ttu-id="cd883-126">İstemci kaynakları ve özellikleri tam olarak yararlanır.</span><span class="sxs-lookup"><span data-stu-id="cd883-126">Fully leverages client resources and capabilities.</span></span>
* <span data-ttu-id="cd883-127">Yük boşaltma, sunucudan istemciye çalışır.</span><span class="sxs-lookup"><span data-stu-id="cd883-127">Offloads work from the server to the client.</span></span>
* <span data-ttu-id="cd883-128">Çevrimdışı senaryolarını destekler.</span><span class="sxs-lookup"><span data-stu-id="cd883-128">Supports offline scenarios.</span></span>

<span data-ttu-id="cd883-129">İstemci tarafı barındırma downsides vardır.</span><span class="sxs-lookup"><span data-stu-id="cd883-129">There are downsides to client-side hosting.</span></span> <span data-ttu-id="cd883-130">İstemci tarafı Blazor:</span><span class="sxs-lookup"><span data-stu-id="cd883-130">Client-side Blazor:</span></span>

* <span data-ttu-id="cd883-131">Uygulama, tarayıcının yeteneklerini kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="cd883-131">Restricts the app to the capabilities of the browser.</span></span>
* <span data-ttu-id="cd883-132">Özellikli istemci donanım ve yazılım (örneğin, WebAssembly desteği) gerektirir.</span><span class="sxs-lookup"><span data-stu-id="cd883-132">Requires capable client hardware and software (for example, WebAssembly support).</span></span>
* <span data-ttu-id="cd883-133">İndirme boyutu daha büyük ve uzun uygulama yükleme süresi vardır.</span><span class="sxs-lookup"><span data-stu-id="cd883-133">Has a larger download size and longer app load time.</span></span>
* <span data-ttu-id="cd883-134">.NET çalışma zamanı ve araç desteği yetişkin daha az olan (örneğin, kısıtlamaları [.NET Standard](/dotnet/standard/net-standard) desteği ve hata ayıklama).</span><span class="sxs-lookup"><span data-stu-id="cd883-134">Has less mature .NET runtime and tooling support (for example, limitations in [.NET Standard](/dotnet/standard/net-standard) support and debugging).</span></span>

## <a name="server-side"></a><span data-ttu-id="cd883-135">Sunucu tarafı</span><span class="sxs-lookup"><span data-stu-id="cd883-135">Server-side</span></span>

<span data-ttu-id="cd883-136">Sunucu tarafı barındırma modeli ile uygulama sunucusunda bir ASP.NET Core uygulaması içinde yürütülür.</span><span class="sxs-lookup"><span data-stu-id="cd883-136">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="cd883-137">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrılarını üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.</span><span class="sxs-lookup"><span data-stu-id="cd883-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Tarayıcı uygulaması (bir ASP.NET Core uygulaması içinde barındırılan) ile sunucu üzerinde bir SignalR bağlantısı üzerinden etkileşim kurar.](hosting-models/_static/server-side.png)

<span data-ttu-id="cd883-139">Sunucu tarafı barındırma modeli kullanarak bir Blazor uygulaması oluşturmak için ASP.NET Core kullanan **Blazor (sunucu tarafı)** şablonu ([dotnet yeni blazorserverside](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="cd883-139">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor (server-side)** template ([dotnet new blazorserverside](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="cd883-140">ASP.NET Core uygulaması, sunucu tarafı uygulamayı barındıran ve istemcilerin eriştikleri SignalR uç noktasını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="cd883-140">The ASP.NET Core app hosts the server-side app and sets up the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="cd883-141">ASP.NET Core uygulaması uygulamanın başvuran `Startup` sınıfı eklemek için:</span><span class="sxs-lookup"><span data-stu-id="cd883-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="cd883-142">Sunucu tarafı hizmetler.</span><span class="sxs-lookup"><span data-stu-id="cd883-142">Server-side services.</span></span>
* <span data-ttu-id="cd883-143">Ardışık Düzen işleme isteği için uygulama.</span><span class="sxs-lookup"><span data-stu-id="cd883-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="cd883-144">*Blazor.server.js* betik&dagger; istemci bağlantı kurar.</span><span class="sxs-lookup"><span data-stu-id="cd883-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="cd883-145">Bunu kalıcı hale getirmek ve gerektiğinde (örneğin, durumunda kayıp ağ bağlantısı) uygulama durumunu geri yüklemek için uygulamanın sorumluluğudur.</span><span class="sxs-lookup"><span data-stu-id="cd883-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="cd883-146">Sunucu tarafı barındırma modeli, çeşitli avantajlar sunar:</span><span class="sxs-lookup"><span data-stu-id="cd883-146">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="cd883-147">Bir istemci-tarafı uygulaması daha önemli ölçüde daha küçük bir uygulama boyutuna sahiptir ve çok daha hızlı yükler.</span><span class="sxs-lookup"><span data-stu-id="cd883-147">Has a significantly smaller app size than a client-side app and loads much faster.</span></span>
* <span data-ttu-id="cd883-148">Sunucu özellikleri, herhangi bir .NET Core uyumlu API'sini kullanma dahil olmak üzere tüm avantajlarından alır.</span><span class="sxs-lookup"><span data-stu-id="cd883-148">Takes full advantage of server capabilities, including using any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="cd883-149">Araç, hata ayıklama gibi mevcut .NET beklendiği gibi çalışır. Bu nedenle .NET Core üzerinde sunucu üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="cd883-149">Runs on .NET Core on the server, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="cd883-150">İnce istemciler ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="cd883-150">Works with thin clients.</span></span> <span data-ttu-id="cd883-151">Örneğin, WebAssembly ve kaynak desteklemeyen tarayıcılar ile çalışır, cihazları sınırlı.</span><span class="sxs-lookup"><span data-stu-id="cd883-151">For example, works with browsers that don't support WebAssembly and resource constrained devices.</span></span>
* <span data-ttu-id="cd883-152">.NET /C# kod tabanına uygulama bileşeni kod dahil olmak üzere, istemcilere hizmet değil.</span><span class="sxs-lookup"><span data-stu-id="cd883-152">The .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="cd883-153">Sunucu tarafı barındırma downsides vardır:</span><span class="sxs-lookup"><span data-stu-id="cd883-153">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="cd883-154">Daha yüksek gecikme süresi: Her bir kullanıcı etkileşimi bir ağ atlama içerir.</span><span class="sxs-lookup"><span data-stu-id="cd883-154">Higher latency: Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="cd883-155">Çevrimdışı desteği: İstemci bağlantı başarısız olursa, Uygulama çalışmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="cd883-155">No offline support: If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="cd883-156">Sınırlı ölçeklenebilirlik: Sunucu, birden çok istemci bağlantılarını yönetme ve istemci durumu işleme.</span><span class="sxs-lookup"><span data-stu-id="cd883-156">Reduced scalability: The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="cd883-157">Bir ASP.NET Core sunucusu uygulama hizmet vermek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="cd883-157">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="cd883-158">Dağıtım bir sunucudan (örneğin, bir CDN) olmadan mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="cd883-158">Deployment without a server (for example, from a CDN) isn't possible.</span></span>

<span data-ttu-id="cd883-159">&dagger;*Blazor.server.js* betik sunulan ASP.NET Core paylaşılan Framework katıştırılmış bir kaynaktan.</span><span class="sxs-lookup"><span data-stu-id="cd883-159">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="cd883-160">Aynı sunucuya yeniden bağlanma</span><span class="sxs-lookup"><span data-stu-id="cd883-160">Reconnection to the same server</span></span>

<span data-ttu-id="cd883-161">Sunucu tarafı uygulamalar Blazor sunucunun etkin bir SignalR bağlantısı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="cd883-161">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="cd883-162">Bir bağlantı kaybedilirse uygulamanın sunucuya yeniden dener.</span><span class="sxs-lookup"><span data-stu-id="cd883-162">If a connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="cd883-163">Bellekte hala istemcinin durum olduğu sürece, istemci oturum durumunu kaybetmeden sürdürür.</span><span class="sxs-lookup"><span data-stu-id="cd883-163">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>
 
<span data-ttu-id="cd883-164">İstemci bağlantısı kesildi algıladığında, istemci yeniden bağlanmayı dener ancak bir varsayılan kullanıcı arabirimini kullanıcıya görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="cd883-164">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="cd883-165">Yeniden bağlanma başarısız olursa, kullanıcı yeniden denemek için seçeneği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="cd883-165">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="cd883-166">Kullanıcı arabirimini özelleştirmek için bir öğe ile tanımlama `components-reconnect-modal` olarak kendi `id`.</span><span class="sxs-lookup"><span data-stu-id="cd883-166">To customize the UI, define an element with `components-reconnect-modal` as its `id`.</span></span> <span data-ttu-id="cd883-167">İstemci bu öğe, bağlantı durumuna bağlı aşağıdaki CSS sınıflarının biri ile güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="cd883-167">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>
 
* <span data-ttu-id="cd883-168">`components-reconnect-show` &ndash; Bağlantı kesildi ve istemci yeniden bağlanmayı deniyor göstermek için uygulamanın UI göstermesi.</span><span class="sxs-lookup"><span data-stu-id="cd883-168">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="cd883-169">`components-reconnect-hide` &ndash; İstemci UI Gizle etkin bir bağlantı vardır.</span><span class="sxs-lookup"><span data-stu-id="cd883-169">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="cd883-170">`components-reconnect-failed` &ndash; Yeniden bağlanma başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="cd883-170">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="cd883-171">Yeniden bağlanmayı yeniden denemek için çağrı `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="cd883-171">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="cd883-172">Prerendering sonra durum bilgisi olan yeniden bağlanma</span><span class="sxs-lookup"><span data-stu-id="cd883-172">Stateful reconnection after prerendering</span></span>
 
<span data-ttu-id="cd883-173">Blazor sunucu tarafı uygulamalar varsayılan olarak sunucuya istemci bağlantı kurulmadan önce sunucuda UI prerender şekilde ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="cd883-173">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="cd883-174">Bu, ayarlanır *_Host.cshtml* Razor sayfası:</span><span class="sxs-lookup"><span data-stu-id="cd883-174">This is set up in the *_Host.cshtml* Razor page:</span></span>
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>())</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```
 
<span data-ttu-id="cd883-175">İstemci uygulama prerender için kullanılan aynı duruma sunucusuna bağlanır.</span><span class="sxs-lookup"><span data-stu-id="cd883-175">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="cd883-176">Uygulamanın durumu bellekte hala varsa, SignalR bağlantı kurulduktan sonra bileşen durumu rerendered değil.</span><span class="sxs-lookup"><span data-stu-id="cd883-176">If the app's state is still in memory, the component state isn't rerendered after the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="cd883-177">Durum bilgisi olan etkileşimli bileşenleri Razor sayfaları ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="cd883-177">Render stateful interactive components from Razor pages and views</span></span>
 
<span data-ttu-id="cd883-178">Durum bilgisi olan etkileşimli bileşenleri bir Razor sayfası ya da Görünüm eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="cd883-178">Stateful interactive components can be added to a Razor page or view.</span></span> <span data-ttu-id="cd883-179">Sayfa veya Görünüm oluşturulduğunda, bileşeni ile prerendered.</span><span class="sxs-lookup"><span data-stu-id="cd883-179">When the page or view renders, the component is prerendered with it.</span></span> <span data-ttu-id="cd883-180">Durumu bellekte hala olduğu sürece, istemci bağlantısı kurulduktan sonra uygulama için bileşen durumu daha sonra yeniden bağlanır.</span><span class="sxs-lookup"><span data-stu-id="cd883-180">The app then reconnects to the component state once the client connection is established as long as the state is still in memory.</span></span>
 
<span data-ttu-id="cd883-181">Örneğin, bir sayaç bileşeni ile form kullanarak belirtilen bir başlangıç sayısı aşağıdaki Razor sayfası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cd883-181">For example, the following Razor page renders a Counter component with an initial count that's specified using a form:</span></span>
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialCount" />
    <button type="submit">Set initial count</button>
</form>
 
@(await Html.RenderComponentAsync<Counter>(new { InitialCount = InitialCount }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialCount { get; set; }
}
```

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="cd883-182">Uygulama prerendering Algıla</span><span class="sxs-lookup"><span data-stu-id="cd883-182">Detect when the app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="cd883-183">SignalR istemcisi Blazor sunucu tarafı uygulamalar için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cd883-183">Configure the SignalR client for Blazor server-side apps</span></span>
 
<span data-ttu-id="cd883-184">Bazen, Blazor sunucu tarafı uygulamalar tarafından kullanılan SignalR istemci yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="cd883-184">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="cd883-185">Örneğin, bir bağlantı sorunu tanılamak için SignalR istemci günlüğe kaydetmeyi yapılandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cd883-185">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>
 
<span data-ttu-id="cd883-186">SignalR istemcisinde yapılandırmak zorunda *sayfaları /\_Host.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="cd883-186">To configure the SignalR client in the *Pages/\_Host.cshtml* file:</span></span>

* <span data-ttu-id="cd883-187">Ekleme bir `autostart="false"` özniteliğini `<script>` etiketinde *blazor.server.js* betiği.</span><span class="sxs-lookup"><span data-stu-id="cd883-187">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="cd883-188">Çağrı `Blazor.start` ve SignalR Oluşturucu belirten bir yapılandırma nesnesi içinde geçirin.</span><span class="sxs-lookup"><span data-stu-id="cd883-188">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>
 
```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging(2); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a><span data-ttu-id="cd883-189">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="cd883-189">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
