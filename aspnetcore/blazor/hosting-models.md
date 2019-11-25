---
title: Blazor barındırma modellerini ASP.NET Core
author: guardrex
description: Blazor WebAssembly ve Blazor sunucusu barındırma modellerini anlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: a017737eacd93ac776afe7ee8024eed602d7edcc
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317220"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a><span data-ttu-id="c9ecd-103">Blazor barındırma modellerini ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c9ecd-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="c9ecd-104">[Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="c9ecd-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="c9ecd-105">, bir [Webassembly](https://webassembly.org/)tabanlı .NET çalışma zamanı ( *Blazor webassembly*) veya ASP.NET Core ( *Blazor Server*) sunucu tarafında tarayıcıda istemci tarafı çalıştırmak için tasarlanan bir Web çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-105"> is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor WebAssembly*) or server-side in ASP.NET Core (*Blazor Server*).</span></span> <span data-ttu-id="c9ecd-106">Barındırma modelinden bağımsız olarak, uygulama ve bileşen modelleri *aynıdır*.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="c9ecd-107">Bu makalede açıklanan barındırma modelleriyle ilgili bir proje oluşturmak için, bkz. <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="c9ecd-108"> WebAssembly</span><span class="sxs-lookup"><span data-stu-id="c9ecd-108"> WebAssembly</span></span>

<span data-ttu-id="c9ecd-109">Blazor için sorumlu barındırma modeli, WebAssembly üzerinde tarayıcıda istemci tarafında çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="c9ecd-110">Blazor uygulaması, bağımlılıkları ve .NET çalışma zamanı tarayıcıya indirilir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="c9ecd-111">Uygulama doğrudan tarayıcı kullanıcı arabirimi iş parçacığında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="c9ecd-112">UI güncelleştirmeleri ve olay işleme aynı işlem içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="c9ecd-113">Uygulamanın varlıkları, istemcilere statik içerik sunan bir Web sunucusuna veya hizmete statik dosyalar olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![[! Üs. NO-LOC (Blazor)]<span data-ttu-id="c9ecd-114"> WebAssembly: [! Üs. NO-LOC (Blazor)] uygulaması, tarayıcı içindeki bir kullanıcı arabirimi iş parçacığında çalışır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-114"> WebAssembly: The Blazor app runs on a UI thread inside the browser.</span></span>](hosting-models/_static/blazor-webassembly.png)

<span data-ttu-id="c9ecd-115">İstemci tarafı barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için, **Blazor WebAssembly uygulama** şablonunu ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)) kullanın.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="c9ecd-116">**Blazor WebAssembly uygulama** şablonunu seçtikten sonra, **ASP.NET Core barındırılan** onay kutusunu ([DotNet New blazorwasm--hosted](/dotnet/core/tools/dotnet-new)) seçerek uygulamayı ASP.NET Core arka ucunu kullanacak şekilde yapılandırma seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="c9ecd-117">ASP.NET Core uygulaması, Blazor uygulamasına istemcilere hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="c9ecd-118">Blazor WebAssembly uygulaması, Web API çağrılarını veya [SignalR](xref:signalr/introduction)kullanarak ağ üzerinden sunucu ile etkileşime geçebilir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-118">The Blazor WebAssembly app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="c9ecd-119">Şablonlar şunları ele alan *blazor. webassembly. js* betiğini içerir:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="c9ecd-120">.NET çalışma zamanını, uygulamayı ve uygulamanın bağımlılıklarını indirme.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="c9ecd-121">Uygulamayı çalıştırmak için çalışma zamanının başlatılması.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="c9ecd-122">Blazor WebAssembly barındırma modeli çeşitli avantajlar sunar:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-122">The Blazor WebAssembly hosting model offers several benefits:</span></span>

