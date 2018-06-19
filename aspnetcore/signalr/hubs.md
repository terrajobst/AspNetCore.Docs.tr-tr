---
title: ASP.NET Core SignalR hub'ları kullanın
author: rachelappel
description: ASP.NET Core SignalR hub'ları kullanmayı öğrenin.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 495aa156dd5e4641d688d7b16df1e5814c9607f4
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819090"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Hub SignalR öğesinde ASP.NET Core için kullanın.

Tarafından [Rachel Appel](https://twitter.com/rachelappel) ve [Kevin Griffin](https://twitter.com/1kevgriff)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(nasıl indirileceğini)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Bir SignalR hub'ı nedir

SignalR hub'ları API sunucudan bağlı istemcilerde yöntemlerini çağırmaya sağlar. Sunucu kodunda istemcisi tarafından çağrıldı yöntemleri tanımlar. İstemci kodu sunucudan adlı yöntemleri tanımlar. SignalR gerçek zamanlı istemci-sunucu ve sunucu istemci iletişimi olanaklı kılan planda her şeyi ilgilenir.

## <a name="configure-signalr-hubs"></a>SignalR hub'ları yapılandırma

SignalR Ara çağırarak yapılandırılmış olan bazı hizmetler gerektirir `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

SignalR işlevi için bir ASP.NET Core uygulama eklerken, SignalR yollar çağırarak Kurulum `app.UseSignalR` içinde `Startup.Configure` yöntemi.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Oluşturma ve hub'ları kullanma

Öğesinden devralınan bir sınıf bildirme tarafından bir hub oluşturmak `Hub`ve genel yöntemler ekleyin. İstemcileri olarak tanımlanan yöntemler çağırabilir `public`.

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

Dönüş türü ve tüm C# yönteminde olduğu gibi karmaşık türler ve diziler dahil olmak üzere parametreleri de belirtebilirsiniz. SignalR seri hale getirme ve seri durumdan çıkarma karmaşık nesneler ve diziler parametreler ve dönüş değerlerini işler.

## <a name="the-clients-object"></a>İstemcileri nesnesi

Her örneği `Hub` sınıfı adlı bir özelliğe sahiptir `Clients` , sunucu ve istemci arasındaki iletişim için aşağıdaki üyeleri içerir:

| Özellik | Açıklama |
| ------ | ----------- |
| `All` | Bağlanan tüm istemciler üzerinde bir yöntemi çağırır |
| `Caller` | Hub yöntemini çağırmış istemciye bir yöntemi çağırır |
| `Others` | Yöntemi çağrıldıktan istemci dışındaki bağlanan tüm istemciler üzerinde bir yöntemi çağırır |


Ayrıca, `Hub.Clients` aşağıdaki yöntemleri içerir:

| Yöntem | Açıklama |
| ------ | ----------- |
| `AllExcept` | Bağlanan tüm istemciler belirtilen bağlantıların dışında bir yöntem çağırır |
| `Client` | Belirli bir bağlı istemci üzerinde bir yöntemi çağırır |
| `Clients` | Belirli bağlı istemcilerde bir yöntemi çağırır |
| `Group` | Belirtilen grubun tüm bağlantılar için bir yöntemi çağırır  |
| `GroupExcept` | Belirtilen bağlantıların dışında belirtilen gruptaki tüm bağlantılar için bir yöntemi çağırır |
| `Groups` | Birden çok grup bağlantıları için bir yöntemi çağırır  |
| `OthersInGroup` | Hub yöntemini çağıran istemci hariç bağlantıları, bir grup için bir yöntem çağırır  |
| `User` | Belirli bir kullanıcıyla ilişkili tüm bağlantılar için bir yöntemi çağırır |
| `Users` | Belirtilen kullanıcılar ile ilişkilendirilmiş tüm bağlantılar için bir yöntem çağırır |

Her bir özellik veya yöntem yukarıdaki tablolarda sahip bir nesne döndürür bir `SendAsync` yöntemi. `SendAsync` Yöntemi çağırmak için istemci yönteminin parametreleri ve adını sağlayın olanak tanır.

## <a name="send-messages-to-clients"></a>İstemcilere iletileri gönder

Belirli istemcileri çağrı yapmak için özelliklerini kullanmak `Clients` nesnesi. Aşağıdaki örnekte, `SendMessageToCaller` hub yöntemini çağırmış bağlantı için ileti gönderme yöntemini gösterir. `SendMessageToGroups` Yöntemi depolanan gruplarına bir ileti gönderir bir `List` adlı `groups`.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Bağlantı olayları işlemek

SignalR hub'ları API sağlar `OnConnectedAsync` ve `OnDisconnectedAsync` yönetmek ve bağlantıları izlemek için sanal yöntemler. Geçersiz kılma `OnConnectedAsync` istemci bir gruba ekleme gibi Hub'ına bağlandığında eylemleri gerçekleştirmek için sanal bir yöntem.

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>Hata işleme

Hub yöntemlerinde oluşturulan özel durumları yöntemini çağırmış istemciye gönderilir. JavaScript istemcide `invoke` yöntemi döndürür bir [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). İstemci bir işleyici ile bir hata olduğunda alır bağlı promise kullanmaya `catch`, çağrılan ve JavaScript geçirilen `Error` nesnesi.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>İlgili kaynaklar

* [ASP.NET Core SignalR giriş](xref:signalr/introduction)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
