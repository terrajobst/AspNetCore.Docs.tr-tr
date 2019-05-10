---
title: ASP.NET Core signalr'da hubs'ı kullanma
author: bradygaster
description: İçinde ASP.NET Core SignalR hub'ı kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/20/2018
uid: signalr/hubs
ms.openlocfilehash: eb87aab2b7f3a58c6cec80f48f7616749f0809e2
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902900"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="bb1c4-103">ASP.NET Core signalr'da hubs'ı kullanma</span><span class="sxs-lookup"><span data-stu-id="bb1c4-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="bb1c4-104">Tarafından [Rachel Appel](https://twitter.com/rachelappel) ve [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="bb1c4-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="bb1c4-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(karşıdan yükleme)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="bb1c4-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="bb1c4-106">Bir SignalR hub'ı nedir</span><span class="sxs-lookup"><span data-stu-id="bb1c4-106">What is a SignalR hub</span></span>

<span data-ttu-id="bb1c4-107">SignalR hub'ları API sunucudan bağlı istemciler üzerinde yöntemleri çağırmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="bb1c4-108">Sunucu kodunda istemci tarafından çağrılan yöntemlere tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="bb1c4-109">İstemci kodda sunucudan çağrılan yöntemlere tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="bb1c4-110">SignalR gerçek zamanlı istemci-sunucu ve sunucu istemci iletişimi olanaklı kılan arka planda her şeyin üstlenir.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="bb1c4-111">SignalR hub'ları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bb1c4-111">Configure SignalR hubs</span></span>

<span data-ttu-id="bb1c4-112">SignalR ara yazılım çağırarak yapılandırılan bazı hizmetler gerektirir `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="bb1c4-113">ASP.NET Core uygulaması için SignalR işlevselliği ekleme, SignalR yollar çağırarak Kurulum `app.UseSignalR` içinde `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="bb1c4-114">Oluşturma ve hub'ları kullanma</span><span class="sxs-lookup"><span data-stu-id="bb1c4-114">Create and use hubs</span></span>

<span data-ttu-id="bb1c4-115">Öğesinden devralınan bir sınıf bildirerek oluşturma `Hub`ve genel yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="bb1c4-116">İstemcileri olarak tanımlanmış olan yöntemleri çağırabilir `public`.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-116">Clients can call methods that are defined as `public`.</span></span>

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

<span data-ttu-id="bb1c4-117">Dönüş türü ve parametreleri, tüm C# yönteminde olduğu gibi karmaşık türler ve diziler de dahil olmak üzere belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="bb1c4-118">SignalR serileştirme ve seri durumundan çıkarma karmaşık nesneler ve diziler parametreleri ve dönüş değerleri işler.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="bb1c4-119">Hub geçici:</span><span class="sxs-lookup"><span data-stu-id="bb1c4-119">Hubs are transient:</span></span>
>
> * <span data-ttu-id="bb1c4-120">Durum hub sınıfına bir özellik içinde depolamayın.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-120">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="bb1c4-121">Her bir hub yöntemi çağrısı, yeni bir hub örneği üzerinde yürütülür.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-121">Every hub method call is executed on a new hub instance.</span></span>  
> * <span data-ttu-id="bb1c4-122">Kullanım `await` Canlı kalma hub'da bağımlı zaman uyumsuz yöntemleri çağrılırken.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-122">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="bb1c4-123">Örneğin, bir yöntem gibi `Clients.All.SendAsync(...)` olmadan çağrılırsa başarısız olabilir `await` ve önce hub yöntemi tamamlayan `SendAsync` tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-123">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="bb1c4-124">Bağlam nesnesi</span><span class="sxs-lookup"><span data-stu-id="bb1c4-124">The Context object</span></span>

<span data-ttu-id="bb1c4-125">`Hub` Sınıfında bir `Context` bağlantı hakkında bilgi içeren aşağıdaki özellikleri içeren özellik:</span><span class="sxs-lookup"><span data-stu-id="bb1c4-125">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="bb1c4-126">Özellik</span><span class="sxs-lookup"><span data-stu-id="bb1c4-126">Property</span></span> | <span data-ttu-id="bb1c4-127">Açıklama</span><span class="sxs-lookup"><span data-stu-id="bb1c4-127">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="bb1c4-128">SignalR tarafından atanan bağlantı için benzersiz kimlik alır.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-128">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="bb1c4-129">Her bağlantı için bir bağlantı kimliği yok.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-129">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="bb1c4-130">Alır [kullanıcı tanımlayıcısı](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="bb1c4-130">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="bb1c4-131">Varsayılan olarak, SignalR kullanır `ClaimTypes.NameIdentifier` gelen `ClaimsPrincipal` kullanıcı tanımlayıcısı olarak bağlantı ile ilişkili.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-131">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="bb1c4-132">Alır `ClaimsPrincipal` geçerli kullanıcı ile ilişkili.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-132">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="bb1c4-133">Bu bağlantının kapsamı içinde veri paylaşmak için kullanılan bir anahtar/değer koleksiyonunu alır.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-133">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="bb1c4-134">Bu koleksiyonda veri depolanabilir ve farklı bir hub yöntemi çağrılarını arasında bağlantı için korunur.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-134">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="bb1c4-135">Bağlantıda özelliklerin koleksiyonunu alır.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-135">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="bb1c4-136">Ayrıntılı olarak henüz belgelenmemiştir için şu an için çoğu senaryoda, bu koleksiyon gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-136">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="bb1c4-137">Alır bir `CancellationToken` bağlantı iptal edildiğinde bildirir.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-137">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="bb1c4-138">`Hub.Context` Ayrıca aşağıdaki yöntemleri içerir:</span><span class="sxs-lookup"><span data-stu-id="bb1c4-138">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="bb1c4-139">Yöntem</span><span class="sxs-lookup"><span data-stu-id="bb1c4-139">Method</span></span> | <span data-ttu-id="bb1c4-140">Açıklama</span><span class="sxs-lookup"><span data-stu-id="bb1c4-140">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="bb1c4-141">Döndürür `HttpContext` bir bağlantı veya `null` bağlantı bir HTTP isteği ile ilişkili değilse.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-141">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="bb1c4-142">HTTP bağlantıları için HTTP üst bilgileri ve sorgu dizeleri gibi bilgileri almak için bu yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-142">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="bb1c4-143">Bağlantıyı durdurur.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-143">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="bb1c4-144">İstemciler nesnesi</span><span class="sxs-lookup"><span data-stu-id="bb1c4-144">The Clients object</span></span>

<span data-ttu-id="bb1c4-145">`Hub` Sınıfında bir `Clients` sunucu ve istemci arasındaki iletişim için aşağıdaki özellikleri içeren özelliği:</span><span class="sxs-lookup"><span data-stu-id="bb1c4-145">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="bb1c4-146">Özellik</span><span class="sxs-lookup"><span data-stu-id="bb1c4-146">Property</span></span> | <span data-ttu-id="bb1c4-147">Açıklama</span><span class="sxs-lookup"><span data-stu-id="bb1c4-147">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="bb1c4-148">Bağlanan tüm istemciler üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="bb1c4-148">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="bb1c4-149">Bir hub yöntemini çağırmış istemciye bir metod çağırır</span><span class="sxs-lookup"><span data-stu-id="bb1c4-149">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="bb1c4-150">Yöntemini çağırmış istemciye dışındaki bağlanan tüm istemciler üzerinde bir yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-150">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="bb1c4-151">`Hub.Clients` Ayrıca aşağıdaki yöntemleri içerir:</span><span class="sxs-lookup"><span data-stu-id="bb1c4-151">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="bb1c4-152">Yöntem</span><span class="sxs-lookup"><span data-stu-id="bb1c4-152">Method</span></span> | <span data-ttu-id="bb1c4-153">Açıklama</span><span class="sxs-lookup"><span data-stu-id="bb1c4-153">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="bb1c4-154">Bağlanan tüm istemciler belirtilen bağlantılar dışında bir yöntem çağırır.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-154">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="bb1c4-155">Belirli bir bağlı istemci üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="bb1c4-155">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="bb1c4-156">Belirli bir bağlı istemciler üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="bb1c4-156">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="bb1c4-157">Belirtilen gruptaki tüm bağlantıları üzerinde bir yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-157">Calls a method on all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="bb1c4-158">Belirtilen gruptaki belirtilen bağlantılar dışında tüm bağlantıları üzerinde bir yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-158">Calls a method on all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="bb1c4-159">Birden çok bağlantı grupları üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="bb1c4-159">Calls a method on multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="bb1c4-160">Bir hub yöntemini çağırmış istemciye hariç bağlantıları, grup üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="bb1c4-160">Calls a method on a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="bb1c4-161">Belirli bir kullanıcıyla ilişkili tüm bağlantıları üzerinde bir yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-161">Calls a method on all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="bb1c4-162">Belirtilen kullanıcılar ile ilişkili tüm bağlantıları üzerinde bir yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-162">Calls a method on all connections associated with the specified users</span></span> |

<span data-ttu-id="bb1c4-163">Her bir özellik veya yöntemin önceki tablolarda sahip bir nesne döndürür. bir `SendAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-163">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="bb1c4-164">`SendAsync` Yöntemi çağırmak için istemci yönteminin parametreleri ve adını girmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-164">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="bb1c4-165">İstemciler için iletileri gönder</span><span class="sxs-lookup"><span data-stu-id="bb1c4-165">Send messages to clients</span></span>

<span data-ttu-id="bb1c4-166">Özel istemciler çağrı yapmak için özelliklerini kullanmak `Clients` nesne.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-166">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="bb1c4-167">Aşağıdaki örnekte, üç Hub yöntemleri vardır:</span><span class="sxs-lookup"><span data-stu-id="bb1c4-167">In the following example, there are three Hub methods:</span></span>

* <span data-ttu-id="bb1c4-168">`SendMessage` kullanan tüm bağlı istemciler için bir ileti gönderir `Clients.All`.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-168">`SendMessage` sends a message to all connected clients, using `Clients.All`.</span></span>
* <span data-ttu-id="bb1c4-169">`SendMessageToCaller` kullanarak çağırana geri, bir ileti gönderir `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-169">`SendMessageToCaller` sends a message back to the caller, using `Clients.Caller`.</span></span>
* <span data-ttu-id="bb1c4-170">`SendMessageToGroups` tüm istemciler için bir ileti gönderir `SignalR Users` grubu.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-170">`SendMessageToGroups` sends a message to all clients in the `SignalR Users` group.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="bb1c4-171">Kesin türü belirtilmiş hub'ları</span><span class="sxs-lookup"><span data-stu-id="bb1c4-171">Strongly typed hubs</span></span>

<span data-ttu-id="bb1c4-172">Kullanmanın bir dezavantajı `SendAsync` olan çağrılacak istemci yöntemi belirtmek için Sihirli bir dize kullanır.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-172">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="bb1c4-173">Bu yöntem adı yanlış yazılmış, çalışma zamanı hataları için kod açık ya da istemciden eksik bırakır.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-173">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="bb1c4-174">Kullanmaya alternatif `SendAsync` kesin yazmaktır `Hub` ile <xref:Microsoft.AspNetCore.SignalR.Hub%601>.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-174">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub%601>.</span></span> <span data-ttu-id="bb1c4-175">Aşağıdaki örnekte, `ChatHub` istemci yöntemleri ayıklandı out adlı bir arabirim `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-175">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="bb1c4-176">Bu arabirim, önceki yeniden kullanılabilir `ChatHub` örnek.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-176">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="bb1c4-177">Kullanarak `Hub<IChatClient>` derleme zamanı istemci yöntemleri denetimini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-177">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="bb1c4-178">Bu Sihirli dize beri kullanımından kaynaklanan sorunları önler `Hub<T>` yalnızca arabirim içinde tanımlanmış yöntemleri erişim sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-178">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="bb1c4-179">Türü kesin belirlenmiş kullanarak `Hub<T>` kullanma yeteneği devre dışı bırakır `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-179">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span> <span data-ttu-id="bb1c4-180">Arabirimde tanımlanan herhangi bir yöntemin zaman uyumsuz olarak yine de tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-180">Any methods defined on the interface can still be defined as asynchronous.</span></span> <span data-ttu-id="bb1c4-181">Aslında, bu yöntemlerin her biri döndürmelidir bir `Task`.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-181">In fact, each of these methods should return a `Task`.</span></span> <span data-ttu-id="bb1c4-182">Bir arabirim olduğundan, kullanmayın `async` anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-182">Since it's an interface, don't use the `async` keyword.</span></span> <span data-ttu-id="bb1c4-183">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bb1c4-183">For example:</span></span>

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> <span data-ttu-id="bb1c4-184">`Async` Soneki olmayan bir yöntem adı kesilmiş.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-184">The `Async` suffix isn't stripped from the method name.</span></span> <span data-ttu-id="bb1c4-185">İstemci yönteminizi tanımlı olmadığı sürece `.on('MyMethodAsync')`, kullanmamalısınız `MyMethodAsync` adı.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-185">Unless your client method is defined with `.on('MyMethodAsync')`, you shouldn't use `MyMethodAsync` as a name.</span></span>

## <a name="change-the-name-of-a-hub-method"></a><span data-ttu-id="bb1c4-186">Hub yönteminin adını değiştirin</span><span class="sxs-lookup"><span data-stu-id="bb1c4-186">Change the name of a hub method</span></span>

<span data-ttu-id="bb1c4-187">Varsayılan olarak, bir sunucu hub yönteminin adı, .NET yöntemi adıdır.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-187">By default, a server hub method name is the name of the .NET method.</span></span> <span data-ttu-id="bb1c4-188">Ancak, kullanabileceğiniz [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) el ile yöntemi için bir ad belirtin ve bu varsayılanı değiştirmek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-188">However, you can use the [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribute to change this default and manually specify a name for the method.</span></span> <span data-ttu-id="bb1c4-189">İstemci yöntemi çağrılırken .NET yöntemi adı yerine bu adı kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-189">The client should use this name, instead of the .NET method name, when invoking the method.</span></span>

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="bb1c4-190">Bağlantı olayları işleme</span><span class="sxs-lookup"><span data-stu-id="bb1c4-190">Handle events for a connection</span></span>

<span data-ttu-id="bb1c4-191">SignalR hub'ları API sağlar `OnConnectedAsync` ve `OnDisconnectedAsync` yönetmek ve bağlantıları izlemek için sanal yöntemler.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-191">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="bb1c4-192">Geçersiz kılma `OnConnectedAsync` bir istemci bir gruba ekleme gibi Hub'ına bağlandığında eylemleri gerçekleştirmek için sanal bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-192">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

<span data-ttu-id="bb1c4-193">Geçersiz kılma `OnDisconnectedAsync` istemci kestiğinde eylemleri gerçekleştirmek için sanal bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-193">Override the `OnDisconnectedAsync` virtual method to perform actions when a client disconnects.</span></span> <span data-ttu-id="bb1c4-194">İstemcinin kasıtlı olarak kesilirse (çağırarak `connection.stop()`, örneğin), `exception` parametre olacak `null`.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-194">If the client disconnects intentionally (by calling `connection.stop()`, for example), the `exception` parameter will be `null`.</span></span> <span data-ttu-id="bb1c4-195">Ancak, istemcinin (örneğin, bir ağ hatası), bir hata nedeniyle kesilmiş `exception` parametresi hatayı açıklayan bir özel durum içerir.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-195">However, if the client is disconnected due to an error (such as a network failure), the `exception` parameter will contain an exception describing the failure.</span></span>

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a><span data-ttu-id="bb1c4-196">Hataları işleme</span><span class="sxs-lookup"><span data-stu-id="bb1c4-196">Handle errors</span></span>

<span data-ttu-id="bb1c4-197">Özel durumlar, hub yöntemlerinde oluşturulan yöntemini çağırmış istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-197">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="bb1c4-198">JavaScript istemci `invoke` yöntemi döndürür bir [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="bb1c4-198">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="bb1c4-199">İstemci bir işleyici ile bir hata aldığında bağlı promise kullanmaya `catch`, çağrılan ve bir JavaScript olarak geçirilen `Error` nesne.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-199">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

<span data-ttu-id="bb1c4-200">Hub'ınıza bir özel durum oluşturursa, kapatılan bağlantılar değildir.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-200">If your Hub throws an exception, connections aren't closed.</span></span> <span data-ttu-id="bb1c4-201">Varsayılan olarak, SignalR istemci için genel bir hata iletisi döndürür.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-201">By default, SignalR returns a generic error message to the client.</span></span> <span data-ttu-id="bb1c4-202">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bb1c4-202">For example:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

<span data-ttu-id="bb1c4-203">Beklenmeyen özel durum, genellikle bir veritabanı sunucusu veritabanı bağlantısı başarısız olduğunda tetiklenen bir özel durum adı gibi hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-203">Unexpected exceptions often contain sensitive information, such as the name of a database server in an exception triggered when the database connection fails.</span></span> <span data-ttu-id="bb1c4-204">SignalR, bir güvenlik önlemi olarak varsayılan olarak bu ayrıntılı hata iletileri ortaya çıkarmıyor.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-204">SignalR doesn't expose these detailed error messages by default as a security measure.</span></span> <span data-ttu-id="bb1c4-205">Bkz: [güvenlik konuları makale](xref:signalr/security#exceptions) neden hakkında daha fazla bilgi için özel durum ayrıntıları görüntülenmez.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-205">See the [Security considerations article](xref:signalr/security#exceptions) for more information on why exception details are suppressed.</span></span>

<span data-ttu-id="bb1c4-206">Varsa olağanüstü bir koşul, *yapmak* istemciye yayılması istiyorsanız, kullanabileceğiniz `HubException` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-206">If you have an exceptional condition you *do* want to propagate to the client, you can use the `HubException` class.</span></span> <span data-ttu-id="bb1c4-207">Durum, bir `HubException` SignalR, hub yönteminden **olacak** tüm ileti üzerinde değişiklik yapılmadan, bir istemciye göndermek.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-207">If you throw a `HubException` from your hub method, SignalR **will** send the entire message to the client, unmodified.</span></span>

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> <span data-ttu-id="bb1c4-208">SignalR yalnızca gönderir `Message` istemciye bir özel durum özelliği.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-208">SignalR only sends the `Message` property of the exception to the client.</span></span> <span data-ttu-id="bb1c4-209">İstemci için yığın izlemesi ve özel durum diğer özellikleri kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="bb1c4-209">The stack trace and other properties on the exception aren't available to the client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="bb1c4-210">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bb1c4-210">Related resources</span></span>

* [<span data-ttu-id="bb1c4-211">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="bb1c4-211">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="bb1c4-212">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="bb1c4-212">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="bb1c4-213">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="bb1c4-213">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