* <span data-ttu-id="c9ecd-123">.NET sunucu tarafı bağımlılığı yoktur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="c9ecd-124">Uygulama, istemciye indirildikten sonra tamamen çalışır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="c9ecd-125">İstemci kaynakları ve yetenekleri tamamen yararlanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="c9ecd-126">İş sunucudan istemciye boşaltılır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="c9ecd-127">Uygulamayı barındırmak için bir ASP.NET Core Web sunucusu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="c9ecd-128">Sunucusuz dağıtım senaryoları mümkündür (örneğin, bir CDN 'den uygulama sunma).</span><span class="sxs-lookup"><span data-stu-id="c9ecd-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="c9ecd-129">WebAssembly barındırma Blazor için aşağı yanlar vardır:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-129">There are downsides to Blazor WebAssembly hosting:</span></span>

* <span data-ttu-id="c9ecd-130">Uygulama tarayıcının özelliklerine kısıtlıdır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="c9ecd-131">Uyumlu istemci donanımı ve yazılımı (örneğin, WebAssembly desteği) gereklidir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="c9ecd-132">İndirme boyutu daha büyüktür ve uygulamaların yüklenmesi daha uzun sürer.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="c9ecd-133">.NET çalışma zamanı ve araç desteği daha az olgun.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="c9ecd-134">Örneğin, [.NET Standard](/dotnet/standard/net-standard) desteğinin ve hata ayıklamada sınırlamalar mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="opno-locblazor-server"></a>Blazor<span data-ttu-id="c9ecd-135"> sunucusu</span><span class="sxs-lookup"><span data-stu-id="c9ecd-135"> Server</span></span>

<span data-ttu-id="c9ecd-136">Blazor sunucusu barındırma modeliyle uygulama, sunucuda bir ASP.NET Core uygulamasının içinden yürütülür.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-136">With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="c9ecd-137">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrıları [SignalR](xref:signalr/introduction) bir bağlantı üzerinden işlenir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Tarayıcı, sunucuda bir [! üzerinde barındırılan uygulamayla (bir ASP.NET Core uygulamasının içinde barındırılır) etkileşime girer. Üs. NO-LOC (SignalR)] bağlantısı.](hosting-models/_static/blazor-server.png)

<span data-ttu-id="c9ecd-139">Blazor sunucusu barındırma modelini kullanarak bir Blazor uygulaması oluşturmak için ASP.NET Core **Blazor Server uygulama** şablonunu ([DotNet New blazorserver](/dotnet/core/tools/dotnet-new)) kullanın.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-139">To create a Blazor app using the Blazor Server hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="c9ecd-140">ASP.NET Core uygulaması Blazor sunucusu uygulamasını barındırır ve istemcilerin bağlanacağı SignalR uç noktasını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-140">The ASP.NET Core app hosts the Blazor Server app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="c9ecd-141">ASP.NET Core uygulama, şu ekleme için uygulamanın `Startup` sınıfına başvurur:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="c9ecd-142">Sunucu tarafı hizmetler.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-142">Server-side services.</span></span>
* <span data-ttu-id="c9ecd-143">İstek işleme işlem hattının uygulaması.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="c9ecd-144">*Blazor. Server. js* komut dosyası&dagger; istemci bağlantısını kurar.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="c9ecd-145">Uygulamanın, uygulama durumunu (örneğin, kayıp ağ bağlantısı durumunda) kalıcı hale getirmek ve geri yüklemek, uygulamanın sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="c9ecd-146">Blazor sunucusu barındırma modeli çeşitli avantajlar sunar:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-146">The Blazor Server hosting model offers several benefits:</span></span>

* <span data-ttu-id="c9ecd-147">İndirme boyutu, Blazor WebAssembly uygulamasından önemli ölçüde küçüktür ve uygulama çok daha hızlı yüklenir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-147">Download size is significantly smaller than a Blazor WebAssembly app, and the app loads much faster.</span></span>
* <span data-ttu-id="c9ecd-148">Uygulama, .NET Core ile uyumlu API 'lerin kullanımı dahil olmak üzere sunucu olanaklarından tam olarak yararlanır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="c9ecd-149">Sunucuda .NET Core, uygulamayı çalıştırmak için kullanılır, bu nedenle hata ayıklama gibi mevcut .NET araçları beklendiği gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="c9ecd-150">Ölçülü istemciler desteklenir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-150">Thin clients are supported.</span></span> <span data-ttu-id="c9ecd-151">Örneğin, Blazor Server Apps, WebAssembly ve kaynak kısıtlı cihazlarda bulunan tarayıcılarla çalışır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-151">For example, Blazor Server apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="c9ecd-152">Uygulamanın bileşen kodu da dahilC# olmak üzere, uygulamanın .NET/kod tabanı istemcilere sunulmuyor.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="c9ecd-153">Sunucu barındırma Blazor için aşağı yanlar vardır:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-153">There are downsides to Blazor Server hosting:</span></span>

* <span data-ttu-id="c9ecd-154">Daha yüksek gecikme süresi genellikle vardır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-154">Higher latency usually exists.</span></span> <span data-ttu-id="c9ecd-155">Her Kullanıcı etkileşimi bir ağ atmasını içerir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="c9ecd-156">Çevrimdışı destek yoktur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-156">There's no offline support.</span></span> <span data-ttu-id="c9ecd-157">İstemci bağlantısı başarısız olursa, uygulama çalışmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="c9ecd-158">Ölçeklenebilirlik, çok sayıda kullanıcısı olan uygulamalar için zorlayıcı bir uygulamalardır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="c9ecd-159">Sunucunun birden çok istemci bağlantısını yönetmesi ve istemci durumunu işlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="c9ecd-160">Uygulamayı çalıştırmak için bir ASP.NET Core sunucusu gerekir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="c9ecd-161">Sunucusuz dağıtım senaryoları mümkün değildir (örneğin, bir CDN 'den uygulama sunma).</span><span class="sxs-lookup"><span data-stu-id="c9ecd-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="c9ecd-162">&dagger;*blazor. Server. js* komut dosyası, ASP.NET Core paylaşılan çerçevesindeki gömülü bir kaynaktan sunulur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="c9ecd-163">Sunucu tarafından işlenmiş Kullanıcı arabirimine karşılaştırma</span><span class="sxs-lookup"><span data-stu-id="c9ecd-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="c9ecd-164">Blazor Server uygulamalarını anlamanın bir yolu, Razor görünümlerini veya Razor Pages kullanarak ASP.NET Core uygulamalarda Kullanıcı arabirimini işlemek için geleneksel modellerden nasıl farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="c9ecd-165">Her iki model de, HTML içeriğini anlatmak için Razor dilini kullanır, ancak biçimlendirmenin nasıl işlendiği konusunda önemli ölçüde farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="c9ecd-166">Bir Razor sayfası veya görünüm işlendiğinde, her Razor kodu satırı metin biçiminde HTML yayar.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="c9ecd-167">Oluşturulduktan sonra sunucu, üretilen herhangi bir durum da dahil olmak üzere sayfayı veya görünüm örneğini ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="c9ecd-168">Sayfa için başka bir istek gerçekleştiğinde, örneğin sunucu doğrulaması başarısız olduğunda ve doğrulama özeti görüntülendiğinde:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="c9ecd-169">Sayfanın tamamı HTML metnine yeniden eklenir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="c9ecd-170">Sayfa istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-170">The page is sent to the client.</span></span>

