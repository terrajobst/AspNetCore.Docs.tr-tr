---
title: Blazor barındırma modellerini ASP.NET Core
author: guardrex
description: Blazor WebAssembly ve Blazor sunucusu barındırma modellerini anlayın.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: 7b4d4aca0bc4650c31bc8e5c4a84ecbad6a49b09
ms.sourcegitcommit: 0e21d4f8111743bcb205a2ae0f8e57910c3e8c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77034097"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="39dec-103">Blazor barındırma modellerini ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="39dec-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="39dec-104">[Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="39dec-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="39dec-105">Blazor, bir [Webassembly](https://webassembly.org/)tabanlı .NET Runtime (*Blazor webassembly*) veya ASP.NET Core (*Blazor Server*) içindeki sunucu tarafında tarayıcıda istemci tarafı çalıştırmak için tasarlanan bir Web çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="39dec-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor WebAssembly*) or server-side in ASP.NET Core (*Blazor Server*).</span></span> <span data-ttu-id="39dec-106">Barındırma modelinden bağımsız olarak, uygulama ve bileşen modelleri *aynıdır*.</span><span class="sxs-lookup"><span data-stu-id="39dec-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="39dec-107">Bu makalede açıklanan barındırma modelleriyle ilgili bir proje oluşturmak için, bkz. <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="39dec-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="39dec-108">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="39dec-108">Blazor WebAssembly</span></span>

<span data-ttu-id="39dec-109">Blazor için sorumlu barındırma modeli, WebAssembly üzerinde tarayıcıda istemci tarafında çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="39dec-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="39dec-110">Blazor uygulaması, bağımlılıkları ve .NET çalışma zamanı tarayıcıya indirilir.</span><span class="sxs-lookup"><span data-stu-id="39dec-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="39dec-111">Uygulama doğrudan tarayıcı kullanıcı arabirimi iş parçacığında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="39dec-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="39dec-112">UI güncelleştirmeleri ve olay işleme aynı işlem içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="39dec-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="39dec-113">Uygulamanın varlıkları, istemcilere statik içerik sunan bir Web sunucusuna veya hizmete statik dosyalar olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="39dec-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor WebAssembly: Blazor uygulaması tarayıcının içindeki bir kullanıcı arabirimi iş parçacığında çalışır.](hosting-models/_static/blazor-webassembly.png)

<span data-ttu-id="39dec-115">İstemci tarafı barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için, **Blazor WebAssembly uygulama** şablonunu ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)) kullanın.</span><span class="sxs-lookup"><span data-stu-id="39dec-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="39dec-116">**Blazor WebAssembly uygulama** şablonunu seçtikten sonra, **ASP.NET Core barındırılan** onay kutusunu ([DotNet New blazorwasm--hosted](/dotnet/core/tools/dotnet-new)) seçerek uygulamayı ASP.NET Core arka ucunu kullanacak şekilde yapılandırma seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="39dec-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="39dec-117">ASP.NET Core uygulaması, Blazor uygulamasını istemcilere sunar.</span><span class="sxs-lookup"><span data-stu-id="39dec-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="39dec-118">Blazor WebAssembly uygulaması, Web API çağrıları veya [SignalR](xref:signalr/introduction) (<xref:tutorials/signalr-blazor-webassembly>) kullanarak ağ üzerinden sunucu ile etkileşime geçebilir.</span><span class="sxs-lookup"><span data-stu-id="39dec-118">The Blazor WebAssembly app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction) (<xref:tutorials/signalr-blazor-webassembly>).</span></span>

<span data-ttu-id="39dec-119">Şablonlar aşağıdakileri işleyecek `blazor.webassembly.js` betiği içerir:</span><span class="sxs-lookup"><span data-stu-id="39dec-119">The templates include the `blazor.webassembly.js` script that handles:</span></span>

* <span data-ttu-id="39dec-120">.NET çalışma zamanını, uygulamayı ve uygulamanın bağımlılıklarını indirme.</span><span class="sxs-lookup"><span data-stu-id="39dec-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="39dec-121">Uygulamayı çalıştırmak için çalışma zamanının başlatılması.</span><span class="sxs-lookup"><span data-stu-id="39dec-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="39dec-122">Blazor WebAssembly barındırma modeli çeşitli avantajlar sunar:</span><span class="sxs-lookup"><span data-stu-id="39dec-122">The Blazor WebAssembly hosting model offers several benefits:</span></span>

* <span data-ttu-id="39dec-123">.NET sunucu tarafı bağımlılığı yoktur.</span><span class="sxs-lookup"><span data-stu-id="39dec-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="39dec-124">Uygulama, istemciye indirildikten sonra tamamen çalışır.</span><span class="sxs-lookup"><span data-stu-id="39dec-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="39dec-125">İstemci kaynakları ve yetenekleri tamamen yararlanılabilir.</span><span class="sxs-lookup"><span data-stu-id="39dec-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="39dec-126">İş sunucudan istemciye boşaltılır.</span><span class="sxs-lookup"><span data-stu-id="39dec-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="39dec-127">Uygulamayı barındırmak için bir ASP.NET Core Web sunucusu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="39dec-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="39dec-128">Sunucusuz dağıtım senaryoları mümkündür (örneğin, bir CDN 'den uygulama sunma).</span><span class="sxs-lookup"><span data-stu-id="39dec-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="39dec-129">Blazor WebAssembly barındırması için aşağı taraf vardır:</span><span class="sxs-lookup"><span data-stu-id="39dec-129">There are downsides to Blazor WebAssembly hosting:</span></span>

