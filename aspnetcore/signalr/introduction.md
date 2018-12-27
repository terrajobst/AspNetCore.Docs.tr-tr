---
title: ASP.NET Core signalr'a giriş
author: tdykstra
description: Uygulamalar için gerçek zamanlı işlevsellik eklemeye ASP.NET Core SignalR kitaplığı nasıl kolaylaştırdığını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: da18837c690d2182589db5f486ae74e537ade931
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425152"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="2f026-103">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="2f026-103">Introduction to ASP.NET Core SignalR</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="2f026-104">SignalR nedir?</span><span class="sxs-lookup"><span data-stu-id="2f026-104">What is SignalR?</span></span>

<span data-ttu-id="2f026-105">ASP.NET Core SignalR, uygulamalara gerçek zamanlı web işlevselliği ekleme basitleştiren bir açık kaynak kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="2f026-105">ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="2f026-106">Gerçek zamanlı web işlevselliği, sunucu tarafı kodu anında içeriği istemcilere anında sağlar.</span><span class="sxs-lookup"><span data-stu-id="2f026-106">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="2f026-107">SignalR için iyi adaylar:</span><span class="sxs-lookup"><span data-stu-id="2f026-107">Good candidates for SignalR:</span></span>

* <span data-ttu-id="2f026-108">Yüksek sıklık düzeyi güncelleştirmeleri sunucudan gerektiren uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="2f026-108">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="2f026-109">Oyunlar, sosyal ağlar, oylama, artırma, haritalar ve GPS uygulamaları verilebilir.</span><span class="sxs-lookup"><span data-stu-id="2f026-109">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="2f026-110">Panoları ve izleme uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="2f026-110">Dashboards and monitoring apps.</span></span> <span data-ttu-id="2f026-111">Örnek şirket panoları, anında satış güncelleştirmeleri içerir ya da seyahat uyarıları.</span><span class="sxs-lookup"><span data-stu-id="2f026-111">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="2f026-112">İşbirliğine dayalı uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="2f026-112">Collaborative apps.</span></span> <span data-ttu-id="2f026-113">Beyaz Tahta uygulamaları ve yazılım toplantı takım işbirliğine dayalı uygulamalar örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="2f026-113">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="2f026-114">Bildirimleri gerektiren uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="2f026-114">Apps that require notifications.</span></span> <span data-ttu-id="2f026-115">Sosyal ağlar, e-posta, sohbet, oyunlar, seyahat uyarılarına ve diğer birçok uygulama bildirimleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="2f026-115">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="2f026-116">SignalR server istemcisi oluşturmak için bir API sağlar [uzaktan yordam çağrısı (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="2f026-116">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="2f026-117">RPC JavaScript işlevlerini sunucu tarafı .NET Core koddan istemcilerde çağırın.</span><span class="sxs-lookup"><span data-stu-id="2f026-117">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="2f026-118">ASP.NET Core için SignalR özelliklerinden bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2f026-118">Here are some features of SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="2f026-119">Bağlantı Yönetimi otomatik olarak işler.</span><span class="sxs-lookup"><span data-stu-id="2f026-119">Handles connection management automatically.</span></span>
* <span data-ttu-id="2f026-120">İletileri eşzamanlı olarak bağlanan tüm istemciler için gönderir.</span><span class="sxs-lookup"><span data-stu-id="2f026-120">Sends messages to all connected clients simultaneously.</span></span> <span data-ttu-id="2f026-121">Sohbet odası.</span><span class="sxs-lookup"><span data-stu-id="2f026-121">For example, a chat room.</span></span>
* <span data-ttu-id="2f026-122">Özel istemciler veya istemci gruplarının iletileri gönderir.</span><span class="sxs-lookup"><span data-stu-id="2f026-122">Sends messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="2f026-123">Artan trafiği işlemeye ölçeklendirir.</span><span class="sxs-lookup"><span data-stu-id="2f026-123">Scales to handle increasing traffic.</span></span>

<span data-ttu-id="2f026-124">Kaynak barındırılan bir [GitHub deposunu SignalR](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span><span class="sxs-lookup"><span data-stu-id="2f026-124">The source is hosted in a [SignalR repository on GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span></span>

## <a name="transports"></a><span data-ttu-id="2f026-125">Taşımalar</span><span class="sxs-lookup"><span data-stu-id="2f026-125">Transports</span></span>

<span data-ttu-id="2f026-126">SignalR, gerçek zamanlı iletişimler işlemek için çeşitli teknikler destekler:</span><span class="sxs-lookup"><span data-stu-id="2f026-126">SignalR supports several techniques for handling real-time communications:</span></span>

* [<span data-ttu-id="2f026-127">WebSockets</span><span class="sxs-lookup"><span data-stu-id="2f026-127">WebSockets</span></span>](https://tools.ietf.org/html/rfc7118)
* <span data-ttu-id="2f026-128">Sunucu tarafından gönderilen olayları</span><span class="sxs-lookup"><span data-stu-id="2f026-128">Server-Sent Events</span></span>
* <span data-ttu-id="2f026-129">Uzun yoklama</span><span class="sxs-lookup"><span data-stu-id="2f026-129">Long Polling</span></span>

<span data-ttu-id="2f026-130">SignalR istemci ve sunucu kapasitesini en iyi aktarım yöntemi otomatik olarak seçer.</span><span class="sxs-lookup"><span data-stu-id="2f026-130">SignalR automatically chooses the best transport method that is within the capabilities of the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="2f026-131">Hub'ları</span><span class="sxs-lookup"><span data-stu-id="2f026-131">Hubs</span></span>

<span data-ttu-id="2f026-132">SignalR kullanan *hubs* istemciler ve sunucular arasında iletişim kurmak için.</span><span class="sxs-lookup"><span data-stu-id="2f026-132">SignalR uses *hubs* to communicate between clients and servers.</span></span>

<span data-ttu-id="2f026-133">Bir hub'ı istemci ve sunucu birbirleri üzerinde yöntemleri çağırmak izin veren bir üst düzey bir işlem hattı ' dir.</span><span class="sxs-lookup"><span data-stu-id="2f026-133">A hub is a high-level pipeline that allows a client and server to call methods on each other.</span></span> <span data-ttu-id="2f026-134">SignalR sunucusunda ve yöntemlerini çağırmak istemcilerin otomatik olarak makine sınırlarında gönderme işler.</span><span class="sxs-lookup"><span data-stu-id="2f026-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa.</span></span> <span data-ttu-id="2f026-135">Model bağlama sağlayan yöntemleri için parametre türü kesin belirlenmiş geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f026-135">You can pass strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="2f026-136">SignalR sağlayan iki yerleşik hub protokol: bir metin protokolü temel JSON ve temel bir ikili Protokolü [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="2f026-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="2f026-137">MessagePack genellikle JSON ile karşılaştırıldığında daha küçük iletileri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2f026-137">MessagePack generally creates smaller messages compared to JSON.</span></span> <span data-ttu-id="2f026-138">Eski tarayıcılar desteklemelidir [XHR Düzey 2](https://caniuse.com/#feat=xhr2) MessagePack protokolü desteği sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="2f026-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="2f026-139">Hub adı ve istemci tarafı yönteminin parametreleri içeren iletiler göndererek istemci-tarafı kodu çağırın.</span><span class="sxs-lookup"><span data-stu-id="2f026-139">Hubs call client-side code by sending messages that contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="2f026-140">Yapılandırılmış protokolü kullanarak yöntem parametreleri olarak gönderilen nesneleri seri.</span><span class="sxs-lookup"><span data-stu-id="2f026-140">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="2f026-141">İstemci, istemci tarafı kod içinde bir yönteme adıyla eşleşecek şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="2f026-141">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="2f026-142">İstemci bir eşleşme bulduğunda yöntemini çağırır ve seri durumdan çıkarılmış parametre verileri geçirir.</span><span class="sxs-lookup"><span data-stu-id="2f026-142">When the client finds a match, it calls the method and passes to it the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f026-143">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2f026-143">Additional resources</span></span>

* [<span data-ttu-id="2f026-144">ASP.NET Core için SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="2f026-144">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="2f026-145">Desteklenen Platformlar</span><span class="sxs-lookup"><span data-stu-id="2f026-145">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="2f026-146">Merkezler</span><span class="sxs-lookup"><span data-stu-id="2f026-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="2f026-147">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="2f026-147">JavaScript client</span></span>](xref:signalr/javascript-client)
