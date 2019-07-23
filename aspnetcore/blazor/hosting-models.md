---
title: Blazor barındırma modellerini ASP.NET Core
author: guardrex
description: Blazor istemci tarafı ve sunucu tarafı barındırma modellerini anlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/hosting-models
ms.openlocfilehash: 9dd96ff6e3539bf1c3e932b33776b16d0fbb2d34
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371794"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="4a9ca-103">Blazor barındırma modellerini ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a9ca-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="4a9ca-104">[Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="4a9ca-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="4a9ca-105">Blazor, bir [Webassembly](https://webassembly.org/)tabanlı .NET çalışma zamanı (*Blazor istemci tarafı*) veya ASP.NET Core (*Blazor sunucu tarafı*) içindeki sunucu tarafında tarayıcıda istemci tarafı çalıştırmak için tasarlanan bir Web çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="4a9ca-106">Barındırma modelinden bağımsız olarak, uygulama ve bileşen modelleri *aynıdır*.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="4a9ca-107">Bu makalede açıklanan barındırma modelleriyle ilgili bir proje oluşturmak için, bkz <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="client-side"></a><span data-ttu-id="4a9ca-108">İstemci tarafı</span><span class="sxs-lookup"><span data-stu-id="4a9ca-108">Client-side</span></span>

<span data-ttu-id="4a9ca-109">Blazor için sorumlu barındırma modeli, WebAssembly üzerinde tarayıcıda istemci tarafında çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="4a9ca-110">Blazor uygulaması, bağımlılıkları ve .NET çalışma zamanı tarayıcıya indirilir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="4a9ca-111">Uygulama doğrudan tarayıcı kullanıcı arabirimi iş parçacığında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="4a9ca-112">UI güncelleştirmeleri ve olay işleme aynı işlem içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="4a9ca-113">Uygulamanın varlıkları, istemcilere statik içerik sunan bir Web sunucusuna veya hizmete statik dosyalar olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor istemci tarafı: Blazor uygulaması, tarayıcı içindeki bir kullanıcı arabirimi iş parçacığında çalışır.](hosting-models/_static/client-side.png)

<span data-ttu-id="4a9ca-115">İstemci tarafı barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için aşağıdaki şablonlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="4a9ca-115">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="4a9ca-116">**Blazor (istemci tarafı)** ([DotNet New blazor](/dotnet/core/tools/dotnet-new)) &ndash; Statik dosyalar kümesi olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-116">**Blazor (client-side)** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="4a9ca-117">**Blazor (ASP.NET Core barındırılan)** ([DotNet New blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Bir ASP.NET Core sunucusu tarafından barındırılır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-117">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="4a9ca-118">ASP.NET Core uygulaması, Blazor uygulamasını istemcilere sunar.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-118">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="4a9ca-119">Blazor istemci tarafı uygulaması, Web API çağrıları veya [SignalR](xref:signalr/introduction)kullanarak ağ üzerinden sunucu ile etkileşime geçebilir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-119">The Blazor client-side app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="4a9ca-120">Şablonlar şunları ele alan *blazor. webassembly. js* betiğini içerir:</span><span class="sxs-lookup"><span data-stu-id="4a9ca-120">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="4a9ca-121">.NET çalışma zamanını, uygulamayı ve uygulamanın bağımlılıklarını indirme.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-121">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="4a9ca-122">Uygulamayı çalıştırmak için çalışma zamanının başlatılması.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-122">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="4a9ca-123">İstemci tarafı barındırma modeli çeşitli avantajlar sunar:</span><span class="sxs-lookup"><span data-stu-id="4a9ca-123">The client-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="4a9ca-124">.NET sunucu tarafı bağımlılığı yoktur.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-124">There's no .NET server-side dependency.</span></span> <span data-ttu-id="4a9ca-125">Uygulama, istemciye indirildikten sonra tamamen çalışır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-125">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="4a9ca-126">İstemci kaynakları ve yetenekleri tamamen yararlanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-126">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="4a9ca-127">İş sunucudan istemciye boşaltılır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-127">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="4a9ca-128">Uygulamayı barındırmak için bir ASP.NET Core Web sunucusu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-128">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="4a9ca-129">Sunucusuz dağıtım senaryoları mümkündür (örneğin, bir CDN 'den uygulama sunma).</span><span class="sxs-lookup"><span data-stu-id="4a9ca-129">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="4a9ca-130">İstemci tarafı barındırma için aşağı taraf vardır:</span><span class="sxs-lookup"><span data-stu-id="4a9ca-130">There are downsides to client-side hosting:</span></span>

