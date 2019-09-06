---
title: Blazor barındırma modellerini ASP.NET Core
author: guardrex
description: Blazor istemci tarafı ve sunucu tarafı barındırma modellerini anlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/hosting-models
ms.openlocfilehash: bf2bce4f89e8bfe6e5aeeb4860c85a60c5eb4b7c
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310412"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="15a41-103">Blazor barındırma modellerini ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15a41-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="15a41-104">[Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="15a41-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="15a41-105">Blazor, bir [Webassembly](https://webassembly.org/)tabanlı .NET çalışma zamanı (*Blazor istemci tarafı*) veya ASP.NET Core (*Blazor sunucu tarafı*) içindeki sunucu tarafında tarayıcıda istemci tarafı çalıştırmak için tasarlanan bir Web çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="15a41-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="15a41-106">Barındırma modelinden bağımsız olarak, uygulama ve bileşen modelleri *aynıdır*.</span><span class="sxs-lookup"><span data-stu-id="15a41-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="15a41-107">Bu makalede açıklanan barındırma modelleriyle ilgili bir proje oluşturmak için, bkz <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="15a41-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="client-side"></a><span data-ttu-id="15a41-108">İstemci tarafı</span><span class="sxs-lookup"><span data-stu-id="15a41-108">Client-side</span></span>

<span data-ttu-id="15a41-109">Blazor için sorumlu barındırma modeli, WebAssembly üzerinde tarayıcıda istemci tarafında çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="15a41-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="15a41-110">Blazor uygulaması, bağımlılıkları ve .NET çalışma zamanı tarayıcıya indirilir.</span><span class="sxs-lookup"><span data-stu-id="15a41-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="15a41-111">Uygulama doğrudan tarayıcı kullanıcı arabirimi iş parçacığında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="15a41-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="15a41-112">UI güncelleştirmeleri ve olay işleme aynı işlem içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="15a41-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="15a41-113">Uygulamanın varlıkları, istemcilere statik içerik sunan bir Web sunucusuna veya hizmete statik dosyalar olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="15a41-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor istemci tarafı: Blazor uygulaması, tarayıcı içindeki bir kullanıcı arabirimi iş parçacığında çalışır.](hosting-models/_static/client-side.png)

<span data-ttu-id="15a41-115">İstemci tarafı barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için, **Blazor WebAssembly uygulama** şablonunu ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)) kullanın.</span><span class="sxs-lookup"><span data-stu-id="15a41-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="15a41-116">**Blazor WebAssembly uygulama** şablonunu seçtikten sonra, **ASP.NET Core barındırılan** onay kutusunu ([DotNet New blazorwasm--hosted](/dotnet/core/tools/dotnet-new)) seçerek uygulamayı ASP.NET Core arka ucunu kullanacak şekilde yapılandırma seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="15a41-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="15a41-117">ASP.NET Core uygulaması, Blazor uygulamasını istemcilere sunar.</span><span class="sxs-lookup"><span data-stu-id="15a41-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="15a41-118">Blazor istemci tarafı uygulaması, Web API çağrıları veya [SignalR](xref:signalr/introduction)kullanarak ağ üzerinden sunucu ile etkileşime geçebilir.</span><span class="sxs-lookup"><span data-stu-id="15a41-118">The Blazor client-side app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="15a41-119">Şablonlar şunları ele alan *blazor. webassembly. js* betiğini içerir:</span><span class="sxs-lookup"><span data-stu-id="15a41-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="15a41-120">.NET çalışma zamanını, uygulamayı ve uygulamanın bağımlılıklarını indirme.</span><span class="sxs-lookup"><span data-stu-id="15a41-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="15a41-121">Uygulamayı çalıştırmak için çalışma zamanının başlatılması.</span><span class="sxs-lookup"><span data-stu-id="15a41-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="15a41-122">İstemci tarafı barındırma modeli çeşitli avantajlar sunar:</span><span class="sxs-lookup"><span data-stu-id="15a41-122">The client-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="15a41-123">.NET sunucu tarafı bağımlılığı yoktur.</span><span class="sxs-lookup"><span data-stu-id="15a41-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="15a41-124">Uygulama, istemciye indirildikten sonra tamamen çalışır.</span><span class="sxs-lookup"><span data-stu-id="15a41-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="15a41-125">İstemci kaynakları ve yetenekleri tamamen yararlanılabilir.</span><span class="sxs-lookup"><span data-stu-id="15a41-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="15a41-126">İş sunucudan istemciye boşaltılır.</span><span class="sxs-lookup"><span data-stu-id="15a41-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="15a41-127">Uygulamayı barındırmak için bir ASP.NET Core Web sunucusu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="15a41-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="15a41-128">Sunucusuz dağıtım senaryoları mümkündür (örneğin, bir CDN 'den uygulama sunma).</span><span class="sxs-lookup"><span data-stu-id="15a41-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="15a41-129">İstemci tarafı barındırma için aşağı taraf vardır:</span><span class="sxs-lookup"><span data-stu-id="15a41-129">There are downsides to client-side hosting:</span></span>