<span data-ttu-id="c9ecd-171">Blazor bir uygulama, *Bileşenler*adlı Kullanıcı arabiriminin yeniden kullanılabilir öğelerinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="c9ecd-172">Bir bileşen kod C# , biçimlendirme ve diğer bileşenleri içerir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="c9ecd-173">Bir bileşen işlendiğinde Blazor, bir HTML veya XML Belge Nesne Modeli (DOM) gibi dahil edilen bileşenlerin bir grafiğini üretir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="c9ecd-174">Bu grafik, özelliklerde ve alanlarında tutulan bileşen durumunu içerir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-174">This graph includes component state held in properties and fields.</span></span> Blazor<span data-ttu-id="c9ecd-175">, biçimlendirme ikili gösterimini üretmek için bileşen grafiğini değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-175"> evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="c9ecd-176">İkili biçimi şu şekilde olabilir:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-176">The binary format can be:</span></span>

* <span data-ttu-id="c9ecd-177">HTML metnine açıldı (prerendering sırasında).</span><span class="sxs-lookup"><span data-stu-id="c9ecd-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="c9ecd-178">Düzenli işleme sırasında biçimlendirmeyi verimli bir şekilde güncelleştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="c9ecd-179">Blazor bir kullanıcı arabirimi güncelleştirmesi tarafından tetiklenir:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="c9ecd-180">Düğme seçme gibi kullanıcı etkileşimi.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="c9ecd-181">Zamanlayıcı gibi uygulama Tetikleyicileri.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="c9ecd-182">Grafik yeniden tanımlanır ve bir UI *farkı* (fark) hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="c9ecd-183">Bu fark, istemcideki Kullanıcı arabirimini güncelleştirmek için gereken en küçük DOM düzenlemelerinin kümesidir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="c9ecd-184">Fark istemciye bir ikili biçimde gönderilir ve tarayıcı tarafından uygulanır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="c9ecd-185">Kullanıcı, istemci üzerinde bundan uzaklaştığında bir bileşen atılmış olur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="c9ecd-186">Bir Kullanıcı bir bileşenle etkileşim kurarken, bileşenin durumu (hizmetler, kaynaklar) sunucunun belleğinde tutulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="c9ecd-187">Birçok bileşenin durumu sunucu tarafından eşzamanlı olarak Korunabileceğinden, bellek tükenmesi sorunu ele alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="c9ecd-188">Sunucu belleğinin en iyi şekilde kullanılmasını sağlamak üzere Blazor sunucu uygulamasının nasıl yazılacağı hakkında yönergeler için bkz. <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server>.</span></span>

