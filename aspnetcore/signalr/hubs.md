---
title: ASP.NET Core SignalR hub'ları kullanın
author: rachelappel
description: ASP.NET Core SignalR hub'ları kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: 5558a5787396c3aa8055175486369eb2534c1fa2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277676"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="1e9ef-103">Hub SignalR öğesinde ASP.NET Core için kullanın.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="1e9ef-104">Tarafından [Rachel Appel](https://twitter.com/rachelappel) ve [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="1e9ef-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="1e9ef-105">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(nasıl indirileceğini)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="1e9ef-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="1e9ef-106">Bir SignalR hub'ı nedir</span><span class="sxs-lookup"><span data-stu-id="1e9ef-106">What is a SignalR hub</span></span>

<span data-ttu-id="1e9ef-107">SignalR hub'ları API sunucudan bağlı istemcilerde yöntemlerini çağırmaya sağlar.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="1e9ef-108">Sunucu kodunda istemcisi tarafından çağrıldı yöntemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="1e9ef-109">İstemci kodu sunucudan adlı yöntemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="1e9ef-110">SignalR gerçek zamanlı istemci-sunucu ve sunucu istemci iletişimi olanaklı kılan planda her şeyi ilgilenir.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="1e9ef-111">SignalR hub'ları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="1e9ef-111">Configure SignalR hubs</span></span>

<span data-ttu-id="1e9ef-112">SignalR Ara çağırarak yapılandırılmış olan bazı hizmetler gerektirir `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="1e9ef-113">SignalR işlevi için bir ASP.NET Core uygulama eklerken, SignalR yollar çağırarak Kurulum `app.UseSignalR` içinde `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="1e9ef-114">Oluşturma ve hub'ları kullanma</span><span class="sxs-lookup"><span data-stu-id="1e9ef-114">Create and use hubs</span></span>

<span data-ttu-id="1e9ef-115">Öğesinden devralınan bir sınıf bildirme tarafından bir hub oluşturmak `Hub`ve genel yöntemler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="1e9ef-116">İstemcileri olarak tanımlanan yöntemler çağırabilir `public`.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="1e9ef-117">Dönüş türü ve tüm C# yönteminde olduğu gibi karmaşık türler ve diziler dahil olmak üzere parametreleri de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="1e9ef-118">SignalR seri hale getirme ve seri durumdan çıkarma karmaşık nesneler ve diziler parametreler ve dönüş değerlerini işler.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="1e9ef-119">İstemcileri nesnesi</span><span class="sxs-lookup"><span data-stu-id="1e9ef-119">The Clients object</span></span>

<span data-ttu-id="1e9ef-120">Her örneği `Hub` sınıfı adlı bir özelliğe sahiptir `Clients` , sunucu ve istemci arasındaki iletişim için aşağıdaki üyeleri içerir:</span><span class="sxs-lookup"><span data-stu-id="1e9ef-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="1e9ef-121">Özellik</span><span class="sxs-lookup"><span data-stu-id="1e9ef-121">Property</span></span> | <span data-ttu-id="1e9ef-122">Açıklama</span><span class="sxs-lookup"><span data-stu-id="1e9ef-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="1e9ef-123">Bağlanan tüm istemciler üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="1e9ef-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="1e9ef-124">Hub yöntemini çağırmış istemciye bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="1e9ef-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="1e9ef-125">Yöntemi çağrıldıktan istemci dışındaki bağlanan tüm istemciler üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="1e9ef-125">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="1e9ef-126">Ayrıca, `Hub.Clients` aşağıdaki yöntemleri içerir:</span><span class="sxs-lookup"><span data-stu-id="1e9ef-126">Additionally, `Hub.Clients` contains the following methods:</span></span>

| <span data-ttu-id="1e9ef-127">Yöntem</span><span class="sxs-lookup"><span data-stu-id="1e9ef-127">Method</span></span> | <span data-ttu-id="1e9ef-128">Açıklama</span><span class="sxs-lookup"><span data-stu-id="1e9ef-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="1e9ef-129">Bağlanan tüm istemciler belirtilen bağlantıların dışında bir yöntem çağırır</span><span class="sxs-lookup"><span data-stu-id="1e9ef-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="1e9ef-130">Belirli bir bağlı istemci üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="1e9ef-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="1e9ef-131">Belirli bağlı istemcilerde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="1e9ef-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="1e9ef-132">Belirtilen grubun tüm bağlantılar için bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="1e9ef-132">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="1e9ef-133">Belirtilen bağlantıların dışında belirtilen gruptaki tüm bağlantılar için bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="1e9ef-133">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="1e9ef-134">Birden çok grup bağlantıları için bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="1e9ef-134">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="1e9ef-135">Hub yöntemini çağıran istemci hariç bağlantıları, bir grup için bir yöntem çağırır</span><span class="sxs-lookup"><span data-stu-id="1e9ef-135">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="1e9ef-136">Belirli bir kullanıcıyla ilişkili tüm bağlantılar için bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="1e9ef-136">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="1e9ef-137">Belirtilen kullanıcılar ile ilişkilendirilmiş tüm bağlantılar için bir yöntem çağırır</span><span class="sxs-lookup"><span data-stu-id="1e9ef-137">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="1e9ef-138">Her bir özellik veya yöntem yukarıdaki tablolarda sahip bir nesne döndürür bir `SendAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="1e9ef-139">`SendAsync` Yöntemi çağırmak için istemci yönteminin parametreleri ve adını sağlayın olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="1e9ef-140">İstemcilere iletileri gönder</span><span class="sxs-lookup"><span data-stu-id="1e9ef-140">Send messages to clients</span></span>

<span data-ttu-id="1e9ef-141">Belirli istemcileri çağrı yapmak için özelliklerini kullanmak `Clients` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="1e9ef-142">Aşağıdaki örnekte, `SendMessageToCaller` hub yöntemini çağırmış bağlantı için ileti gönderme yöntemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-142">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="1e9ef-143">`SendMessageToGroups` Yöntemi depolanan gruplarına bir ileti gönderir bir `List` adlı `groups`.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="1e9ef-144">Bağlantı olayları işlemek</span><span class="sxs-lookup"><span data-stu-id="1e9ef-144">Handle events for a connection</span></span>

<span data-ttu-id="1e9ef-145">SignalR hub'ları API sağlar `OnConnectedAsync` ve `OnDisconnectedAsync` yönetmek ve bağlantıları izlemek için sanal yöntemler.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="1e9ef-146">Geçersiz kılma `OnConnectedAsync` istemci bir gruba ekleme gibi Hub'ına bağlandığında eylemleri gerçekleştirmek için sanal bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="1e9ef-147">Hata işleme</span><span class="sxs-lookup"><span data-stu-id="1e9ef-147">Handle errors</span></span>

<span data-ttu-id="1e9ef-148">Hub yöntemlerinde oluşturulan özel durumları yöntemini çağırmış istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="1e9ef-149">JavaScript istemcide `invoke` yöntemi döndürür bir [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="1e9ef-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="1e9ef-150">İstemci bir işleyici ile bir hata olduğunda alır bağlı promise kullanmaya `catch`, çağrılan ve JavaScript geçirilen `Error` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="1e9ef-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="1e9ef-151">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1e9ef-151">Related resources</span></span>

* [<span data-ttu-id="1e9ef-152">ASP.NET Core SignalR giriş</span><span class="sxs-lookup"><span data-stu-id="1e9ef-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="1e9ef-153">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="1e9ef-153">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="1e9ef-154">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="1e9ef-154">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
