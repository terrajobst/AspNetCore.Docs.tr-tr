---
title: ASP.NET Core SignalR hub'ları kullanın
author: rachelappel
description: ASP.NET Core SignalR hub'ları kullanmayı öğrenin.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 7da0c4832b1aa6a844172bf751a46b280a02f37a
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="d46da-103">Hub SignalR öğesinde ASP.NET Core için kullanın.</span><span class="sxs-lookup"><span data-stu-id="d46da-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="d46da-104">Tarafından [Rachel Appel](https://twitter.com/rachelappel) ve [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="d46da-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="d46da-105">Bir SignalR hub'ı nedir</span><span class="sxs-lookup"><span data-stu-id="d46da-105">What is a SignalR hub</span></span>

<span data-ttu-id="d46da-106">SignalR hub'ları API sunucudan bağlı istemcilerde yöntemlerini çağırmaya sağlar.</span><span class="sxs-lookup"><span data-stu-id="d46da-106">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="d46da-107">Sunucu kodunda istemcisi tarafından çağrıldı yöntemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d46da-107">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="d46da-108">İstemci kodu sunucudan adlı yöntemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d46da-108">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="d46da-109">SignalR gerçek zamanlı istemci-sunucu ve sunucu istemci iletişimi olanaklı kılan planda her şeyi ilgilenir.</span><span class="sxs-lookup"><span data-stu-id="d46da-109">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="d46da-110">SignalR hub'ları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d46da-110">Configure SignalR hubs</span></span>

<span data-ttu-id="d46da-111">SignalR Ara çağırarak yapılandırılmış olan bazı hizmetler gerektirir `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="d46da-111">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=35)]

<span data-ttu-id="d46da-112">SignalR işlevi için bir ASP.NET Core uygulama eklerken, SignalR yollar çağırarak Kurulum `app.UseSignalR` içinde `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d46da-112">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=55-58)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="d46da-113">Oluşturma ve hub'ları kullanma</span><span class="sxs-lookup"><span data-stu-id="d46da-113">Create and use hubs</span></span>

<span data-ttu-id="d46da-114">Öğesinden devralınan bir sınıf bildirme tarafından bir hub oluşturmak `Hub`ve genel yöntemler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d46da-114">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="d46da-115">İstemcileri olarak tanımlanan yöntemler çağırabilir `public`.</span><span class="sxs-lookup"><span data-stu-id="d46da-115">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/chathub.cs?range=10-13)]

<span data-ttu-id="d46da-116">Dönüş türü ve tüm C# yönteminde olduğu gibi karmaşık türler ve diziler dahil olmak üzere parametreleri de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d46da-116">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="d46da-117">SignalR seri hale getirme ve seri durumdan çıkarma karmaşık nesneler ve diziler parametreler ve dönüş değerlerini işler.</span><span class="sxs-lookup"><span data-stu-id="d46da-117">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="d46da-118">İstemcileri nesnesi</span><span class="sxs-lookup"><span data-stu-id="d46da-118">The Clients object</span></span>

<span data-ttu-id="d46da-119">Her örneği `Hub` sınıfı adlı bir özelliğe sahiptir `Clients` , sunucu ve istemci arasındaki iletişim için aşağıdaki üyeleri içerir:</span><span class="sxs-lookup"><span data-stu-id="d46da-119">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="d46da-120">Özellik</span><span class="sxs-lookup"><span data-stu-id="d46da-120">Property</span></span> | <span data-ttu-id="d46da-121">Açıklama</span><span class="sxs-lookup"><span data-stu-id="d46da-121">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="d46da-122">Bağlanan tüm istemciler üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="d46da-122">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="d46da-123">Hub yöntemini çağırmış istemciye bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="d46da-123">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="d46da-124">Yöntemi çağrıldıktan istemci dışındaki bağlanan tüm istemciler üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="d46da-124">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="d46da-125">Ayrıca, `Hub` sınıf aşağıdaki yöntemleri içerir:</span><span class="sxs-lookup"><span data-stu-id="d46da-125">Additionally, the `Hub` class contains the following methods:</span></span>

