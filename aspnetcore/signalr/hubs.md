---
title: ASP.NET Core SignalR hub 'ları kullanma
author: bradygaster
description: ASP.NET Core SignalRhub 'larını nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- SignalR
uid: signalr/hubs
ms.openlocfilehash: e5bc12c5ccafe2b5273d72e6bde0f631ca043428
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294634"
---
# <a name="use-hubs-in-opno-locsignalr-for-aspnet-core"></a>ASP.NET Core için SignalR hub 'ları kullanma

, [Rachel Appel](https://twitter.com/rachelappel) ve [Kevin Griffin](https://twitter.com/1kevgriff) tarafından

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(nasıl indirileceği)](xref:index#how-to-download-a-sample)

## <a name="what-is-a-opno-locsignalr-hub"></a>SignalR hub nedir?

SignalR hub 'Ları API 'SI, bağlı istemcilerdeki yöntemleri sunucudan çağırmanızı sağlar. Sunucu kodunda, istemci tarafından çağrılan yöntemleri tanımlarsınız. İstemci kodunda, sunucudan çağrılan yöntemleri tanımlarsınız. SignalR, gerçek zamanlı istemciden sunucuya ve sunucudan istemciye iletişimleri olanaklı kılan arka planda her şeyi ele alır.

## <a name="configure-opno-locsignalr-hubs"></a>SignalR hub 'ları yapılandırma

SignalR ara yazılımı, `services.AddSignalR`çağırarak yapılandırılmış bazı hizmetler gerektirir.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

::: moniker range=">= aspnetcore-3.0"

Bir ASP.NET Core uygulamasına SignalR işlevselliği eklerken, `Startup.Configure` yönteminin `app.UseEndpoints` geri aramasında `endpoint.MapHub` çağırarak SignalR yollar ayarlayın.

```csharp
app.UseRouting();
app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/chathub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Bir ASP.NET Core uygulamasına SignalR işlevselliği eklerken, `Startup.Configure` yöntemindeki `app.UseSignalR` çağırarak SignalR yollar ayarlayın.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

::: moniker-end

## <a name="create-and-use-hubs"></a>Hub 'lar oluşturma ve kullanma

`Hub`devralan bir sınıf bildirerek bir hub oluşturun ve buna ortak yöntemler ekleyin. İstemciler, `public`olarak tanımlanan yöntemleri çağırabilir.

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

Herhangi C# bir yöntemde yaptığınız gibi, karmaşık türler ve diziler dahil olmak üzere bir dönüş türü ve parametreleri belirtebilirsiniz. SignalR parametrelerinizin ve dönüş değerlerindeki karmaşık nesne ve dizilerin serileştirilmesi ve serisini kaldırma işlemi gerçekleştirir.

> [!NOTE]
> Hub 'lar geçicidir:
>
> * Durumu hub sınıfının bir özelliğinde depolamayın. Her hub yöntemi çağrısı yeni bir hub örneğinde yürütülür.
> * Hub 'a bağlı olan zaman uyumsuz yöntemleri çağırırken `await` kullanın. Örneğin, `Clients.All.SendAsync(...)` gibi bir yöntem, `await` olmadan çağrılırsa ve hub yöntemi `SendAsync` bitmeden önce tamamlandığında başarısız olabilir.

## <a name="the-context-object"></a>Bağlam nesnesi

`Hub` sınıfı, bağlantıyla ilgili bilgilerle aşağıdaki özellikleri içeren bir `Context` özelliğine sahiptir:

| Özellik | Açıklama |
| ------ | ----------- |
| `ConnectionId` | SignalRtarafından atanan bağlantının benzersiz KIMLIĞINI alır. Her bağlantı için bir bağlantı KIMLIĞI vardır.|
| `UserIdentifier` | [Kullanıcı tanımlayıcısını](xref:signalr/groups)alır. Varsayılan olarak, SignalR, Kullanıcı tanımlayıcısı olarak bağlantıyla ilişkili `ClaimsPrincipal` `ClaimTypes.NameIdentifier` kullanır. |
| `User` | Geçerli kullanıcıyla ilişkili `ClaimsPrincipal` alır. |
| `Items` | Bu bağlantının kapsamındaki verileri paylaşmak için kullanılabilecek bir anahtar/değer koleksiyonu alır. Veriler bu koleksiyonda depolanabilir ve farklı hub yöntemi etkinleştirmeleri arasında bağlantı için kalıcı hale gelir. |
| `Features` | Bağlantıda kullanılabilen özelliklerin koleksiyonunu alır. Şimdilik bu koleksiyon Çoğu senaryoda gerekli değildir, bu nedenle henüz ayrıntılı olarak açıklanmamıştır. |
| `ConnectionAborted` | Bağlantı iptal edildiğinde bildirim veren bir `CancellationToken` alır. |

`Hub.Context` aşağıdaki yöntemleri de içerir:

| Yöntem | Açıklama |
| ------ | ----------- |
| `GetHttpContext` | Bağlantı için `HttpContext` döndürür veya bağlantı bir HTTP isteğiyle ilişkilendirilmediği durumlarda `null`. HTTP bağlantılarında, HTTP üstbilgileri ve sorgu dizeleri gibi bilgileri almak için bu yöntemi kullanabilirsiniz. |
| `Abort` | Bağlantıyı durdurur. |

## <a name="the-clients-object"></a>Istemciler nesnesi

`Hub` sınıfı, sunucu ve istemci arasındaki iletişim için aşağıdaki özellikleri içeren bir `Clients` özelliğine sahiptir:

| Özellik | Açıklama |
| ------ | ----------- |
| `All` | Tüm bağlı istemcilerde bir yöntemi çağırır |
| `Caller` | İstemciye, hub yöntemini çağıran bir yöntemi çağırır |
| `Others` | Yöntemi çağıran istemci hariç tüm bağlı istemcilerde bir yöntemi çağırır |

`Hub.Clients` aşağıdaki yöntemleri de içerir:

| Yöntem | Açıklama |
| ------ | ----------- |
| `AllExcept` | Belirtilen bağlantılar hariç tüm bağlı istemcilerde bir yöntemi çağırır |
| `Client` | Belirli bir bağlı istemcide bir yöntemi çağırır |
| `Clients` | Belirli bağlı istemcilerde bir yöntemi çağırır |
| `Group` | Belirtilen gruptaki tüm bağlantılarda bir yöntemi çağırır  |
| `GroupExcept` | Belirtilen bağlantılar dışında, belirtilen gruptaki tüm bağlantılarda bir yöntemi çağırır |
| `Groups` | Birden çok bağlantı grubunda bir yöntemi çağırır  |
| `OthersInGroup` | Hub yöntemini çağıran istemci hariç bir bağlantı grubu üzerinde bir yöntemi çağırır  |
| `User` | Belirli bir kullanıcıyla ilişkili tüm bağlantılarda bir yöntemi çağırır |
| `Users` | Belirtilen kullanıcılarla ilişkili tüm bağlantılarda bir yöntemi çağırır |

Yukarıdaki tablolardaki her bir özellik veya yöntem `SendAsync` yöntemi olan bir nesne döndürür. `SendAsync` yöntemi, çağrılacak istemci yönteminin adını ve parametrelerini girmenize olanak sağlar.

## <a name="send-messages-to-clients"></a>İstemcilere ileti gönderme

Belirli istemcilere çağrı yapmak için `Clients` nesnesinin özelliklerini kullanın. Aşağıdaki örnekte, üç hub yöntemi vardır:

* `SendMessage`, `Clients.All`kullanarak tüm bağlı istemcilere bir ileti gönderir.
* `SendMessageToCaller`, `Clients.Caller`kullanarak çağırana geri ileti gönderir.
* `SendMessageToGroups`, `SignalR Users` grubundaki tüm istemcilere bir ileti gönderir.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a>Türü kesin belirlenmiş hub 'lar

`SendAsync` kullanmanın bir dezavantajı, çağrılacak istemci yöntemini belirtmek üzere bir sihirli dize kullanır. Bu, yöntem adı yanlış yazıldığında veya istemcide eksikse kodu, çalışma zamanı hatalarına açık bırakır.

`SendAsync` kullanmanın bir alternatifi <xref:Microsoft.AspNetCore.SignalR.Hub%601>ile `Hub` kesin olarak yazmadır. Aşağıdaki örnekte, `ChatHub` istemci yöntemleri `IChatClient`adlı bir arabirimde ayıklanır.

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

Bu arabirim, önceki `ChatHub` örneğini yeniden düzenleme için kullanılabilir.

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

`Hub<IChatClient>` kullanmak, istemci yöntemlerinin derleme zamanı denetimini mümkün yapar. Bu, `Hub<T>` yalnızca arabirimde tanımlanan yöntemlere erişim sağlayabileceğinizden, sihirli dizeler kullanılarak oluşan sorunları önler.

Türü kesin belirlenmiş `Hub<T>` kullanmak `SendAsync`kullanma özelliğini devre dışı bırakır. Arabirim üzerinde tanımlanan Yöntemler hala zaman uyumsuz olarak tanımlanabilir. Aslında, bu yöntemlerin her biri bir `Task`döndürmelidir. Bir arabirim olduğundan, `async` anahtar sözcüğünü kullanmayın. Örneğin:

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> `Async` soneki yöntem adından çıkarılır. İstemci yönteminiz `.on('MyMethodAsync')`ile tanımlanmamışsa, ad olarak `MyMethodAsync` kullanmamalısınız.

## <a name="change-the-name-of-a-hub-method"></a>Bir hub yönteminin adını değiştirme

Varsayılan olarak, bir sunucu hub 'ı Yöntem adı .NET yönteminin adıdır. Ancak, bu Varsayılanı değiştirmek ve yöntemi için el ile bir ad belirtmek için [Hubmethodname](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) özniteliğini kullanabilirsiniz. İstemci, yöntemi çağırırken .NET Yöntem adı yerine bu adı kullanmalıdır.

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a>Bir bağlantı için olayları işleyin

SignalR hub 'Ları API 'SI, bağlantıları yönetmek ve izlemek için `OnConnectedAsync` ve `OnDisconnectedAsync` sanal yöntemleri sağlar. Bir istemci hub 'a bağlanırken bir gruba ekleme gibi işlemleri gerçekleştirmek için `OnConnectedAsync` sanal yöntemini geçersiz kılın.

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

Bir istemcinin bağlantısı kesildiğinde eylemler gerçekleştirmek için `OnDisconnectedAsync` sanal yöntemini geçersiz kılın. İstemci kasıtlı olarak bağlantı kesildiğinde (örneğin `connection.stop()`çağırarak), `exception` parametresi `null`olur. Ancak, istemcinin bağlantısı bir hata nedeniyle kesildiyse (örneğin, bir ağ hatası), `exception` parametresi hatayı açıklayan bir özel durum içerir.

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

[!INCLUDE[](~/includes/connectionid-signalr.md)]

## <a name="handle-errors"></a>Hataları işleme

Hub yöntemleriniz içinde oluşturulan özel durumlar, yöntemi çağıran istemciye gönderilir. JavaScript istemcisinde `invoke` yöntemi bir [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)döndürür. İstemci, `catch`kullanarak Promise 'e eklenmiş bir işleyiciyle bir hata aldığında, çağrılır ve bir JavaScript `Error` nesnesi olarak geçirilir.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

Hub 'ınız bir özel durum oluşturursa, bağlantılar kapanmamıştır. Varsayılan olarak, SignalR istemciye genel bir hata iletisi döndürür. Örneğin:

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

Beklenmeyen özel durumlar genellikle veritabanı bağlantısı başarısız olduğunda tetiklenen bir özel durumda veritabanı sunucusunun adı gibi hassas bilgiler içerir. SignalR, bu ayrıntılı hata iletilerini varsayılan olarak bir güvenlik ölçüsü olarak kullanıma sunmaz. Özel durum ayrıntılarının neden bastırıldığına ilişkin daha fazla bilgi için [güvenlik konuları makalesine](xref:signalr/security#exceptions) bakın.

İstemciye *yaymak istediğiniz olağanüstü* bir koşulunuz varsa `HubException` sınıfını kullanabilirsiniz. Hub yönteinizden bir `HubException` oluşturursanız **, SignalR iletinin** tamamını, değiştirilmemiş olarak istemciye gönderir.

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> SignalR yalnızca özel durumun `Message` özelliğini istemciye gönderir. Özel durumun yığın izlemesi ve diğer özellikleri istemci tarafından kullanılamaz.

## <a name="related-resources"></a>İlgili kaynaklar

* [ASP.NET Core SignalR giriş](xref:signalr/introduction)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
