---
title: SignalR HubContext
author: rachelappel
description: Bir hub dışında istemcilere bildirimleri göndermek için ASP.NET Core SignalR HubContext hizmeti kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: ccfcdc8337275fd26e09c1a43db36cf9ab90cf46
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277767"
---
# <a name="send-messages-from-outside-a-hub"></a>Bir hub dışında ileti gönderme

Tarafından [Mikael Mengistu](https://twitter.com/MikaelM_12)

SignalR hub'ı SignalR sunucuya bağlı olan istemcilerin ileti göndermek için çekirdek soyutlamadır. Uygulama kullanarak diğer yerlerden iletileri göndermek mümkündür `IHubContext` hizmet. Bu makalede, bir SignalR erişmek açıklanmaktadır `IHubContext` hub dışında istemcilere bildirimleri göndermek için.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(nasıl indirileceğini)](xref:tutorials/index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Örneği Al `IHubContext`

ASP.NET Core SignalR öğesinde örneği erişebilirsiniz `IHubContext` bağımlılık ekleme aracılığıyla. Bir örneğini ekleme `IHubContext` bir denetleyicide, ara yazılım veya diğer dı hizmet uygulamasına. İstemcilere iletileri göndermek için örneği kullanın.

> [!NOTE]
> Bu GlobalHost erişim sağlamak için kullanılan ASP.NET SignalR farklıdır `IHubContext`. ASP.NET Core genel bu singleton gereksinimini ortadan kaldırır bir bağımlılık ekleme framework sahiptir.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Bir örneğini ekleme `IHubContext` bir denetleyicide

Bir örneğini ekleme `IHubContext` , oluşturucuya ekleyerek bir denetleyici içine:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Şimdi, bir örneğine erişimi olan `IHubContext`, hub, değilmiş gibi hub yöntemleri çağırabilirsiniz.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Bir örneğini almak `IHubContext` Ara yazılımında

Erişim `IHubContext` ara yazılım ardışık düzenini içinde şu şekilde:

```csharp
app.Use(next => (context) =>
{
    var hubContext = (IHubContext<MyHub>)context
                        .RequestServices
                        .GetServices<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Ne zaman hub yöntemleri çağrılmadan gelen dışında `Hub` sınıfı, çağırmayla ilgili hiçbir çağıran yoktur. Bu nedenle, hiçbir erişim yoktur `ConnectionId`, `Caller`, ve `Others` özellikleri.

## <a name="related-resources"></a>İlgili kaynaklar

* [Kullanmaya başlama](xref:tutorials/signalr)
* [Merkezler](xref:signalr/hubs)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