### <a name="circuits"></a><span data-ttu-id="c9ecd-189">Uygulanıp</span><span class="sxs-lookup"><span data-stu-id="c9ecd-189">Circuits</span></span>

<span data-ttu-id="c9ecd-190">Blazor sunucusu uygulaması [ASP.NET Core SignalR](xref:signalr/introduction)üzerine kurulmuştur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-190">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="c9ecd-191">Her istemci, bir *devre*olarak adlandırılan bir veya daha fazla SignalR bağlantı üzerinden sunucu ile iletişim kurar.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-191">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="c9ecd-192">Devre, geçici ağ kesintilerine tolerans sağlayan SignalR bağlantıları üzerinden Blazorsoyutlamasıdır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-192">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="c9ecd-193">Blazor istemci SignalR bağlantısının kesileceğini gördüğünde, yeni bir SignalR bağlantısı kullanarak sunucuya yeniden bağlanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-193">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="c9ecd-194">Bir Blazor sunucusu uygulamasına bağlı her tarayıcı ekranı (tarayıcı sekmesi veya IFRAME) SignalR bir bağlantı kullanır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-194">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="c9ecd-195">Bu, tipik sunucu tarafından işlenmiş uygulamalarla karşılaştırıldığında daha önemli bir ayırım ifade etmiştir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-195">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="c9ecd-196">Sunucu tarafından işlenen bir uygulamada, aynı uygulamayı birden çok tarayıcı ekranında açmak genellikle sunucuda ek kaynak taleplerine çevirilmez.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-196">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="c9ecd-197">Blazor sunucusu uygulamasında, her tarayıcı ekranı, sunucu tarafından yönetilecek ayrı bir devre ve bileşen durumunun ayrı örneklerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-197">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

Blazor<span data-ttu-id="c9ecd-198"> bir tarayıcı sekmesini kapatmayı *veya bir dış* URL 'ye gidilmesini göz önünde bulundurur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-198"> considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="c9ecd-199">Düzgün sonlandırma durumunda, devre ve ilişkili kaynaklar hemen serbest bırakılır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-199">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="c9ecd-200">Bir istemci, örneğin bir ağ kesintisi nedeniyle düzgün şekilde kesilmeyen bir şekilde kesilebilir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-200">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> Blazor<span data-ttu-id="c9ecd-201"> sunucusu, istemcinin yeniden bağlanmasına izin vermek için, yapılandırılabilir bir Aralık için bağlantısı kesilen devreleri depolar.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-201"> Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="c9ecd-202">Daha fazla bilgi için [aynı sunucuya yeniden bağlanma](#reconnection-to-the-same-server) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-202">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="c9ecd-203">UI gecikmesi</span><span class="sxs-lookup"><span data-stu-id="c9ecd-203">UI Latency</span></span>

