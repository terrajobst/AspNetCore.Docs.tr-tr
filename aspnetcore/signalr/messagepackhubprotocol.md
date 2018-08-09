---
title: MessagePack Hub protokol SignalR öğesinde ASP.NET Core için kullanın.
author: tdykstra
description: MessagePack Hub protokolü için ASP.NET Core SignalR ekleyin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 78b708c50ce7a8101c9eaa558171540e61c0d7f0
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095001"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>MessagePack Hub protokol SignalR öğesinde ASP.NET Core için kullanın.

Tarafından [Brennan Conroy](https://github.com/BrennanConroy)

Bu makalede, okuyucu, konu başlıkları hakkında bilgi sahibi olduğunu varsayar [Başlarken](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>MessagePack nedir?

[MessagePack](https://msgpack.org/index.html) hızlı ve sıkıştırılmış bir ikili serileştirme biçimidir. İle karşılaştırıldığında daha küçük ileti oluşturduğundan performans ve bant genişliği bir sorun olduğunda kullanışlıdır [JSON](https://www.json.org/). Bir ikili biçimi olduğundan, iletileri bayt MessagePack inceleyicisi üzerinden geçirilir sürece ağ izlemelerini ve günlüklerini ararken okunamaz. SignalR MessagePack biçimi için yerleşik destek içerir ve istemci ve sunucu kullanmak için API'ler sağlar.

## <a name="configure-messagepack-on-the-server"></a>MessagePack yapılandırın

Sunucuda MessagePack Hub Protokolü etkinleştirmek için yükleme `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` uygulamanızı bir pakette. Ekleme Startup.cs dosyasındaki `AddMessagePackProtocol` için `AddSignalR` sunucuda MessagePack desteğini etkinleştirmek için çağrı.

> [!NOTE]
> JSON, varsayılan olarak etkindir. MessagePack ekleme, hem JSON hem de MessagePack istemciler için destek sağlar.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

MessagePack verilerinizi, nasıl biçimlendirecek özelleştirmek için `AddMessagePackProtocol` seçeneklerini yapılandırmak için bir temsilciyi alır. Bu temsilci `FormatterResolvers` özelliği, MessagePack serileştirme seçeneklerini yapılandırmak için kullanılabilir. MessagePack kitaplık Çözümleyicileri nasıl çalıştığı hakkında daha fazla bilgi için ziyaret [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp). Öznitelikleri nasıl ele tanımlamak için seri hale getirmek istediğiniz nesneler üzerinde kullanılabilir.

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

.NET istemci MessagePack etkinleştirmek için yükleme `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paket ve çağrı `AddMessagePackProtocol` üzerinde `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Bu `AddMessagePackProtocol` çağrı sunucu gibi seçenekleri yapılandırmak için bir temsilciyi alır.

### <a name="javascript-client"></a>JavaScript istemcisi

Javascript istemci MessagePack desteği tarafından sağlanır `@aspnet/signalr-protocol-msgpack` NPM paketi.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Npm paket yüklendikten sonra modülü doğrudan bir JavaScript Modülü Yükleyicisi kullanılabilir veya başvurarak tarayıcıya içe *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* dosya. Bir tarayıcıda `msgpack5` kitaplığı da başvurulmalıdır. Kullanım bir `<script>` başvuru oluşturmak için etiket. Kitaplık şu yolda bulunabilir: *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Kullanırken `<script>` öğesi sırası önemlidir. Varsa *signalr protokolünü msgpack.js* önce başvurulan *msgpack5.js*, MessagePack ile bağlanmaya çalışırken bir hata oluşur. *signalr.js* daha önce de gereklidir *signalr protokolünü msgpack.js*.

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
