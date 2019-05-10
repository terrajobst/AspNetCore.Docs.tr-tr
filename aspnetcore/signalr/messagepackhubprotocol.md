---
title: MessagePack Hub protokol SignalR öğesinde ASP.NET Core için kullanın.
author: bradygaster
description: MessagePack Hub protokolü için ASP.NET Core SignalR ekleyin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/27/2019
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 7742f6f8bb53fb3c299ff98ae52a0da519ff396c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902594"
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

> [!NOTE]
> JSON desteklenen istemciler için varsayılan olarak etkindir. İstemcileri yalnızca tek bir protokolü destekler. MessagePack destek herhangi daha önce değiştirecek ekleme protokolleri yapılandırılmış.

### <a name="net-client"></a>.NET istemcisi

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

JavaScript istemci MessagePack desteği tarafından sağlanır `@aspnet/signalr-protocol-msgpack` npm paketi.

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

## <a name="messagepack-quirks"></a>MessagePack quirks

MessagePack Hub protokolünü kullanırken dikkat edilmesi gereken birkaç sorun vardır.

### <a name="messagepack-is-case-sensitive"></a>MessagePack büyük/küçük harfe duyarlıdır

MessagePack protokolü, büyük/küçük harf duyarlıdır. Örneğin, aşağıdakileri dikkate alın C# sınıfı:

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

JavaScript istemciden gönderirken kullanmalısınız `PascalCased` özellik adları büyük/küçük harf eşleşmesi gerekir bu yana C# tam olarak sınıf. Örneğin:

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

Kullanarak `camelCased` adlar olmaz düzgün bir şekilde bağlanma C# sınıfı. Bu geçici bir çözüm kullanarak geçici `Key` MessagePack özelliği için farklı bir ad belirtmek için özniteliği. Daha fazla bilgi için [MessagePack-CSharp belgeleri](https://github.com/neuecc/MessagePack-CSharp#object-serialization).

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>Serileştirme/seri durumdan çıkarılırken zaman DateTime.Kind korunmaz

MessagePack Protokolü kodlamak için bir yol sağlamaz `Kind` değerini bir `DateTime`. Sonuç olarak, bir tarih işlenirken MessagePack Hub protokol gelen tarihi UTC biçiminde olduğunu varsayar. İle çalışıyorsanız `DateTime` değerlerini yerel saat öneririz göndermeden önce UTC'ye dönüştürme. Bunları aldığınızda bunları UTC'den yerel saate dönüştürün.

Bu sınırlama hakkında daha fazla bilgi için bkz. GitHub sorunu [aspnet/SignalR #2632](https://github.com/aspnet/SignalR/issues/2632).

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>Küçük JavaScript'te MessagePack tarafından desteklenmiyor

[Msgpack5](https://github.com/mcollina/msgpack5) SignalR JavaScript istemci tarafından kullanılan kitaplığı desteklemiyor `timestamp96` MessagePack yazın. Bu tür, çok büyük tarih değerleri (veya çok erken bir aşamasında geçmiş kadar çok ileride) kodlamak için kullanılır. Değerini `DateTime.MinValue` olduğu `January 1, 0001` hangi gerekir kodlanmış bir `timestamp96` değeri. Bu, gönderme nedeniyle `DateTime.MinValue` JavaScript istemci desteklenmiyor. Zaman `DateTime.MinValue` alınan aşağıdaki hata JavaScript istemci tarafından oluşturulur:

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

Genellikle, `DateTime.MinValue` kodlamak için kullanılan bir "eksik" veya `null` değeri. MessagePack değeri kodlamak gerekiyorsa, boş değer atanabilir bir kullanın `DateTime` değeri (`DateTime?`) veya ayrı bir kodla `bool` tarih mevcut olup olmadığını gösteren değer.

Bu sınırlama hakkında daha fazla bilgi için bkz. GitHub sorunu [aspnet/SignalR #2228](https://github.com/aspnet/SignalR/issues/2228).

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>"Önceden-ın-time" derleme ortamında MessagePack desteği

[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) .NET istemcisi ve sunucusu tarafından kullanılan kitaplığı serileştirme iyileştirmek için kod oluşturma kullanır. Sonuç olarak, varsayılan olarak "üretim-ın-time" derleme (örneğin, Xamarin iOS veya Unity) kullanan ortamlar desteklenmez. "Önceden, seri hale getirici/seri durumdan çıkarıcının kod oluşturarak" Bu ortamlarda MessagePack kullanmak mümkündür. Daha fazla bilgi için [MessagePack-CSharp belgeleri](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports). Seri hale getiricileri genişletme önceden oluşturduktan sonra geçirilen yapılandırma temsilci kullanarak bunları kaydedebilirsiniz `AddMessagePackProtocol`:

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.GeneratedResolver.Instance,
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

### <a name="type-checks-are-more-strict-in-messagepack"></a>MessagePack içinde daha katı tür denetimleri

JSON Hub protokol seri durumundan çıkarma sırasında tür dönüştürmeleri gerçekleştirir. Gelen nesne bir özellik değeri gibi bir sayı ise (`{ foo: 42 }`) ancak özelliği .NET sınıf türünü `string`, değer dönüştürülür. Ancak, MessagePack bu dönüştürme gerçekleştirmez ve sunucu tarafı günlüklerini (ve konsolundaki) görülebilir bir özel durum atar:

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

Bu sınırlama hakkında daha fazla bilgi için bkz. GitHub sorunu [aspnet/SignalR #2937](https://github.com/aspnet/SignalR/issues/2937).

## <a name="related-resources"></a>İlgili kaynaklar

* [Başlarken](xref:tutorials/signalr)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [JavaScript istemcisi](xref:signalr/javascript-client)