<span data-ttu-id="c9ecd-204">UI gecikme süresi, başlatılan bir eylemden Kullanıcı arabiriminin güncelleştirildiği zamana kadar geçen süredir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-204">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="c9ecd-205">Bir uygulamanın kullanıcıya yanıt vermesi için kullanıcı ARABIRIMI gecikmesi için daha küçük değerler zorunludur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-205">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="c9ecd-206">Blazor sunucu uygulamasında her bir eylem sunucusuna gönderilir, işlenir ve bir UI farkı geri gönderilir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-206">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="c9ecd-207">Sonuç olarak, UI gecikmesi ağ gecikme süresinin toplamı ve eylemi işlerken sunucu gecikmesi sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-207">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="c9ecd-208">Özel bir kurumsal ağla sınırlı bir iş kolu uygulaması için, ağ gecikmesi nedeniyle kullanıcı gecikmesi algılarını üzerindeki etki, genellikle çok sayıda CEPSİZ olur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-208">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="c9ecd-209">Internet üzerinden dağıtılan bir uygulama için, özellikle de kullanıcılar coğrafi olarak coğrafi olarak dağıtılmışsa gecikme süresi kullanıcılara karşı farklılık gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-209">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="c9ecd-210">Bellek kullanımı ayrıca uygulama gecikme süresine de katkıda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-210">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="c9ecd-211">Daha fazla bellek kullanımı, her ikisi de uygulama performansının düşmesine neden olan ve bu nedenle kullanıcı arabirimi gecikmesini arttığı diskte sık görülen çöp toplama veya disk belleği belleği</span><span class="sxs-lookup"><span data-stu-id="c9ecd-211">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="c9ecd-212">Daha fazla bilgi için bkz. <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-212">For more information, see <xref:security/blazor/server>.</span></span>

Blazor<span data-ttu-id="c9ecd-213"> sunucu uygulamaları, ağ gecikmesini ve bellek kullanımını azaltarak UI gecikmesini en aza indirmek için iyileştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-213"> Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="c9ecd-214">Ağ gecikmesini ölçmeye yönelik bir yaklaşım için bkz. <xref:host-and-deploy/blazor/server#measure-network-latency>.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-214">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server#measure-network-latency>.</span></span> <span data-ttu-id="c9ecd-215">SignalR ve Blazorhakkında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-215">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a><span data-ttu-id="c9ecd-216">Sunucuyla bağlantı</span><span class="sxs-lookup"><span data-stu-id="c9ecd-216">Connection to the server</span></span>

Blazor<span data-ttu-id="c9ecd-217"> Server uygulamaları sunucuya etkin bir SignalR bağlantısı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-217"> Server apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="c9ecd-218">Bağlantı kaybolursa, uygulama sunucuya yeniden bağlanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-218">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="c9ecd-219">İstemcinin durumu hala bellekte olduğu sürece, istemci oturumu durum kaybı olmadan devam eder.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-219">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

#### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="c9ecd-220">Aynı sunucuya yeniden bağlanma</span><span class="sxs-lookup"><span data-stu-id="c9ecd-220">Reconnection to the same server</span></span>

<span data-ttu-id="c9ecd-221">Sunucu üzerinde kullanıcı arabirimi durumunu ayarlayan ilk istemci isteğine yanıt olarak önceden bir Blazor sunucusu uygulaması ön ekler.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-221">A Blazor Server app prerenders in response to the first client request, which sets up the UI state on the server.</span></span> <span data-ttu-id="c9ecd-222">İstemci bir SignalR bağlantısı oluşturmayı denediğinde, istemci aynı sunucuya yeniden bağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-222">When the client attempts to create a SignalR connection, the client must reconnect to the same server.</span></span> <span data-ttu-id="c9ecd-223">birden fazla arka uç sunucusu kullanan Blazor Server uygulamalarının SignalR bağlantıları için *yapışkan oturumlar* uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-223">Blazor Server apps that use more than one backend server should implement *sticky sessions* for SignalR connections.</span></span>

