---
title: ASP.NET Core için SignalR içinde MessagePack hub protokolünü kullanın
author: bradygaster
description: SignalR ASP.NET Core MessagePack hub protokolünü ekleyin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 10/08/2019
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: fe09b646eba5ae15cbd9e568b276aaf7763e4b1b
ms.sourcegitcommit: fcdf9aaa6c45c1a926bd870ed8f893bdb4935152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72165400"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>ASP.NET Core için SignalR içinde MessagePack hub protokolünü kullanın

[Brennan Conroy](https://github.com/BrennanConroy) tarafından

Bu makalede, okuyucunun [Başlarken](xref:tutorials/signalr)bölümünde ele alınan konular hakkında bilgi sahibi olduğu varsayılır.

## <a name="what-is-messagepack"></a>MessagePack nedir?

[MessagePack](https://msgpack.org/index.html) hızlı ve kompakt bir ikili serileştirme biçimidir. Bu, [JSON](https://www.json.org/)ile karşılaştırıldığında daha küçük iletiler oluşturduğundan performans ve bant genişliği açısından yararlıdır. İkili bir biçim olduğundan, baytlar bir MessagePack ayrıştırıcısı üzerinden geçirilmediği takdirde ağ izlemeleri ve günlükleri aranırken iletiler okunamaz. SignalR, MessagePack biçimi için yerleşik desteğe sahiptir ve istemci ve sunucunun kullanması için API 'Ler sağlar.

## <a name="configure-messagepack-on-the-server"></a>Sunucuda MessagePack 'i yapılandırma

Sunucuda MessagePack hub protokolünü etkinleştirmek için `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paketini uygulamanıza takın. Startup.cs dosyasında, sunucuda MessagePack desteğini etkinleştirmek için `AddMessagePackProtocol` `AddSignalR` çağrısına ekleyin.

> [!NOTE]
> JSON varsayılan olarak etkindir. MessagePack eklemek, hem JSON hem de MessagePack istemcileri için destek sunar.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

MessagePack 'in verilerinizi nasıl biçimlendiğini özelleştirmek için `AddMessagePackProtocol`, seçenekleri yapılandırmak için bir temsilci alır. Bu temsilcisinde, `FormatterResolvers` özelliği MessagePack serileştirme seçeneklerini yapılandırmak için kullanılabilir. Çözümleyiciler nasıl çalıştığı hakkında daha fazla bilgi için, [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)konumundaki MessagePack kitaplığını ziyaret edin. Öznitelikler, serileştirilmeleri nasıl ele alınacağını tanımlamak için seri hale getirmek istediğiniz nesnelerde kullanılabilir.

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

## <a name="configure-messagepack-on-the-client"></a>İstemcide MessagePack 'i yapılandırma

> [!NOTE]
> JSON, desteklenen istemciler için varsayılan olarak etkindir. İstemciler yalnızca tek bir protokolü destekleyebilir. MessagePack desteği eklemek, önceden yapılandırılmış tüm protokollerin yerini alır.

### <a name="net-client"></a>.NET istemcisi

.NET Istemcisinde MessagePack 'i etkinleştirmek için `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paketini yükleyip `HubConnectionBuilder` üzerinde `AddMessagePackProtocol` ' i çağırın.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Bu `AddMessagePackProtocol` çağrısı, sunucu gibi seçenekleri yapılandırmak için bir temsilci alır.

### <a name="javascript-client"></a>JavaScript istemcisi

::: moniker range=">= aspnetcore-3.0"

JavaScript istemcisi için MessagePack desteği `@microsoft/signalr-protocol-msgpack` NPM paketi tarafından sağlanır. Aşağıdaki komutu bir komut kabuğu 'nda yürüterek paketi yüklemelisiniz:

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

JavaScript istemcisi için MessagePack desteği `@aspnet/signalr-protocol-msgpack` NPM paketi tarafından sağlanır. Aşağıdaki komutu bir komut kabuğu 'nda yürüterek paketi yüklemelisiniz:

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

NPM paketini yükledikten sonra modül, doğrudan bir JavaScript Modül Yükleyicisi aracılığıyla veya aşağıdaki dosyaya başvurarak tarayıcıya içeri aktarılabilecek şekilde kullanılabilir:

::: moniker range=">= aspnetcore-3.0"

*node_modules @ no__t-1 @ no__t-2* 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*node_modules @ no__t-1 @ no__t-2* 

::: moniker-end

Bir tarayıcıda `msgpack5` kitaplığına da başvurulmalıdır. Bir başvuru oluşturmak için `<script>` etiketi kullanın. Kitaplığı *node_modules\msgpack5\dist\msgpack5.js*adresinde bulabilirsiniz.

> [!NOTE]
> @No__t-0 öğesi kullanılırken, sıra önemlidir. *SignalR-Protocol-msgpack. js* ' *msgpack5. js*' den önce başvuruluyorsa, MessagePack ile bağlantı kurmaya çalışırken bir hata oluşur. *SignalR. js* , *SignalR-Protocol-msgpack. js*' den önce de gereklidir.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

@No__t-0 ' a `HubConnectionBuilder` ' i eklemek, istemciyi bir sunucuya bağlanırken MessagePack protokolünü kullanmak üzere yapılandırır.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> Şu anda JavaScript istemcisinde MessagePack protokolü için yapılandırma seçeneği yoktur.

## <a name="messagepack-quirks"></a>MessagePack süslemeler

MessagePack hub protokolünü kullanırken dikkat etmeniz birkaç sorun vardır.

### <a name="messagepack-is-case-sensitive"></a>MessagePack büyük/küçük harfe duyarlıdır

MessagePack Protokolü büyük/küçük harfe duyarlıdır. Örneğin, aşağıdaki C# sınıfı göz önünde bulundurun:

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

JavaScript istemcisinden gönderirken, büyük/küçük harf C# sınıfıyla tam olarak eşleşmesi gerektiğinden `PascalCased` özellik adlarını kullanmanız gerekir. Örneğin:

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

@No__t-0 adları kullanmak C# sınıfa düzgün şekilde bağlanmaz. MessagePack özelliği için farklı bir ad belirtmek üzere `Key` özniteliğini kullanarak bu soruna geçici bir çözüm bulabilirsiniz. Daha fazla bilgi için bkz. [MessagePack-CSharp belgeleri](https://github.com/neuecc/MessagePack-CSharp#object-serialization).

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>Seri hale getirme/seri durumdan çıkarma sırasında DateTime. Kind korunmaz

MessagePack protokolü, bir `DateTime` `Kind` değerini kodlamak için bir yol sağlamaz. Sonuç olarak, bir tarih serisi kaldırılırken, MessagePack hub protokolü gelen tarihin UTC biçiminde olduğunu varsayar. Yerel saat içinde `DateTime` değerleriyle çalışıyorsanız, göndermeden önce UTC 'ye dönüştürmeniz önerilir. Bunları aldığınızda UTC 'den yerel saate dönüştürün.

Bu sınırlama hakkında daha fazla bilgi için bkz. GitHub sorunu [ASPNET/SignalR # 2632](https://github.com/aspnet/SignalR/issues/2632).

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>DateTime. MinValue, JavaScript 'te MessagePack tarafından desteklenmiyor

SignalR JavaScript istemcisi tarafından kullanılan [msgpack5](https://github.com/mcollina/msgpack5) kitaplığı, MessagePack içinde `timestamp96` türünü desteklemez. Bu tür, çok büyük tarih değerlerini kodlamak için kullanılır (daha önce geçmişte veya daha önce bir süre içinde çok erken). @No__t-0 değeri, `timestamp96` değerinde kodlanması gereken `January 1, 0001` ' dir. Bu nedenle, bir JavaScript istemcisine `DateTime.MinValue` gönderilmesi desteklenmez. JavaScript istemcisi tarafından `DateTime.MinValue` alındığında, aşağıdaki hata oluşur:

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

Genellikle, `DateTime.MinValue` bir "eksik" veya `null` değeri kodlamak için kullanılır. Bu değeri MessagePack 'te kodlamak gerekirse, null yapılabilir `DateTime` değeri (`DateTime?`) kullanın veya tarihin mevcut olup olmadığını belirten ayrı bir `bool` değeri kodlayın.

Bu sınırlama hakkında daha fazla bilgi için bkz. GitHub sorunu [ASPNET/SignalR # 2228](https://github.com/aspnet/SignalR/issues/2228).

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>"Zamanında" derleme ortamında MessagePack desteği

.NET istemcisi ve sunucusu tarafından kullanılan [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) kitaplığı, serileştirme işlemini iyileştirmek için kod oluşturmayı kullanır. Sonuç olarak, "güncel olmayan" derleme (Xamarin iOS veya Unity gibi) kullanan ortamlarda varsayılan olarak desteklenmez. Seri hale getirici/seri hale getirici kodunu "önceden oluşturma" yoluyla bu ortamlarda MessagePack kullanmak mümkündür. Daha fazla bilgi için bkz. [MessagePack-CSharp belgeleri](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports). Serileştiriciler önceden oluşturulduktan sonra, `AddMessagePackProtocol` ' a geçirilen yapılandırma temsilcisini kullanarak kaydedebilirsiniz:

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

### <a name="type-checks-are-more-strict-in-messagepack"></a>Tür denetimleri MessagePack 'te daha sıkı

JSON hub 'ı protokolü, seri durumdan çıkarma sırasında tür dönüştürmeleri gerçekleştirecek. Örneğin, gelen nesnenin bir sayı olan (`{ foo: 42 }`) bir özellik değeri varsa, ancak .NET sınıfındaki özelliği `string` türünde ise, değer dönüştürülür. Ancak, MessagePack bu dönüştürmeyi gerçekleştirmez ve sunucu tarafı günlüklerinde (ve konsolunda) görünebileceğini bir özel durum oluşturur:

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

Bu sınırlama hakkında daha fazla bilgi için bkz. GitHub sorunu [ASPNET/SignalR # 2937](https://github.com/aspnet/SignalR/issues/2937).

## <a name="related-resources"></a>İlgili kaynaklar

* [Başlarken](xref:tutorials/signalr)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [JavaScript istemcisi](xref:signalr/javascript-client)