* <span data-ttu-id="39dec-130">Uygulama tarayıcının özelliklerine kısıtlıdır.</span><span class="sxs-lookup"><span data-stu-id="39dec-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="39dec-131">Uyumlu istemci donanımı ve yazılımı (örneğin, WebAssembly desteği) gereklidir.</span><span class="sxs-lookup"><span data-stu-id="39dec-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="39dec-132">İndirme boyutu daha büyüktür ve uygulamaların yüklenmesi daha uzun sürer.</span><span class="sxs-lookup"><span data-stu-id="39dec-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="39dec-133">.NET çalışma zamanı ve araç desteği daha az olgun.</span><span class="sxs-lookup"><span data-stu-id="39dec-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="39dec-134">Örneğin, [.NET Standard](/dotnet/standard/net-standard) desteğinin ve hata ayıklamada sınırlamalar mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="39dec-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="39dec-135">Blazor Server</span><span class="sxs-lookup"><span data-stu-id="39dec-135">Blazor Server</span></span>

<span data-ttu-id="39dec-136">Blazor sunucusu barındırma modeliyle, uygulama sunucuda ASP.NET Core bir uygulama içinden yürütülür.</span><span class="sxs-lookup"><span data-stu-id="39dec-136">With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="39dec-137">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrıları bir [SignalR](xref:signalr/introduction) bağlantısı üzerinden işlenir.</span><span class="sxs-lookup"><span data-stu-id="39dec-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Tarayıcı, bir SignalR bağlantısı üzerinden sunucusunda (bir ASP.NET Core uygulamasının içinde barındırılan) uygulamayla etkileşime girer.](hosting-models/_static/blazor-server.png)