<span data-ttu-id="c9ecd-224">Blazor Server uygulamaları için [Azure SignalR hizmetini](/azure/azure-signalr) kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-224">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="c9ecd-225">Hizmet, Blazor sunucu uygulamasının ölçeğini çok sayıda eşzamanlı SignalR bağlantı ile ölçeklendirmeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-225">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="c9ecd-226">Azure SignalR hizmeti için, hizmetin `ServerStickyMode` seçeneği veya yapılandırma değeri `Required`olarak ayarlanarak yapışkan oturumlar etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-226">Sticky sessions are enabled for the Azure SignalR Service by setting the service's `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="c9ecd-227">Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/server#signalr-configuration>.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-227">For more information, see <xref:host-and-deploy/blazor/server#signalr-configuration>.</span></span>

<span data-ttu-id="c9ecd-228">IIS kullanırken, yapışkan oturumlar uygulama Isteği yönlendirme ile etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-228">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="c9ecd-229">Daha fazla bilgi için bkz. [uygulama Isteği yönlendirme kullanarak HTTP yük dengelemesi](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span><span class="sxs-lookup"><span data-stu-id="c9ecd-229">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="c9ecd-230">Kullanıcı arabirimindeki bağlantı durumunu yansıtır</span><span class="sxs-lookup"><span data-stu-id="c9ecd-230">Reflect the connection state in the UI</span></span>

<span data-ttu-id="c9ecd-231">İstemci bağlantının kaybolduğunu algıladığında, istemci yeniden bağlanmayı denediğinde kullanıcıya varsayılan bir kullanıcı arabirimi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-231">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="c9ecd-232">Yeniden bağlantı başarısız olursa, kullanıcıya yeniden deneme seçeneği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-232">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="c9ecd-233">Kullanıcı arabirimini özelleştirmek için *_Host. cshtml* Razor sayfasının `<body>` `components-reconnect-modal` `id` bir öğe tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-233">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```html
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="c9ecd-234">Aşağıdaki tabloda `components-reconnect-modal` öğesine uygulanan CSS sınıfları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-234">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="c9ecd-235">CSS sınıfı</span><span class="sxs-lookup"><span data-stu-id="c9ecd-235">CSS class</span></span>                       | <span data-ttu-id="c9ecd-236">&hellip; gösterir</span><span class="sxs-lookup"><span data-stu-id="c9ecd-236">Indicates&hellip;</span></span> |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | <span data-ttu-id="c9ecd-237">Kayıp bir bağlantı.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-237">A lost connection.</span></span> <span data-ttu-id="c9ecd-238">İstemci yeniden bağlanmaya çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-238">The client is attempting to reconnect.</span></span> <span data-ttu-id="c9ecd-239">Kalıcı olarak göster.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-239">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="c9ecd-240">Etkin bir bağlantı sunucuya yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-240">An active connection is re-established to the server.</span></span> <span data-ttu-id="c9ecd-241">Kalıcı olarak gizleyin.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-241">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="c9ecd-242">Muhtemelen bir ağ hatasından dolayı yeniden bağlantı başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-242">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="c9ecd-243">Yeniden bağlanmayı denemek için `window.Blazor.reconnect()`çağırın.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-243">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="c9ecd-244">Yeniden bağlantı reddedildi.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-244">Reconnection rejected.</span></span> <span data-ttu-id="c9ecd-245">Sunucuya ulaşıldı ancak bağlantı reddedildi ve kullanıcının sunucudaki durumu kayboldu.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-245">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="c9ecd-246">Uygulamayı yeniden yüklemek için `location.reload()`çağırın.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-246">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="c9ecd-247">Bu bağlantı durumu şu durumlarda oluşabilir:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-247">This connection state may result when:</span></span><ul><li><span data-ttu-id="c9ecd-248">Sunucu tarafında devre dışı bir kilitlenme oluşur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-248">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="c9ecd-249">Sunucunun kullanıcının durumunu bırakması için istemcinin bağlantısı yeterince uzun değil.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-249">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="c9ecd-250">Kullanıcının etkileşimde bulunduğu bileşenlerin örnekleri atıldı.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-250">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="c9ecd-251">Sunucu yeniden başlatıldı veya uygulamanın çalışan işlemi geri dönüştürüldü.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-251">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="c9ecd-252">Prerendering sonrasında durum bilgisi olan yeniden bağlanma</span><span class="sxs-lookup"><span data-stu-id="c9ecd-252">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="c9ecd-253">sunucu bağlantısı kurumadan önce sunucu üzerindeki kullanıcı arabirimine varsayılan olarak, Blazor Server uygulamaları varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-253">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="c9ecd-254">Bu, *_Host. cshtml* Razor sayfasında ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-254">This is set up in the *_Host.cshtml* Razor page:</span></span>

