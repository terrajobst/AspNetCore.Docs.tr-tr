---
title: ASP.NET Core Blazor barındırma modelleri
author: guardrex
description: İstemci tarafı ve sunucu tarafı modelleri barındırma Blazor anlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/hosting-models
ms.openlocfilehash: 893cde6ee6f4cbcc4051453c66b7405153a55d36
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500441"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="249e2-103">ASP.NET Core Blazor barındırma modelleri</span><span class="sxs-lookup"><span data-stu-id="249e2-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="249e2-104">Tarafından [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="249e2-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="249e2-105">Blazor istemci-tarafı çalışmak üzere tasarlanmış bir web çerçevesi olan tarayıcıda bir [WebAssembly](http://webassembly.org/)-.NET çalışma zamanı tabanlı (*Blazor istemci-tarafı*) veya sunucu tarafı ASP.NET Core (*Blazor sunucu tarafı* ).</span><span class="sxs-lookup"><span data-stu-id="249e2-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](http://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="249e2-106">Barındırma modeli, uygulama ve bileşen modelleri bakılmaksızın *aynı*.</span><span class="sxs-lookup"><span data-stu-id="249e2-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="249e2-107">Bu makalede açıklanan barındırma modellerine yönelik bir proje oluşturmak için bkz: <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="249e2-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="client-side"></a><span data-ttu-id="249e2-108">İstemci tarafı</span><span class="sxs-lookup"><span data-stu-id="249e2-108">Client-side</span></span>

<span data-ttu-id="249e2-109">Asıl barındırma için Blazor WebAssembly tarayıcıda çalışan istemci-tarafı modelidir.</span><span class="sxs-lookup"><span data-stu-id="249e2-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="249e2-110">Tarayıcıya .NET çalışma zamanı Blazor uygulamayı ve bağımlılıkları indirilir.</span><span class="sxs-lookup"><span data-stu-id="249e2-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="249e2-111">Uygulamayı doğrudan tarayıcıda kullanıcı Arabirimi iş parçacığında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="249e2-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="249e2-112">Kullanıcı Arabirimi güncelleştirmeleri ve olay işleme, aynı işlem içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="249e2-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="249e2-113">Statik dosya olarak bir web sunucusu veya hizmeti statik içeriği istemcilere hizmet uygulamasının varlıklarını dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="249e2-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor istemci-tarafı: Tarayıcı içinde bir kullanıcı Arabirimi iş parçacığında Blazor uygulama çalışır.](hosting-models/_static/client-side.png)

<span data-ttu-id="249e2-115">İstemci tarafı barındırma modeli kullanarak bir Blazor uygulaması oluşturmak için aşağıdaki şablonlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="249e2-115">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="249e2-116">**(İstemci-tarafı) Blazor** ([dotnet yeni blazor](/dotnet/core/tools/dotnet-new)) &ndash; statik dosyalar bir dizi dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="249e2-116">**Blazor (client-side)** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="249e2-117">**Blazor (ASP.NET Core barındırılan)** ([dotnet yeni blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; bir ASP.NET Core sunucusu tarafından barındırılan.</span><span class="sxs-lookup"><span data-stu-id="249e2-117">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="249e2-118">ASP.NET Core uygulaması Blazor uygulama istemcilere hizmet.</span><span class="sxs-lookup"><span data-stu-id="249e2-118">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="249e2-119">Web API çağrıları kullanarak ağ üzerinden Blazor istemci tarafı uygulama sunucusu ile etkileşim kurabilir veya [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="249e2-119">The Blazor client-side app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="249e2-120">Şablonları içerir *blazor.webassembly.js* işleyen betik:</span><span class="sxs-lookup"><span data-stu-id="249e2-120">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="249e2-121">.NET çalışma zamanı, uygulama ve uygulamanın bağımlılıklarını karşıdan yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="249e2-121">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="249e2-122">Uygulamayı çalıştırmak için çalışma zamanı başlatma.</span><span class="sxs-lookup"><span data-stu-id="249e2-122">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="249e2-123">İstemci tarafı barındırma modeli, çeşitli avantajlar sunar:</span><span class="sxs-lookup"><span data-stu-id="249e2-123">The client-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="249e2-124">Hiçbir .NET sunucu tarafı bağımlılık yoktur.</span><span class="sxs-lookup"><span data-stu-id="249e2-124">There's no .NET server-side dependency.</span></span> <span data-ttu-id="249e2-125">Uygulama için istemciyi indirdikten sonra tam olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="249e2-125">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="249e2-126">İstemci kaynakları ve özellikleri tam olarak yararlanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="249e2-126">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="249e2-127">İş, sunucudan istemciye boşaltılır.</span><span class="sxs-lookup"><span data-stu-id="249e2-127">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="249e2-128">Uygulamayı barındırmak için bir ASP.NET Core web sunucusu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="249e2-128">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="249e2-129">Sunucusuz dağıtım senaryolarında (örneğin, uygulamayı bir CDN'den sunulması) mümkündür.</span><span class="sxs-lookup"><span data-stu-id="249e2-129">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="249e2-130">İstemci tarafı barındırma downsides vardır:</span><span class="sxs-lookup"><span data-stu-id="249e2-130">There are downsides to client-side hosting:</span></span>

* <span data-ttu-id="249e2-131">Uygulama, tarayıcının yeteneklerini sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="249e2-131">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="249e2-132">Özellikli istemci donanım ve yazılım (örneğin, WebAssembly desteği) gereklidir.</span><span class="sxs-lookup"><span data-stu-id="249e2-132">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="249e2-133">İndirme boyutu ve uygulamaların yüklenmesi daha uzun sürecektir.</span><span class="sxs-lookup"><span data-stu-id="249e2-133">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="249e2-134">.NET çalışma zamanı ve araç desteği daha az olgun.</span><span class="sxs-lookup"><span data-stu-id="249e2-134">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="249e2-135">Sınırlamalar, mevcut [.NET Standard](/dotnet/standard/net-standard) desteği ve hata ayıklama.</span><span class="sxs-lookup"><span data-stu-id="249e2-135">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="server-side"></a><span data-ttu-id="249e2-136">Sunucu tarafı</span><span class="sxs-lookup"><span data-stu-id="249e2-136">Server-side</span></span>

<span data-ttu-id="249e2-137">Sunucu tarafı barındırma modeli ile uygulama sunucusunda bir ASP.NET Core uygulaması içinde yürütülür.</span><span class="sxs-lookup"><span data-stu-id="249e2-137">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="249e2-138">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrılarını üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.</span><span class="sxs-lookup"><span data-stu-id="249e2-138">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Tarayıcı uygulaması (bir ASP.NET Core uygulaması içinde barındırılan) ile sunucu üzerinde bir SignalR bağlantısı üzerinden etkileşim kurar.](hosting-models/_static/server-side.png)

<span data-ttu-id="249e2-140">Sunucu tarafı barındırma modeli kullanarak bir Blazor uygulaması oluşturmak için ASP.NET Core kullanan **Blazor (sunucu tarafı)** şablonu ([dotnet yeni blazorserverside](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="249e2-140">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor (server-side)** template ([dotnet new blazorserverside](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="249e2-141">ASP.NET Core uygulaması, sunucu tarafı uygulamayı barındıran ve istemcilerin eriştikleri SignalR uç noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="249e2-141">The ASP.NET Core app hosts the server-side app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="249e2-142">ASP.NET Core uygulaması uygulamanın başvuran `Startup` sınıfı eklemek için:</span><span class="sxs-lookup"><span data-stu-id="249e2-142">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="249e2-143">Sunucu tarafı hizmetler.</span><span class="sxs-lookup"><span data-stu-id="249e2-143">Server-side services.</span></span>
* <span data-ttu-id="249e2-144">Ardışık Düzen işleme isteği için uygulama.</span><span class="sxs-lookup"><span data-stu-id="249e2-144">The app to the request handling pipeline.</span></span>

<span data-ttu-id="249e2-145">*Blazor.server.js* betik&dagger; istemci bağlantı kurar.</span><span class="sxs-lookup"><span data-stu-id="249e2-145">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="249e2-146">Bunu kalıcı hale getirmek ve gerektiğinde (örneğin, durumunda kayıp ağ bağlantısı) uygulama durumunu geri yüklemek için uygulamanın sorumluluğudur.</span><span class="sxs-lookup"><span data-stu-id="249e2-146">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="249e2-147">Sunucu tarafı barındırma modeli, çeşitli avantajlar sunar:</span><span class="sxs-lookup"><span data-stu-id="249e2-147">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="249e2-148">Bir istemci-tarafı uygulaması önemli ölçüde daha küçük indirme boyutu ve uygulamayı çok daha hızlı yükler.</span><span class="sxs-lookup"><span data-stu-id="249e2-148">Download size is significantly smaller than a client-side app, and the app loads much faster.</span></span>
* <span data-ttu-id="249e2-149">Uygulama sunucusu özellikleri, herhangi bir .NET Core uyumlu API kullanımı dahil olmak üzere tam yararlanır.</span><span class="sxs-lookup"><span data-stu-id="249e2-149">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="249e2-150">Sunucu üzerinde .NET core araçları, hata ayıklama gibi mevcut .NET beklendiği gibi çalışır. Bu nedenle, uygulamayı çalıştırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="249e2-150">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="249e2-151">İnce istemciler desteklenir.</span><span class="sxs-lookup"><span data-stu-id="249e2-151">Thin clients are supported.</span></span> <span data-ttu-id="249e2-152">Örneğin, sunucu tarafı uygulamalar ile WebAssembly desteklemeyen tarayıcılar ve kaynak kısıtlı cihazlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="249e2-152">For example, server-side apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="249e2-153">Uygulamanın .NET /C# kod tabanına uygulama bileşeni kod dahil olmak üzere, istemcilere hizmet değil.</span><span class="sxs-lookup"><span data-stu-id="249e2-153">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="249e2-154">Sunucu tarafı barındırma downsides vardır:</span><span class="sxs-lookup"><span data-stu-id="249e2-154">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="249e2-155">Daha yüksek gecikme süresi genellikle var.</span><span class="sxs-lookup"><span data-stu-id="249e2-155">Higher latency usually exists.</span></span> <span data-ttu-id="249e2-156">Her bir kullanıcı etkileşimi bir ağ atlama içerir.</span><span class="sxs-lookup"><span data-stu-id="249e2-156">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="249e2-157">Çevrimdışı desteği yoktur.</span><span class="sxs-lookup"><span data-stu-id="249e2-157">There's no offline support.</span></span> <span data-ttu-id="249e2-158">İstemci bağlantı başarısız olursa, Uygulama çalışmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="249e2-158">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="249e2-159">Ölçeklenebilirlik, çok sayıda kullanıcı içeren uygulamalar için zordur.</span><span class="sxs-lookup"><span data-stu-id="249e2-159">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="249e2-160">Sunucu, birden çok istemci bağlantılarını yönetme ve istemci durumu işleme.</span><span class="sxs-lookup"><span data-stu-id="249e2-160">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="249e2-161">Bir ASP.NET Core sunucusu uygulama hizmet vermek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="249e2-161">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="249e2-162">Sunucusuz dağıtım senaryolarında (örneğin, uygulamayı bir CDN'den sunulması) mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="249e2-162">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="249e2-163">&dagger;*Blazor.server.js* betik sunulan ASP.NET Core paylaşılan Framework katıştırılmış bir kaynaktan.</span><span class="sxs-lookup"><span data-stu-id="249e2-163">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="249e2-164">Aynı sunucuya yeniden bağlanma</span><span class="sxs-lookup"><span data-stu-id="249e2-164">Reconnection to the same server</span></span>

<span data-ttu-id="249e2-165">Sunucu tarafı uygulamalar Blazor sunucunun etkin bir SignalR bağlantısı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="249e2-165">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="249e2-166">Bağlantı kaybedilirse uygulamanın sunucuya yeniden dener.</span><span class="sxs-lookup"><span data-stu-id="249e2-166">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="249e2-167">Bellekte hala istemcinin durum olduğu sürece, istemci oturum durumunu kaybetmeden sürdürür.</span><span class="sxs-lookup"><span data-stu-id="249e2-167">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>
 
<span data-ttu-id="249e2-168">İstemci bağlantısı kesildi algıladığında, istemci yeniden bağlanmayı dener ancak bir varsayılan kullanıcı arabirimini kullanıcıya görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="249e2-168">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="249e2-169">Yeniden bağlanma başarısız olursa, kullanıcı yeniden denemek için seçeneği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="249e2-169">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="249e2-170">Kullanıcı arabirimini özelleştirmek için bir öğe ile tanımlama `components-reconnect-modal` olarak kendi `id`.</span><span class="sxs-lookup"><span data-stu-id="249e2-170">To customize the UI, define an element with `components-reconnect-modal` as its `id`.</span></span> <span data-ttu-id="249e2-171">İstemci bu öğe, bağlantı durumuna bağlı aşağıdaki CSS sınıflarının biri ile güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="249e2-171">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>
 
* <span data-ttu-id="249e2-172">`components-reconnect-show` &ndash; Bağlantı kesildi ve istemci yeniden bağlanmayı deniyor göstermek için uygulamanın UI göstermesi.</span><span class="sxs-lookup"><span data-stu-id="249e2-172">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="249e2-173">`components-reconnect-hide` &ndash; İstemci UI Gizle etkin bir bağlantı vardır.</span><span class="sxs-lookup"><span data-stu-id="249e2-173">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="249e2-174">`components-reconnect-failed` &ndash; Yeniden bağlanma başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="249e2-174">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="249e2-175">Yeniden bağlanmayı yeniden denemek için çağrı `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="249e2-175">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="249e2-176">Prerendering sonra durum bilgisi olan yeniden bağlanma</span><span class="sxs-lookup"><span data-stu-id="249e2-176">Stateful reconnection after prerendering</span></span>
 
<span data-ttu-id="249e2-177">Blazor sunucu tarafı uygulamalar varsayılan olarak sunucuya istemci bağlantı kurulmadan önce sunucuda UI prerender şekilde ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="249e2-177">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="249e2-178">Bu, ayarlanır *_Host.cshtml* Razor sayfası:</span><span class="sxs-lookup"><span data-stu-id="249e2-178">This is set up in the *_Host.cshtml* Razor page:</span></span>
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>())</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```
 
<span data-ttu-id="249e2-179">İstemci uygulama prerender için kullanılan aynı duruma sunucusuna bağlanır.</span><span class="sxs-lookup"><span data-stu-id="249e2-179">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="249e2-180">Uygulamanın durumu bellekte hala varsa, SignalR bağlantı kurulduktan sonra bileşen durumu rerendered değil.</span><span class="sxs-lookup"><span data-stu-id="249e2-180">If the app's state is still in memory, the component state isn't rerendered after the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="249e2-181">Durum bilgisi olan etkileşimli bileşenleri Razor sayfaları ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="249e2-181">Render stateful interactive components from Razor pages and views</span></span>
 
<span data-ttu-id="249e2-182">Durum bilgisi olan etkileşimli bileşenleri bir Razor sayfası ya da Görünüm eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="249e2-182">Stateful interactive components can be added to a Razor page or view.</span></span> <span data-ttu-id="249e2-183">Sayfa veya Görünüm oluşturulduğunda, bileşeni ile prerendered.</span><span class="sxs-lookup"><span data-stu-id="249e2-183">When the page or view renders, the component is prerendered with it.</span></span> <span data-ttu-id="249e2-184">Durumu bellekte hala olduğu sürece, istemci bağlantısı kurulduktan sonra uygulama için bileşen durumu daha sonra yeniden bağlanır.</span><span class="sxs-lookup"><span data-stu-id="249e2-184">The app then reconnects to the component state once the client connection is established as long as the state is still in memory.</span></span>
 
<span data-ttu-id="249e2-185">Örneğin, aşağıdaki Razor sayfası oluşturur bir `Counter` bileşeni ile form kullanarak belirtilen bir başlangıç sayısı:</span><span class="sxs-lookup"><span data-stu-id="249e2-185">For example, the following Razor page renders a `Counter` component with an initial count that's specified using a form:</span></span>
 
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

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="249e2-186">Uygulama prerendering Algıla</span><span class="sxs-lookup"><span data-stu-id="249e2-186">Detect when the app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="249e2-187">SignalR istemcisi Blazor sunucu tarafı uygulamalar için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="249e2-187">Configure the SignalR client for Blazor server-side apps</span></span>
 
<span data-ttu-id="249e2-188">Bazen, Blazor sunucu tarafı uygulamalar tarafından kullanılan SignalR istemci yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="249e2-188">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="249e2-189">Örneğin, bir bağlantı sorunu tanılamak için SignalR istemci günlüğe kaydetmeyi yapılandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="249e2-189">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>
 
<span data-ttu-id="249e2-190">SignalR istemcisinde yapılandırmak zorunda *Pages/_Host.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="249e2-190">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="249e2-191">Ekleme bir `autostart="false"` özniteliğini `<script>` etiketinde *blazor.server.js* betiği.</span><span class="sxs-lookup"><span data-stu-id="249e2-191">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="249e2-192">Çağrı `Blazor.start` ve SignalR Oluşturucu belirten bir yapılandırma nesnesi içinde geçirin.</span><span class="sxs-lookup"><span data-stu-id="249e2-192">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>
 
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

## <a name="additional-resources"></a><span data-ttu-id="249e2-193">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="249e2-193">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
