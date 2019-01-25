---
title: SignalR HubContext
author: bradygaster
description: Bir hub dışında istemcilere bildirimleri gönderme için ASP.NET Core SignalR HubContext hizmetini kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/01/2018
uid: signalr/hubcontext
ms.openlocfilehash: 73cf2c9d30ed5e409a75827fdab1f22b20427884
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837942"
---
# <a name="send-messages-from-outside-a-hub"></a>Bir hub dışında ileti gönderme

Tarafından [Mikael Mengistu](https://twitter.com/MikaelM_12)

SignalR hub'ı, SignalR sunucuya bağlı istemcilere ileti göndermek için çekirdek soyutlamadır. Diğer yerlerden uygulama kullanarak ileti göndermek mümkündür `IHubContext` hizmeti. Bu makalede, bir SignalR erişmeye açıklanmaktadır `IHubContext` hub dışında istemcilere bildirimleri göndermek için.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(karşıdan yükleme)](xref:index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>IHubContext örneğini al

ASP.NET Core SignalR öğesinde bir örneğini erişebileceğiniz `IHubContext` aracılığıyla bağımlılık ekleme. Örneği ekleyebilir `IHubContext` bir denetleyici, ara yazılım veya diğer DI hizmeti. İstemcilere göndermek için örneği kullanın.

> [!NOTE]
> Bu, ASP.NET tarafından farklıdır 4.x GlobalHost erişim sağlamak için kullanılan SignalR `IHubContext`. ASP.NET Core, bu genel tekil gereksinimini ortadan kaldırır, bir bağımlılık ekleme çerçeve vardır.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Bir denetleyici IHubContext örneğinde ekleme

Örneği ekleyebilir `IHubContext` , oluşturucuya ekleyerek bir denetleyici içinde:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Şimdi, örneğine erişimi olan `IHubContext`, hub içinde değilmiş gibi hub yöntemlerini çağırabilirsiniz.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Ara yazılım IHubContext örneğinde Al

Erişim `IHubContext` ara yazılım ardışık düzenini içinde şu şekilde:

```csharp
app.Use(async (context, next) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Ne zaman hub yöntemleri çağrıldığında gelen dışında `Hub` çağırmayla ilgili hiçbir arayan olduğunda, sınıf. Bu nedenle, erişim yoktur `ConnectionId`, `Caller`, ve `Others` özellikleri.

### <a name="inject-a-strongly-typed-hubcontext"></a>Kesin türü belirtilmiş bir HubContext ekleme

Hub'ınıza devraldığı kesin türü belirtilmiş bir HubContext eklemesine olun `Hub<T>`. Kullanarak ekleme `IHubContext<THub, T>` arabirimi yerine `IHubContext<THub>`.

```csharp
public class ChatController : Controller
{
    public IHubContext<ChatHub, IChatClient> _strongChatHubContext { get; }

    public ChatController(IHubContext<ChatHub, IChatClient> chatHubContext)
    {
        _strongChatHubContext = chatHubContext;
    }

    public async Task SendMessage(string message)
    {
        await _strongChatHubContext.Clients.All.ReceiveMessage(message);
    }
}
```

## <a name="related-resources"></a>İlgili kaynaklar

* [Kullanmaya başlama](xref:tutorials/signalr)
* [Merkezler](xref:signalr/hubs)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
