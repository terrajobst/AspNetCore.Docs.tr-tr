---
title: ASP.NET Core signalr'da hubs'ı kullanma
author: tdykstra
description: İçinde ASP.NET Core SignalR hub'ı kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: be39666373e2b099054bb71f4a7fcf17aeb9a01c
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095287"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="7b7e0-103">ASP.NET Core signalr'da hubs'ı kullanma</span><span class="sxs-lookup"><span data-stu-id="7b7e0-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="7b7e0-104">Tarafından [Rachel Appel](https://twitter.com/rachelappel) ve [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="7b7e0-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="7b7e0-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(karşıdan yükleme)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="7b7e0-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="7b7e0-106">Bir SignalR hub'ı nedir</span><span class="sxs-lookup"><span data-stu-id="7b7e0-106">What is a SignalR hub</span></span>

<span data-ttu-id="7b7e0-107">SignalR hub'ları API sunucudan bağlı istemciler üzerinde yöntemleri çağırmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="7b7e0-108">Sunucu kodunda istemci tarafından çağrılan yöntemlere tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="7b7e0-109">İstemci kodda sunucudan çağrılan yöntemlere tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="7b7e0-110">SignalR gerçek zamanlı istemci-sunucu ve sunucu istemci iletişimi olanaklı kılan arka planda her şeyin üstlenir.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="7b7e0-111">SignalR hub'ları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7b7e0-111">Configure SignalR hubs</span></span>

<span data-ttu-id="7b7e0-112">SignalR ara yazılım çağırarak yapılandırılan bazı hizmetler gerektirir `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="7b7e0-113">ASP.NET Core uygulaması için SignalR işlevselliği ekleme, SignalR yollar çağırarak Kurulum `app.UseSignalR` içinde `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="7b7e0-114">Oluşturma ve hub'ları kullanma</span><span class="sxs-lookup"><span data-stu-id="7b7e0-114">Create and use hubs</span></span>

<span data-ttu-id="7b7e0-115">Öğesinden devralınan bir sınıf bildirerek oluşturma `Hub`ve genel yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="7b7e0-116">İstemcileri olarak tanımlanmış olan yöntemleri çağırabilir `public`.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="7b7e0-117">Dönüş türü ve parametreleri, tüm C# yönteminde olduğu gibi karmaşık türler ve diziler de dahil olmak üzere belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="7b7e0-118">SignalR serileştirme ve seri durumundan çıkarma karmaşık nesneler ve diziler parametreleri ve dönüş değerleri işler.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="7b7e0-119">İstemciler nesnesi</span><span class="sxs-lookup"><span data-stu-id="7b7e0-119">The Clients object</span></span>

<span data-ttu-id="7b7e0-120">Her bir örneği `Hub` sınıfında adlı bir özellik `Clients` , sunucu ve istemci arasındaki iletişim için aşağıdaki üyeleri içerir:</span><span class="sxs-lookup"><span data-stu-id="7b7e0-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="7b7e0-121">Özellik</span><span class="sxs-lookup"><span data-stu-id="7b7e0-121">Property</span></span> | <span data-ttu-id="7b7e0-122">Açıklama</span><span class="sxs-lookup"><span data-stu-id="7b7e0-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="7b7e0-123">Bağlanan tüm istemciler üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="7b7e0-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="7b7e0-124">Bir hub yöntemini çağırmış istemciye bir metod çağırır</span><span class="sxs-lookup"><span data-stu-id="7b7e0-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="7b7e0-125">Yöntemini çağırmış istemciye dışındaki bağlanan tüm istemciler üzerinde bir yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-125">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="7b7e0-126">Ayrıca, `Hub.Clients` aşağıdaki yöntemleri içerir:</span><span class="sxs-lookup"><span data-stu-id="7b7e0-126">Additionally, `Hub.Clients` contains the following methods:</span></span>

| <span data-ttu-id="7b7e0-127">Yöntem</span><span class="sxs-lookup"><span data-stu-id="7b7e0-127">Method</span></span> | <span data-ttu-id="7b7e0-128">Açıklama</span><span class="sxs-lookup"><span data-stu-id="7b7e0-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="7b7e0-129">Bağlanan tüm istemciler belirtilen bağlantılar dışında bir yöntem çağırır.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="7b7e0-130">Belirli bir bağlı istemci üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="7b7e0-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="7b7e0-131">Belirli bir bağlı istemciler üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="7b7e0-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="7b7e0-132">Belirtilen grubun tüm bağlantılar için bir yöntem çağırır</span><span class="sxs-lookup"><span data-stu-id="7b7e0-132">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="7b7e0-133">Belirtilen gruptaki belirtilen bağlantılar dışında tüm bağlantılar için bir yöntem çağırır.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-133">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="7b7e0-134">Birden çok gruba bağlantılarının bir metod çağırır</span><span class="sxs-lookup"><span data-stu-id="7b7e0-134">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="7b7e0-135">Bir hub yöntemini çağırmış istemciye hariç bağlantıları, grup için bir yöntem çağırır</span><span class="sxs-lookup"><span data-stu-id="7b7e0-135">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="7b7e0-136">Belirli bir kullanıcıyla ilişkili tüm bağlantıları için bir yöntem çağırır</span><span class="sxs-lookup"><span data-stu-id="7b7e0-136">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="7b7e0-137">Belirtilen kullanıcılar ile ilişkili tüm bağlantıları için bir yöntem çağırır</span><span class="sxs-lookup"><span data-stu-id="7b7e0-137">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="7b7e0-138">Her bir özellik veya yöntemin önceki tablolarda sahip bir nesne döndürür. bir `SendAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="7b7e0-139">`SendAsync` Yöntemi çağırmak için istemci yönteminin parametreleri ve adını girmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="7b7e0-140">İstemciler için iletileri gönder</span><span class="sxs-lookup"><span data-stu-id="7b7e0-140">Send messages to clients</span></span>

<span data-ttu-id="7b7e0-141">Özel istemciler çağrı yapmak için özelliklerini kullanmak `Clients` nesne.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="7b7e0-142">Aşağıdaki örnekte, `SendMessageToCaller` yöntemi gösterir hub yöntemini çağırmış bağlantıya ileti gönderme.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-142">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="7b7e0-143">`SendMessageToGroups` Yöntemi depolanan gruplara bir ileti gönderen bir `List` adlı `groups`.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="7b7e0-144">Bağlantı olayları işleme</span><span class="sxs-lookup"><span data-stu-id="7b7e0-144">Handle events for a connection</span></span>

<span data-ttu-id="7b7e0-145">SignalR hub'ları API sağlar `OnConnectedAsync` ve `OnDisconnectedAsync` yönetmek ve bağlantıları izlemek için sanal yöntemler.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="7b7e0-146">Geçersiz kılma `OnConnectedAsync` bir istemci bir gruba ekleme gibi Hub'ına bağlandığında eylemleri gerçekleştirmek için sanal bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="7b7e0-147">Hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="7b7e0-147">Handle errors</span></span>

<span data-ttu-id="7b7e0-148">Özel durumlar, hub yöntemlerinde oluşturulan yöntemini çağırmış istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="7b7e0-149">JavaScript istemci `invoke` yöntemi döndürür bir [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="7b7e0-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="7b7e0-150">İstemci bir işleyici ile bir hata aldığında bağlı promise kullanmaya `catch`, çağrılan ve bir JavaScript olarak geçirilen `Error` nesne.</span><span class="sxs-lookup"><span data-stu-id="7b7e0-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="7b7e0-151">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7b7e0-151">Related resources</span></span>

* [<span data-ttu-id="7b7e0-152">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="7b7e0-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="7b7e0-153">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="7b7e0-153">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="7b7e0-154">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="7b7e0-154">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