| <span data-ttu-id="d46da-126">Yöntem</span><span class="sxs-lookup"><span data-stu-id="d46da-126">Method</span></span> | <span data-ttu-id="d46da-127">Açıklama</span><span class="sxs-lookup"><span data-stu-id="d46da-127">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="d46da-128">Bağlanan tüm istemciler belirtilen bağlantıların dışında bir yöntem çağırır</span><span class="sxs-lookup"><span data-stu-id="d46da-128">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="d46da-129">Belirli bir bağlı istemci üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="d46da-129">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="d46da-130">Belirli bağlı istemcilerde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="d46da-130">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="d46da-131">Belirtilen gruptaki tüm bağlantılara bir ileti gönderir</span><span class="sxs-lookup"><span data-stu-id="d46da-131">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="d46da-132">Belirtilen bağlantıların dışında belirtilen gruptaki tüm bağlantılara bir ileti gönderir</span><span class="sxs-lookup"><span data-stu-id="d46da-132">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="d46da-133">Bağlantıları birden çok gruba ileti gönderir</span><span class="sxs-lookup"><span data-stu-id="d46da-133">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="d46da-134">Bir hub yöntemini çağıran istemci hariç bağlantıları, gruba bir ileti gönderir</span><span class="sxs-lookup"><span data-stu-id="d46da-134">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="d46da-135">Belirli bir kullanıcıyla ilişkili tüm bağlantılara bir ileti gönderir</span><span class="sxs-lookup"><span data-stu-id="d46da-135">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="d46da-136">Belirtilen kullanıcılar ile ilişkili tüm bağlantılara bir ileti gönderir</span><span class="sxs-lookup"><span data-stu-id="d46da-136">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="d46da-137">Her bir özellik veya yöntem yukarıdaki tablolarda sahip bir nesne döndürür bir `SendAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d46da-137">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="d46da-138">`SendAsync` Yöntemi çağırmak için istemci yönteminin parametreleri ve adını sağlayın olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="d46da-138">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="d46da-139">İstemcilere iletileri gönder</span><span class="sxs-lookup"><span data-stu-id="d46da-139">Send messages to clients</span></span>

<span data-ttu-id="d46da-140">Belirli istemcileri çağrı yapmak için özelliklerini kullanmak `Clients` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="d46da-140">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="d46da-141">Aşağıdaki örnekte, aşağıdaki `SendMessageToCaller` hub yöntemini çağırmış bağlantı için ileti gönderme yöntemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="d46da-141">In the following In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="d46da-142">`SendMessageToGroups` Yöntemi depolanan gruplarına bir ileti gönderir bir `List` adlı `groups`.</span><span class="sxs-lookup"><span data-stu-id="d46da-142">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="d46da-143">Bağlantı olayları işlemek</span><span class="sxs-lookup"><span data-stu-id="d46da-143">Handle events for a connection</span></span>

<span data-ttu-id="d46da-144">SignalR hub'ları API sağlar `OnConnectedAsync` ve `OnDisconnectedAsync` yönetmek ve bağlantıları izlemek için sanal yöntemler.</span><span class="sxs-lookup"><span data-stu-id="d46da-144">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="d46da-145">Geçersiz kılma `OnConnectedAsync` istemci bir gruba ekleme gibi Hub'ına bağlandığında eylemleri gerçekleştirmek için sanal bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="d46da-145">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/chathub.cs?range=26-30)]

## <a name="handle-errors"></a><span data-ttu-id="d46da-146">Hata işleme</span><span class="sxs-lookup"><span data-stu-id="d46da-146">Handle errors</span></span>

<span data-ttu-id="d46da-147">Hub yöntemlerinde oluşturulan özel durumları yöntemini çağırmış istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="d46da-147">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="d46da-148">JavaScript istemcide `invoke` yöntemi döndürür bir [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="d46da-148">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="d46da-149">İstemci bir işleyici ile bir hata olduğunda alır bağlı promise kullanmaya `catch`, çağrılan ve JavaScript geçirilen `Error` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="d46da-149">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/chat.js?range=20)]
[!code-javascript[Error](hubs/sample/chat.js?range=16-18)]

## <a name="related-resources"></a><span data-ttu-id="d46da-150">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d46da-150">Related resources</span></span>

[<span data-ttu-id="d46da-151">ASP.NET Core SignalR giriş</span><span class="sxs-lookup"><span data-stu-id="d46da-151">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)