* <span data-ttu-id="15a41-130">Uygulama tarayıcının özelliklerine kısıtlıdır.</span><span class="sxs-lookup"><span data-stu-id="15a41-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="15a41-131">Uyumlu istemci donanımı ve yazılımı (örneğin, WebAssembly desteği) gereklidir.</span><span class="sxs-lookup"><span data-stu-id="15a41-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="15a41-132">İndirme boyutu daha büyüktür ve uygulamaların yüklenmesi daha uzun sürer.</span><span class="sxs-lookup"><span data-stu-id="15a41-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="15a41-133">.NET çalışma zamanı ve araç desteği daha az olgun.</span><span class="sxs-lookup"><span data-stu-id="15a41-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="15a41-134">Örneğin, [.NET Standard](/dotnet/standard/net-standard) desteğinin ve hata ayıklamada sınırlamalar mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="15a41-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="server-side"></a><span data-ttu-id="15a41-135">Sunucu tarafı</span><span class="sxs-lookup"><span data-stu-id="15a41-135">Server-side</span></span>

<span data-ttu-id="15a41-136">Sunucu tarafı barındırma modeliyle, uygulama sunucuda ASP.NET Core bir uygulama içinden yürütülür.</span><span class="sxs-lookup"><span data-stu-id="15a41-136">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="15a41-137">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrıları bir [SignalR](xref:signalr/introduction) bağlantısı üzerinden işlenir.</span><span class="sxs-lookup"><span data-stu-id="15a41-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Tarayıcı, bir SignalR bağlantısı üzerinden sunucusunda (bir ASP.NET Core uygulamasının içinde barındırılan) uygulamayla etkileşime girer.](hosting-models/_static/server-side.png)

