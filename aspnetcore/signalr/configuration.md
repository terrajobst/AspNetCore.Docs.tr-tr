---
title: ASP.NET Core SignalR yapılandırma
author: tdykstra
description: ASP.NET Core SignalR uygulamalarını yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/configuration
ms.openlocfilehash: fee6e3382c14e818dff408f95770e711603f769d
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44039997"
---
# <a name="aspnet-core-signalr-configuration"></a>ASP.NET Core SignalR yapılandırma

## <a name="jsonmessagepack-serialization-options"></a>JSON/MessagePack serileştirme seçenekleri

ASP.NET Core SignalR iletileri kodlama için iki protokollerini destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html). Her protokolü, serileştirme yapılandırma seçenekleri vardır.

JSON seri hale getirme kullanılarak sunucuda yapılandırılabilir [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) sonra eklenen genişletme yöntemi [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) içinde `Startup.ConfigureServices` yöntemi. `AddJsonProtocol` Aldığında bir temsilci yöntemi alır bir `options` nesne. [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) özelliği bu nesne üzerinde bir JSON.NET `JsonSerializerSettings` bağımsız değişkenleri serileştirilmesi yapılandırmak ve dönüş değerleri için kullanılan nesne. Bkz: [JSON.NET belgeleri](https://www.newtonsoft.com/json/help/html/Introduction.htm) daha fazla ayrıntı için.

Örneğin, "PascalCase" özellik adları, varsayılan "camelCase" adları yerine kullanılacak serileştirici yapılandırmak için aşağıdaki kodu kullanın:

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

.NET istemci, aynı `AddJsonHubProtocol` genişletme yöntemi var. [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). `Microsoft.Extensions.DependencyInjection` Ad alanı içe, genişletme yönteminin çözmek için:

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
> JSON seri hale getirme, şu anda JavaScript istemci yapılandırmak mümkün değildir.

### <a name="messagepack-serialization-options"></a>MessagePack seri hale getirme seçenekleri

MessagePack serileştirme için temsilci sağlayarak yapılandırılabilir [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağırın. Bkz: [signalr'da MessagePack](xref:signalr/messagepackhubprotocol) daha fazla ayrıntı için.

> [!NOTE]
> MessagePack serileştirme JavaScript istemci şu anda yapılandırmak mümkün değildir.

## <a name="configure-server-options"></a>Sunucu seçenekleri yapılandırma

Aşağıdaki tabloda, SignalR hub'ları yapılandırma seçenekleri açıklanmaktadır:

| Seçenek | Varsayılan Değer | Açıklama |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | 15 saniye | İstemci, bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermediği durumlarda bağlantı kapalı. Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır. Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 saniye | Sunucu bu aralıkta bir ileti gönderdi taşınmadığından, PING iletisi bağlantıyı açık tutma için otomatik olarak gönderilir. Değiştirilirken `KeepAliveInterval`, değiştirme `ServerTimeout` / `serverTimeoutInMilliseconds` istemcide ayarlama. Önerilen `ServerTimeout` / `serverTimeoutInMilliseconds` değeri çift `KeepAliveInterval` değeri.  |
| `SupportedProtocols` | Yüklü tüm protokolleri | Bu hub tarafından desteklenen protokoller. Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak belirli protokoller için tek tek hub'ları devre dışı bırakmak için bu listedeki protokolleri kaldırılabilir. |
| `EnableDetailedErrors` | `false` | Varsa `true`, ayrıntılı bir Hub yöntemi bir özel durum oluştuğunda, özel durum iletileri istemcilere döndürülür. Varsayılan `false`gibi bu özel durum iletileri, hassas bilgiler içerebilir. |

Seçenekler yapılandırılabilir tüm hub'ları için bir seçenekler temsilciye sağlayarak `AddSignalR` Çağır `Startup.ConfigureServices`.

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

Tek bir hub seçeneklerini geçersiz kılmak, sağlanan genel seçenekleri `AddSignalR` ve kullanılarak yapılandırılabilir [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

Kullanım `HttpConnectionDispatcherOptions` aktarımları ve bellek arabellek yönetimi ile ilgili gelişmiş ayarları yapılandırmak için. Bu seçenekler bir temsilciye geçirerek yapılandırılan [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).

| Seçenek | Varsayılan Değer | Açıklama |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | İstemciden alınan bayt sayısı, sunucu arabellekleri. Bu değeri artırmak, daha büyük iletileri almasına olanak sağlar, ancak bellek tüketimi olumsuz yönde etkileyebilir. |
| `AuthorizationData` | Verileri otomatik olarak toplanan `Authorize` Hub sınıfına uygulanan öznitelikleri. | Listesini [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) bir istemciye hub'a bağlanmak için yetki verilip verilmediğini belirlemek için kullanılan nesne. |
| `TransportMaxBufferSize` | 32 KB | Uygulama tarafından gönderilen bayt sayısı, sunucu arabellekleri. Bu değeri artırmak daha büyük iletiler göndermek sunucuyı sağlar, ancak bellek tüketimi olumsuz yönde etkileyebilir. |
| `Transports` | Tüm aktarımları etkinleştirilir. | Bir bit maskesi, `HttpTransportType` taşımalar kısıtlayabilirsiniz değerleri bir istemci bağlanmak için kullanabilirsiniz. |
| `LongPolling` | Aşağıya bakın. | Uzun yoklama taşıma imkanı kullanılarak belirli ek seçenekler. |
| `WebSockets` | Aşağıya bakın. | Ek seçenekler belirli WebSockets taşıma imkanı. |

Uzun yoklama taşıma kullanılarak yapılandırılabilir ek seçenekler sahip `LongPolling` özelliği:

| Seçenek | Varsayılan Değer | Açıklama |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 saniye | En uzun süreyi sunucunun tek yoklama isteği sonlandırmadan önce istemciye gönderilecek bir ileti bekler. Bu değeri azaltmak daha sık verme yeni bir yoklama isteği göndermek istemci neden olur. |

WebSocket taşıma kullanılarak yapılandırılabilir ek seçenekler sahip `WebSockets` özelliği:

| Seçenek | Varsayılan Değer | Açıklama |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 saniye | Bu zaman aralığı içinde kapatmak istemci başarısız olursa sunucunun kapatıldıktan sonra bağlantı sonlandırılır. |
| `SubProtocolSelector` | `null` | Ayarlamak için kullanılan bir temsilci `Sec-WebSocket-Protocol` üst bilgisi için özel bir değer. Temsilci, giriş olarak istemci tarafından istenen değerleri alır ve istenen değeri döndürmesi beklenir. |

## <a name="configure-client-options"></a>İstemci seçeneklerini yapılandırın

İstemci seçenekleri yapılandırılabilir `HubConnectionBuilder` türü (kullanılabilir), hem .NET hem de JavaScript istemcileri de itibariyle `HubConnection` kendisi.

### <a name="configure-logging"></a>Günlük tutmayı yapılandırma

Günlük kaydı kullanarak .NET istemci yapılandırılmış `ConfigureLogging` yöntemi. Günlüğe kaydetme sağlayıcıları ve filtreleri sunucuda oldukları gibi aynı şekilde kaydedilebilir. Bkz: [ASP.NET Core günlüğü](xref:fundamentals/logging/index#how-to-add-providers) daha fazla bilgi için belgelere bakın.

> [!NOTE]
> Günlüğe kaydetme sağlayıcıları kaydetmek için gerekli paketleri yüklemeniz gerekir. Bkz: [yerleşik günlük sağlayıcıları](xref:fundamentals/logging/index#built-in-logging-providers) tam listesi için belgelere bölümü.

Örneğin, konsol günlüğe kaydetmeyi etkinleştirmek için yükleme `Microsoft.Extensions.Logging.Console` NuGet paketi. Çağrı `AddConsole` genişletme yöntemi:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

Benzer bir JavaScript istemci `configureLogging` yöntem vardır. Sağlayan bir `LogLevel` günlük iletilerini oluşturmak için en düşük düzeyini belirten değer. Tarayıcı konsol penceresine günlüklerine yazılır.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> Tamamen günlüğünü devre dışı bırakmanız belirtin `signalR.LogLevel.None` içinde `configureLogging` yöntemi.

JavaScript istemci için kullanılabilen günlük düzeyleri aşağıda listelenmiştir. Günlük düzeyi şu değerlerden birini ayarlamak, iletileri günlüğe kaydedilmesini sağlar **veya yukarıdaki** düzeyi.

| Düzey | Açıklama |
| ----- | ----------- |
| `None` | Günlüğe ileti kaydedilmedi. |
| `Critical` | Bir uygulamanın tamamında hata iletileri. |
| `Error` | Geçerli işlem bir hata iletileri. |
| `Warning` | Önemli olmayan bir sorunu işaret eden iletileri. |
| `Information` | Bilgilendirme iletileri. |
| `Debug` | Tanılama iletileri hata ayıklama için yararlıdır. |
| `Trace` | Belirli sorunları tanılamak için tasarlanan çok ayrıntılı tanılama iletileri. |

### <a name="configure-allowed-transports"></a>İzin verilen taşımalar yapılandırın

SignalR tarafından kullanılan taşımalar yapılandırılabilir `WithUrl` çağırın (`withUrl` JavaScript dilinde). Bir bit düzeyinde OR değerlerinin `HttpTransportType` istemciye yalnızca belirtilen taşımalar kullanmasını kısıtlamak için kullanılabilir. Tüm aktarımları varsayılan olarak etkindir.

Örneğin, Server-Sent olayları taşıma devre dışı bırakmak için ancak WebSockets ve uzun yoklama bağlantılarına izin ver:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

JavaScript istemci taşımaları ayarlayarak yapılandırılmış `transport` için sağlanan seçenekleri nesne ile sekmesindeki `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>Taşıyıcı kimlik doğrulaması yapılandırma

SignalR istekleri ile birlikte kimlik doğrulama verilerini sağlamak için kullanmayı `AccessTokenProvider` seçeneği (`accessTokenFactory` javascript'teki) istenen erişim belirteci döndüren bir işlev belirtmek için. .NET istemci, bu erişim belirteci bir HTTP "Taşıyıcı kimlik doğrulaması" geçirilen belirteç (kullanma `Authorization` başlığı türünde `Bearer`). JavaScript istemci erişim belirtecini bir taşıyıcı belirteç kullanılan **dışında** tarayıcı API'leri kısıtlama (özellikle de Server-Sent olayları ve WebSockets istekleri) üst bilgileri uygulama özelliği burada birkaç durumda. Bu durumlarda, erişim belirtecini bir sorgu dizesi değeri sağlanan `access_token`.

.NET istemci `AccessTokenProvider` seçenekleri Temsilcide kullanılarak seçeneği belirtilebilir `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

JavaScript istemcisinde ayarlayarak erişim belirteci yapılandırılmıştır `accessTokenFactory` seçenekleri nesnesinde ile sekmesindeki `withUrl`:

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

Zaman aşımı ve tutma davranışını yapılandırmak için ek seçenekler bulunur `HubConnection` nesnenin kendisini:

| .NET seçeneği | JavaScript seçeneği | Varsayılan Değer | Açıklama |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | 30 saniye (30.000 milisaniye) | Sunucu etkinliği için zaman aşımı. Sunucu bir ileti bu aralık içinde gönderilebilen taşınmadığından, istemci sunucusu bağlantısı kesildi ve Tetikleyicileri dikkate `Closed` olay (`onclose` JavaScript dilinde). Bu değer sunucudan gönderilecek PING iletisi için yeterince büyük **ve** zaman aşımı aralığı içinde istemci tarafından alındı. Önerilen değer: bir sayının en az iki sunucu `KeepAliveInterval` ping gelmesi için zaman tanınması için değer. |
| `HandshakeTimeout` | Yapılandırılamaz | 15 saniye | İlk sunucu el sıkışma için zaman aşımı. Sunucu bu aralığı el sıkışması yanıt göndermediği durumlarda, istemciyi el sıkışması ve tetikleyiciler iptal `Closed` olay (`onclose` JavaScript dilinde). Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır. Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

.NET istemci zaman aşımı değerlerini olarak belirtilen `TimeSpan` değerleri. JavaScript istemci zaman aşımı değerlerini, milisaniye cinsinden süre belirten bir sayı olarak belirtilir.

### <a name="configure-additional-options"></a>Ek seçenekleri yapılandırma

Ek seçenekler yapılandırılabilir `WithUrl` (`withUrl` JavaScript'te) metodunda `HubConnectionBuilder`:

| .NET seçeneği | JavaScript seçeneği | Varsayılan Değer | Açıklama |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | HTTP isteklerinde bir taşıyıcı kimlik doğrulaması belirteci olarak sağlanan bir dize döndüren bir işlev. |
| `SkipNegotiation` | `skipNegotiation` | `false` | Bu ayar `true` anlaşma adımı atlamak için. **WebSockets aktarım yalnızca etkin aktarım olduğunda yalnızca desteklenen**. Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez. |
| `ClientCertificates` | Yapılandırılamaz * | boş | TLS sertifikalarını isteklerinde kimlik doğrulamak üzere göndermek için bir koleksiyonu. |
| `Cookies` | Yapılandırılamaz * | boş | Her HTTP isteği göndermek için HTTP tanımlama bilgileri koleksiyonu. |
| `Credentials` | Yapılandırılamaz * | boş | Her HTTP isteği göndermek için kimlik bilgileri. |
| `CloseTimeout` | Yapılandırılamaz * | 5 saniye | Yalnızca WebSockets. En uzun süreyi sunucusunun Kapat isteği onaylamak Kapanıştan sonra istemci bekler. Bu süre içinde sunucu kapatma bildirimi değil, istemci bağlantısını keser. |
| `Headers` | Yapılandırılamaz * | boş | Her HTTP isteği göndermek için ek HTTP üstbilgileri sözlüğü. |
| `HttpMessageHandlerFactory` | Yapılandırılamaz * | `null` | Yapılandırma veya değiştirmek için kullanılan bir temsilci `HttpMessageHandler` HTTP istekleri göndermek için kullanılır. WebSocket bağlantılarını için kullanılmaz. Bu temsilci, bir null olmayan değer döndürmelidir ve varsayılan değer bir parametre olarak alır. Bu varsayılan ayarları değiştirmek ve onu döndürür ya da yeni bir dönüş `HttpMessageHandler` örneği. **Aksi takdirde işleyici değiştirerek yaptığınızda, sağlanan işleyicisinden tutmak istediğiniz ayarları kopyaladığınızdan emin (örneğin, tanımlama bilgileri ve üst) yapılandırılmış seçenekler için yeni işleyici uygulanmayacak.** |
| `Proxy` | Yapılandırılamaz * | `null` | HTTP istekleri gönderirken HTTP proxy. |
| `UseDefaultCredentials` | Yapılandırılamaz * | `false` | HTTP ve Websocket'istekleri için varsayılan kimlik bilgilerini göndermek için bu boolean ayarlayın. Bu, Windows kimlik doğrulaması sağlar. |
| `WebSocketConfiguration` | Yapılandırılamaz * | `null` | Ek WebSocket seçeneklerini yapılandırmak için kullanılan bir temsilci. Örneğini alır [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) seçeneklerini yapılandırmak için kullanılabilir. |

Bir yıldız işareti (*) ile seçenekleri JavaScript istemci tarayıcısında API sınırlamaları nedeniyle yapılandırılabilir değildir.

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

JavaScript istemci, bu seçenekler için sağlanan bir JavaScript nesnesi içinde sağlanabilir `withUrl`:

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
