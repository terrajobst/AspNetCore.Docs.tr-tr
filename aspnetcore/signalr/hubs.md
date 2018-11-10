---
title: ASP.NET Core signalr'da hubs'ı kullanma
author: tdykstra
description: İçinde ASP.NET Core SignalR hub'ı kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/12/2018
uid: signalr/hubs
ms.openlocfilehash: 27aedc5b2f2060d961070fbd1ff5304eaa3956d1
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225362"
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

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

Dönüş türü ve parametreleri, tüm C# yönteminde olduğu gibi karmaşık türler ve diziler de dahil olmak üzere belirtebilirsiniz. SignalR serileştirme ve seri durumundan çıkarma karmaşık nesneler ve diziler parametreleri ve dönüş değerleri işler.

> [!NOTE]
> Hub geçici:
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

## <a name="strongly-typed-hubs"></a>Kesin türü belirtilmiş hub'ları

Kullanmanın bir dezavantajı `SendAsync` olan çağrılacak istemci yöntemi belirtmek için Sihirli bir dize kullanır. Bu yöntem adı yanlış yazılmış, çalışma zamanı hataları için kod açık ya da istemciden eksik bırakır.

Kullanmaya alternatif `SendAsync` kesin yazmaktır `Hub` ile <xref:Microsoft.AspNetCore.SignalR.Hub`1>. Aşağıdaki örnekte, `ChatHub` istemci yöntemleri ayıklandı out adlı bir arabirim `IChatClient`.  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

Bu arabirim, önceki yeniden kullanılabilir `ChatHub` örnek.

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

Kullanarak `Hub<IChatClient>` derleme zamanı istemci yöntemleri denetimini etkinleştirir. Bu Sihirli dize beri kullanımından kaynaklanan sorunları önler `Hub<T>` yalnızca arabirim içinde tanımlanmış yöntemleri erişim sağlayabilir.

Türü kesin belirlenmiş kullanarak `Hub<T>` kullanma yeteneği devre dışı bırakır `SendAsync`.

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
