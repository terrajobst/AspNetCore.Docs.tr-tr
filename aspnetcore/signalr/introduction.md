---
title: ASP.NET Core signalr'a giriş
author: tdykstra
description: Uygulamalar için gerçek zamanlı işlevsellik eklemeye ASP.NET Core SignalR kitaplığı nasıl kolaylaştırdığını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: bc6f25c3f35e7fb0c2c68220697f2e0fdc6a9958
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095395"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="f1d53-103">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="f1d53-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="f1d53-104">Tarafından [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="f1d53-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="f1d53-105">SignalR nedir?</span><span class="sxs-lookup"><span data-stu-id="f1d53-105">What is SignalR?</span></span>

<span data-ttu-id="f1d53-106">ASP.NET Core SignalR uygulamalarına gerçek zamanlı web işlevselliği ekleme basitleştiren bir kitaplıktır.</span><span class="sxs-lookup"><span data-stu-id="f1d53-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="f1d53-107">Gerçek zamanlı web işlevselliği, sunucu tarafı kodu anında içeriği istemcilere anında sağlar.</span><span class="sxs-lookup"><span data-stu-id="f1d53-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="f1d53-108">SignalR için iyi adaylar:</span><span class="sxs-lookup"><span data-stu-id="f1d53-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="f1d53-109">Yüksek sıklık düzeyi güncelleştirmeleri sunucudan gerektiren uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="f1d53-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="f1d53-110">Oyunlar, sosyal ağlar, oylama, artırma, haritalar ve GPS uygulamaları verilebilir.</span><span class="sxs-lookup"><span data-stu-id="f1d53-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="f1d53-111">Panoları ve izleme uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="f1d53-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="f1d53-112">Örnek şirket panoları, anında satış güncelleştirmeleri içerir ya da seyahat uyarıları.</span><span class="sxs-lookup"><span data-stu-id="f1d53-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="f1d53-113">İşbirliğine dayalı uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="f1d53-113">Collaborative apps.</span></span> <span data-ttu-id="f1d53-114">Beyaz Tahta uygulamaları ve yazılım toplantı takım işbirliğine dayalı uygulamalar örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="f1d53-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="f1d53-115">Bildirimleri gerektiren uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="f1d53-115">Apps that require notifications.</span></span> <span data-ttu-id="f1d53-116">Sosyal ağlar, e-posta, sohbet, oyunlar, seyahat uyarılarına ve diğer birçok uygulama bildirimleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="f1d53-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="f1d53-117">SignalR server istemcisi oluşturmak için bir API sağlar [uzaktan yordam çağrısı (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="f1d53-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="f1d53-118">RPC JavaScript işlevlerini sunucu tarafı .NET Core koddan istemcilerde çağırın.</span><span class="sxs-lookup"><span data-stu-id="f1d53-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="f1d53-119">ASP.NET Core için SignalR:</span><span class="sxs-lookup"><span data-stu-id="f1d53-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="f1d53-120">Bağlantı Yönetimi otomatik olarak işler.</span><span class="sxs-lookup"><span data-stu-id="f1d53-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="f1d53-121">İletileri eşzamanlı olarak bağlanan tüm istemciler için yayın etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="f1d53-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="f1d53-122">Sohbet odası.</span><span class="sxs-lookup"><span data-stu-id="f1d53-122">For example, a chat room.</span></span>
* <span data-ttu-id="f1d53-123">Özel istemciler veya istemci gruplarının ileti gönderilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="f1d53-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="f1d53-124">Açık kaynak haline getirildi adresindeki [GitHub](https://github.com/aspnet/signalr).</span><span class="sxs-lookup"><span data-stu-id="f1d53-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="f1d53-125">Ölçeklenebilir.</span><span class="sxs-lookup"><span data-stu-id="f1d53-125">Scalable.</span></span>

<span data-ttu-id="f1d53-126">Bir HTTP bağlantısı farklı olarak istemci ve sunucu arasındaki bağlantıyı kalıcıdır.</span><span class="sxs-lookup"><span data-stu-id="f1d53-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="f1d53-127">Taşımalar</span><span class="sxs-lookup"><span data-stu-id="f1d53-127">Transports</span></span>

<span data-ttu-id="f1d53-128">Gerçek zamanlı web uygulamaları oluşturmaya yönelik teknikleri sayısı üzerinden SignalR özetleri.</span><span class="sxs-lookup"><span data-stu-id="f1d53-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="f1d53-129">[WebSockets](https://tools.ietf.org/html/rfc7118) en iyi Aktarım, ancak bu kullanılamayan Server-Sent olayları ve uzun yoklama gibi başka teknikler kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f1d53-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="f1d53-130">SignalR otomatik olarak algılar ve sunucu ve istemci desteklenen özelliklere bağlı olarak uygun bir taşıma başlatılamıyor.</span><span class="sxs-lookup"><span data-stu-id="f1d53-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="f1d53-131">Hub'ları</span><span class="sxs-lookup"><span data-stu-id="f1d53-131">Hubs</span></span>

<span data-ttu-id="f1d53-132">SignalR hub'ları, istemciler ve sunucular arasında iletişim kurmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="f1d53-132">SignalR uses hubs to communicate between clients and servers.</span></span>

<span data-ttu-id="f1d53-133">Bir hub'ı, istemci ve sunucu birbirleri üzerinde yöntemleri çağırmak izin veren bir üst düzey bir işlem hattı ' dir.</span><span class="sxs-lookup"><span data-stu-id="f1d53-133">A hub is a high-level pipeline that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="f1d53-134">SignalR istemcilerinin yerel yöntemler olarak kolayca ve tersi olarak, sunucu üzerinde yöntemleri çağırmak otomatik olarak makine sınırlarında gönderme işler.</span><span class="sxs-lookup"><span data-stu-id="f1d53-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="f1d53-135">Hub yöntemleri için kesin türü belirtilmiş parametre geçirme model bağlama sağlayan izin verin.</span><span class="sxs-lookup"><span data-stu-id="f1d53-135">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="f1d53-136">SignalR sağlayan iki yerleşik hub protokol: bir metin protokolü temel JSON ve temel bir ikili Protokolü [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="f1d53-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="f1d53-137">MessagePack genellikle JSON kullanırken daha küçük ileti oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f1d53-137">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="f1d53-138">Eski tarayıcılar desteklemelidir [XHR Düzey 2](https://caniuse.com/#feat=xhr2) MessagePack protokolü desteği sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="f1d53-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="f1d53-139">Hub'ları, istemci tarafı kod kullanarak etkin iletiler göndererek çağırın.</span><span class="sxs-lookup"><span data-stu-id="f1d53-139">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="f1d53-140">İletiler, adı ve istemci tarafı yönteminin parametreleri içerir.</span><span class="sxs-lookup"><span data-stu-id="f1d53-140">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="f1d53-141">Yapılandırılmış protokolü kullanarak yöntem parametreleri olarak gönderilen nesneleri seri.</span><span class="sxs-lookup"><span data-stu-id="f1d53-141">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="f1d53-142">İstemci, istemci tarafı kod içinde bir yönteme adıyla eşleşecek şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="f1d53-142">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="f1d53-143">Bir eşleşme olduğunda, istemci yöntemi kullanılarak seri durumdan çıkarılmış parametre veri çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="f1d53-143">When a match happens, the client method runs using the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f1d53-144">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f1d53-144">Additional resources</span></span>

* [<span data-ttu-id="f1d53-145">ASP.NET Core için SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="f1d53-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="f1d53-146">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="f1d53-146">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="f1d53-147">Merkezler</span><span class="sxs-lookup"><span data-stu-id="f1d53-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="f1d53-148">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="f1d53-148">JavaScript client</span></span>](xref:signalr/javascript-client)