<span data-ttu-id="15a41-139">Sunucu tarafı barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için ASP.NET Core **Blazor Server uygulama** şablonunu ([DotNet New blazorserver](/dotnet/core/tools/dotnet-new)) kullanın.</span><span class="sxs-lookup"><span data-stu-id="15a41-139">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="15a41-140">ASP.NET Core uygulaması, sunucu tarafı uygulamayı barındırır ve istemcilerin bağlanacağı SignalR uç noktasını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="15a41-140">The ASP.NET Core app hosts the server-side app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="15a41-141">ASP.NET Core uygulama, eklenecek uygulamanın `Startup` sınıfına başvurur:</span><span class="sxs-lookup"><span data-stu-id="15a41-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="15a41-142">Sunucu tarafı hizmetler.</span><span class="sxs-lookup"><span data-stu-id="15a41-142">Server-side services.</span></span>
* <span data-ttu-id="15a41-143">İstek işleme işlem hattının uygulaması.</span><span class="sxs-lookup"><span data-stu-id="15a41-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="15a41-144">*Blazor. Server. js* betiği&dagger; , istemci bağlantısını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="15a41-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="15a41-145">Uygulamanın, uygulama durumunu (örneğin, kayıp ağ bağlantısı durumunda) kalıcı hale getirmek ve geri yüklemek, uygulamanın sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="15a41-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="15a41-146">Sunucu tarafı barındırma modeli çeşitli avantajlar sunar:</span><span class="sxs-lookup"><span data-stu-id="15a41-146">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="15a41-147">İndirme boyutu bir istemci tarafı uygulamadan önemli ölçüde küçüktür ve uygulama çok daha hızlı yüklenir.</span><span class="sxs-lookup"><span data-stu-id="15a41-147">Download size is significantly smaller than a client-side app, and the app loads much faster.</span></span>
* <span data-ttu-id="15a41-148">Uygulama, .NET Core ile uyumlu API 'lerin kullanımı dahil olmak üzere sunucu olanaklarından tam olarak yararlanır.</span><span class="sxs-lookup"><span data-stu-id="15a41-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="15a41-149">Sunucuda .NET Core, uygulamayı çalıştırmak için kullanılır, bu nedenle hata ayıklama gibi mevcut .NET araçları beklendiği gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="15a41-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="15a41-150">Ölçülü istemciler desteklenir.</span><span class="sxs-lookup"><span data-stu-id="15a41-150">Thin clients are supported.</span></span> <span data-ttu-id="15a41-151">Örneğin, sunucu tarafı uygulamalar, WebAssembly ve kaynak kısıtlı cihazlarda bulunan tarayıcılarla çalışır.</span><span class="sxs-lookup"><span data-stu-id="15a41-151">For example, server-side apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="15a41-152">Uygulamanın bileşen kodu da dahilC# olmak üzere, uygulamanın .NET/kod tabanı istemcilere sunulmuyor.</span><span class="sxs-lookup"><span data-stu-id="15a41-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="15a41-153">Sunucu tarafı barındırma için aşağı taraf vardır:</span><span class="sxs-lookup"><span data-stu-id="15a41-153">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="15a41-154">Daha yüksek gecikme süresi genellikle vardır.</span><span class="sxs-lookup"><span data-stu-id="15a41-154">Higher latency usually exists.</span></span> <span data-ttu-id="15a41-155">Her Kullanıcı etkileşimi bir ağ atmasını içerir.</span><span class="sxs-lookup"><span data-stu-id="15a41-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="15a41-156">Çevrimdışı destek yoktur.</span><span class="sxs-lookup"><span data-stu-id="15a41-156">There's no offline support.</span></span> <span data-ttu-id="15a41-157">İstemci bağlantısı başarısız olursa, uygulama çalışmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="15a41-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="15a41-158">Ölçeklenebilirlik, çok sayıda kullanıcısı olan uygulamalar için zorlayıcı bir uygulamalardır.</span><span class="sxs-lookup"><span data-stu-id="15a41-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="15a41-159">Sunucunun birden çok istemci bağlantısını yönetmesi ve istemci durumunu işlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="15a41-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="15a41-160">Uygulamayı çalıştırmak için bir ASP.NET Core sunucusu gerekir.</span><span class="sxs-lookup"><span data-stu-id="15a41-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="15a41-161">Sunucusuz dağıtım senaryoları mümkün değildir (örneğin, bir CDN 'den uygulama sunma).</span><span class="sxs-lookup"><span data-stu-id="15a41-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="15a41-162">&dagger;*Blazor. Server. js* betiği ASP.NET Core paylaşılan çerçevede eklenmiş bir kaynaktan sunulur.</span><span class="sxs-lookup"><span data-stu-id="15a41-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="15a41-163">Aynı sunucuya yeniden bağlanma</span><span class="sxs-lookup"><span data-stu-id="15a41-163">Reconnection to the same server</span></span>

<span data-ttu-id="15a41-164">Blazor sunucu tarafı uygulamalar sunucuya etkin bir SignalR bağlantısı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="15a41-164">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="15a41-165">Bağlantı kaybolursa, uygulama sunucuya yeniden bağlanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="15a41-165">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="15a41-166">İstemcinin durumu hala bellekte olduğu sürece, istemci oturumu durum kaybı olmadan devam eder.</span><span class="sxs-lookup"><span data-stu-id="15a41-166">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>
 
<span data-ttu-id="15a41-167">İstemci bağlantının kaybolduğunu algıladığında, istemci yeniden bağlanmayı denediğinde kullanıcıya varsayılan bir kullanıcı arabirimi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="15a41-167">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="15a41-168">Yeniden bağlantı başarısız olursa, kullanıcıya yeniden deneme seçeneği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="15a41-168">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="15a41-169">Kullanıcı arabirimini özelleştirmek için, `components-reconnect-modal` *_host. cshtml* Razor sayfasında `id` olarak bir öğesi tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="15a41-169">To customize the UI, define an element with `components-reconnect-modal` as its `id` in the *_Host.cshtml* Razor page.</span></span> <span data-ttu-id="15a41-170">İstemci bu öğeyi bağlantı durumuna göre aşağıdaki CSS sınıflarından biriyle güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="15a41-170">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>
 
* <span data-ttu-id="15a41-171">`components-reconnect-show`&ndash; Bağlantının kaybolduğunu ve istemcinin yeniden bağlanmaya çalışıyor olduğunu göstermek için Kullanıcı arabirimini görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="15a41-171">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="15a41-172">`components-reconnect-hide`&ndash; İstemcinin etkin bir bağlantısı vardır ve Kullanıcı arabirimini gizleyin.</span><span class="sxs-lookup"><span data-stu-id="15a41-172">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="15a41-173">`components-reconnect-failed`&ndash; Yeniden bağlantı kurulamadı.</span><span class="sxs-lookup"><span data-stu-id="15a41-173">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="15a41-174">Yeniden bağlanmayı yeniden denemek için çağrısı `window.Blazor.reconnect()`yapın.</span><span class="sxs-lookup"><span data-stu-id="15a41-174">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="15a41-175">Prerendering sonrasında durum bilgisi olan yeniden bağlanma</span><span class="sxs-lookup"><span data-stu-id="15a41-175">Stateful reconnection after prerendering</span></span>
 
<span data-ttu-id="15a41-176">Blazor sunucu tarafında bulunan uygulamalar, sunucu bağlantısı yapılmadan önce sunucu üzerindeki kullanıcı arabirimini varsayılan olarak PreRender 'a ayarlar.</span><span class="sxs-lookup"><span data-stu-id="15a41-176">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="15a41-177">Bu, *_Host. cshtml* Razor sayfasında ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="15a41-177">This is set up in the *_Host.cshtml* Razor page:</span></span>
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="15a41-178">`RenderMode`bileşenin şunları yapıp kullanmadığını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="15a41-178">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="15a41-179">, Sayfaya ön gönderilir.</span><span class="sxs-lookup"><span data-stu-id="15a41-179">Is prerendered into the page.</span></span>
* <span data-ttu-id="15a41-180">, Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.</span><span class="sxs-lookup"><span data-stu-id="15a41-180">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="15a41-181">Açıklama</span><span class="sxs-lookup"><span data-stu-id="15a41-181">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="15a41-182">Bileşeni statik HTML olarak işler ve Blazor sunucu tarafı uygulaması için bir işaret içerir.</span><span class="sxs-lookup"><span data-stu-id="15a41-182">Renders the component into static HTML and includes a marker for a Blazor server-side app.</span></span> <span data-ttu-id="15a41-183">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasını önyüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="15a41-183">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="15a41-184">Parametreler desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="15a41-184">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="15a41-185">Blazor sunucu tarafı uygulaması için bir işaret oluşturur.</span><span class="sxs-lookup"><span data-stu-id="15a41-185">Renders a marker for a Blazor server-side app.</span></span> <span data-ttu-id="15a41-186">Bileşen çıkışı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="15a41-186">Output from the component isn't included.</span></span> <span data-ttu-id="15a41-187">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasını önyüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="15a41-187">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="15a41-188">Parametreler desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="15a41-188">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="15a41-189">Bileşeni statik HTML olarak işler.</span><span class="sxs-lookup"><span data-stu-id="15a41-189">Renders the component into static HTML.</span></span> <span data-ttu-id="15a41-190">Parametreler destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="15a41-190">Parameters are supported.</span></span> |

<span data-ttu-id="15a41-191">Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="15a41-191">Rendering server components from a static HTML page isn't supported.</span></span>
 
<span data-ttu-id="15a41-192">İstemci, uygulamayı PreRender 'da kullanılan aynı durum ile sunucuya yeniden bağlanır.</span><span class="sxs-lookup"><span data-stu-id="15a41-192">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="15a41-193">Uygulamanın durumu hala bellekte ise, SignalR bağlantısı kurulduktan sonra bileşen durumu tekrar verilmez.</span><span class="sxs-lookup"><span data-stu-id="15a41-193">If the app's state is still in memory, the component state isn't rerendered after the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="15a41-194">Razor sayfaları ve görünümlerinden durum bilgisi olan etkileşimli bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="15a41-194">Render stateful interactive components from Razor pages and views</span></span>
 
<span data-ttu-id="15a41-195">Durum bilgisi olan etkileşimli bileşenler Razor sayfasına veya görünümüne eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="15a41-195">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="15a41-196">Sayfa veya görünüm şunları işler:</span><span class="sxs-lookup"><span data-stu-id="15a41-196">When the page or view renders:</span></span>

* <span data-ttu-id="15a41-197">Bileşen sayfa veya görünümle birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="15a41-197">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="15a41-198">Prerendering için kullanılan ilk bileşen durumu kayboldu.</span><span class="sxs-lookup"><span data-stu-id="15a41-198">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="15a41-199">SignalR bağlantısı oluşturulduğunda yeni bileşen durumu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="15a41-199">New component state is created when the SignalR connection is established.</span></span>
 
<span data-ttu-id="15a41-200">Aşağıdaki Razor sayfası bir `Counter` bileşeni işler:</span><span class="sxs-lookup"><span data-stu-id="15a41-200">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>
 
@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="15a41-201">Razor sayfaları ve görünümlerinden etkileşimsiz bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="15a41-201">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="15a41-202">Aşağıdaki Razor sayfasında, `MyComponent` bileşen bir form kullanılarak belirtilen bir başlangıç değeriyle statik olarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="15a41-202">In the following Razor page, the `MyComponent` component is statically rendered with an initial value that's specified using a form:</span></span>
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>
 
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.Static, 
    new { InitialValue = InitialValue }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="15a41-203">Statik `MyComponent` olarak işlendiğinden bileşen etkileşimli olamaz.</span><span class="sxs-lookup"><span data-stu-id="15a41-203">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="15a41-204">Uygulamanın ne zaman prerendering olduğunu Algıla</span><span class="sxs-lookup"><span data-stu-id="15a41-204">Detect when the app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="15a41-205">Blazor sunucu tarafı uygulamaları için SignalR istemcisini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="15a41-205">Configure the SignalR client for Blazor server-side apps</span></span>
 
<span data-ttu-id="15a41-206">Bazen, Blazor sunucu tarafı uygulamaları tarafından kullanılan SignalR istemcisini yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="15a41-206">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="15a41-207">Örneğin, bir bağlantı sorununu tanılamak için SignalR istemcisinde günlüğe kaydetmeyi yapılandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="15a41-207">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>
 
<span data-ttu-id="15a41-208">*/_Host. cshtml* dosyasında SignalR istemcisini yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="15a41-208">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="15a41-209">`autostart="false"` *Blazor. Server. js* betiği için `<script>` etikete bir öznitelik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="15a41-209">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="15a41-210">SignalR oluşturucuyu belirten bir yapılandırma nesnesi çağırın `Blazor.start` ve geçirin.</span><span class="sxs-lookup"><span data-stu-id="15a41-210">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>
 
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

## <a name="additional-resources"></a><span data-ttu-id="15a41-211">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="15a41-211">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