* <span data-ttu-id="4a9ca-131">Uygulama tarayıcının özelliklerine kısıtlıdır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-131">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="4a9ca-132">Uyumlu istemci donanımı ve yazılımı (örneğin, WebAssembly desteği) gereklidir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-132">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="4a9ca-133">İndirme boyutu daha büyüktür ve uygulamaların yüklenmesi daha uzun sürer.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-133">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="4a9ca-134">.NET çalışma zamanı ve araç desteği daha az olgun.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-134">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="4a9ca-135">Örneğin, [.NET Standard](/dotnet/standard/net-standard) desteğinin ve hata ayıklamada sınırlamalar mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-135">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="server-side"></a><span data-ttu-id="4a9ca-136">Sunucu tarafı</span><span class="sxs-lookup"><span data-stu-id="4a9ca-136">Server-side</span></span>

<span data-ttu-id="4a9ca-137">Sunucu tarafı barındırma modeliyle, uygulama sunucuda ASP.NET Core bir uygulama içinden yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-137">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="4a9ca-138">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrıları bir [SignalR](xref:signalr/introduction) bağlantısı üzerinden işlenir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-138">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Tarayıcı, bir SignalR bağlantısı üzerinden sunucusunda (bir ASP.NET Core uygulamasının içinde barındırılan) uygulamayla etkileşime girer.](hosting-models/_static/server-side.png)

<span data-ttu-id="4a9ca-140">Sunucu tarafı barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için ASP.NET Core **Blazor Server uygulama** şablonunu ([DotNet New blazorserverside](/dotnet/core/tools/dotnet-new)) kullanın.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-140">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserverside](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="4a9ca-141">ASP.NET Core uygulaması, sunucu tarafı uygulamayı barındırır ve istemcilerin bağlanacağı SignalR uç noktasını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-141">The ASP.NET Core app hosts the server-side app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="4a9ca-142">ASP.NET Core uygulama, eklenecek uygulamanın `Startup` sınıfına başvurur:</span><span class="sxs-lookup"><span data-stu-id="4a9ca-142">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="4a9ca-143">Sunucu tarafı hizmetler.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-143">Server-side services.</span></span>
* <span data-ttu-id="4a9ca-144">İstek işleme işlem hattının uygulaması.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-144">The app to the request handling pipeline.</span></span>

