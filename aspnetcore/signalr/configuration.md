---
title: ASP.NET SignalR Core yapılandırma
author: rachelappel
description: ASP.NET Core SignalR uygulamalarını yapılandırma öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: 167b8828efd093d755aeb1c91e080dbd22b5d485
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36962464"
---
# <a name="aspnet-core-signalr-configuration"></a>ASP.NET SignalR Core yapılandırma

## <a name="jsonmessagepack-serialization-options"></a>JSON/MessagePack serileştirme seçenekleri

ASP.NET Core SignalR iletileri kodlama için iki protokollerini destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html). Her protokolün serileştirme yapılandırma seçenekleri vardır.

JSON seri hale getirme kullanarak sunucu üzerinde yapılandırılabilir [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) sonra eklenmiş genişletme yöntemi [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) içinde `Startup.ConfigureServices` yöntemi. `AddJsonProtocol` Yöntemi alır alan temsilci bir `options` nesnesi. [ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) Özelliktir bu nesne üzerinde bir JSON.NET `JsonSerializerSettings` bağımsız değişkenleri serileştirmek yapılandırmak ve dönüş değerleri için kullanılan nesne. Bkz: [JSON.NET belgelerine](https://www.newtonsoft.com/json/help/html/Introduction.htm) daha fazla ayrıntı için.

Bir örnek olarak "PascalCase" özellik adları, varsayılan "camelCase" adları yerine kullanılacak serileştirici yapılandırmak için aşağıdaki kodu kullanabilirsiniz:

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

Aynı .NET İstemcisi'nde `AddJsonHubProtocol` genişletme yöntemi var. [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). `Microsoft.Extensions.DependencyInjection` Ad alanı içe, genişletme yöntemi gidermek için:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> JSON seri hale getirme JavaScript istemcisinde şu anda yapılandırmak mümkün değil.

### <a name="messagepack-serialization-options"></a>MessagePack serileştirme seçenekleri

Bir temsilciye sağlayarak MessagePack serileştirme yapılandırılabilir [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağırın. Bkz: [SignalR MessagePack](xref:signalr/messagepackhubprotocol) daha fazla ayrıntı için.

> [!NOTE]
> Şu anda JavaScript istemci MessagePack serileştirme yapılandırmak mümkün değil.

## <a name="configure-server-options"></a>Sunucu seçeneklerini yapılandır

Aşağıdaki tabloda SignalR hub'ları yapılandırma seçenekleri açıklanmaktadır:

| Seçenek | Açıklama |
| ------ | ----------- |
| `HandshakeTimeout` | İstemci bu zaman aralığında bir ilk el sıkışma iletisi göndermez, bağlantı kapalı. |
| `KeepAliveInterval` | Sunucu bu aralıkta bir ileti gönderdi taşınmadığından, ping iletisini bağlantıyı açık tutmak için otomatik olarak gönderilir. |
| `SupportedProtocols` | Bu hub tarafından desteklenen protokolleri. Varsayılan olarak, sunucuda kayıtlı tüm protokollerine izin verilir, ancak tek tek hub'ları için belirli protokolleri devre dışı bırakmak için bu listeden protokolleri kaldırılabilir. |
| `EnableDetailedErrors` | Varsa `true`, ayrıntılı bir Hub yöntemi bir özel durum oluştuğunda özel durum iletilerini istemcilere döndürülür. Varsayılan değer `false`, bu özel durum iletilerini gizli bilgiler içerebilir. |

Seçenekler yapılandırılabilir tüm hub'ları için seçenekleri temsilciye sağlayarak `AddSignalR` Çağır `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

Tek bir hub seçeneklerini geçersiz kıl sağlanan genel seçenekleri `AddSignalR` ve kullanılarak yapılandırılabilir [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

Kullanım `HttpConnectionDispatcherOptions` aktarımları ve bellek arabellek yönetimi ile ilgili gelişmiş ayarları yapılandırmak için. Bu seçenekler için temsilci geçirerek yapılandırılmış [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).

| Seçenek | Açıklama |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | İstemciden alınan bayt sayısını, sunucu arabellekleri. Bu değer artırıldığında sunucusunun büyük iletileri almasına izin verir, ancak bellek tüketimi olumsuz yönde etkileyebilir. Varsayılan değer 32 KB'tır. |
| `AuthorizationData` | Listesini [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) bir istemciye hub'a bağlanmak için yetki verilip verilmediğini belirlemek için kullanılan nesne. Varsayılan olarak, bu değerlerle doldurulur `Authorize` Hub sınıfına uygulanan öznitelikleri. |
| `TransportMaxBufferSize` | Uygulama tarafından gönderilen bayt sayısı, sunucu arabellekleri. Bu değer artırıldığında büyük iletileri göndermesini sağlar, ancak bellek tüketimi olumsuz yönde etkileyebilir. Varsayılan değer 32 KB'tır. |
| `Transports` | Bir bit maskesi olan `HttpTransportType` taşımalar kısıtlayabilirsiniz değerleri bir istemci bağlamak için kullanabilirsiniz. Tüm aktarımları varsayılan olarak etkinleştirilir. |
| `LongPolling` | Uzun yoklama taşıma için özel ek seçenekleri. |
| `WebSockets` | WebSockets taşıma için özel ek seçenekleri. |

Uzun yoklama taşıma kullanılarak yapılandırılabilir ek seçenekler bulunur `LongPolling` özelliği:

| Seçenek | Açıklama |
| ------ | ----------- |
| `PollTimeout` | En uzun süreyi sunucunun tek yoklama isteği sonlandırmadan önce istemciye gönderilecek bir ileti bekler. Bu değeri azaltmak yeni yoklama istekleri daha sık vermek istemci neden olur. Varsayılan değeri 90 saniyedir. |

WebSocket aktarımı kullanılarak yapılandırılabilir ek seçenekler bulunur `WebSockets` özelliği:

| Seçenek | Açıklama |
| ------ | ----------- |
| `CloseTimeout` | Bu zaman aralığında kapatmak istemci başarısız olursa sunucunun kapandıktan sonra bağlantı sonlandırılır. |
| `SubProtocolSelector` | Ayarlamak için kullanılan bir temsilci `Sec-WebSocket-Protocol` başlığına özel bir değer. Temsilci giriş olarak istemci tarafından istenen değerleri alır ve istenen değeri döndürmesi beklenir. |

## <a name="configure-client-options"></a>İstemci seçeneklerini yapılandırma

İstemci seçenekleri yapılandırılabilir `HubConnectionBuilder` türü (kullanılabilir), hem .NET hem de JavaScript istemciler üzerinde de olarak `HubConnection` kendisi.

### <a name="configure-logging"></a>Günlük tutmayı yapılandırma

Günlük kaydı kullanarak .NET istemcisine yapılandırılmış `ConfigureLogging` yöntemi. Sunucuda oldukları gibi aynı şekilde sağlayıcıları ve filtreleri günlüğü kaydedilebilir. Bkz: [ASP.NET çekirdek günlüğü](xref:fundamentals/logging/index#how-to-add-providers) daha fazla bilgi için.

> [!NOTE]
> Günlüğe kaydetme sağlayıcıları kaydetmek için gerekli paketleri yüklemeniz gerekir. Bkz: [yerleşik günlük sağlayıcıları](xref:fundamentals/logging/index#built-in-logging-providers) tam listesi için belgeleri bölümü.

Örneğin, konsol günlük kaydını etkinleştirmek için yükleyin `Microsoft.Extensions.Logging.Console` NuGet paketi. Çağrı `AddConsole` genişletme yöntemi:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

Benzer bir JavaScript istemci `configureLogging` yöntem vardır. Sağlayan bir `LogLevel` günlük iletilerini oluşturmak için en düşük düzeyde belirten değer. Günlükleri tarayıcı konsol penceresine yazılır.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> Tamamen günlüğü devre dışı bırakmak için belirtmek `signalR.LogLevel.None` içinde `configureLogging` yöntemi.

Günlük düzeyleri JavaScript istemci için kullanılabilen aşağıda listelenmiştir. Günlük düzeyini şu değerlerden birine ayarlamak, iletilere göz günlüğe kaydedilmesini sağlar **veya yukarıdaki** o düzeyi.

| Düzey | Açıklama |
| ----- | ----------- |
| `None` | İleti günlüğe kaydedilir. |
| `Critical` | Tüm uygulama hatası olduğunu gösteren iletileri. |
| `Error` | Geçerli işlemde hatası olduğunu gösteren iletileri. |
| `Warning` | Önemli olmayan bir sorunu işaret eder iletileri. |
| `Information` | Bilgilendirme iletileri. |
| `Debug` | Hata ayıklama için yararlı tanılama iletileri. |
| `Trace` | Çok ayrıntılı tanılama iletileri belirli sorunları tanılamak için tasarlanmıştır. |

### <a name="configure-allowed-transports"></a>İzin verilen taşımaları yapılandırın

SignalR tarafından kullanılan taşımalar yapılandırılabilir `WithUrl` çağrı (`withUrl` JavaScript'te). Bir bit düzeyinde-OR değerlerinin `HttpTransportType` istemciye yalnızca belirtilen taşımalar kullanmasını kısıtlamak için kullanılabilir. Tüm aktarımları varsayılan olarak etkinleştirilir.

Örneğin, Server-Sent olayları aktarım devre dışı bırakmak için ancak WebSockets ve uzun yoklama bağlantılarına izin ver:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

JavaScript istemci taşımaları ayarlayarak yapılandırılmış `transport` için sağlanan seçenekleri nesne üzerinde alan `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>Taşıyıcı kimlik doğrulaması yapılandırma

Kimlik doğrulama verilerinin SignalR istekleri yanı sıra sağlamak için kullanın `AccessTokenProvider` seçeneği (`accessTokenFactory` JavaScript'te) istenen erişim belirteci döndüren bir işlev belirtmek için. .NET istemcisi, bu erişim belirteci bir HTTP "Taşıyıcı kimlik doğrulaması" geçirilen belirteç (kullanma `Authorization` türü üstbilgiyle `Bearer`). JavaScript istemci erişim belirteci taşıyıcı belirteci olarak kullanılan **dışında** birkaç durumlarda burada tarayıcı API'leri kısıtlamak üstbilgilerinde (özellikle Server-Sent olayları ve WebSockets istekleri) uygulama özelliği. Bu durumlarda, erişim belirteci bir sorgu dizesi değeri olarak sağlanan `access_token`.

.NET istemci `AccessTokenProvider` seçeneği seçenekleri temsilci kullanılarak belirtilebilir `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

JavaScript istemci erişim belirteci ayarlayarak yapılandırılmış `accessTokenFactory` alanında seçenekleri nesnesinde bulunan `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>Zaman aşımı ve tutma seçeneklerini yapılandırın

Zaman aşımı ve tutma davranışını yapılandırmak için ek seçenekleri bulunur `HubConnection` nesnenin kendisini:

| .NET seçeneği | JavaScript seçeneği | Açıklama |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | Sunucu etkinliğini için zaman aşımı. Sunucusu herhangi bir iletisi bu aralığa kurmadı gönderirse, istemci sunucu bağlantısı kesilmiş ve tetikleyici göz önünde bulundurur `Closed` olayı (`onclose` JavaScript'te). |
| `HandshakeTimeout` | Yapılandırılamaz | İlk sunucu el sıkışma için zaman aşımı. Sunucu bu aralığa anlaşma yanıtı göndermez, istemci el sıkışma ve tetikleyici iptal `Closed` olayı (`onclose` JavaScript'te). |

Olarak belirtilen .NET istemcisi, zaman aşımı değerleri `TimeSpan` değerleri. JavaScript istemci zaman aşımı değerlerini sayı olarak belirtilir. Sayıları süre değerleri milisaniye cinsinden temsil eder.

### <a name="configure-additional-options"></a>Ek seçenekler yapılandırma

Ek seçenekler yapılandırılabilir `WithUrl` (`withUrl` JavaScript'te) yöntemini `HubConnectionBuilder`:

| .NET seçeneği | JavaScript seçeneği | Açıklama |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | HTTP isteklerinde taşıyıcı kimlik doğrulaması belirteci olarak sağlanan bir dize döndüren bir işlev. |
| `SkipNegotiation` | `skipNegotiation` | Bu ayar `true` anlaşma adımı atlamak için. **WebSockets aktarım yalnızca etkin aktarım olduğunda yalnızca desteklenen**. Bu ayar, Azure SignalR hizmetini kullanırken etkinleştirilemez. |
| `ClientCertificates` | Yapılandırılamaz * | Kimlik doğrulaması istekleri göndermek için TLS sertifikalar koleksiyonu. |
| `Cookies` | Yapılandırılamaz * | İçeren tüm HTTP istekleri göndermek için HTTP tanımlama bilgileri koleksiyonu. |
| `Credentials` | Yapılandırılamaz * | Kimlik bilgilerini içeren tüm HTTP istekleri göndermek için. |
| `CloseTimeout` | Yapılandırılamaz * | Yalnızca WebSockets. En uzun süreyi sunucusunun Kapat isteği onaylamak kapanış sonra istemcinin bekler. Sunucusu close bu süre içinde kabul değil, istemci bağlantısını keser. |
| `Headers` | Yapılandırılamaz * | Daha fazla HTTP üstbilgileri içeren tüm HTTP istekleri göndermek için sözlüğü. |
| `HttpMessageHandlerFactory` | Yapılandırılamaz * | Yapılandırma veya değiştirmek için kullanılan bir temsilci `HttpMessageHandler` HTTP istekleri göndermek için kullanılır. WebSocket bağlantıları için kullanılmaz. Bu temsilci null olmayan bir değer döndürmesi gerekir ve varsayılan değer bir parametre olarak alır. Bu varsayılan değeri ayarlarını değiştirin ve onu döndürür ya da tamamen yeni bir dönüş `HttpMessageHandler` örneği. |
| `Proxy` | Yapılandırılamaz * | HTTP istekleri gönderirken kullanmak için bir HTTP proxy. |
| `UseDefaultCredentials` | Yapılandırılamaz * | HTTP ve WebSockets istekleri için varsayılan kimlik bilgilerini göndermek için bu boolean ayarlayın. Bu, Windows kimlik doğrulaması kullanımını etkinleştirir. |
| `WebSocketConfiguration` | Yapılandırılamaz * | Ek WebSocket seçeneklerini yapılandırmak için kullanılan bir temsilci. Örneğini alır [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) seçeneklerini yapılandırmak için kullanılabilir. |

Yıldız işareti (*) seçenekleri API'leri tarayıcıda sınırlamaları nedeniyle JavaScript istemci yapılandırılabilir değildir.

.NET istemci, bu seçenekler için sağlanan seçenekleri temsilci tarafından değiştirilebilir `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

JavaScript istemci, bu seçenekler için sağlanan bir JavaScript nesnesinde sağlanabilir `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
