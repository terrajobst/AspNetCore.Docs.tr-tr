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
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>ASP.NET Core signalr'da hubs'ı kullanma

Tarafından [Rachel Appel](https://twitter.com/rachelappel) ve [Kevin Griffin](https://twitter.com/1kevgriff)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(karşıdan yükleme)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Bir SignalR hub'ı nedir

SignalR hub'ları API sunucudan bağlı istemciler üzerinde yöntemleri çağırmak sağlar. Sunucu kodunda istemci tarafından çağrılan yöntemlere tanımlayın. İstemci kodda sunucudan çağrılan yöntemlere tanımlayın. SignalR gerçek zamanlı istemci-sunucu ve sunucu istemci iletişimi olanaklı kılan arka planda her şeyin üstlenir.

## <a name="configure-signalr-hubs"></a>SignalR hub'ları yapılandırma

SignalR ara yazılım çağırarak yapılandırılan bazı hizmetler gerektirir `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

ASP.NET Core uygulaması için SignalR işlevselliği ekleme, SignalR yollar çağırarak Kurulum `app.UseSignalR` içinde `Startup.Configure` yöntemi.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Oluşturma ve hub'ları kullanma

Öğesinden devralınan bir sınıf bildirerek oluşturma `Hub`ve genel yöntemleri ekleyin. İstemcileri olarak tanımlanmış olan yöntemleri çağırabilir `public`.

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

Dönüş türü ve parametreleri, tüm C# yönteminde olduğu gibi karmaşık türler ve diziler de dahil olmak üzere belirtebilirsiniz. SignalR serileştirme ve seri durumundan çıkarma karmaşık nesneler ve diziler parametreleri ve dönüş değerleri işler.

## <a name="the-clients-object"></a>İstemciler nesnesi

Her bir örneği `Hub` sınıfında adlı bir özellik `Clients` , sunucu ve istemci arasındaki iletişim için aşağıdaki üyeleri içerir:

| Özellik | Açıklama |
| ------ | ----------- |
| `All` | Bağlanan tüm istemciler üzerinde bir yöntemi çağırır |
| `Caller` | Bir hub yöntemini çağırmış istemciye bir metod çağırır |
| `Others` | Yöntemini çağırmış istemciye dışındaki bağlanan tüm istemciler üzerinde bir yöntemi çağırır. |


Ayrıca, `Hub.Clients` aşağıdaki yöntemleri içerir:

| Yöntem | Açıklama |
| ------ | ----------- |
| `AllExcept` | Bağlanan tüm istemciler belirtilen bağlantılar dışında bir yöntem çağırır. |
| `Client` | Belirli bir bağlı istemci üzerinde bir yöntemi çağırır |
| `Clients` | Belirli bir bağlı istemciler üzerinde bir yöntemi çağırır |
| `Group` | Belirtilen grubun tüm bağlantılar için bir yöntem çağırır  |
| `GroupExcept` | Belirtilen gruptaki belirtilen bağlantılar dışında tüm bağlantılar için bir yöntem çağırır. |
| `Groups` | Birden çok gruba bağlantılarının bir metod çağırır  |
| `OthersInGroup` | Bir hub yöntemini çağırmış istemciye hariç bağlantıları, grup için bir yöntem çağırır  |
| `User` | Belirli bir kullanıcıyla ilişkili tüm bağlantıları için bir yöntem çağırır |
| `Users` | Belirtilen kullanıcılar ile ilişkili tüm bağlantıları için bir yöntem çağırır |

Her bir özellik veya yöntemin önceki tablolarda sahip bir nesne döndürür. bir `SendAsync` yöntemi. `SendAsync` Yöntemi çağırmak için istemci yönteminin parametreleri ve adını girmesini sağlar.

## <a name="send-messages-to-clients"></a>İstemciler için iletileri gönder

Özel istemciler çağrı yapmak için özelliklerini kullanmak `Clients` nesne. Aşağıdaki örnekte, `SendMessageToCaller` yöntemi gösterir hub yöntemini çağırmış bağlantıya ileti gönderme. `SendMessageToGroups` Yöntemi depolanan gruplara bir ileti gönderen bir `List` adlı `groups`.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Bağlantı olayları işleme

SignalR hub'ları API sağlar `OnConnectedAsync` ve `OnDisconnectedAsync` yönetmek ve bağlantıları izlemek için sanal yöntemler. Geçersiz kılma `OnConnectedAsync` bir istemci bir gruba ekleme gibi Hub'ına bağlandığında eylemleri gerçekleştirmek için sanal bir yöntem.

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>Hatalarını işleme

Özel durumlar, hub yöntemlerinde oluşturulan yöntemini çağırmış istemciye gönderilir. JavaScript istemci `invoke` yöntemi döndürür bir [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). İstemci bir işleyici ile bir hata aldığında bağlı promise kullanmaya `catch`, çağrılan ve bir JavaScript olarak geçirilen `Error` nesne.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>İlgili kaynaklar

* [ASP.NET Core signalr'a giriş](xref:signalr/introduction)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