<span data-ttu-id="4a9ca-145">*Blazor. Server. js* betiği&dagger; , istemci bağlantısını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-145">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="4a9ca-146">Uygulamanın, uygulama durumunu (örneğin, kayıp ağ bağlantısı durumunda) kalıcı hale getirmek ve geri yüklemek, uygulamanın sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-146">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="4a9ca-147">Sunucu tarafı barındırma modeli çeşitli avantajlar sunar:</span><span class="sxs-lookup"><span data-stu-id="4a9ca-147">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="4a9ca-148">İndirme boyutu bir istemci tarafı uygulamadan önemli ölçüde küçüktür ve uygulama çok daha hızlı yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-148">Download size is significantly smaller than a client-side app, and the app loads much faster.</span></span>
* <span data-ttu-id="4a9ca-149">Uygulama, .NET Core ile uyumlu API 'lerin kullanımı dahil olmak üzere sunucu olanaklarından tam olarak yararlanır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-149">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="4a9ca-150">Sunucuda .NET Core, uygulamayı çalıştırmak için kullanılır, bu nedenle hata ayıklama gibi mevcut .NET araçları beklendiği gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-150">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="4a9ca-151">Ölçülü istemciler desteklenir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-151">Thin clients are supported.</span></span> <span data-ttu-id="4a9ca-152">Örneğin, sunucu tarafı uygulamalar, WebAssembly ve kaynak kısıtlı cihazlarda bulunan tarayıcılarla çalışır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-152">For example, server-side apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="4a9ca-153">Uygulamanın bileşen kodu da dahilC# olmak üzere, uygulamanın .NET/kod tabanı istemcilere sunulmuyor.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-153">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="4a9ca-154">Sunucu tarafı barındırma için aşağı taraf vardır:</span><span class="sxs-lookup"><span data-stu-id="4a9ca-154">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="4a9ca-155">Daha yüksek gecikme süresi genellikle vardır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-155">Higher latency usually exists.</span></span> <span data-ttu-id="4a9ca-156">Her Kullanıcı etkileşimi bir ağ atmasını içerir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-156">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="4a9ca-157">Çevrimdışı destek yoktur.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-157">There's no offline support.</span></span> <span data-ttu-id="4a9ca-158">İstemci bağlantısı başarısız olursa, uygulama çalışmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-158">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="4a9ca-159">Ölçeklenebilirlik, çok sayıda kullanıcısı olan uygulamalar için zorlayıcı bir uygulamalardır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-159">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="4a9ca-160">Sunucunun birden çok istemci bağlantısını yönetmesi ve istemci durumunu işlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-160">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="4a9ca-161">Uygulamayı çalıştırmak için bir ASP.NET Core sunucusu gerekir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-161">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="4a9ca-162">Sunucusuz dağıtım senaryoları mümkün değildir (örneğin, bir CDN 'den uygulama sunma).</span><span class="sxs-lookup"><span data-stu-id="4a9ca-162">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="4a9ca-163">&dagger;*Blazor. Server. js* betiği ASP.NET Core paylaşılan çerçevede eklenmiş bir kaynaktan sunulur.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-163">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="4a9ca-164">Aynı sunucuya yeniden bağlanma</span><span class="sxs-lookup"><span data-stu-id="4a9ca-164">Reconnection to the same server</span></span>

<span data-ttu-id="4a9ca-165">Blazor sunucu tarafı uygulamalar sunucuya etkin bir SignalR bağlantısı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-165">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="4a9ca-166">Bağlantı kaybolursa, uygulama sunucuya yeniden bağlanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-166">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="4a9ca-167">İstemcinin durumu hala bellekte olduğu sürece, istemci oturumu durum kaybı olmadan devam eder.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-167">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>
 
<span data-ttu-id="4a9ca-168">İstemci bağlantının kaybolduğunu algıladığında, istemci yeniden bağlanmayı denediğinde kullanıcıya varsayılan bir kullanıcı arabirimi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-168">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="4a9ca-169">Yeniden bağlantı başarısız olursa, kullanıcıya yeniden deneme seçeneği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-169">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="4a9ca-170">Kullanıcı arabirimini özelleştirmek için, `components-reconnect-modal` *_host. cshtml* Razor sayfasında `id` olarak bir öğesi tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-170">To customize the UI, define an element with `components-reconnect-modal` as its `id` in the *_Host.cshtml* Razor page.</span></span> <span data-ttu-id="4a9ca-171">İstemci bu öğeyi bağlantı durumuna göre aşağıdaki CSS sınıflarından biriyle güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="4a9ca-171">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>
 
* <span data-ttu-id="4a9ca-172">`components-reconnect-show`&ndash; Bağlantının kaybolduğunu ve istemcinin yeniden bağlanmaya çalışıyor olduğunu göstermek için Kullanıcı arabirimini görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-172">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="4a9ca-173">`components-reconnect-hide`&ndash; İstemcinin etkin bir bağlantısı vardır ve Kullanıcı arabirimini gizleyin.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-173">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="4a9ca-174">`components-reconnect-failed`&ndash; Yeniden bağlantı kurulamadı.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-174">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="4a9ca-175">Yeniden bağlanmayı yeniden denemek için çağrısı `window.Blazor.reconnect()`yapın.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-175">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="4a9ca-176">Prerendering sonrasında durum bilgisi olan yeniden bağlanma</span><span class="sxs-lookup"><span data-stu-id="4a9ca-176">Stateful reconnection after prerendering</span></span>
 
<span data-ttu-id="4a9ca-177">Blazor sunucu tarafında bulunan uygulamalar, sunucu bağlantısı yapılmadan önce sunucu üzerindeki kullanıcı arabirimini varsayılan olarak PreRender 'a ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-177">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="4a9ca-178">Bu, *_Host. cshtml* Razor sayfasında ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="4a9ca-178">This is set up in the *_Host.cshtml* Razor page:</span></span>
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>())</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```
 
<span data-ttu-id="4a9ca-179">İstemci, uygulamayı PreRender 'da kullanılan aynı durum ile sunucuya yeniden bağlanır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-179">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="4a9ca-180">Uygulamanın durumu hala bellekte ise, SignalR bağlantısı kurulduktan sonra bileşen durumu tekrar verilmez.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-180">If the app's state is still in memory, the component state isn't rerendered after the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="4a9ca-181">Razor sayfaları ve görünümlerinden durum bilgisi olan etkileşimli bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="4a9ca-181">Render stateful interactive components from Razor pages and views</span></span>
 
<span data-ttu-id="4a9ca-182">Durum bilgisi olan etkileşimli bileşenler Razor sayfasına veya görünümüne eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-182">Stateful interactive components can be added to a Razor page or view.</span></span> <span data-ttu-id="4a9ca-183">Sayfa veya görünüm ne zaman işler, bileşen onunla birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-183">When the page or view renders, the component is prerendered with it.</span></span> <span data-ttu-id="4a9ca-184">Daha sonra uygulama, durum hala bellekte olduğu sürece istemci bağlantısı kurulduktan sonra bileşen durumuna yeniden bağlanır.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-184">The app then reconnects to the component state once the client connection is established as long as the state is still in memory.</span></span>
 
<span data-ttu-id="4a9ca-185">Örneğin, aşağıdaki Razor sayfası bir form kullanılarak belirtilen `Counter` başlangıç sayısıyla bir bileşen oluşturur:</span><span class="sxs-lookup"><span data-stu-id="4a9ca-185">For example, the following Razor page renders a `Counter` component with an initial count that's specified using a form:</span></span>
 
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

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="4a9ca-186">Uygulamanın ne zaman prerendering olduğunu Algıla</span><span class="sxs-lookup"><span data-stu-id="4a9ca-186">Detect when the app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="4a9ca-187">Blazor sunucu tarafı uygulamaları için SignalR istemcisini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4a9ca-187">Configure the SignalR client for Blazor server-side apps</span></span>
 
<span data-ttu-id="4a9ca-188">Bazen, Blazor sunucu tarafı uygulamaları tarafından kullanılan SignalR istemcisini yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-188">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="4a9ca-189">Örneğin, bir bağlantı sorununu tanılamak için SignalR istemcisinde günlüğe kaydetmeyi yapılandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-189">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>
 
<span data-ttu-id="4a9ca-190">*/_Host. cshtml* dosyasında SignalR istemcisini yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="4a9ca-190">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="4a9ca-191">`autostart="false"` *Blazor. Server. js* betiği için `<script>` etikete bir öznitelik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-191">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="4a9ca-192">SignalR oluşturucuyu belirten bir yapılandırma nesnesi çağırın `Blazor.start` ve geçirin.</span><span class="sxs-lookup"><span data-stu-id="4a9ca-192">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>
 
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

## <a name="additional-resources"></a><span data-ttu-id="4a9ca-193">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4a9ca-193">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
