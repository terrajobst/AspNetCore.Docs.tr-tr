---
title: MessagePack Hub Protokolü SignalR öğesinde ASP.NET çekirdek için kullanın.
author: rachelappel
description: MessagePack Hub Protokolü ASP.NET Core SignalR ekleyin.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 702c77502868d6666cb2634b6959f029e036d14e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274995"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>MessagePack Hub Protokolü SignalR öğesinde ASP.NET çekirdek için kullanın.

Tarafından [Brennan Conroy](https://github.com/BrennanConroy)

Bu makalede okuyucu ele konuları hakkında bilgi sahibi olduğunu varsayar [Get Started](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>MessagePack nedir?

[MessagePack](https://msgpack.org/index.html) hızlı ve sıkıştırılmış bir ikili seri hale getirme biçimi. Karşılaştırılan küçük ileti oluşturduğundan performans ve bant genişliği ilgili bir sorun olduğunda yararlıdır [JSON](https://www.json.org/). Bir ikili biçimi olduğundan, iletileri bayt MessagePack inceleyicisi üzerinden geçirilir sürece ağ izlerini ve günlükleri bakarken okunamaz. SignalR MessagePack biçimi için yerleşik desteğe sahiptir ve istemci ve sunucu kullanmak API'ler sağlar.

## <a name="configure-messagepack-on-the-server"></a>Sunucuda MessagePack yapılandırın

Sunucuda MessagePack Hub Protokolü etkinleştirmek için yükleme `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` uygulamanızda paket. Haline dosyasında ekleyin `AddMessagePackProtocol` için `AddSignalR` çağrısı sunucusunda MessagePack desteğini etkinleştirin.

> [!NOTE]
> JSON, varsayılan olarak etkindir. MessagePack ekleme hem JSON hem de MessagePack istemciler için destek sağlar.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

MessagePack, verilerinizi nasıl biçimlendirecek özelleştirmek için `AddMessagePackProtocol` seçeneklerini yapılandırmak için bir temsilciyi alır. Bu temsilci olarak `FormatterResolvers` özelliği, MessagePack serileştirme seçeneklerini yapılandırmak için kullanılabilir. MessagePack kitaplık çözümleyiciler nasıl çalıştığı hakkında daha fazla bilgi için ziyaret [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp). Öznitelikleri nasıl işleneceğini tanımlamak için seri hale getirmek istediğiniz nesneleri üzerinde kullanılabilir.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="configure-messagepack-on-the-client"></a>İstemcide MessagePack yapılandırın

### <a name="net-client"></a>.NET istemci

.NET istemci MessagePack etkinleştirmek için yükleme `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paket ve çağrısı `AddMessagePackProtocol` üzerinde `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Bu `AddMessagePackProtocol` çağrısı sunucu gibi seçeneklerini yapılandırmak için bir temsilciyi alır.

### <a name="javascript-client"></a>JavaScript istemci

Javascript istemci MessagePack desteği tarafından sağlanan `@aspnet/signalr-protocol-msgpack` NPM paket.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Npm paket yükledikten sonra modülün bulunabilir doğrudan bir JavaScript Modülü Yükleyicisi kullanılan veya başvurarak tarayıcısına içe *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  dosya. Bir tarayıcıda `msgpack5` kitaplığı gerekir da başvurulabilir. Kullanım bir `<script>` başvuru oluşturmak için etiketi. Kitaplık şu adreste bulunabilir: *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Kullanırken `<script>` öğesi sırası önemlidir. Varsa *signalr Protokolü msgpack.js* önce başvurulan *msgpack5.js*, MessagePack ile bağlanmaya çalışırken bir hata oluşur. *signalr.js* önce de gerekli *signalr Protokolü msgpack.js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

Ekleme `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` için `HubConnectionBuilder` MessagePack protokolü bir sunucuya bağlanırken kullanmak üzere istemciyi yapılandırır.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> Şu anda hiçbir JavaScript istemci MessagePack protokolü için yapılandırma seçenekleri vardır.

## <a name="related-resources"></a>İlgili kaynaklar

* [Başlarken](xref:tutorials/signalr)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [JavaScript istemcisi](xref:signalr/javascript-client)