<span data-ttu-id="39dec-139">Blazor Server barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için, ASP.NET Core **Blazor Server uygulama** şablonunu kullanın ([DotNet yeni blazorserver](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="39dec-139">To create a Blazor app using the Blazor Server hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="39dec-140">ASP.NET Core uygulaması, Blazor sunucu uygulamasını barındırır ve istemcilerin bağlanacağı SignalR uç noktasını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="39dec-140">The ASP.NET Core app hosts the Blazor Server app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="39dec-141">ASP.NET Core uygulama, şu ekleme için uygulamanın `Startup` sınıfına başvurur:</span><span class="sxs-lookup"><span data-stu-id="39dec-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="39dec-142">Sunucu tarafı hizmetler.</span><span class="sxs-lookup"><span data-stu-id="39dec-142">Server-side services.</span></span>
* <span data-ttu-id="39dec-143">İstek işleme işlem hattının uygulaması.</span><span class="sxs-lookup"><span data-stu-id="39dec-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="39dec-144">`blazor.server.js` betiği&dagger; istemci bağlantısını kurar.</span><span class="sxs-lookup"><span data-stu-id="39dec-144">The `blazor.server.js` script&dagger; establishes the client connection.</span></span> <span data-ttu-id="39dec-145">Uygulamanın, uygulama durumunu (örneğin, kayıp ağ bağlantısı durumunda) kalıcı hale getirmek ve geri yüklemek, uygulamanın sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="39dec-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="39dec-146">Blazor sunucusu barındırma modeli çeşitli avantajlar sunar:</span><span class="sxs-lookup"><span data-stu-id="39dec-146">The Blazor Server hosting model offers several benefits:</span></span>

* <span data-ttu-id="39dec-147">İndirme boyutu bir Blazor WebAssembly uygulamasından önemli ölçüde küçüktür ve uygulama çok daha hızlı yüklenir.</span><span class="sxs-lookup"><span data-stu-id="39dec-147">Download size is significantly smaller than a Blazor WebAssembly app, and the app loads much faster.</span></span>
* <span data-ttu-id="39dec-148">Uygulama, .NET Core ile uyumlu API 'lerin kullanımı dahil olmak üzere sunucu olanaklarından tam olarak yararlanır.</span><span class="sxs-lookup"><span data-stu-id="39dec-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="39dec-149">Sunucuda .NET Core, uygulamayı çalıştırmak için kullanılır, bu nedenle hata ayıklama gibi mevcut .NET araçları beklendiği gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="39dec-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="39dec-150">Ölçülü istemciler desteklenir.</span><span class="sxs-lookup"><span data-stu-id="39dec-150">Thin clients are supported.</span></span> <span data-ttu-id="39dec-151">Örneğin, Blazor Server Apps, WebAssembly ve kaynak kısıtlı cihazlarda bulunan tarayıcılarla çalışır.</span><span class="sxs-lookup"><span data-stu-id="39dec-151">For example, Blazor Server apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="39dec-152">Uygulamanın bileşen kodu da dahilC# olmak üzere, uygulamanın .NET/kod tabanı istemcilere sunulmuyor.</span><span class="sxs-lookup"><span data-stu-id="39dec-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="39dec-153">Blazor sunucusu barındırma için aşağı taraf vardır:</span><span class="sxs-lookup"><span data-stu-id="39dec-153">There are downsides to Blazor Server hosting:</span></span>

* <span data-ttu-id="39dec-154">Daha yüksek gecikme süresi genellikle vardır.</span><span class="sxs-lookup"><span data-stu-id="39dec-154">Higher latency usually exists.</span></span> <span data-ttu-id="39dec-155">Her Kullanıcı etkileşimi bir ağ atmasını içerir.</span><span class="sxs-lookup"><span data-stu-id="39dec-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="39dec-156">Çevrimdışı destek yoktur.</span><span class="sxs-lookup"><span data-stu-id="39dec-156">There's no offline support.</span></span> <span data-ttu-id="39dec-157">İstemci bağlantısı başarısız olursa, uygulama çalışmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="39dec-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="39dec-158">Ölçeklenebilirlik, çok sayıda kullanıcısı olan uygulamalar için zorlayıcı bir uygulamalardır.</span><span class="sxs-lookup"><span data-stu-id="39dec-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="39dec-159">Sunucunun birden çok istemci bağlantısını yönetmesi ve istemci durumunu işlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="39dec-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="39dec-160">Uygulamayı çalıştırmak için bir ASP.NET Core sunucusu gerekir.</span><span class="sxs-lookup"><span data-stu-id="39dec-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="39dec-161">Sunucusuz dağıtım senaryoları mümkün değildir (örneğin, bir CDN 'den uygulama sunma).</span><span class="sxs-lookup"><span data-stu-id="39dec-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="39dec-162">&dagger;`blazor.server.js` betiği ASP.NET Core paylaşılan çerçevede eklenmiş bir kaynaktan sunulur.</span><span class="sxs-lookup"><span data-stu-id="39dec-162">&dagger;The `blazor.server.js` script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="39dec-163">Sunucu tarafından işlenmiş Kullanıcı arabirimine karşılaştırma</span><span class="sxs-lookup"><span data-stu-id="39dec-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="39dec-164">Blazor Server uygulamalarını anlamanın bir yolu, Razor görünümlerini veya Razor Pages kullanarak ASP.NET Core uygulamalarda Kullanıcı arabirimi oluşturma için geleneksel modellerden nasıl farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="39dec-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="39dec-165">Her iki model de, HTML içeriğini anlatmak için Razor dilini kullanır, ancak biçimlendirmenin nasıl işlendiği konusunda önemli ölçüde farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="39dec-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="39dec-166">Bir Razor sayfası veya görünüm işlendiğinde, her Razor kodu satırı metin biçiminde HTML yayar.</span><span class="sxs-lookup"><span data-stu-id="39dec-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="39dec-167">Oluşturulduktan sonra sunucu, üretilen herhangi bir durum da dahil olmak üzere sayfayı veya görünüm örneğini ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="39dec-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="39dec-168">Sayfa için başka bir istek gerçekleştiğinde, örneğin sunucu doğrulaması başarısız olduğunda ve doğrulama özeti görüntülendiğinde:</span><span class="sxs-lookup"><span data-stu-id="39dec-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="39dec-169">Sayfanın tamamı HTML metnine yeniden eklenir.</span><span class="sxs-lookup"><span data-stu-id="39dec-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="39dec-170">Sayfa istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="39dec-170">The page is sent to the client.</span></span>

<span data-ttu-id="39dec-171">Blazor uygulaması, *bileşen*olarak adlandırılan UI 'ın yeniden kullanılabilir öğelerinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="39dec-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="39dec-172">Bir bileşen kod C# , biçimlendirme ve diğer bileşenleri içerir.</span><span class="sxs-lookup"><span data-stu-id="39dec-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="39dec-173">Bir bileşen işlendiğinde, Blazor bir HTML veya XML Belge Nesne Modeli (DOM) gibi dahil edilen bileşenlerin bir grafiğini üretir.</span><span class="sxs-lookup"><span data-stu-id="39dec-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="39dec-174">Bu grafik, özelliklerde ve alanlarında tutulan bileşen durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="39dec-174">This graph includes component state held in properties and fields.</span></span> <span data-ttu-id="39dec-175">Blazor, biçimlendirmenin ikili gösterimini üretmek için bileşen grafiğini değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="39dec-175">Blazor evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="39dec-176">İkili biçimi şu şekilde olabilir:</span><span class="sxs-lookup"><span data-stu-id="39dec-176">The binary format can be:</span></span>

* <span data-ttu-id="39dec-177">HTML metnine açıldı (prerendering sırasında).</span><span class="sxs-lookup"><span data-stu-id="39dec-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="39dec-178">Düzenli işleme sırasında biçimlendirmeyi verimli bir şekilde güncelleştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="39dec-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="39dec-179">Blazor içinde bir kullanıcı arabirimi güncelleştirmesi şu şekilde tetiklenir:</span><span class="sxs-lookup"><span data-stu-id="39dec-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="39dec-180">Düğme seçme gibi kullanıcı etkileşimi.</span><span class="sxs-lookup"><span data-stu-id="39dec-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="39dec-181">Zamanlayıcı gibi uygulama Tetikleyicileri.</span><span class="sxs-lookup"><span data-stu-id="39dec-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="39dec-182">Grafik yeniden tanımlanır ve bir UI *farkı* (fark) hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="39dec-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="39dec-183">Bu fark, istemcideki Kullanıcı arabirimini güncelleştirmek için gereken en küçük DOM düzenlemelerinin kümesidir.</span><span class="sxs-lookup"><span data-stu-id="39dec-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="39dec-184">Fark istemciye bir ikili biçimde gönderilir ve tarayıcı tarafından uygulanır.</span><span class="sxs-lookup"><span data-stu-id="39dec-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="39dec-185">Kullanıcı, istemci üzerinde bundan uzaklaştığında bir bileşen atılmış olur.</span><span class="sxs-lookup"><span data-stu-id="39dec-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="39dec-186">Bir Kullanıcı bir bileşenle etkileşim kurarken, bileşenin durumu (hizmetler, kaynaklar) sunucunun belleğinde tutulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="39dec-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="39dec-187">Birçok bileşenin durumu sunucu tarafından eşzamanlı olarak Korunabileceğinden, bellek tükenmesi sorunu ele alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="39dec-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="39dec-188">Sunucu belleğinin en iyi şekilde kullanılmasını sağlamak üzere bir Blazor sunucu uygulamasının nasıl yazılacağı hakkında yönergeler için bkz. <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="39dec-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server>.</span></span>

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="39dec-189">Razor bileşenlerini Razor Pages ve MVC uygulamalarıyla tümleştirin</span><span class="sxs-lookup"><span data-stu-id="39dec-189">Integrate Razor components into Razor Pages and MVC apps</span></span>

#### <a name="use-components-in-pages-and-views"></a><span data-ttu-id="39dec-190">Sayfalar ve görünümlerde bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="39dec-190">Use components in pages and views</span></span>

<span data-ttu-id="39dec-191">Mevcut bir Razor Pages veya MVC uygulaması, Razor bileşenlerini sayfalarla ve görünümleriyle tümleştirebilir:</span><span class="sxs-lookup"><span data-stu-id="39dec-191">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="39dec-192">Uygulamanın düzen dosyasında ( *_Layout. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="39dec-192">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="39dec-193">Aşağıdaki `<base>` etiketini `<head>` öğesine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="39dec-193">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="39dec-194">Yukarıdaki örnekteki `href` değeri ( *uygulama temel yolu*), UYGULAMANıN kök URL yolunda (`/`) bulunduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="39dec-194">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="39dec-195">Uygulama bir alt uygulama ise, <xref:host-and-deploy/blazor/index#app-base-path> makalesinin *uygulama temel yolu* bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="39dec-195">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="39dec-196">*_Layout. cshtml* dosyası, bir MVC uygulamasında bir Razor Pages uygulamasının veya *görünümlerinin/paylaşılan* klasörünün *Sayfalar/paylaşılan* klasöründe bulunur.</span><span class="sxs-lookup"><span data-stu-id="39dec-196">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="39dec-197">Kapanış `</body>` etiketinin içindeki *blazor. Server. js* betiği için bir `<script>` etiketi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="39dec-197">Add a `<script>` tag for the *blazor.server.js* script inside of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="39dec-198">Framework, *blazor. Server. js* betiğini uygulamaya ekler.</span><span class="sxs-lookup"><span data-stu-id="39dec-198">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="39dec-199">Betiği uygulamaya el ile eklemeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="39dec-199">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="39dec-200">Aşağıdaki içerikle projenin kök klasörüne bir *_Imports. Razor* dosyası ekleyin (son ad alanını, `MyAppNamespace`, uygulamanın ad alanına değiştirin):</span><span class="sxs-lookup"><span data-stu-id="39dec-200">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```csharp
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="39dec-201">`Startup.ConfigureServices`, Blazor Server hizmetini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="39dec-201">In `Startup.ConfigureServices`, add the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="39dec-202">`Startup.Configure`, Blazor hub uç noktasını `app.UseEndpoints`öğesine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="39dec-202">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="39dec-203">Bileşenleri herhangi bir sayfa veya görünümle tümleştirin.</span><span class="sxs-lookup"><span data-stu-id="39dec-203">Integrate components into any page or view.</span></span> <span data-ttu-id="39dec-204">Daha fazla bilgi için, <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> makalesinin *bileşenleri Razor Pages ve MVC uygulamalarına tümleştirme* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="39dec-204">For more information, see the *Integrate components into Razor Pages and MVC apps* section of the <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> article.</span></span>

#### <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="39dec-205">Razor Pages uygulamasında yönlendirilebilir bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="39dec-205">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="39dec-206">Razor Pages uygulamalarda yönlendirilebilir Razor bileşenlerini desteklemek için:</span><span class="sxs-lookup"><span data-stu-id="39dec-206">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="39dec-207">[Sayfalar ve görünümlerde bileşenleri kullanma](#use-components-in-pages-and-views) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="39dec-207">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="39dec-208">Aşağıdaki içeriğe sahip projenin köküne bir *app. Razor* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="39dec-208">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="39dec-209">*Sayfalar* klasörüne aşağıdaki içeriğe sahip bir *_Host. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="39dec-209">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="39dec-210">Bileşenler, düzeni için paylaşılan *_Layout. cshtml* dosyasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="39dec-210">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="39dec-211">`Startup.Configure`' de uç nokta yapılandırmasına *_Host. cshtml* sayfası için düşük öncelikli bir yol ekleyin:</span><span class="sxs-lookup"><span data-stu-id="39dec-211">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="39dec-212">Uygulamaya yönlendirilebilir bileşenler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="39dec-212">Add routable components to the app.</span></span> <span data-ttu-id="39dec-213">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="39dec-213">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="39dec-214">Uygulamanın bileşenlerini tutmak için özel bir klasör kullanırken, klasörü *sayfaları/_ViewImports. cshtml* dosyasına temsil eden ad alanını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="39dec-214">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Pages/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="39dec-215">Daha fazla bilgi için bkz. <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="39dec-215">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

#### <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="39dec-216">MVC uygulamasında yönlendirilebilir bileşenleri kullanma</span><span class="sxs-lookup"><span data-stu-id="39dec-216">Use routable components in an MVC app</span></span>

<span data-ttu-id="39dec-217">MVC uygulamalarında yönlendirilebilir Razor bileşenlerini desteklemek için:</span><span class="sxs-lookup"><span data-stu-id="39dec-217">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="39dec-218">[Sayfalar ve görünümlerde bileşenleri kullanma](#use-components-in-pages-and-views) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="39dec-218">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="39dec-219">Aşağıdaki içeriğe sahip projenin köküne bir *app. Razor* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="39dec-219">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="39dec-220">Aşağıdaki içeriğe sahip *Görünümler/giriş* klasörüne bir *_Host. cshtml* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="39dec-220">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="39dec-221">Bileşenler, düzeni için paylaşılan *_Layout. cshtml* dosyasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="39dec-221">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="39dec-222">Ana denetleyiciye bir eylem ekleyin:</span><span class="sxs-lookup"><span data-stu-id="39dec-222">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="39dec-223">`Startup.Configure`' deki uç nokta yapılandırmasına *_Host. cshtml* görünümünü döndüren denetleyici eylemi için düşük öncelikli bir yol ekleyin:</span><span class="sxs-lookup"><span data-stu-id="39dec-223">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="39dec-224">Bir *Sayfalar* klasörü oluşturun ve uygulamaya yönlendirilebilir bileşenler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="39dec-224">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="39dec-225">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="39dec-225">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="39dec-226">Uygulamanın bileşenlerini tutmak için özel bir klasör kullanırken, klasörü *görüntüleme/_ViewImports. cshtml* dosyasına temsil eden ad alanını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="39dec-226">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="39dec-227">Daha fazla bilgi için bkz. <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="39dec-227">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

### <a name="circuits"></a><span data-ttu-id="39dec-228">Uygulanıp</span><span class="sxs-lookup"><span data-stu-id="39dec-228">Circuits</span></span>

<span data-ttu-id="39dec-229">Bir Blazor sunucu uygulaması [ASP.NET Core SignalR](xref:signalr/introduction)'nin üzerine kurulmuştur.</span><span class="sxs-lookup"><span data-stu-id="39dec-229">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="39dec-230">Her istemci, *devre*adlı bir veya daha fazla SignalR bağlantısı üzerinden sunucusuyla iletişim kurar.</span><span class="sxs-lookup"><span data-stu-id="39dec-230">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="39dec-231">Bir devre, geçici ağ kesintilerine tolerans sağlayan SignalR bağlantıları üzerinden Blazor.</span><span class="sxs-lookup"><span data-stu-id="39dec-231">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="39dec-232">Bir Blazor istemcisi, SignalR bağlantısının kesileceğini gördüğünde, yeni bir SignalR bağlantısı kullanarak sunucuya yeniden bağlanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="39dec-232">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="39dec-233">Bir Blazor sunucu uygulamasına bağlı her tarayıcı ekranı (tarayıcı sekmesi veya iframe) bir SignalR bağlantısı kullanır.</span><span class="sxs-lookup"><span data-stu-id="39dec-233">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="39dec-234">Bu, tipik sunucu tarafından işlenmiş uygulamalarla karşılaştırıldığında daha önemli bir ayırım ifade etmiştir.</span><span class="sxs-lookup"><span data-stu-id="39dec-234">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="39dec-235">Sunucu tarafından işlenen bir uygulamada, aynı uygulamayı birden çok tarayıcı ekranında açmak genellikle sunucuda ek kaynak taleplerine çevirilmez.</span><span class="sxs-lookup"><span data-stu-id="39dec-235">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="39dec-236">Bir Blazor sunucu uygulamasında, her tarayıcı ekranı, sunucu tarafından yönetilecek ayrı bir devre ve bileşen durumunun ayrı örneklerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="39dec-236">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

<span data-ttu-id="39dec-237">Blazor bir tarayıcı sekmesini kapatmayı veya bir dış URL 'ye gidilerek *düzgün* bir şekilde sonlandırılmasını kabul eder.</span><span class="sxs-lookup"><span data-stu-id="39dec-237">Blazor considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="39dec-238">Düzgün sonlandırma durumunda, devre ve ilişkili kaynaklar hemen serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="39dec-238">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="39dec-239">Bir istemci, örneğin bir ağ kesintisi nedeniyle düzgün şekilde kesilmeyen bir şekilde kesilebilir.</span><span class="sxs-lookup"><span data-stu-id="39dec-239">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> <span data-ttu-id="39dec-240">Blazor sunucusu, istemcinin yeniden bağlanmasına izin vermek üzere yapılandırılabilir bir Aralık için bağlantısı kesilen devreleri depolar.</span><span class="sxs-lookup"><span data-stu-id="39dec-240">Blazor Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="39dec-241">Daha fazla bilgi için [aynı sunucuya yeniden bağlanma](#reconnection-to-the-same-server) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="39dec-241">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="39dec-242">UI gecikmesi</span><span class="sxs-lookup"><span data-stu-id="39dec-242">UI Latency</span></span>

<span data-ttu-id="39dec-243">UI gecikme süresi, başlatılan bir eylemden Kullanıcı arabiriminin güncelleştirildiği zamana kadar geçen süredir.</span><span class="sxs-lookup"><span data-stu-id="39dec-243">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="39dec-244">Bir uygulamanın kullanıcıya yanıt vermesi için kullanıcı ARABIRIMI gecikmesi için daha küçük değerler zorunludur.</span><span class="sxs-lookup"><span data-stu-id="39dec-244">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="39dec-245">Bir Blazor sunucu uygulamasında her bir eylem sunucuya gönderilir, işlenir ve bir UI farkı geri gönderilir.</span><span class="sxs-lookup"><span data-stu-id="39dec-245">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="39dec-246">Sonuç olarak, UI gecikmesi ağ gecikme süresinin toplamı ve eylemi işlerken sunucu gecikmesi sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="39dec-246">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="39dec-247">Özel bir kurumsal ağla sınırlı bir iş kolu uygulaması için, ağ gecikmesi nedeniyle kullanıcı gecikmesi algılarını üzerindeki etki, genellikle çok sayıda CEPSİZ olur.</span><span class="sxs-lookup"><span data-stu-id="39dec-247">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="39dec-248">Internet üzerinden dağıtılan bir uygulama için, özellikle de kullanıcılar coğrafi olarak coğrafi olarak dağıtılmışsa gecikme süresi kullanıcılara karşı farklılık gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="39dec-248">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="39dec-249">Bellek kullanımı ayrıca uygulama gecikme süresine de katkıda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="39dec-249">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="39dec-250">Daha fazla bellek kullanımı, her ikisi de uygulama performansının düşmesine neden olan ve bu nedenle kullanıcı arabirimi gecikmesini arttığı diskte sık görülen çöp toplama veya disk belleği belleği</span><span class="sxs-lookup"><span data-stu-id="39dec-250">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="39dec-251">Daha fazla bilgi için bkz. <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="39dec-251">For more information, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="39dec-252">Blazor sunucu uygulamaları, ağ gecikmesini ve bellek kullanımını azaltarak UI gecikmesini en aza indirmek için iyileştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="39dec-252">Blazor Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="39dec-253">Ağ gecikmesini ölçmeye yönelik bir yaklaşım için bkz. <xref:host-and-deploy/blazor/server#measure-network-latency>.</span><span class="sxs-lookup"><span data-stu-id="39dec-253">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server#measure-network-latency>.</span></span> <span data-ttu-id="39dec-254">SignalR ve Blazor hakkında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="39dec-254">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a><span data-ttu-id="39dec-255">Sunucuyla bağlantı</span><span class="sxs-lookup"><span data-stu-id="39dec-255">Connection to the server</span></span>

<span data-ttu-id="39dec-256">Blazor Server uygulamaları, sunucusuna etkin bir SignalR bağlantısı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="39dec-256">Blazor Server apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="39dec-257">Bağlantı kaybolursa, uygulama sunucuya yeniden bağlanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="39dec-257">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="39dec-258">İstemcinin durumu hala bellekte olduğu sürece, istemci oturumu durum kaybı olmadan devam eder.</span><span class="sxs-lookup"><span data-stu-id="39dec-258">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

#### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="39dec-259">Aynı sunucuya yeniden bağlanma</span><span class="sxs-lookup"><span data-stu-id="39dec-259">Reconnection to the same server</span></span>

<span data-ttu-id="39dec-260">Sunucu üzerinde kullanıcı arabirimi durumunu ayarlayan ilk istemci isteğine yanıt olarak önceden bir Blazor Server uygulaması ön ekler.</span><span class="sxs-lookup"><span data-stu-id="39dec-260">A Blazor Server app prerenders in response to the first client request, which sets up the UI state on the server.</span></span> <span data-ttu-id="39dec-261">İstemci bir SignalR bağlantısı oluşturmayı denediğinde, istemci aynı sunucuya yeniden bağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="39dec-261">When the client attempts to create a SignalR connection, the client must reconnect to the same server.</span></span> <span data-ttu-id="39dec-262">Birden fazla arka uç sunucusu kullanan Blazor sunucu uygulamaları, SignalR bağlantıları için *yapışkan oturumlar* uygulamalıdır.</span><span class="sxs-lookup"><span data-stu-id="39dec-262">Blazor Server apps that use more than one backend server should implement *sticky sessions* for SignalR connections.</span></span>

<span data-ttu-id="39dec-263">Blazor Server uygulamaları için [Azure SignalR hizmetini](/azure/azure-signalr) kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="39dec-263">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="39dec-264">Hizmet, Blazor sunucu uygulamasını çok sayıda eşzamanlı SignalR bağlantısına ölçeklendirmeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="39dec-264">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="39dec-265">Azure SignalR hizmeti için, hizmetin `ServerStickyMode` seçeneği veya yapılandırma değeri `Required`olarak ayarlanarak yapışkan oturumlar etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="39dec-265">Sticky sessions are enabled for the Azure SignalR Service by setting the service's `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="39dec-266">Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/server#signalr-configuration>.</span><span class="sxs-lookup"><span data-stu-id="39dec-266">For more information, see <xref:host-and-deploy/blazor/server#signalr-configuration>.</span></span>

<span data-ttu-id="39dec-267">IIS kullanırken, yapışkan oturumlar uygulama Isteği yönlendirme ile etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="39dec-267">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="39dec-268">Daha fazla bilgi için bkz. [uygulama Isteği yönlendirme kullanarak HTTP yük dengelemesi](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span><span class="sxs-lookup"><span data-stu-id="39dec-268">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="39dec-269">Kullanıcı arabirimindeki bağlantı durumunu yansıtır</span><span class="sxs-lookup"><span data-stu-id="39dec-269">Reflect the connection state in the UI</span></span>

<span data-ttu-id="39dec-270">İstemci bağlantının kaybolduğunu algıladığında, istemci yeniden bağlanmayı denediğinde kullanıcıya varsayılan bir kullanıcı arabirimi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="39dec-270">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="39dec-271">Yeniden bağlantı başarısız olursa, kullanıcıya yeniden deneme seçeneği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="39dec-271">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="39dec-272">Kullanıcı arabirimini özelleştirmek için *_Host. cshtml* Razor sayfasının `<body>` `components-reconnect-modal` `id` bir öğe tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="39dec-272">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="39dec-273">Aşağıdaki tabloda `components-reconnect-modal` öğesine uygulanan CSS sınıfları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="39dec-273">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="39dec-274">CSS sınıfı</span><span class="sxs-lookup"><span data-stu-id="39dec-274">CSS class</span></span>                       | <span data-ttu-id="39dec-275">&hellip; gösterir</span><span class="sxs-lookup"><span data-stu-id="39dec-275">Indicates&hellip;</span></span> |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | <span data-ttu-id="39dec-276">Kayıp bir bağlantı.</span><span class="sxs-lookup"><span data-stu-id="39dec-276">A lost connection.</span></span> <span data-ttu-id="39dec-277">İstemci yeniden bağlanmaya çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="39dec-277">The client is attempting to reconnect.</span></span> <span data-ttu-id="39dec-278">Kalıcı olarak göster.</span><span class="sxs-lookup"><span data-stu-id="39dec-278">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="39dec-279">Etkin bir bağlantı sunucuya yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="39dec-279">An active connection is re-established to the server.</span></span> <span data-ttu-id="39dec-280">Kalıcı olarak gizleyin.</span><span class="sxs-lookup"><span data-stu-id="39dec-280">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="39dec-281">Muhtemelen bir ağ hatasından dolayı yeniden bağlantı başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="39dec-281">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="39dec-282">Yeniden bağlanmayı denemek için `window.Blazor.reconnect()`çağırın.</span><span class="sxs-lookup"><span data-stu-id="39dec-282">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="39dec-283">Yeniden bağlantı reddedildi.</span><span class="sxs-lookup"><span data-stu-id="39dec-283">Reconnection rejected.</span></span> <span data-ttu-id="39dec-284">Sunucuya ulaşıldı ancak bağlantı reddedildi ve kullanıcının sunucudaki durumu kayboldu.</span><span class="sxs-lookup"><span data-stu-id="39dec-284">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="39dec-285">Uygulamayı yeniden yüklemek için `location.reload()`çağırın.</span><span class="sxs-lookup"><span data-stu-id="39dec-285">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="39dec-286">Bu bağlantı durumu şu durumlarda oluşabilir:</span><span class="sxs-lookup"><span data-stu-id="39dec-286">This connection state may result when:</span></span><ul><li><span data-ttu-id="39dec-287">Sunucu tarafında devre dışı bir kilitlenme oluşur.</span><span class="sxs-lookup"><span data-stu-id="39dec-287">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="39dec-288">Sunucunun kullanıcının durumunu bırakması için istemcinin bağlantısı yeterince uzun değil.</span><span class="sxs-lookup"><span data-stu-id="39dec-288">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="39dec-289">Kullanıcının etkileşimde bulunduğu bileşenlerin örnekleri atıldı.</span><span class="sxs-lookup"><span data-stu-id="39dec-289">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="39dec-290">Sunucu yeniden başlatıldı veya uygulamanın çalışan işlemi geri dönüştürüldü.</span><span class="sxs-lookup"><span data-stu-id="39dec-290">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="39dec-291">Prerendering sonrasında durum bilgisi olan yeniden bağlanma</span><span class="sxs-lookup"><span data-stu-id="39dec-291">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="39dec-292">Blazor sunucu uygulamaları, sunucu bağlantısı oluşturulmadan önce sunucudaki kullanıcı arabirimini varsayılan olarak PreRender 'a ayarlar.</span><span class="sxs-lookup"><span data-stu-id="39dec-292">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="39dec-293">Bu, *_Host. cshtml* Razor sayfasında ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="39dec-293">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="39dec-294">`RenderMode`, bileşenin şunları yapıp kullanmadığını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="39dec-294">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="39dec-295">, Sayfaya ön gönderilir.</span><span class="sxs-lookup"><span data-stu-id="39dec-295">Is prerendered into the page.</span></span>
* <span data-ttu-id="39dec-296">, Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.</span><span class="sxs-lookup"><span data-stu-id="39dec-296">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="39dec-297">Açıklama</span><span class="sxs-lookup"><span data-stu-id="39dec-297">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="39dec-298">Bileşeni statik HTML olarak işler ve Blazor sunucusu uygulaması için bir işaret içerir.</span><span class="sxs-lookup"><span data-stu-id="39dec-298">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="39dec-299">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="39dec-299">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="39dec-300">Blazor sunucusu uygulaması için bir işaret oluşturur.</span><span class="sxs-lookup"><span data-stu-id="39dec-300">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="39dec-301">Bileşen çıkışı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="39dec-301">Output from the component isn't included.</span></span> <span data-ttu-id="39dec-302">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="39dec-302">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="39dec-303">Bileşeni statik HTML olarak işler.</span><span class="sxs-lookup"><span data-stu-id="39dec-303">Renders the component into static HTML.</span></span> |

<span data-ttu-id="39dec-304">Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="39dec-304">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="39dec-305">`RenderMode` `ServerPrerendered`, bileşen başlangıçta sayfanın bir parçası olarak statik olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="39dec-305">When `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="39dec-306">Tarayıcı sunucuya geri bir bağlantı kurduğunda, bileşen *yeniden*işlenir ve bileşen artık etkileşimli olur.</span><span class="sxs-lookup"><span data-stu-id="39dec-306">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="39dec-307">Bileşeni başlatmak için [Onbaşlatılmış {Async}](xref:blazor/lifecycle#component-initialization-methods) yaşam döngüsü yöntemi varsa, yöntem *iki kez*yürütülür:</span><span class="sxs-lookup"><span data-stu-id="39dec-307">If the [OnInitialized{Async}](xref:blazor/lifecycle#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="39dec-308">Bileşen statik olarak önceden kullanılırken.</span><span class="sxs-lookup"><span data-stu-id="39dec-308">When the component is prerendered statically.</span></span>
* <span data-ttu-id="39dec-309">Sunucu bağlantısı kurulduktan sonra.</span><span class="sxs-lookup"><span data-stu-id="39dec-309">After the server connection has been established.</span></span>

<span data-ttu-id="39dec-310">Bu, bileşen son işlendiğinde Kullanıcı arabiriminde görünen verilerde fark edilebilir bir değişikliğe neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="39dec-310">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="39dec-311">Blazor sunucu uygulamasında çift işleme senaryosunu önlemek için:</span><span class="sxs-lookup"><span data-stu-id="39dec-311">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="39dec-312">Prerendering sırasında durumu önbelleğe almak için kullanılabilecek bir tanımlayıcı geçirin ve uygulamayı yeniden başlattıktan sonra durumu alma.</span><span class="sxs-lookup"><span data-stu-id="39dec-312">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="39dec-313">Bileşen durumunu kaydetmek için prerendering sırasında tanımlayıcıyı kullanın.</span><span class="sxs-lookup"><span data-stu-id="39dec-313">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="39dec-314">Önbelleğe alınan durumu almak için prerendering öğesinden sonra tanımlayıcıyı kullanın.</span><span class="sxs-lookup"><span data-stu-id="39dec-314">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="39dec-315">Aşağıdaki kod, Çift işlemeyi engelleyen şablon tabanlı Blazor sunucu uygulamasındaki güncelleştirilmiş bir `WeatherForecastService` gösterir:</span><span class="sxs-lookup"><span data-stu-id="39dec-315">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] _summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = _summaries[rng.Next(_summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="39dec-316">Razor sayfaları ve görünümlerinden durum bilgisi olan etkileşimli bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="39dec-316">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="39dec-317">Durum bilgisi olan etkileşimli bileşenler Razor sayfasına veya görünümüne eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="39dec-317">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="39dec-318">Sayfa veya görünüm şunları işler:</span><span class="sxs-lookup"><span data-stu-id="39dec-318">When the page or view renders:</span></span>

* <span data-ttu-id="39dec-319">Bileşen sayfa veya görünümle birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="39dec-319">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="39dec-320">Prerendering için kullanılan ilk bileşen durumu kayboldu.</span><span class="sxs-lookup"><span data-stu-id="39dec-320">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="39dec-321">SignalR bağlantısı oluşturulduğunda yeni bileşen durumu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="39dec-321">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="39dec-322">Aşağıdaki Razor sayfası bir `Counter` bileşeni işler:</span><span class="sxs-lookup"><span data-stu-id="39dec-322">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="39dec-323">Razor sayfaları ve görünümlerinden etkileşimsiz bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="39dec-323">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="39dec-324">Aşağıdaki Razor sayfasında, `Counter` bileşen bir form kullanılarak belirtilen bir başlangıç değeri ile statik olarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="39dec-324">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="39dec-325">`MyComponent` statik olarak işlendiğinde, bileşen etkileşimli olamaz.</span><span class="sxs-lookup"><span data-stu-id="39dec-325">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="39dec-326">Uygulamanın ne zaman prerendering olduğunu Algıla</span><span class="sxs-lookup"><span data-stu-id="39dec-326">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="39dec-327">Blazor Server uygulamaları için SignalR istemcisini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="39dec-327">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="39dec-328">Bazen Blazor Server uygulamaları tarafından kullanılan SignalR istemcisini yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="39dec-328">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="39dec-329">Örneğin, bir bağlantı sorununu tanılamak için SignalR istemcisinde günlüğe kaydetmeyi yapılandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="39dec-329">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="39dec-330">*Pages/_Host. cshtml* dosyasında SignalR istemcisini yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="39dec-330">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="39dec-331">`blazor.server.js` betiği için `<script>` etiketine bir `autostart="false"` özniteliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="39dec-331">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="39dec-332">`Blazor.start` çağırın ve SignalR oluşturucuyu belirten bir yapılandırma nesnesini geçirin.</span><span class="sxs-lookup"><span data-stu-id="39dec-332">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a><span data-ttu-id="39dec-333">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="39dec-333">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
* <xref:tutorials/signalr-blazor-webassembly>