::: moniker range=">= aspnetcore-3.1"

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

<span data-ttu-id="c9ecd-255">`RenderMode`, bileşenin şunları yapıp kullanmadığını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-255">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="c9ecd-256">, Sayfaya ön gönderilir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-256">Is prerendered into the page.</span></span>
* <span data-ttu-id="c9ecd-257">, Sayfada statik HTML olarak veya Kullanıcı aracısından bir Blazor uygulamasını önyüklemek için gerekli bilgileri içeriyorsa.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-257">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

::: moniker range=">= aspnetcore-3.1"

| `RenderMode`        | <span data-ttu-id="c9ecd-258">Açıklama</span><span class="sxs-lookup"><span data-stu-id="c9ecd-258">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="c9ecd-259">Bileşeni statik HTML olarak işler ve Blazor sunucusu uygulaması için bir işaret içerir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-259">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="c9ecd-260">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-260">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="c9ecd-261">Blazor sunucusu uygulaması için bir işaret oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-261">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="c9ecd-262">Bileşen çıkışı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-262">Output from the component isn't included.</span></span> <span data-ttu-id="c9ecd-263">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-263">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="c9ecd-264">Bileşeni statik HTML olarak işler.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-264">Renders the component into static HTML.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.1"

| `RenderMode`        | <span data-ttu-id="c9ecd-265">Açıklama</span><span class="sxs-lookup"><span data-stu-id="c9ecd-265">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="c9ecd-266">Bileşeni statik HTML olarak işler ve Blazor sunucusu uygulaması için bir işaret içerir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-266">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="c9ecd-267">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-267">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="c9ecd-268">Parametreler desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-268">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="c9ecd-269">Blazor sunucusu uygulaması için bir işaret oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-269">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="c9ecd-270">Bileşen çıkışı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-270">Output from the component isn't included.</span></span> <span data-ttu-id="c9ecd-271">Kullanıcı Aracısı başladığında, bu işaretleyici bir Blazor uygulamasının önyüklemesi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-271">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="c9ecd-272">Parametreler desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-272">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="c9ecd-273">Bileşeni statik HTML olarak işler.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-273">Renders the component into static HTML.</span></span> <span data-ttu-id="c9ecd-274">Parametreler destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-274">Parameters are supported.</span></span> |

::: moniker-end

