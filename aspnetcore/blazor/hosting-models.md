---
title: Blazor barındırma modellerini ASP.NET Core
author: guardrex
description: Blazor istemci tarafı ve sunucu tarafı barındırma modellerini anlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/07/2019
uid: blazor/hosting-models
ms.openlocfilehash: 7880affa59af1fa4fc47aee3dc98ae9aa53729af
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878349"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="6873d-103">Blazor barındırma modellerini ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6873d-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="6873d-104">[Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="6873d-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="6873d-105">Blazor, bir [Webassembly](https://webassembly.org/)tabanlı .NET çalışma zamanı (*Blazor istemci tarafı*) veya ASP.NET Core (*Blazor sunucu tarafı*) içindeki sunucu tarafında tarayıcıda istemci tarafı çalıştırmak için tasarlanan bir Web çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="6873d-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="6873d-106">Barındırma modelinden bağımsız olarak, uygulama ve bileşen modelleri *aynıdır*.</span><span class="sxs-lookup"><span data-stu-id="6873d-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="6873d-107">Bu makalede açıklanan barındırma modelleriyle ilgili bir proje oluşturmak için, bkz <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="6873d-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="client-side"></a><span data-ttu-id="6873d-108">İstemci tarafı</span><span class="sxs-lookup"><span data-stu-id="6873d-108">Client-side</span></span>

<span data-ttu-id="6873d-109">Blazor için sorumlu barındırma modeli, WebAssembly üzerinde tarayıcıda istemci tarafında çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="6873d-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="6873d-110">Blazor uygulaması, bağımlılıkları ve .NET çalışma zamanı tarayıcıya indirilir.</span><span class="sxs-lookup"><span data-stu-id="6873d-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="6873d-111">Uygulama doğrudan tarayıcı kullanıcı arabirimi iş parçacığında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="6873d-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="6873d-112">UI güncelleştirmeleri ve olay işleme aynı işlem içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="6873d-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="6873d-113">Uygulamanın varlıkları, istemcilere statik içerik sunan bir Web sunucusuna veya hizmete statik dosyalar olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="6873d-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor istemci tarafı: Blazor uygulaması, tarayıcı içindeki bir kullanıcı arabirimi iş parçacığında çalışır.](hosting-models/_static/client-side.png)

<span data-ttu-id="6873d-115">İstemci tarafı barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için, **Blazor WebAssembly uygulama** şablonunu ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)) kullanın.</span><span class="sxs-lookup"><span data-stu-id="6873d-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="6873d-116">**Blazor WebAssembly uygulama** şablonunu seçtikten sonra, **ASP.NET Core barındırılan** onay kutusunu ([DotNet New blazorwasm--hosted](/dotnet/core/tools/dotnet-new)) seçerek uygulamayı ASP.NET Core arka ucunu kullanacak şekilde yapılandırma seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="6873d-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="6873d-117">ASP.NET Core uygulaması, Blazor uygulamasını istemcilere sunar.</span><span class="sxs-lookup"><span data-stu-id="6873d-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="6873d-118">Blazor istemci tarafı uygulaması, Web API çağrıları veya [SignalR](xref:signalr/introduction)kullanarak ağ üzerinden sunucu ile etkileşime geçebilir.</span><span class="sxs-lookup"><span data-stu-id="6873d-118">The Blazor client-side app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="6873d-119">Şablonlar şunları ele alan *blazor. webassembly. js* betiğini içerir:</span><span class="sxs-lookup"><span data-stu-id="6873d-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="6873d-120">.NET çalışma zamanını, uygulamayı ve uygulamanın bağımlılıklarını indirme.</span><span class="sxs-lookup"><span data-stu-id="6873d-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="6873d-121">Uygulamayı çalıştırmak için çalışma zamanının başlatılması.</span><span class="sxs-lookup"><span data-stu-id="6873d-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="6873d-122">İstemci tarafı barındırma modeli çeşitli avantajlar sunar:</span><span class="sxs-lookup"><span data-stu-id="6873d-122">The client-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="6873d-123">.NET sunucu tarafı bağımlılığı yoktur.</span><span class="sxs-lookup"><span data-stu-id="6873d-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="6873d-124">Uygulama, istemciye indirildikten sonra tamamen çalışır.</span><span class="sxs-lookup"><span data-stu-id="6873d-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="6873d-125">İstemci kaynakları ve yetenekleri tamamen yararlanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6873d-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="6873d-126">İş sunucudan istemciye boşaltılır.</span><span class="sxs-lookup"><span data-stu-id="6873d-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="6873d-127">Uygulamayı barındırmak için bir ASP.NET Core Web sunucusu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="6873d-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="6873d-128">Sunucusuz dağıtım senaryoları mümkündür (örneğin, bir CDN 'den uygulama sunma).</span><span class="sxs-lookup"><span data-stu-id="6873d-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="6873d-129">İstemci tarafı barındırma için aşağı taraf vardır:</span><span class="sxs-lookup"><span data-stu-id="6873d-129">There are downsides to client-side hosting:</span></span>

* <span data-ttu-id="6873d-130">Uygulama tarayıcının özelliklerine kısıtlıdır.</span><span class="sxs-lookup"><span data-stu-id="6873d-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="6873d-131">Uyumlu istemci donanımı ve yazılımı (örneğin, WebAssembly desteği) gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6873d-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="6873d-132">İndirme boyutu daha büyüktür ve uygulamaların yüklenmesi daha uzun sürer.</span><span class="sxs-lookup"><span data-stu-id="6873d-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="6873d-133">.NET çalışma zamanı ve araç desteği daha az olgun.</span><span class="sxs-lookup"><span data-stu-id="6873d-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="6873d-134">Örneğin, [.NET Standard](/dotnet/standard/net-standard) desteğinin ve hata ayıklamada sınırlamalar mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="6873d-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="server-side"></a><span data-ttu-id="6873d-135">Sunucu tarafı</span><span class="sxs-lookup"><span data-stu-id="6873d-135">Server-side</span></span>

<span data-ttu-id="6873d-136">Sunucu tarafı barındırma modeliyle, uygulama sunucuda ASP.NET Core bir uygulama içinden yürütülür.</span><span class="sxs-lookup"><span data-stu-id="6873d-136">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="6873d-137">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrıları bir [SignalR](xref:signalr/introduction) bağlantısı üzerinden işlenir.</span><span class="sxs-lookup"><span data-stu-id="6873d-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Tarayıcı, bir SignalR bağlantısı üzerinden sunucusunda (bir ASP.NET Core uygulamasının içinde barındırılan) uygulamayla etkileşime girer.](hosting-models/_static/server-side.png)

<span data-ttu-id="6873d-139">Sunucu tarafı barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için ASP.NET Core **Blazor Server uygulama** şablonunu ([DotNet New blazorserver](/dotnet/core/tools/dotnet-new)) kullanın.</span><span class="sxs-lookup"><span data-stu-id="6873d-139">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="6873d-140">ASP.NET Core uygulaması, sunucu tarafı uygulamayı barındırır ve istemcilerin bağlanacağı SignalR uç noktasını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6873d-140">The ASP.NET Core app hosts the server-side app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="6873d-141">ASP.NET Core uygulama, eklenecek uygulamanın `Startup` sınıfına başvurur:</span><span class="sxs-lookup"><span data-stu-id="6873d-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="6873d-142">Sunucu tarafı hizmetler.</span><span class="sxs-lookup"><span data-stu-id="6873d-142">Server-side services.</span></span>
* <span data-ttu-id="6873d-143">İstek işleme işlem hattının uygulaması.</span><span class="sxs-lookup"><span data-stu-id="6873d-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="6873d-144">*Blazor. Server. js* betiği&dagger; , istemci bağlantısını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6873d-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="6873d-145">Uygulamanın, uygulama durumunu (örneğin, kayıp ağ bağlantısı durumunda) kalıcı hale getirmek ve geri yüklemek, uygulamanın sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="6873d-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="6873d-146">Sunucu tarafı barındırma modeli çeşitli avantajlar sunar:</span><span class="sxs-lookup"><span data-stu-id="6873d-146">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="6873d-147">İndirme boyutu bir istemci tarafı uygulamadan önemli ölçüde küçüktür ve uygulama çok daha hızlı yüklenir.</span><span class="sxs-lookup"><span data-stu-id="6873d-147">Download size is significantly smaller than a client-side app, and the app loads much faster.</span></span>
* <span data-ttu-id="6873d-148">Uygulama, .NET Core ile uyumlu API 'lerin kullanımı dahil olmak üzere sunucu olanaklarından tam olarak yararlanır.</span><span class="sxs-lookup"><span data-stu-id="6873d-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="6873d-149">Sunucuda .NET Core, uygulamayı çalıştırmak için kullanılır, bu nedenle hata ayıklama gibi mevcut .NET araçları beklendiği gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="6873d-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="6873d-150">Ölçülü istemciler desteklenir.</span><span class="sxs-lookup"><span data-stu-id="6873d-150">Thin clients are supported.</span></span> <span data-ttu-id="6873d-151">Örneğin, sunucu tarafı uygulamalar, WebAssembly ve kaynak kısıtlı cihazlarda bulunan tarayıcılarla çalışır.</span><span class="sxs-lookup"><span data-stu-id="6873d-151">For example, server-side apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="6873d-152">Uygulamanın bileşen kodu da dahilC# olmak üzere, uygulamanın .NET/kod tabanı istemcilere sunulmuyor.</span><span class="sxs-lookup"><span data-stu-id="6873d-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="6873d-153">Sunucu tarafı barındırma için aşağı taraf vardır:</span><span class="sxs-lookup"><span data-stu-id="6873d-153">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="6873d-154">Daha yüksek gecikme süresi genellikle vardır.</span><span class="sxs-lookup"><span data-stu-id="6873d-154">Higher latency usually exists.</span></span> <span data-ttu-id="6873d-155">Her Kullanıcı etkileşimi bir ağ atmasını içerir.</span><span class="sxs-lookup"><span data-stu-id="6873d-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="6873d-156">Çevrimdışı destek yoktur.</span><span class="sxs-lookup"><span data-stu-id="6873d-156">There's no offline support.</span></span> <span data-ttu-id="6873d-157">İstemci bağlantısı başarısız olursa, uygulama çalışmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="6873d-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="6873d-158">Ölçeklenebilirlik, çok sayıda kullanıcısı olan uygulamalar için zorlayıcı bir uygulamalardır.</span><span class="sxs-lookup"><span data-stu-id="6873d-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="6873d-159">Sunucunun birden çok istemci bağlantısını yönetmesi ve istemci durumunu işlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6873d-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="6873d-160">Uygulamayı çalıştırmak için bir ASP.NET Core sunucusu gerekir.</span><span class="sxs-lookup"><span data-stu-id="6873d-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="6873d-161">Sunucusuz dağıtım senaryoları mümkün değildir (örneğin, bir CDN 'den uygulama sunma).</span><span class="sxs-lookup"><span data-stu-id="6873d-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="6873d-162">&dagger;*Blazor. Server. js* betiği ASP.NET Core paylaşılan çerçevede eklenmiş bir kaynaktan sunulur.</span><span class="sxs-lookup"><span data-stu-id="6873d-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="6873d-163">Sunucu tarafından işlenmiş Kullanıcı arabirimine karşılaştırma</span><span class="sxs-lookup"><span data-stu-id="6873d-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="6873d-164">Blazor Server uygulamalarını anlamanın bir yolu, Razor görünümlerini veya Razor Pages kullanarak ASP.NET Core uygulamalarda Kullanıcı arabirimi oluşturma için geleneksel modellerden nasıl farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="6873d-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="6873d-165">Her iki model de, HTML içeriğini anlatmak için Razor dilini kullanır, ancak biçimlendirmenin nasıl işlendiği konusunda önemli ölçüde farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="6873d-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="6873d-166">Bir Razor sayfası veya görünüm işlendiğinde, her Razor kodu satırı metin biçiminde HTML yayar.</span><span class="sxs-lookup"><span data-stu-id="6873d-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="6873d-167">Oluşturulduktan sonra sunucu, üretilen herhangi bir durum da dahil olmak üzere sayfayı veya görünüm örneğini ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="6873d-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="6873d-168">Sayfa için başka bir istek gerçekleştiğinde, örneğin sunucu doğrulaması başarısız olduğunda ve doğrulama özeti görüntülendiğinde:</span><span class="sxs-lookup"><span data-stu-id="6873d-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="6873d-169">Sayfanın tamamı HTML metnine yeniden eklenir.</span><span class="sxs-lookup"><span data-stu-id="6873d-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="6873d-170">Sayfa istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="6873d-170">The page is sent to the client.</span></span>

<span data-ttu-id="6873d-171">Blazor uygulaması, *bileşen*olarak adlandırılan UI 'ın yeniden kullanılabilir öğelerinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="6873d-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="6873d-172">Bir bileşen kod C# , biçimlendirme ve diğer bileşenleri içerir.</span><span class="sxs-lookup"><span data-stu-id="6873d-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="6873d-173">Bir bileşen işlendiğinde, Blazor bir HTML veya XML Belge Nesne Modeli (DOM) gibi dahil edilen bileşenlerin bir grafiğini üretir.</span><span class="sxs-lookup"><span data-stu-id="6873d-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="6873d-174">Bu grafik, özelliklerde ve alanlarında tutulan bileşen durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="6873d-174">This graph includes component state held in properties and fields.</span></span> <span data-ttu-id="6873d-175">Blazor, biçimlendirmenin ikili gösterimini üretmek için bileşen grafiğini değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="6873d-175">Blazor evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="6873d-176">İkili biçimi şu şekilde olabilir:</span><span class="sxs-lookup"><span data-stu-id="6873d-176">The binary format can be:</span></span>

* <span data-ttu-id="6873d-177">HTML metnine açıldı (prerendering sırasında).</span><span class="sxs-lookup"><span data-stu-id="6873d-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="6873d-178">Düzenli işleme sırasında biçimlendirmeyi verimli bir şekilde güncelleştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6873d-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="6873d-179">Blazor içinde bir kullanıcı arabirimi güncelleştirmesi şu şekilde tetiklenir:</span><span class="sxs-lookup"><span data-stu-id="6873d-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="6873d-180">Düğme seçme gibi kullanıcı etkileşimi.</span><span class="sxs-lookup"><span data-stu-id="6873d-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="6873d-181">Zamanlayıcı gibi uygulama Tetikleyicileri.</span><span class="sxs-lookup"><span data-stu-id="6873d-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="6873d-182">Grafik yeniden tanımlanır ve bir UI *farkı* (fark) hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="6873d-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="6873d-183">Bu fark, istemcideki Kullanıcı arabirimini güncelleştirmek için gereken en küçük DOM düzenlemelerinin kümesidir.</span><span class="sxs-lookup"><span data-stu-id="6873d-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="6873d-184">Fark istemciye bir ikili biçimde gönderilir ve tarayıcı tarafından uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6873d-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="6873d-185">Kullanıcı, istemci üzerinde bundan uzaklaştığında bir bileşen atılmış olur.</span><span class="sxs-lookup"><span data-stu-id="6873d-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="6873d-186">Bir Kullanıcı bir bileşenle etkileşim kurarken, bileşenin durumu (hizmetler, kaynaklar) sunucunun belleğinde tutulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6873d-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="6873d-187">Birçok bileşenin durumu sunucu tarafından eşzamanlı olarak Korunabileceğinden, bellek tükenmesi sorunu ele alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6873d-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="6873d-188">Sunucu belleğinin en iyi şekilde kullanılmasını sağlamak üzere bir Blazor sunucu uygulamasının nasıl yazılacağı hakkında yönergeler için bkz <xref:security/blazor/server-side>.</span><span class="sxs-lookup"><span data-stu-id="6873d-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server-side>.</span></span>

### <a name="circuits"></a><span data-ttu-id="6873d-189">Uygulanıp</span><span class="sxs-lookup"><span data-stu-id="6873d-189">Circuits</span></span>

<span data-ttu-id="6873d-190">Bir Blazor sunucu uygulaması [ASP.NET Core SignalR](xref:signalr/introduction)'nin üzerine kurulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6873d-190">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="6873d-191">Her istemci, *devre*adlı bir veya daha fazla SignalR bağlantısı üzerinden sunucusuyla iletişim kurar.</span><span class="sxs-lookup"><span data-stu-id="6873d-191">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="6873d-192">Bir devre, geçici ağ kesintilerine tolerans sağlayan SignalR bağlantıları üzerinden Blazor.</span><span class="sxs-lookup"><span data-stu-id="6873d-192">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="6873d-193">Bir Blazor istemcisi, SignalR bağlantısının kesileceğini gördüğünde, yeni bir SignalR bağlantısı kullanarak sunucuya yeniden bağlanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="6873d-193">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="6873d-194">Bir Blazor sunucu uygulamasına bağlı her tarayıcı ekranı (tarayıcı sekmesi veya iframe) bir SignalR bağlantısı kullanır.</span><span class="sxs-lookup"><span data-stu-id="6873d-194">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="6873d-195">Bu, tipik sunucu tarafından işlenmiş uygulamalarla karşılaştırıldığında daha önemli bir ayırım ifade etmiştir.</span><span class="sxs-lookup"><span data-stu-id="6873d-195">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="6873d-196">Sunucu tarafından işlenen bir uygulamada, aynı uygulamayı birden çok tarayıcı ekranında açmak genellikle sunucuda ek kaynak taleplerine çevirilmez.</span><span class="sxs-lookup"><span data-stu-id="6873d-196">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="6873d-197">Bir Blazor sunucu uygulamasında, her tarayıcı ekranı, sunucu tarafından yönetilecek ayrı bir devre ve bileşen durumunun ayrı örneklerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6873d-197">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

<span data-ttu-id="6873d-198">Blazor bir tarayıcı sekmesini kapatmayı veya bir dış URL 'ye gidilerek *düzgün* bir şekilde sonlandırılmasını kabul eder.</span><span class="sxs-lookup"><span data-stu-id="6873d-198">Blazor considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="6873d-199">Düzgün sonlandırma durumunda, devre ve ilişkili kaynaklar hemen serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="6873d-199">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="6873d-200">Bir istemci, örneğin bir ağ kesintisi nedeniyle düzgün şekilde kesilmeyen bir şekilde kesilebilir.</span><span class="sxs-lookup"><span data-stu-id="6873d-200">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> <span data-ttu-id="6873d-201">Blazor sunucusu, istemcinin yeniden bağlanmasına izin vermek üzere yapılandırılabilir bir Aralık için bağlantısı kesilen devreleri depolar.</span><span class="sxs-lookup"><span data-stu-id="6873d-201">Blazor Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="6873d-202">Daha fazla bilgi için [aynı sunucuya yeniden bağlanma](#reconnection-to-the-same-server) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="6873d-202">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="6873d-203">UI gecikmesi</span><span class="sxs-lookup"><span data-stu-id="6873d-203">UI Latency</span></span>

<span data-ttu-id="6873d-204">UI gecikme süresi, başlatılan bir eylemden Kullanıcı arabiriminin güncelleştirildiği zamana kadar geçen süredir.</span><span class="sxs-lookup"><span data-stu-id="6873d-204">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="6873d-205">Bir uygulamanın kullanıcıya yanıt vermesi için kullanıcı ARABIRIMI gecikmesi için daha küçük değerler zorunludur.</span><span class="sxs-lookup"><span data-stu-id="6873d-205">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="6873d-206">Bir Blazor sunucu uygulamasında her bir eylem sunucuya gönderilir, işlenir ve bir UI farkı geri gönderilir.</span><span class="sxs-lookup"><span data-stu-id="6873d-206">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="6873d-207">Sonuç olarak, UI gecikmesi ağ gecikme süresinin toplamı ve eylemi işlerken sunucu gecikmesi sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="6873d-207">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="6873d-208">Özel bir kurumsal ağla sınırlı bir iş kolu uygulaması için, ağ gecikmesi nedeniyle kullanıcı gecikmesi algılarını üzerindeki etki, genellikle çok sayıda CEPSİZ olur.</span><span class="sxs-lookup"><span data-stu-id="6873d-208">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="6873d-209">Internet üzerinden dağıtılan bir uygulama için, özellikle de kullanıcılar coğrafi olarak coğrafi olarak dağıtılmışsa gecikme süresi kullanıcılara karşı farklılık gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="6873d-209">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="6873d-210">Bellek kullanımı ayrıca uygulama gecikme süresine de katkıda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="6873d-210">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="6873d-211">Daha fazla bellek kullanımı, her ikisi de uygulama performansının düşmesine neden olan ve bu nedenle kullanıcı arabirimi gecikmesini arttığı diskte sık görülen çöp toplama veya disk belleği belleği</span><span class="sxs-lookup"><span data-stu-id="6873d-211">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="6873d-212">Daha fazla bilgi için bkz. <xref:security/blazor/server-side>.</span><span class="sxs-lookup"><span data-stu-id="6873d-212">For more information, see <xref:security/blazor/server-side>.</span></span>

<span data-ttu-id="6873d-213">Blazor sunucu uygulamaları, ağ gecikmesini ve bellek kullanımını azaltarak UI gecikmesini en aza indirmek için iyileştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="6873d-213">Blazor Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="6873d-214">Ağ gecikmesini ölçmeye yönelik bir yaklaşım için bkz <xref:host-and-deploy/blazor/server-side#measure-network-latency>.</span><span class="sxs-lookup"><span data-stu-id="6873d-214">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server-side#measure-network-latency>.</span></span> <span data-ttu-id="6873d-215">SignalR ve Blazor hakkında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="6873d-215">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server-side>
* <xref:security/blazor/server-side>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="6873d-216">Aynı sunucuya yeniden bağlanma</span><span class="sxs-lookup"><span data-stu-id="6873d-216">Reconnection to the same server</span></span>

<span data-ttu-id="6873d-217">Blazor sunucu tarafı uygulamalar sunucuya etkin bir SignalR bağlantısı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6873d-217">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="6873d-218">Bağlantı kaybolursa, uygulama sunucuya yeniden bağlanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="6873d-218">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="6873d-219">İstemcinin durumu hala bellekte olduğu sürece, istemci oturumu durum kaybı olmadan devam eder.</span><span class="sxs-lookup"><span data-stu-id="6873d-219">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

<span data-ttu-id="6873d-220">İstemci bağlantının kaybolduğunu algıladığında, istemci yeniden bağlanmayı denediğinde kullanıcıya varsayılan bir kullanıcı arabirimi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6873d-220">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="6873d-221">Yeniden bağlantı başarısız olursa, kullanıcıya yeniden deneme seçeneği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6873d-221">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="6873d-222">Kullanıcı arabirimini özelleştirmek için, `components-reconnect-modal` *_host. cshtml* Razor sayfasında `id` olarak bir öğesi tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="6873d-222">To customize the UI, define an element with `components-reconnect-modal` as its `id` in the *_Host.cshtml* Razor page.</span></span> <span data-ttu-id="6873d-223">İstemci bu öğeyi bağlantı durumuna göre aşağıdaki CSS sınıflarından biriyle güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="6873d-223">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>

* <span data-ttu-id="6873d-224">`components-reconnect-show`&ndash; Bağlantının kaybolduğunu ve istemcinin yeniden bağlanmaya çalışıyor olduğunu göstermek için Kullanıcı arabirimini görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="6873d-224">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="6873d-225">`components-reconnect-hide`&ndash; İstemcinin etkin bir bağlantısı vardır ve Kullanıcı arabirimini gizleyin.</span><span class="sxs-lookup"><span data-stu-id="6873d-225">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="6873d-226">`components-reconnect-failed`&ndash; Yeniden bağlantı kurulamadı.</span><span class="sxs-lookup"><span data-stu-id="6873d-226">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="6873d-227">Yeniden bağlanmayı yeniden denemek için çağrısı `window.Blazor.reconnect()`yapın.</span><span class="sxs-lookup"><span data-stu-id="6873d-227">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="6873d-228">Prerendering sonrasında durum bilgisi olan yeniden bağlanma</span><span class="sxs-lookup"><span data-stu-id="6873d-228">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="6873d-229">Blazor sunucu tarafında bulunan uygulamalar, sunucu bağlantısı yapılmadan önce sunucu üzerindeki kullanıcı arabirimini varsayılan olarak PreRender 'a ayarlar.</span><span class="sxs-lookup"><span data-stu-id="6873d-229">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="6873d-230">Bu, *_Host. cshtml* Razor sayfasında ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="6873d-230">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="6873d-231">`RenderMode`bileşenin şunları yapıp kullanmadığını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="6873d-231">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="6873d-232">, Sayfaya ön gönderilir.</span><span class="sxs-lookup"><span data-stu-id="6873d-232">Is prerendered into the page.</span></span>
* <span data-ttu-id="6873d-233">, Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.</span><span class="sxs-lookup"><span data-stu-id="6873d-233">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="6873d-234">Açıklama</span><span class="sxs-lookup"><span data-stu-id="6873d-234">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="6873d-235">Bileşeni statik HTML olarak işler ve Blazor sunucu tarafı uygulaması için bir işaret içerir.</span><span class="sxs-lookup"><span data-stu-id="6873d-235">Renders the component into static HTML and includes a marker for a Blazor server-side app.</span></span> <span data-ttu-id="6873d-236">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasını önyüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6873d-236">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="6873d-237">Parametreler desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="6873d-237">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="6873d-238">Blazor sunucu tarafı uygulaması için bir işaret oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6873d-238">Renders a marker for a Blazor server-side app.</span></span> <span data-ttu-id="6873d-239">Bileşen çıkışı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="6873d-239">Output from the component isn't included.</span></span> <span data-ttu-id="6873d-240">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasını önyüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6873d-240">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="6873d-241">Parametreler desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="6873d-241">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="6873d-242">Bileşeni statik HTML olarak işler.</span><span class="sxs-lookup"><span data-stu-id="6873d-242">Renders the component into static HTML.</span></span> <span data-ttu-id="6873d-243">Parametreler destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="6873d-243">Parameters are supported.</span></span> |

<span data-ttu-id="6873d-244">Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="6873d-244">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="6873d-245">İstemci, uygulamayı PreRender 'da kullanılan aynı durum ile sunucuya yeniden bağlanır.</span><span class="sxs-lookup"><span data-stu-id="6873d-245">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="6873d-246">Uygulamanın durumu hala bellekte ise, SignalR bağlantısı kurulduktan sonra bileşen durumu tekrar verilmez.</span><span class="sxs-lookup"><span data-stu-id="6873d-246">If the app's state is still in memory, the component state isn't rerendered after the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="6873d-247">Razor sayfaları ve görünümlerinden durum bilgisi olan etkileşimli bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="6873d-247">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="6873d-248">Durum bilgisi olan etkileşimli bileşenler Razor sayfasına veya görünümüne eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="6873d-248">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="6873d-249">Sayfa veya görünüm şunları işler:</span><span class="sxs-lookup"><span data-stu-id="6873d-249">When the page or view renders:</span></span>

* <span data-ttu-id="6873d-250">Bileşen sayfa veya görünümle birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6873d-250">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="6873d-251">Prerendering için kullanılan ilk bileşen durumu kayboldu.</span><span class="sxs-lookup"><span data-stu-id="6873d-251">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="6873d-252">SignalR bağlantısı oluşturulduğunda yeni bileşen durumu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6873d-252">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="6873d-253">Aşağıdaki Razor sayfası bir `Counter` bileşeni işler:</span><span class="sxs-lookup"><span data-stu-id="6873d-253">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="6873d-254">Razor sayfaları ve görünümlerinden etkileşimsiz bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="6873d-254">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="6873d-255">Aşağıdaki Razor sayfasında, `MyComponent` bileşen bir form kullanılarak belirtilen bir başlangıç değeriyle statik olarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="6873d-255">In the following Razor page, the `MyComponent` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="6873d-256">Statik `MyComponent` olarak işlendiğinden bileşen etkileşimli olamaz.</span><span class="sxs-lookup"><span data-stu-id="6873d-256">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="6873d-257">Uygulamanın ne zaman prerendering olduğunu Algıla</span><span class="sxs-lookup"><span data-stu-id="6873d-257">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="6873d-258">Blazor sunucu tarafı uygulamaları için SignalR istemcisini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6873d-258">Configure the SignalR client for Blazor server-side apps</span></span>

<span data-ttu-id="6873d-259">Bazen, Blazor sunucu tarafı uygulamaları tarafından kullanılan SignalR istemcisini yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6873d-259">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="6873d-260">Örneğin, bir bağlantı sorununu tanılamak için SignalR istemcisinde günlüğe kaydetmeyi yapılandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6873d-260">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="6873d-261">*/_Host. cshtml* dosyasında SignalR istemcisini yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="6873d-261">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="6873d-262">`autostart="false"` *Blazor. Server. js* betiği için `<script>` etikete bir öznitelik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6873d-262">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="6873d-263">SignalR oluşturucuyu belirten bir yapılandırma nesnesi çağırın `Blazor.start` ve geçirin.</span><span class="sxs-lookup"><span data-stu-id="6873d-263">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="6873d-264">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6873d-264">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
