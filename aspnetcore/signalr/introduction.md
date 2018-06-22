---
title: ASP.NET Core SignalR giriş
author: rachelappel
description: Uygulamalar için gerçek zamanlı işlevsellik ekleme ASP.NET Core SignalR kitaplığı nasıl basitleştirir öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 0c833acea139d22883a69a02c2357a71f3ac8db8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277910"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="7de16-103">ASP.NET Core SignalR giriş</span><span class="sxs-lookup"><span data-stu-id="7de16-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="7de16-104">Tarafından [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="7de16-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="7de16-105">SignalR nedir?</span><span class="sxs-lookup"><span data-stu-id="7de16-105">What is SignalR?</span></span>

<span data-ttu-id="7de16-106">ASP.NET Core SignalR uygulamalarını ekleme gerçek zamanlı web işlevselliği basitleştiren bir kitaplıktır.</span><span class="sxs-lookup"><span data-stu-id="7de16-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="7de16-107">Gerçek zamanlı web işlevselliği sunucu tarafı kodu anında içeriği istemcilere hemen sağlar.</span><span class="sxs-lookup"><span data-stu-id="7de16-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="7de16-108">SignalR için iyi adaylar:</span><span class="sxs-lookup"><span data-stu-id="7de16-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="7de16-109">Yüksek yoğunlukta güncelleştirmeleri sunucudan gerektiren uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="7de16-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="7de16-110">Örnek oyunlara, sosyal ağlara, oylama, artırma, eşlemeleri ve GPS uygulamaları verilebilir.</span><span class="sxs-lookup"><span data-stu-id="7de16-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="7de16-111">Panolar ve izleme uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="7de16-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="7de16-112">Örnek şirket panoları, anlık satış güncelleştirmeleri dahil veya uyarıları seyahat.</span><span class="sxs-lookup"><span data-stu-id="7de16-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="7de16-113">İşbirlikçi uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="7de16-113">Collaborative apps.</span></span> <span data-ttu-id="7de16-114">Beyaz Tahta uygulamaları ve yazılım toplantı takım işbirliği uygulamalara örnektir.</span><span class="sxs-lookup"><span data-stu-id="7de16-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="7de16-115">Bildirimleri gerektiren uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="7de16-115">Apps that require notifications.</span></span> <span data-ttu-id="7de16-116">Sosyal ağlar, e-posta, sohbet, oyunlar, seyahat uyarıları ve diğer birçok uygulama bildirimleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="7de16-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="7de16-117">SignalR server istemcisi oluşturmak için bir API sağlar [uzaktan yordam çağrısı (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="7de16-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="7de16-118">RPC'ler JavaScript işlevlerini sunucu tarafı .NET Core koddan istemcilerde çağırın.</span><span class="sxs-lookup"><span data-stu-id="7de16-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="7de16-119">SignalR ASP.NET Core için:</span><span class="sxs-lookup"><span data-stu-id="7de16-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="7de16-120">Bağlantı Yönetimi otomatik olarak yönetir.</span><span class="sxs-lookup"><span data-stu-id="7de16-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="7de16-121">İletileri aynı anda bağlanan tüm istemciler için yayın etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="7de16-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="7de16-122">Sohbet yer.</span><span class="sxs-lookup"><span data-stu-id="7de16-122">For example, a chat room.</span></span>
* <span data-ttu-id="7de16-123">Belirli istemciler veya istemci gruplarının ileti gönderilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="7de16-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="7de16-124">Açık kaynaklıdır konumunda olduğundan [GitHub](https://github.com/aspnet/signalr).</span><span class="sxs-lookup"><span data-stu-id="7de16-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="7de16-125">Ölçeklenebilir.</span><span class="sxs-lookup"><span data-stu-id="7de16-125">Scalable.</span></span>

<span data-ttu-id="7de16-126">Bir HTTP bağlantısı farklı olarak istemci ve sunucu arasındaki bağlantının kalıcıdır.</span><span class="sxs-lookup"><span data-stu-id="7de16-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="7de16-127">Taşımalar</span><span class="sxs-lookup"><span data-stu-id="7de16-127">Transports</span></span>

<span data-ttu-id="7de16-128">Gerçek zamanlı web uygulamaları oluşturmak için teknikleri sayısı üzerinden SignalR özetleri.</span><span class="sxs-lookup"><span data-stu-id="7de16-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="7de16-129">[WebSockets](https://tools.ietf.org/html/rfc7118) en iyi Aktarım, ancak bunlar kullanılamaz Server-Sent olayları ve uzun yoklama gibi başka teknikler kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7de16-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="7de16-130">SignalR otomatik olarak algılar ve sunucu ve istemci üzerinde desteklenen özelliklere bağlı olarak uygun bir taşıma başlatılamıyor.</span><span class="sxs-lookup"><span data-stu-id="7de16-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="7de16-131">Hub'ları</span><span class="sxs-lookup"><span data-stu-id="7de16-131">Hubs</span></span>

<span data-ttu-id="7de16-132">SignalR hub'ları istemcileri ve sunucuları arasında iletişim kurmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="7de16-132">SignalR uses hubs to communicate between clients and servers.</span></span>

<span data-ttu-id="7de16-133">Bir hub istemci ve sunucu birbirine yöntemlerini çağırmaya izin veren üst düzey bir işlem hattı ' dir.</span><span class="sxs-lookup"><span data-stu-id="7de16-133">A hub is a high-level pipeline that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="7de16-134">SignalR makine sınırları içinde otomatik olarak göndermeyi kolayca yerel yöntemleri ve tersi olarak sunucuda yöntemlerini çağırmaya istemcilere izin verme işler.</span><span class="sxs-lookup"><span data-stu-id="7de16-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="7de16-135">Hub yöntemleri için kesin tür belirtilmiş parametreleri geçirme model bağlama sağlayan izin verir.</span><span class="sxs-lookup"><span data-stu-id="7de16-135">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="7de16-136">SignalR iki yerleşik hub protokol sağlar: bir metin protokolü temel JSON ve temel bir ikili protokol [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="7de16-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="7de16-137">MessagePack genellikle JSON kullanırken daha küçük ileti oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7de16-137">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="7de16-138">Eski tarayıcılar desteklemelidir [XHR Düzey 2](https://caniuse.com/#feat=xhr2) MessagePack protokolü desteği sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="7de16-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="7de16-139">Hub etkin taşımanın kullanarak iletiler göndererek istemci-tarafı kodu çağırma.</span><span class="sxs-lookup"><span data-stu-id="7de16-139">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="7de16-140">İletileri adını ve istemci tarafı yönteminin parametrelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="7de16-140">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="7de16-141">Nesneleri yöntem parametreleri, yapılandırılmış protokolü kullanılarak seri durumdan olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="7de16-141">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="7de16-142">İstemci, istemci tarafı kodlar yönteminde adına eşleştirmeyi dener.</span><span class="sxs-lookup"><span data-stu-id="7de16-142">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="7de16-143">Bir eşleşme gerçekleştiğinde, istemci yöntemi seri durumdan çıkarılmış parametre veri kullanarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="7de16-143">When a match happens, the client method runs using the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7de16-144">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7de16-144">Additional resources</span></span>

* [<span data-ttu-id="7de16-145">ASP.NET Core için SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="7de16-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="7de16-146">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="7de16-146">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="7de16-147">Merkezler</span><span class="sxs-lookup"><span data-stu-id="7de16-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="7de16-148">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="7de16-148">JavaScript client</span></span>](xref:signalr/javascript-client)
