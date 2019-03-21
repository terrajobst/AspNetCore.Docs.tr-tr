---
title: ASP.NET Core signalr'da hubs'ı kullanma
author: bradygaster
description: İçinde ASP.NET Core SignalR hub'ı kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/20/2018
uid: signalr/hubs
ms.openlocfilehash: 244ddc40e647bfcc3ca8cda2797c51bc49174822
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320153"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>ASP.NET Core signalr'da hubs'ı kullanma

Tarafından [Rachel Appel](https://twitter.com/rachelappel) ve [Kevin Griffin](https://twitter.com/1kevgriff)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(karşıdan yükleme)](xref:index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Bir SignalR hub'ı nedir

SignalR hub'ları API sunucudan bağlı istemciler üzerinde yöntemleri çağırmak sağlar. Sunucu kodunda istemci tarafından çağrılan yöntemlere tanımlayın. İstemci kodda sunucudan çağrılan yöntemlere tanımlayın. SignalR gerçek zamanlı istemci-sunucu ve sunucu istemci iletişimi olanaklı kılan arka planda her şeyin üstlenir.

## <a name="configure-signalr-hubs"></a>SignalR hub'ları yapılandırma

SignalR ara yazılım çağırarak yapılandırılan bazı hizmetler gerektirir `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

ASP.NET Core uygulaması için SignalR işlevselliği ekleme, SignalR yollar çağırarak Kurulum `app.UseSignalR` içinde `Startup.Configure` yöntemi.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Oluşturma ve hub'ları kullanma

Öğesinden devralınan bir sınıf bildirerek oluşturma `Hub`ve genel yöntemleri ekleyin. İstemcileri olarak tanımlanmış olan yöntemleri çağırabilir `public`.

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

Dönüş türü ve parametreleri, tüm C# yönteminde olduğu gibi karmaşık türler ve diziler de dahil olmak üzere belirtebilirsiniz. SignalR serileştirme ve seri durumundan çıkarma karmaşık nesneler ve diziler parametreleri ve dönüş değerleri işler.

> [!NOTE]
> Hub geçici:
>
> * Durum hub sınıfına bir özellik içinde depolamayın. Her bir hub yöntemi çağrısı, yeni bir hub örneği üzerinde yürütülür.  
> * Kullanım `await` Canlı kalma hub'da bağımlı zaman uyumsuz yöntemleri çağrılırken. Örneğin, bir yöntem gibi `Clients.All.SendAsync(...)` olmadan çağrılırsa başarısız olabilir `await` ve önce hub yöntemi tamamlayan `SendAsync` tamamlanır.

## <a name="the-context-object"></a>Bağlam nesnesi

`Hub` Sınıfında bir `Context` bağlantı hakkında bilgi içeren aşağıdaki özellikleri içeren özellik:

| Özellik | Açıklama |
| ------ | ----------- |
| `ConnectionId` | SignalR tarafından atanan bağlantı için benzersiz kimlik alır. Her bağlantı için bir bağlantı kimliği yok.|
| `UserIdentifier` | Alır [kullanıcı tanımlayıcısı](xref:signalr/groups). Varsayılan olarak, SignalR kullanır `ClaimTypes.NameIdentifier` gelen `ClaimsPrincipal` kullanıcı tanımlayıcısı olarak bağlantı ile ilişkili. |
| `User` | Alır `ClaimsPrincipal` geçerli kullanıcı ile ilişkili. |
| `Items` | Bu bağlantının kapsamı içinde veri paylaşmak için kullanılan bir anahtar/değer koleksiyonunu alır. Bu koleksiyonda veri depolanabilir ve farklı bir hub yöntemi çağrılarını arasında bağlantı için korunur. |
| `Features` | Bağlantıda özelliklerin koleksiyonunu alır. Ayrıntılı olarak henüz belgelenmemiştir için şu an için çoğu senaryoda, bu koleksiyon gerek yoktur. |
| `ConnectionAborted` | Alır bir `CancellationToken` bağlantı iptal edildiğinde bildirir. |

`Hub.Context` Ayrıca aşağıdaki yöntemleri içerir:

| Yöntem | Açıklama |
| ------ | ----------- |
| `GetHttpContext` | Döndürür `HttpContext` bir bağlantı veya `null` bağlantı bir HTTP isteği ile ilişkili değilse. HTTP bağlantıları için HTTP üst bilgileri ve sorgu dizeleri gibi bilgileri almak için bu yöntemi kullanabilirsiniz. |
| `Abort` | Bağlantıyı durdurur. |

## <a name="the-clients-object"></a>İstemciler nesnesi

`Hub` Sınıfında bir `Clients` sunucu ve istemci arasındaki iletişim için aşağıdaki özellikleri içeren özelliği:

| Özellik | Açıklama |
| ------ | ----------- |
| `All` | Bağlanan tüm istemciler üzerinde bir yöntemi çağırır |
| `Caller` | Bir hub yöntemini çağırmış istemciye bir metod çağırır |
| `Others` | Yöntemini çağırmış istemciye dışındaki bağlanan tüm istemciler üzerinde bir yöntemi çağırır. |

`Hub.Clients` Ayrıca aşağıdaki yöntemleri içerir:

| Yöntem | Açıklama |
| ------ | ----------- |
| `AllExcept` | Bağlanan tüm istemciler belirtilen bağlantılar dışında bir yöntem çağırır. |
| `Client` | Belirli bir bağlı istemci üzerinde bir yöntemi çağırır |
| `Clients` | Belirli bir bağlı istemciler üzerinde bir yöntemi çağırır |
| `Group` | Belirtilen gruptaki tüm bağlantıları üzerinde bir yöntemi çağırır.  |
| `GroupExcept` | Belirtilen gruptaki belirtilen bağlantılar dışında tüm bağlantıları üzerinde bir yöntemi çağırır. |
| `Groups` | Birden çok bağlantı grupları üzerinde bir yöntemi çağırır  |
| `OthersInGroup` | Bir hub yöntemini çağırmış istemciye hariç bağlantıları, grup üzerinde bir yöntemi çağırır  |
| `User` | Belirli bir kullanıcıyla ilişkili tüm bağlantıları üzerinde bir yöntemi çağırır. |
| `Users` | Belirtilen kullanıcılar ile ilişkili tüm bağlantıları üzerinde bir yöntemi çağırır. |

Her bir özellik veya yöntemin önceki tablolarda sahip bir nesne döndürür. bir `SendAsync` yöntemi. `SendAsync` Yöntemi çağırmak için istemci yönteminin parametreleri ve adını girmesini sağlar.

## <a name="send-messages-to-clients"></a>İstemciler için iletileri gönder

Özel istemciler çağrı yapmak için özelliklerini kullanmak `Clients` nesne. Aşağıdaki örnekte, üç Hub yöntemleri vardır:

* `SendMessage` kullanan tüm bağlı istemciler için bir ileti gönderir `Clients.All`.
* `SendMessageToCaller` kullanarak çağırana geri, bir ileti gönderir `Clients.Caller`.
* `SendMessageToGroups` tüm istemciler için bir ileti gönderir `SignalR Users` grubu.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a>Kesin türü belirtilmiş hub'ları

Kullanmanın bir dezavantajı `SendAsync` olan çağrılacak istemci yöntemi belirtmek için Sihirli bir dize kullanır. Bu yöntem adı yanlış yazılmış, çalışma zamanı hataları için kod açık ya da istemciden eksik bırakır.

Kullanmaya alternatif `SendAsync` kesin yazmaktır `Hub` ile <xref:Microsoft.AspNetCore.SignalR.Hub`1>. Aşağıdaki örnekte, `ChatHub` istemci yöntemleri ayıklandı out adlı bir arabirim `IChatClient`.  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

Bu arabirim, önceki yeniden kullanılabilir `ChatHub` örnek.

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

Kullanarak `Hub<IChatClient>` derleme zamanı istemci yöntemleri denetimini etkinleştirir. Bu Sihirli dize beri kullanımından kaynaklanan sorunları önler `Hub<T>` yalnızca arabirim içinde tanımlanmış yöntemleri erişim sağlayabilir.

Türü kesin belirlenmiş kullanarak `Hub<T>` kullanma yeteneği devre dışı bırakır `SendAsync`. Arabirimde tanımlanan herhangi bir yöntemin zaman uyumsuz olarak yine de tanımlanabilir. Aslında, bu yöntemlerin her biri döndürmelidir bir `Task`. Bir arabirim olduğundan, kullanmayın `async` anahtar sözcüğü. Örneğin:

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> `Async` Soneki olmayan bir yöntem adı kesilmiş. İstemci yönteminizi tanımlı olmadığı sürece `.on('MyMethodAsync')`, kullanmamalısınız `MyMethodAsync` adı.

## <a name="change-the-name-of-a-hub-method"></a>Hub yönteminin adını değiştirin

Varsayılan olarak, bir sunucu hub yönteminin adı, .NET yöntemi adıdır. Ancak, kullanabileceğiniz [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) el ile yöntemi için bir ad belirtin ve bu varsayılanı değiştirmek için özniteliği. İstemci yöntemi çağrılırken .NET yöntemi adı yerine bu adı kullanmalıdır.

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a>Bağlantı olayları işleme

SignalR hub'ları API sağlar `OnConnectedAsync` ve `OnDisconnectedAsync` yönetmek ve bağlantıları izlemek için sanal yöntemler. Geçersiz kılma `OnConnectedAsync` bir istemci bir gruba ekleme gibi Hub'ına bağlandığında eylemleri gerçekleştirmek için sanal bir yöntem.

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

Geçersiz kılma `OnDisconnectedAsync` istemci kestiğinde eylemleri gerçekleştirmek için sanal bir yöntem. İstemcinin kasıtlı olarak kesilirse (çağırarak `connection.stop()`, örneğin), `exception` parametre olacak `null`. Ancak, istemcinin (örneğin, bir ağ hatası), bir hata nedeniyle kesilmiş `exception` parametresi hatayı açıklayan bir özel durum içerir.

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a>Hataları işleme

Özel durumlar, hub yöntemlerinde oluşturulan yöntemini çağırmış istemciye gönderilir. JavaScript istemci `invoke` yöntemi döndürür bir [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). İstemci bir işleyici ile bir hata aldığında bağlı promise kullanmaya `catch`, çağrılan ve bir JavaScript olarak geçirilen `Error` nesne.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

Hub'ınıza bir özel durum oluşturursa, kapatılan bağlantılar değildir. Varsayılan olarak, SignalR istemci için genel bir hata iletisi döndürür. Örneğin:

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

Beklenmeyen özel durum, genellikle bir veritabanı sunucusu veritabanı bağlantısı başarısız olduğunda tetiklenen bir özel durum adı gibi hassas bilgiler içerebilir. SignalR, bir güvenlik önlemi olarak varsayılan olarak bu ayrıntılı hata iletileri ortaya çıkarmıyor. Bkz: [güvenlik konuları makale](xref:signalr/security#exceptions) neden hakkında daha fazla bilgi için özel durum ayrıntıları görüntülenmez.

Varsa olağanüstü bir koşul, *yapmak* istemciye yayılması istiyorsanız, kullanabileceğiniz `HubException` sınıfı. Durum, bir `HubException` SignalR, hub yönteminden **olacak** tüm ileti üzerinde değişiklik yapılmadan, bir istemciye göndermek.

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> SignalR yalnızca gönderir `Message` istemciye bir özel durum özelliği. İstemci için yığın izlemesi ve özel durum diğer özellikleri kullanılamaz.

## <a name="related-resources"></a>İlgili kaynaklar

* [ASP.NET Core signalr'a giriş](xref:signalr/introduction)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
