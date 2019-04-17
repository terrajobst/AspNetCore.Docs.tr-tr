---
title: Blazor barındırma modelleri
author: guardrex
description: İstemci tarafı Blazor ve sunucu tarafı ASP.NET Core Razor modelleri barındırma bileşenlerini anlayın.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/hosting-models
ms.openlocfilehash: 0e7598c9f27201a989d1088764e09461a37beae4
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614848"
---
# <a name="blazor-hosting-models"></a><span data-ttu-id="11a42-103">Blazor barındırma modelleri</span><span class="sxs-lookup"><span data-stu-id="11a42-103">Blazor hosting models</span></span>

<span data-ttu-id="11a42-104">Tarafından [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="11a42-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="11a42-105">Blazor istemci-tarafı çalışmak üzere tasarlanmış bir web çerçevesi olan tarayıcıda bir [WebAssembly](http://webassembly.org/)-.NET çalışma zamanı tabanlı (*Blazor*) veya sunucu tarafı ASP.NET Core (*ASP.NET Core Razor bileşenleri*).</span><span class="sxs-lookup"><span data-stu-id="11a42-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](http://webassembly.org/)-based .NET runtime (*Blazor*) or server-side in ASP.NET Core (*ASP.NET Core Razor Components*).</span></span> <span data-ttu-id="11a42-106">Barındırma modeli, uygulama ve bileşen modelleri bakılmaksızın *aynı kalması*.</span><span class="sxs-lookup"><span data-stu-id="11a42-106">Regardless of the hosting model, the app and component models *remain the same*.</span></span> <span data-ttu-id="11a42-107">Bu makalede, kullanılabilen barındırma modelleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="11a42-107">This article discusses the available hosting models:</span></span>

* [<span data-ttu-id="11a42-108">İstemci tarafı Blazor</span><span class="sxs-lookup"><span data-stu-id="11a42-108">Client-side Blazor</span></span>](#client-side-hosting-model)
* [<span data-ttu-id="11a42-109">Sunucu tarafı ASP.NET Core Razor bileşenleri</span><span class="sxs-lookup"><span data-stu-id="11a42-109">Server-side ASP.NET Core Razor Components</span></span>](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a><span data-ttu-id="11a42-110">İstemci tarafı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="11a42-110">Client-side hosting model</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="11a42-111">Asıl barındırma için Blazor WebAssembly tarayıcıda çalışan istemci-tarafı modelidir.</span><span class="sxs-lookup"><span data-stu-id="11a42-111">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="11a42-112">Tarayıcıya .NET çalışma zamanı Blazor uygulamayı ve bağımlılıkları indirilir.</span><span class="sxs-lookup"><span data-stu-id="11a42-112">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="11a42-113">Uygulamayı doğrudan tarayıcıda kullanıcı Arabirimi iş parçacığında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="11a42-113">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="11a42-114">Kullanıcı Arabirimi güncelleştirmeleri ve olay işleme, aynı işlem içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="11a42-114">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="11a42-115">Statik dosya olarak bir web sunucusu veya hizmeti statik içeriği istemcilere hizmet uygulamasının varlıklarını dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="11a42-115">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor istemci-tarafı: Tarayıcı içinde bir kullanıcı Arabirimi iş parçacığında Blazor uygulama çalışır.](hosting-models/_static/client-side.png)

<span data-ttu-id="11a42-117">İstemci tarafı barındırma modeli kullanarak bir Blazor uygulaması oluşturmak için aşağıdaki şablonlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="11a42-117">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="11a42-118">**Blazor** ([dotnet yeni blazor](/dotnet/core/tools/dotnet-new)) &ndash; statik dosyalar bir dizi dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="11a42-118">**Blazor** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="11a42-119">**Blazor (ASP.NET Core barındırılan)** ([dotnet yeni blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; bir ASP.NET Core sunucusu tarafından barındırılan.</span><span class="sxs-lookup"><span data-stu-id="11a42-119">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="11a42-120">ASP.NET Core uygulaması Blazor uygulama istemcilere hizmet.</span><span class="sxs-lookup"><span data-stu-id="11a42-120">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="11a42-121">Web API çağrıları kullanarak ağ üzerinden istemci-tarafı Blazor uygulama sunucusu ile etkileşim kurabilir veya [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="11a42-121">The client-side Blazor app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="11a42-122">Şablonları içerir *components.webassembly.js* işleyen betik:</span><span class="sxs-lookup"><span data-stu-id="11a42-122">The templates include the *components.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="11a42-123">.NET çalışma zamanı, uygulama ve uygulamanın bağımlılıklarını karşıdan yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="11a42-123">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="11a42-124">Uygulamayı çalıştırmak için çalışma zamanı başlatma.</span><span class="sxs-lookup"><span data-stu-id="11a42-124">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="11a42-125">İstemci tarafı barındırma modeli, çeşitli avantajlar sunar.</span><span class="sxs-lookup"><span data-stu-id="11a42-125">The client-side hosting model offers several benefits.</span></span> <span data-ttu-id="11a42-126">İstemci tarafı Blazor:</span><span class="sxs-lookup"><span data-stu-id="11a42-126">Client-side Blazor:</span></span>

* <span data-ttu-id="11a42-127">.NET sunucu tarafı bağımlılığı yoktur.</span><span class="sxs-lookup"><span data-stu-id="11a42-127">Has no .NET server-side dependency.</span></span>
* <span data-ttu-id="11a42-128">İstemci kaynakları ve özellikleri tam olarak yararlanır.</span><span class="sxs-lookup"><span data-stu-id="11a42-128">Fully leverages client resources and capabilities.</span></span>
* <span data-ttu-id="11a42-129">Yük boşaltma, sunucudan istemciye çalışır.</span><span class="sxs-lookup"><span data-stu-id="11a42-129">Offloads work from the server to the client.</span></span>
* <span data-ttu-id="11a42-130">Çevrimdışı senaryolarını destekler.</span><span class="sxs-lookup"><span data-stu-id="11a42-130">Supports offline scenarios.</span></span>

<span data-ttu-id="11a42-131">İstemci tarafı barındırma downsides vardır.</span><span class="sxs-lookup"><span data-stu-id="11a42-131">There are downsides to client-side hosting.</span></span> <span data-ttu-id="11a42-132">İstemci tarafı Blazor:</span><span class="sxs-lookup"><span data-stu-id="11a42-132">Client-side Blazor:</span></span>

* <span data-ttu-id="11a42-133">Uygulama, tarayıcının yeteneklerini kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="11a42-133">Restricts the app to the capabilities of the browser.</span></span>
* <span data-ttu-id="11a42-134">Özellikli istemci donanım ve yazılım (örneğin, WebAssembly desteği) gerektirir.</span><span class="sxs-lookup"><span data-stu-id="11a42-134">Requires capable client hardware and software (for example, WebAssembly support).</span></span>
* <span data-ttu-id="11a42-135">İndirme boyutu daha büyük ve uzun uygulama yükleme süresi vardır.</span><span class="sxs-lookup"><span data-stu-id="11a42-135">Has a larger download size and longer app load time.</span></span>
* <span data-ttu-id="11a42-136">.NET çalışma zamanı ve araç desteği yetişkin daha az olan (örneğin, kısıtlamaları [.NET Standard](/dotnet/standard/net-standard) desteği ve hata ayıklama).</span><span class="sxs-lookup"><span data-stu-id="11a42-136">Has less mature .NET runtime and tooling support (for example, limitations in [.NET Standard](/dotnet/standard/net-standard) support and debugging).</span></span>

## <a name="server-side-hosting-model"></a><span data-ttu-id="11a42-137">Sunucu tarafı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="11a42-137">Server-side hosting model</span></span>

<span data-ttu-id="11a42-138">Sunucu tarafı barındırma modeli ile uygulama sunucusunda bir ASP.NET Core uygulaması içinde yürütülür.</span><span class="sxs-lookup"><span data-stu-id="11a42-138">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="11a42-139">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrılarını üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.</span><span class="sxs-lookup"><span data-stu-id="11a42-139">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Tarayıcı uygulaması (bir ASP.NET Core uygulaması içinde barındırılan) ile sunucu üzerinde bir SignalR bağlantısı üzerinden etkileşim kurar.](hosting-models/_static/server-side.png)

<span data-ttu-id="11a42-141">Sunucu tarafı barındırma modeli kullanarak bir Blazor uygulaması oluşturmak için ASP.NET Core kullanan **Razor bileşenleri** şablonu ([dotnet yeni razorcomponents](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="11a42-141">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Razor Components** template ([dotnet new razorcomponents](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="11a42-142">ASP.NET Core uygulaması Razor bileşenleri sunucu-tarafı uygulamasını barındıran ve istemcilerin eriştikleri SignalR uç noktasını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="11a42-142">The ASP.NET Core app hosts the Razor Components server-side app and sets up the SignalR endpoint where clients connect.</span></span> <span data-ttu-id="11a42-143">ASP.NET Core uygulaması uygulamanın başvuran `Startup` sınıfı eklemek için:</span><span class="sxs-lookup"><span data-stu-id="11a42-143">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="11a42-144">Sunucu tarafı Razor bileşenleri Hizmetleri.</span><span class="sxs-lookup"><span data-stu-id="11a42-144">Server-side Razor Components services.</span></span>
* <span data-ttu-id="11a42-145">Ardışık Düzen işleme isteği için uygulama.</span><span class="sxs-lookup"><span data-stu-id="11a42-145">The app to the request handling pipeline.</span></span>

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

<span data-ttu-id="11a42-146">*Components.server.js* betik&dagger; istemci bağlantı kurar.</span><span class="sxs-lookup"><span data-stu-id="11a42-146">The *components.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="11a42-147">Bunu kalıcı hale getirmek ve gerektiğinde (örneğin, durumunda kayıp ağ bağlantısı) uygulama durumunu geri yüklemek için uygulamanın sorumluluğudur.</span><span class="sxs-lookup"><span data-stu-id="11a42-147">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="11a42-148">Sunucu tarafı barındırma modeli, çeşitli avantajlar sunar.</span><span class="sxs-lookup"><span data-stu-id="11a42-148">The server-side hosting model offers several benefits.</span></span> <span data-ttu-id="11a42-149">Sunucu tarafı Razor bileşenler:</span><span class="sxs-lookup"><span data-stu-id="11a42-149">Server-side Razor Components:</span></span>

* <span data-ttu-id="11a42-150">Bir istemci-tarafı Blazor uygulaması daha önemli ölçüde daha küçük bir uygulama boyuta sahiptir ve çok daha hızlı yükleyin.</span><span class="sxs-lookup"><span data-stu-id="11a42-150">Have a significantly smaller app size than a client-side Blazor app and load much faster.</span></span>
* <span data-ttu-id="11a42-151">Sunucu özellikleri, herhangi bir .NET Core uyumlu API'sini kullanma dahil olmak üzere tüm avantajlarından yararlanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="11a42-151">Can take full advantage of server capabilities, including using any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="11a42-152">Araç, hata ayıklama gibi mevcut .NET beklendiği gibi çalışır. Bu nedenle .NET Core üzerine sunucusunda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="11a42-152">Run on .NET Core on the server, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="11a42-153">Çalışan ince istemciler (örneğin, WebAssembly ve kaynak desteklemeyen tarayıcılar cihazları kısıtlı).</span><span class="sxs-lookup"><span data-stu-id="11a42-153">Works with thin clients (for example, browsers that don't support WebAssembly and resource constrained devices).</span></span>
* <span data-ttu-id="11a42-154">.NET /C# kod tabanına uygulama bileşeni kod dahil olmak üzere, istemcilere hizmet değil.</span><span class="sxs-lookup"><span data-stu-id="11a42-154">.NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="11a42-155">Sunucu tarafı barındırma downsides vardır.</span><span class="sxs-lookup"><span data-stu-id="11a42-155">There are downsides to server-side hosting.</span></span> <span data-ttu-id="11a42-156">Sunucu tarafı Razor bileşenler:</span><span class="sxs-lookup"><span data-stu-id="11a42-156">Server-side Razor Components:</span></span>

* <span data-ttu-id="11a42-157">Daha yüksek gecikme süresi vardır: Her bir kullanıcı etkileşimi bir ağ atlama içerir.</span><span class="sxs-lookup"><span data-stu-id="11a42-157">Have higher latency: Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="11a42-158">Çevrimdışı desteği sunar: İstemci bağlantı başarısız olursa, Uygulama çalışmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="11a42-158">Offer no offline support: If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="11a42-159">Ölçeklenebilirlik tüketildikleri: Sunucu, birden çok istemci bağlantılarını yönetme ve istemci durumu işleme.</span><span class="sxs-lookup"><span data-stu-id="11a42-159">Have reduced scalability: The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="11a42-160">Uygulama hizmet vermek için bir ASP.NET Core sunucusu gerekir.</span><span class="sxs-lookup"><span data-stu-id="11a42-160">Require an ASP.NET Core server to serve the app.</span></span> <span data-ttu-id="11a42-161">Dağıtım bir sunucudan (örneğin, bir CDN) olmadan mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="11a42-161">Deployment without a server (for example, from a CDN) isn't possible.</span></span>

<span data-ttu-id="11a42-162">&dagger;*Components.server.js* betik aşağıdaki yola yayımlanan: *bin / {hata ayıklama | Yayın} / {hedef çerçeve} /publish/ {uygulama-adı}. Uygulama/dağıtım/_framework*.</span><span class="sxs-lookup"><span data-stu-id="11a42-162">&dagger;The *components.server.js* script is published to the following path: *bin/{Debug|Release}/{TARGET FRAMEWORK}/publish/{APPLICATION NAME}.App/dist/_framework*.</span></span>
