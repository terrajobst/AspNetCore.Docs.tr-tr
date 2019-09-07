---
title: ASP.NET Core SignalR 'de hub 'ları kullanma
author: bradygaster
description: ASP.NET Core SignalR 'de hub 'ları kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/20/2018
uid: signalr/hubs
ms.openlocfilehash: 4922d6d780b18727d3ac181b94dbf75458d74fe6
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746520"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="87198-103">ASP.NET Core için SignalR içindeki hub 'ları kullanma</span><span class="sxs-lookup"><span data-stu-id="87198-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="87198-104">, [Rachel Appel](https://twitter.com/rachelappel) ve [Kevin Griffin](https://twitter.com/1kevgriff) tarafından</span><span class="sxs-lookup"><span data-stu-id="87198-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="87198-105">[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(indirme)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="87198-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="87198-106">SignalR hub nedir?</span><span class="sxs-lookup"><span data-stu-id="87198-106">What is a SignalR hub</span></span>

<span data-ttu-id="87198-107">SignalR hub 'Ları API 'SI, bağlı istemcilerdeki yöntemleri sunucudan çağırmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="87198-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="87198-108">Sunucu kodunda, istemci tarafından çağrılan yöntemleri tanımlarsınız.</span><span class="sxs-lookup"><span data-stu-id="87198-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="87198-109">İstemci kodunda, sunucudan çağrılan yöntemleri tanımlarsınız.</span><span class="sxs-lookup"><span data-stu-id="87198-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="87198-110">SignalR, gerçek zamanlı istemciden sunucuya ve sunucudan istemciye iletişimleri mümkün kılan arka planda her şeyi üstlenir.</span><span class="sxs-lookup"><span data-stu-id="87198-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="87198-111">SignalR hub 'larını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="87198-111">Configure SignalR hubs</span></span>

<span data-ttu-id="87198-112">SignalR ara yazılımı, çağırarak `services.AddSignalR`yapılandırılan bazı hizmetler gerektirir.</span><span class="sxs-lookup"><span data-stu-id="87198-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="87198-113">Bir ASP.NET Core uygulamasına SignalR işlevselliği eklerken, `endpoint.MapHub` `Startup.Configure` metodun `app.UseEndpoints` geri çağrısında arayarak SignalR yollarını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="87198-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `endpoint.MapHub` in the `Startup.Configure` method's `app.UseEndpoints` callback.</span></span>

```csharp
app.UseRouting();
app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/chathub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="87198-114">Bir ASP.NET Core uygulamasına SignalR işlevselliği eklerken, `app.UseSignalR` `Startup.Configure` yöntemini çağırarak SignalR yollarını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="87198-114">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

::: moniker-end

## <a name="create-and-use-hubs"></a><span data-ttu-id="87198-115">Hub 'lar oluşturma ve kullanma</span><span class="sxs-lookup"><span data-stu-id="87198-115">Create and use hubs</span></span>

<span data-ttu-id="87198-116">Öğesinden `Hub`devralan bir sınıf bildirerek bir hub oluşturun ve ona ortak yöntemler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="87198-116">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="87198-117">İstemcileri, olarak `public`tanımlanan yöntemleri çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="87198-117">Clients can call methods that are defined as `public`.</span></span>

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

<span data-ttu-id="87198-118">Herhangi C# bir yöntemde yaptığınız gibi, karmaşık türler ve diziler dahil olmak üzere bir dönüş türü ve parametreleri belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="87198-118">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="87198-119">SignalR parametrelerinizin ve dönüş değerlerindeki karmaşık nesne ve dizilerin serileştirilmesi ve serisini kaldırma işlemi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="87198-119">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="87198-120">Hub 'lar geçicidir:</span><span class="sxs-lookup"><span data-stu-id="87198-120">Hubs are transient:</span></span>
>
> * <span data-ttu-id="87198-121">Durumu hub sınıfının bir özelliğinde depolamayın.</span><span class="sxs-lookup"><span data-stu-id="87198-121">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="87198-122">Her hub yöntemi çağrısı yeni bir hub örneğinde yürütülür.</span><span class="sxs-lookup"><span data-stu-id="87198-122">Every hub method call is executed on a new hub instance.</span></span>
> * <span data-ttu-id="87198-123">Hub `await` 'a bağlı olan zaman uyumsuz yöntemleri çağırırken kullanım etkin kalmakta.</span><span class="sxs-lookup"><span data-stu-id="87198-123">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="87198-124">Örneğin, gibi bir yöntem `Clients.All.SendAsync(...)` , olmadan `await` çağrılırsa ve hub yöntemi bitmeden önce `SendAsync` tamamlandığında başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="87198-124">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="87198-125">Bağlam nesnesi</span><span class="sxs-lookup"><span data-stu-id="87198-125">The Context object</span></span>

<span data-ttu-id="87198-126">Sınıfı, bağlantıyla ilgili `Context` bilgiler içeren aşağıdaki özellikleri içeren bir özelliğe sahiptir: `Hub`</span><span class="sxs-lookup"><span data-stu-id="87198-126">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="87198-127">Özellik</span><span class="sxs-lookup"><span data-stu-id="87198-127">Property</span></span> | <span data-ttu-id="87198-128">Açıklama</span><span class="sxs-lookup"><span data-stu-id="87198-128">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="87198-129">SignalR tarafından atanan bağlantının benzersiz KIMLIĞINI alır.</span><span class="sxs-lookup"><span data-stu-id="87198-129">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="87198-130">Her bağlantı için bir bağlantı KIMLIĞI vardır.</span><span class="sxs-lookup"><span data-stu-id="87198-130">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="87198-131">[Kullanıcı tanımlayıcısını](xref:signalr/groups)alır.</span><span class="sxs-lookup"><span data-stu-id="87198-131">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="87198-132">Varsayılan olarak, SignalR, `ClaimTypes.NameIdentifier` `ClaimsPrincipal` Kullanıcı tanımlayıcısı ile ilişkilendirilen bağlantıyla ilişkili ' ı kullanır.</span><span class="sxs-lookup"><span data-stu-id="87198-132">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="87198-133">Geçerli kullanıcıyla `ClaimsPrincipal` ilişkili öğesini alır.</span><span class="sxs-lookup"><span data-stu-id="87198-133">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="87198-134">Bu bağlantının kapsamındaki verileri paylaşmak için kullanılabilecek bir anahtar/değer koleksiyonu alır.</span><span class="sxs-lookup"><span data-stu-id="87198-134">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="87198-135">Veriler bu koleksiyonda depolanabilir ve farklı hub yöntemi etkinleştirmeleri arasında bağlantı için kalıcı hale gelir.</span><span class="sxs-lookup"><span data-stu-id="87198-135">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="87198-136">Bağlantıda kullanılabilen özelliklerin koleksiyonunu alır.</span><span class="sxs-lookup"><span data-stu-id="87198-136">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="87198-137">Şimdilik bu koleksiyon Çoğu senaryoda gerekli değildir, bu nedenle henüz ayrıntılı olarak açıklanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="87198-137">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="87198-138">Bağlantı iptal `CancellationToken` edildiğinde bir bildirim alır.</span><span class="sxs-lookup"><span data-stu-id="87198-138">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="87198-139">`Hub.Context`Aşağıdaki yöntemleri de içerir:</span><span class="sxs-lookup"><span data-stu-id="87198-139">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="87198-140">Yöntem</span><span class="sxs-lookup"><span data-stu-id="87198-140">Method</span></span> | <span data-ttu-id="87198-141">Açıklama</span><span class="sxs-lookup"><span data-stu-id="87198-141">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="87198-142">Bağlantı için veya `null` bağlantı bir HTTP isteğiyle ilişkilendirilmediği takdirde. `HttpContext`</span><span class="sxs-lookup"><span data-stu-id="87198-142">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="87198-143">HTTP bağlantılarında, HTTP üstbilgileri ve sorgu dizeleri gibi bilgileri almak için bu yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="87198-143">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="87198-144">Bağlantıyı iptal eder.</span><span class="sxs-lookup"><span data-stu-id="87198-144">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="87198-145">Istemciler nesnesi</span><span class="sxs-lookup"><span data-stu-id="87198-145">The Clients object</span></span>

<span data-ttu-id="87198-146">Sınıfı, sunucu ve `Clients` istemci arasındaki iletişim için aşağıdaki özellikleri içeren bir özelliğe sahiptir: `Hub`</span><span class="sxs-lookup"><span data-stu-id="87198-146">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="87198-147">Özellik</span><span class="sxs-lookup"><span data-stu-id="87198-147">Property</span></span> | <span data-ttu-id="87198-148">Açıklama</span><span class="sxs-lookup"><span data-stu-id="87198-148">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="87198-149">Tüm bağlı istemcilerde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="87198-149">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="87198-150">İstemciye, hub yöntemini çağıran bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="87198-150">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="87198-151">Yöntemi çağıran istemci hariç tüm bağlı istemcilerde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="87198-151">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="87198-152">`Hub.Clients`Aşağıdaki yöntemleri de içerir:</span><span class="sxs-lookup"><span data-stu-id="87198-152">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="87198-153">Yöntem</span><span class="sxs-lookup"><span data-stu-id="87198-153">Method</span></span> | <span data-ttu-id="87198-154">Açıklama</span><span class="sxs-lookup"><span data-stu-id="87198-154">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="87198-155">Belirtilen bağlantılar hariç tüm bağlı istemcilerde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="87198-155">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="87198-156">Belirli bir bağlı istemcide bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="87198-156">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="87198-157">Belirli bağlı istemcilerde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="87198-157">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="87198-158">Belirtilen gruptaki tüm bağlantılarda bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="87198-158">Calls a method on all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="87198-159">Belirtilen bağlantılar dışında, belirtilen gruptaki tüm bağlantılarda bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="87198-159">Calls a method on all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="87198-160">Birden çok bağlantı grubunda bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="87198-160">Calls a method on multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="87198-161">Hub yöntemini çağıran istemci hariç bir bağlantı grubu üzerinde bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="87198-161">Calls a method on a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="87198-162">Belirli bir kullanıcıyla ilişkili tüm bağlantılarda bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="87198-162">Calls a method on all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="87198-163">Belirtilen kullanıcılarla ilişkili tüm bağlantılarda bir yöntemi çağırır</span><span class="sxs-lookup"><span data-stu-id="87198-163">Calls a method on all connections associated with the specified users</span></span> |

<span data-ttu-id="87198-164">Yukarıdaki tablolardaki her bir özellik veya yöntem bir `SendAsync` yöntemine sahip bir nesne döndürür.</span><span class="sxs-lookup"><span data-stu-id="87198-164">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="87198-165">`SendAsync` Yöntemi, çağrılacak istemci yönteminin adını ve parametrelerini girmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="87198-165">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="87198-166">İstemcilere ileti gönderme</span><span class="sxs-lookup"><span data-stu-id="87198-166">Send messages to clients</span></span>

<span data-ttu-id="87198-167">Belirli istemcilere çağrı yapmak için `Clients` nesnesinin özelliklerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="87198-167">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="87198-168">Aşağıdaki örnekte, üç hub yöntemi vardır:</span><span class="sxs-lookup"><span data-stu-id="87198-168">In the following example, there are three Hub methods:</span></span>

* <span data-ttu-id="87198-169">`SendMessage`kullanarak `Clients.All`tüm bağlı istemcilere bir ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="87198-169">`SendMessage` sends a message to all connected clients, using `Clients.All`.</span></span>
* <span data-ttu-id="87198-170">`SendMessageToCaller`kullanarak `Clients.Caller`, çağırana geri bir ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="87198-170">`SendMessageToCaller` sends a message back to the caller, using `Clients.Caller`.</span></span>
* <span data-ttu-id="87198-171">`SendMessageToGroups``SignalR Users` gruptaki tüm istemcilere bir ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="87198-171">`SendMessageToGroups` sends a message to all clients in the `SignalR Users` group.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="87198-172">Türü kesin belirlenmiş hub 'lar</span><span class="sxs-lookup"><span data-stu-id="87198-172">Strongly typed hubs</span></span>

<span data-ttu-id="87198-173">Kullanmanın `SendAsync` bir dezavantajı, çağrılacak istemci yöntemini belirtmek için bir sihirli dize kullanır.</span><span class="sxs-lookup"><span data-stu-id="87198-173">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="87198-174">Bu, yöntem adı yanlış yazıldığında veya istemcide eksikse kodu, çalışma zamanı hatalarına açık bırakır.</span><span class="sxs-lookup"><span data-stu-id="87198-174">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="87198-175">Kullanmanın `SendAsync` alternatifi, `Hub` ile <xref:Microsoft.AspNetCore.SignalR.Hub%601>kesin bir şekilde yazmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="87198-175">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub%601>.</span></span> <span data-ttu-id="87198-176">Aşağıdaki örnekte, `ChatHub` istemci yöntemleri adlı `IChatClient`bir arabirimde çıkarılmıştır.</span><span class="sxs-lookup"><span data-stu-id="87198-176">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="87198-177">Bu arabirim, önceki `ChatHub` örneği yeniden düzenleme için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="87198-177">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="87198-178">Kullanımı `Hub<IChatClient>` , istemci yöntemlerinin derleme zamanı denetimini sunar.</span><span class="sxs-lookup"><span data-stu-id="87198-178">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="87198-179">Bu, yalnızca arabirimde tanımlanan yöntemlere erişim `Hub<T>` sağlayabileceğinizden, sihirli dizeler kullanılarak oluşan sorunları önler.</span><span class="sxs-lookup"><span data-stu-id="87198-179">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="87198-180">Türü kesin belirlenmiş `Hub<T>` kullanılması, kullanma `SendAsync`yeteneğini devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="87198-180">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span> <span data-ttu-id="87198-181">Arabirim üzerinde tanımlanan Yöntemler hala zaman uyumsuz olarak tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="87198-181">Any methods defined on the interface can still be defined as asynchronous.</span></span> <span data-ttu-id="87198-182">Aslında, bu yöntemlerin her biri bir `Task`döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="87198-182">In fact, each of these methods should return a `Task`.</span></span> <span data-ttu-id="87198-183">Bir arabirim olduğundan `async` anahtar sözcüğünü kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="87198-183">Since it's an interface, don't use the `async` keyword.</span></span> <span data-ttu-id="87198-184">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="87198-184">For example:</span></span>

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> <span data-ttu-id="87198-185">`Async` Sonek, yöntem adından çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="87198-185">The `Async` suffix isn't stripped from the method name.</span></span> <span data-ttu-id="87198-186">İstemci yönteminiz ile `.on('MyMethodAsync')`tanımlanmadıysa, ad olarak kullanmamalısınız `MyMethodAsync` .</span><span class="sxs-lookup"><span data-stu-id="87198-186">Unless your client method is defined with `.on('MyMethodAsync')`, you shouldn't use `MyMethodAsync` as a name.</span></span>

## <a name="change-the-name-of-a-hub-method"></a><span data-ttu-id="87198-187">Bir hub yönteminin adını değiştirme</span><span class="sxs-lookup"><span data-stu-id="87198-187">Change the name of a hub method</span></span>

<span data-ttu-id="87198-188">Varsayılan olarak, bir sunucu hub 'ı Yöntem adı .NET yönteminin adıdır.</span><span class="sxs-lookup"><span data-stu-id="87198-188">By default, a server hub method name is the name of the .NET method.</span></span> <span data-ttu-id="87198-189">Ancak, bu Varsayılanı değiştirmek ve yöntemi için el ile bir ad belirtmek için [Hubmethodname](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) özniteliğini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="87198-189">However, you can use the [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribute to change this default and manually specify a name for the method.</span></span> <span data-ttu-id="87198-190">İstemci, yöntemi çağırırken .NET Yöntem adı yerine bu adı kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="87198-190">The client should use this name, instead of the .NET method name, when invoking the method.</span></span>

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="87198-191">Bir bağlantı için olayları işleyin</span><span class="sxs-lookup"><span data-stu-id="87198-191">Handle events for a connection</span></span>

<span data-ttu-id="87198-192">SignalR hub 'ları API 'si, `OnConnectedAsync` bağlantıları `OnDisconnectedAsync` yönetmek ve izlemek için ve sanal yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="87198-192">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="87198-193">Bir istemci hub 'a bağlanırken bir gruba ekleme gibi işlemleri gerçekleştirmek için sanalyöntemigeçersizkılın.`OnConnectedAsync`</span><span class="sxs-lookup"><span data-stu-id="87198-193">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

<span data-ttu-id="87198-194">Bir istemcinin bağlantısı kesildiğinde işlemleri gerçekleştirmek için sanalyöntemigeçersizkılın.`OnDisconnectedAsync`</span><span class="sxs-lookup"><span data-stu-id="87198-194">Override the `OnDisconnectedAsync` virtual method to perform actions when a client disconnects.</span></span> <span data-ttu-id="87198-195">İstemci kasıtlı olarak bağlantı kesildiğinde (örneğin, `connection.stop()`çağırarak) `exception` , parametresi `null`olur.</span><span class="sxs-lookup"><span data-stu-id="87198-195">If the client disconnects intentionally (by calling `connection.stop()`, for example), the `exception` parameter will be `null`.</span></span> <span data-ttu-id="87198-196">Ancak, istemcinin bağlantısı bir hata nedeniyle kesildiyse (örneğin, bir ağ arızası), `exception` Bu parametre hatayı açıklayan bir özel durum içerir.</span><span class="sxs-lookup"><span data-stu-id="87198-196">However, if the client is disconnected due to an error (such as a network failure), the `exception` parameter will contain an exception describing the failure.</span></span>

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a><span data-ttu-id="87198-197">Hataları işleme</span><span class="sxs-lookup"><span data-stu-id="87198-197">Handle errors</span></span>

<span data-ttu-id="87198-198">Hub yöntemleriniz içinde oluşturulan özel durumlar, yöntemi çağıran istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="87198-198">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="87198-199">JavaScript istemcisinde, `invoke` yöntemi bir [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)döndürür.</span><span class="sxs-lookup"><span data-stu-id="87198-199">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="87198-200">İstemci, promise `catch`öğesine eklenmiş bir işleyiciyle bir hata aldığında, çağrılır ve bir JavaScript `Error` nesnesi olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="87198-200">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

<span data-ttu-id="87198-201">Hub 'ınız bir özel durum oluşturursa, bağlantılar kapanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="87198-201">If your Hub throws an exception, connections aren't closed.</span></span> <span data-ttu-id="87198-202">Varsayılan olarak, SignalR istemciye genel bir hata iletisi döndürür.</span><span class="sxs-lookup"><span data-stu-id="87198-202">By default, SignalR returns a generic error message to the client.</span></span> <span data-ttu-id="87198-203">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="87198-203">For example:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

<span data-ttu-id="87198-204">Beklenmeyen özel durumlar genellikle veritabanı bağlantısı başarısız olduğunda tetiklenen bir özel durumda veritabanı sunucusunun adı gibi hassas bilgiler içerir.</span><span class="sxs-lookup"><span data-stu-id="87198-204">Unexpected exceptions often contain sensitive information, such as the name of a database server in an exception triggered when the database connection fails.</span></span> <span data-ttu-id="87198-205">SignalR, varsayılan olarak güvenlik önlemi olarak bu ayrıntılı hata iletilerini açığa çıkarır.</span><span class="sxs-lookup"><span data-stu-id="87198-205">SignalR doesn't expose these detailed error messages by default as a security measure.</span></span> <span data-ttu-id="87198-206">Özel durum ayrıntılarının neden bastırıldığına ilişkin daha fazla bilgi için [güvenlik konuları makalesine](xref:signalr/security#exceptions) bakın.</span><span class="sxs-lookup"><span data-stu-id="87198-206">See the [Security considerations article](xref:signalr/security#exceptions) for more information on why exception details are suppressed.</span></span>

<span data-ttu-id="87198-207">İstemciye *yaymak istediğiniz olağanüstü* bir koşulunuz varsa, `HubException` sınıfını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="87198-207">If you have an exceptional condition you *do* want to propagate to the client, you can use the `HubException` class.</span></span> <span data-ttu-id="87198-208">Hub yönteinizden bir `HubException` oluşturursanız, **SignalR tüm** iletiyi istemciye, değiştirilmemiş olarak gönderir.</span><span class="sxs-lookup"><span data-stu-id="87198-208">If you throw a `HubException` from your hub method, SignalR **will** send the entire message to the client, unmodified.</span></span>

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> <span data-ttu-id="87198-209">SignalR yalnızca özel durumun `Message` özelliğini istemciye gönderir.</span><span class="sxs-lookup"><span data-stu-id="87198-209">SignalR only sends the `Message` property of the exception to the client.</span></span> <span data-ttu-id="87198-210">Özel durumun yığın izlemesi ve diğer özellikleri istemci tarafından kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="87198-210">The stack trace and other properties on the exception aren't available to the client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="87198-211">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="87198-211">Related resources</span></span>

* [<span data-ttu-id="87198-212">ASP.NET Core SignalR 'ye giriş</span><span class="sxs-lookup"><span data-stu-id="87198-212">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="87198-213">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="87198-213">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="87198-214">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="87198-214">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