<span data-ttu-id="c9ecd-275">Statik HTML sayfasından sunucu bileşenleri işleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-275">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="c9ecd-276">`RenderMode` `ServerPrerendered`, bileşen başlangıçta sayfanın bir parçası olarak statik olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-276">When `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="c9ecd-277">Tarayıcı sunucuya geri bir bağlantı kurduğunda, bileşen *yeniden*işlenir ve bileşen artık etkileşimli olur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-277">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="c9ecd-278">Bileşeni başlatmak için bir [yaşam döngüsü yöntemi](xref:blazor/components#lifecycle-methods) varsa (`OnInitialized{Async}`), yöntemi *iki kez*yürütülür:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-278">If a [lifecycle method](xref:blazor/components#lifecycle-methods) for initializing the component (`OnInitialized{Async}`) is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="c9ecd-279">Bileşen statik olarak önceden kullanılırken.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-279">When the component is prerendered statically.</span></span>
* <span data-ttu-id="c9ecd-280">Sunucu bağlantısı kurulduktan sonra.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-280">After the server connection has been established.</span></span>

<span data-ttu-id="c9ecd-281">Bu, bileşen son işlendiğinde Kullanıcı arabiriminde görünen verilerde fark edilebilir bir değişikliğe neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-281">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="c9ecd-282">Blazor sunucu uygulamasında çift işleme senaryosunu önlemek için:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-282">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="c9ecd-283">Prerendering sırasında durumu önbelleğe almak için kullanılabilecek bir tanımlayıcı geçirin ve uygulamayı yeniden başlattıktan sonra durumu alma.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-283">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="c9ecd-284">Bileşen durumunu kaydetmek için prerendering sırasında tanımlayıcıyı kullanın.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-284">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="c9ecd-285">Önbelleğe alınan durumu almak için prerendering öğesinden sonra tanımlayıcıyı kullanın.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-285">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="c9ecd-286">Aşağıdaki kod, Çift işlemeyi engelleyen şablon tabanlı Blazor sunucu uygulamasındaki güncelleştirilmiş bir `WeatherForecastService` gösterir:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-286">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] Summaries = new[]
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
                Summary = Summaries[rng.Next(Summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="c9ecd-287">Razor sayfaları ve görünümlerinden durum bilgisi olan etkileşimli bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="c9ecd-287">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="c9ecd-288">Durum bilgisi olan etkileşimli bileşenler Razor sayfasına veya görünümüne eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-288">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="c9ecd-289">Sayfa veya görünüm şunları işler:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-289">When the page or view renders:</span></span>

* <span data-ttu-id="c9ecd-290">Bileşen sayfa veya görünümle birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-290">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="c9ecd-291">Prerendering için kullanılan ilk bileşen durumu kayboldu.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-291">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="c9ecd-292">SignalR bağlantısı oluşturulduğunda yeni bileşen durumu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-292">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="c9ecd-293">Aşağıdaki Razor sayfası bir `Counter` bileşeni işler:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-293">The following Razor page renders a `Counter` component:</span></span>

::: moniker range=">= aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="c9ecd-294">Razor sayfaları ve görünümlerinden etkileşimsiz bileşenleri işleme</span><span class="sxs-lookup"><span data-stu-id="c9ecd-294">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="c9ecd-295">Aşağıdaki Razor sayfasında, `MyComponent` bileşen bir form kullanılarak belirtilen bir başlangıç değeri ile statik olarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-295">In the following Razor page, the `MyComponent` component is statically rendered with an initial value that's specified using a form:</span></span>

::: moniker range=">= aspnetcore-3.1"

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

::: moniker-end

::: moniker range="< aspnetcore-3.1"

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

::: moniker-end

<span data-ttu-id="c9ecd-296">`MyComponent` statik olarak işlendiğinde, bileşen etkileşimli olamaz.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-296">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="c9ecd-297">Uygulamanın ne zaman prerendering olduğunu Algıla</span><span class="sxs-lookup"><span data-stu-id="c9ecd-297">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="c9ecd-298">Blazor Server uygulamaları için SignalR istemcisini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c9ecd-298">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="c9ecd-299">Bazen Blazor Server uygulamaları tarafından kullanılan SignalR istemcisini yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-299">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="c9ecd-300">Örneğin, bir bağlantı sorununu tanılamak için SignalR istemcisinde günlüğe kaydetmeyi yapılandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-300">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="c9ecd-301">*Pages/_Host. cshtml* dosyasında SignalR istemcisini yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="c9ecd-301">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="c9ecd-302">*Blazor. Server. js* betiği için `<script>` etiketine bir `autostart="false"` özniteliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-302">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="c9ecd-303">`Blazor.start` çağırın ve SignalR oluşturucuyu belirten bir yapılandırma nesnesini geçirin.</span><span class="sxs-lookup"><span data-stu-id="c9ecd-303">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="c9ecd-304">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c9ecd-304">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
