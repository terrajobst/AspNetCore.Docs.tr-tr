---
title: ASP.NET Core SignalR hub'ları kullanın
author: rachelappel
description: ASP.NET Core SignalR hub'ları kullanmayı öğrenin.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 73397ba6951f2752653575920bdf739439874366
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Hub SignalR öğesinde ASP.NET Core için kullanın.

Tarafından [Rachel Appel](https://twitter.com/rachelappel) ve [Kevin Griffin](https://twitter.com/1kevgriff)

## <a name="what-is-a-signalr-hub"></a>Bir SignalR hub'ı nedir

SignalR hub'ları API sunucudan bağlı istemcilerde yöntemlerini çağırmaya sağlar. Sunucu kodunda istemcisi tarafından çağrıldı yöntemleri tanımlar. İstemci kodu sunucudan adlı yöntemleri tanımlar. SignalR gerçek zamanlı istemci-sunucu ve sunucu istemci iletişimi olanaklı kılan planda her şeyi ilgilenir.

## <a name="configure-signalr-hubs"></a>SignalR hub'ları yapılandırma

SignalR Ara çağırarak yapılandırılmış olan bazı hizmetler gerektirir `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=35)]

SignalR işlevi için bir ASP.NET Core uygulama eklerken, SignalR yollar çağırarak Kurulum `app.UseSignalR` içinde `Startup.Configure` yöntemi.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=55-58)]

## <a name="create-and-use-hubs"></a>Oluşturma ve hub'ları kullanma

Öğesinden devralınan bir sınıf bildirme tarafından bir hub oluşturmak `Hub`ve genel yöntemler ekleyin. İstemcileri olarak tanımlanan yöntemler çağırabilir `public`.

[!code-csharp[Create and use hubs](hubs/sample/chathub.cs?range=10-13)]

Dönüş türü ve tüm C# yönteminde olduğu gibi karmaşık türler ve diziler dahil olmak üzere parametreleri de belirtebilirsiniz. SignalR seri hale getirme ve seri durumdan çıkarma karmaşık nesneler ve diziler parametreler ve dönüş değerlerini işler.

## <a name="the-clients-object"></a>İstemcileri nesnesi

Her örneği `Hub` sınıfı adlı bir özelliğe sahiptir `Clients` , sunucu ve istemci arasındaki iletişim için aşağıdaki üyeleri içerir:

| Özellik | Açıklama |
| ------ | ----------- |
| `All` | Bağlanan tüm istemciler üzerinde bir yöntemi çağırır |
| `Caller` | Hub yöntemini çağırmış istemciye bir yöntemi çağırır |
| `Others` | Yöntemi çağrıldıktan istemci dışındaki bağlanan tüm istemciler üzerinde bir yöntemi çağırır |

Ayrıca, `Hub` sınıf aşağıdaki yöntemleri içerir:

| Yöntem | Açıklama |
| ------ | ----------- |
| `AllExcept` | Bağlanan tüm istemciler belirtilen bağlantıların dışında bir yöntem çağırır |
| `Client` | Belirli bir bağlı istemci üzerinde bir yöntemi çağırır |
| `Clients` | Belirli bağlı istemcilerde bir yöntemi çağırır |
| `Group` | Belirtilen gruptaki tüm bağlantılara bir ileti gönderir  |
| `GroupExcept` | Belirtilen bağlantıların dışında belirtilen gruptaki tüm bağlantılara bir ileti gönderir |
| `Groups` | Bağlantıları birden çok gruba ileti gönderir  |
| `OthersInGroup` | Bir hub yöntemini çağıran istemci hariç bağlantıları, gruba bir ileti gönderir  |
| `User` | Belirli bir kullanıcıyla ilişkili tüm bağlantılara bir ileti gönderir |
| `Users` | Belirtilen kullanıcılar ile ilişkili tüm bağlantılara bir ileti gönderir |

Her bir özellik veya yöntem yukarıdaki tablolarda sahip bir nesne döndürür bir `SendAsync` yöntemi. `SendAsync` Yöntemi çağırmak için istemci yönteminin parametreleri ve adını sağlayın olanak tanır.

## <a name="send-messages-to-clients"></a>İstemcilere iletileri gönder

Belirli istemcileri çağrı yapmak için özelliklerini kullanmak `Clients` nesnesi. Aşağıdaki örnekte, aşağıdaki `SendMessageToCaller` hub yöntemini çağırmış bağlantı için ileti gönderme yöntemini gösterir. `SendMessageToGroups` Yöntemi depolanan gruplarına bir ileti gönderir bir `List` adlı `groups`.

[!code-csharp[Send messages](hubs/sample/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Bağlantı olayları işlemek

SignalR hub'ları API sağlar `OnConnectedAsync` ve `OnDisconnectedAsync` yönetmek ve bağlantıları izlemek için sanal yöntemler. Geçersiz kılma `OnConnectedAsync` istemci bir gruba ekleme gibi Hub'ına bağlandığında eylemleri gerçekleştirmek için sanal bir yöntem.

[!code-csharp[Handle events](hubs/sample/chathub.cs?range=26-30)]

## <a name="handle-errors"></a>Hata işleme

Hub yöntemlerinde oluşturulan özel durumları yöntemini çağırmış istemciye gönderilir. JavaScript istemcide `invoke` yöntemi döndürür bir [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). İstemci bir işleyici ile bir hata olduğunda alır bağlı promise kullanmaya `catch`, çağrılan ve JavaScript geçirilen `Error` nesnesi.

[!code-javascript[Error](hubs/sample/chat.js?range=20)]
[!code-javascript[Error](hubs/sample/chat.js?range=16-18)]

## <a name="related-resources"></a>İlgili kaynaklar

[ASP.NET Core SignalR giriş](xref:signalr/introduction)