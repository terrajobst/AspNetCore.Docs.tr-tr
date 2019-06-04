---
title: ASP.NET Core SignalR yapılandırma
author: bradygaster
description: ASP.NET Core SignalR uygulamalarını yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/03/2019
uid: signalr/configuration
ms.openlocfilehash: 6c7bd602e621917c491bfb1e26ff0fcfc3a565b0
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470361"
---
# <a name="aspnet-core-signalr-configuration"></a>ASP.NET Core SignalR yapılandırma

## <a name="jsonmessagepack-serialization-options"></a>JSON/MessagePack serileştirme seçenekleri

ASP.NET Core SignalR iletileri kodlama için iki protokol destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html). Her protokolü, serileştirme yapılandırma seçenekleri vardır.

JSON seri hale getirme kullanılarak sunucuda yapılandırılabilir [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) sonra eklenen genişletme yöntemi [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) içinde `Startup.ConfigureServices` yöntemi. `AddJsonProtocol` Aldığında bir temsilci yöntemi alır bir `options` nesne. [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) özelliği bu nesne üzerinde bir JSON.NET `JsonSerializerSettings` bağımsız değişkenleri serileştirilmesi yapılandırmak ve dönüş değerleri için kullanılan nesne. Bkz: [JSON.NET belgeleri](https://www.newtonsoft.com/json/help/html/Introduction.htm) daha fazla ayrıntı için.

Örneğin, "PascalCase" özellik adları, varsayılan "camelCase" adları yerine kullanılacak serileştirici yapılandırmak için aşağıdaki kodu kullanın:

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

.NET istemci, aynı `AddJsonProtocol` genişletme yöntemi var. [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). `Microsoft.Extensions.DependencyInjection` Ad alanı içe, genişletme yönteminin çözmek için:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    })
    .Build();
```

> [!NOTE]
> JSON seri hale getirme, şu anda JavaScript istemci yapılandırmak mümkün değildir.

### <a name="messagepack-serialization-options"></a>MessagePack seri hale getirme seçenekleri

MessagePack serileştirme için temsilci sağlayarak yapılandırılabilir [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağırın. Bkz: [signalr'da MessagePack](xref:signalr/messagepackhubprotocol) daha fazla ayrıntı için.

> [!NOTE]
> MessagePack serileştirme JavaScript istemci şu anda yapılandırmak mümkün değildir.

## <a name="configure-server-options"></a>Sunucu seçenekleri yapılandırma

Aşağıdaki tabloda, SignalR hub'ları yapılandırma seçenekleri açıklanmaktadır:

::: moniker range=">= aspnetcore-3.0"

| Seçenek | Varsayılan Değer | Açıklama |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 saniye | Sunucunun istemci dikkate alacaktır (tutma dahil) bir ileti bu aralıkta yer aldığı edilmemiş olursa bağlantısı kesildi. Aslında, nasıl bu uygulandığını nedeniyle, bağlantısız işaretlenmesini istemcisi için bu zaman aşımı aralığından daha uzun sürebilir. Önerilen değer çift `KeepAliveInterval` değeri.|
| `HandshakeTimeout` | 15 saniye | İstemci, bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermediği durumlarda bağlantı kapalı. Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır. Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 saniye | Sunucu bu aralıkta bir ileti gönderdi taşınmadığından, PING iletisi bağlantıyı açık tutma için otomatik olarak gönderilir. Değiştirilirken `KeepAliveInterval`, değiştirme `ServerTimeout` / `serverTimeoutInMilliseconds` istemcide ayarlama. Önerilen `ServerTimeout` / `serverTimeoutInMilliseconds` değeri çift `KeepAliveInterval` değeri.  |
| `SupportedProtocols` | Yüklü tüm protokolleri | Bu hub tarafından desteklenen protokoller. Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak belirli protokoller için tek tek hub'ları devre dışı bırakmak için bu listedeki protokolleri kaldırılabilir. |
| `EnableDetailedErrors` | `false` | Varsa `true`, ayrıntılı bir Hub yöntemi bir özel durum oluştuğunda, özel durum iletileri istemcilere döndürülür. Varsayılan `false`gibi bu özel durum iletileri, hassas bilgiler içerebilir. |
| `StreamBufferCapacity` | `10` | İstemci için arabelleğe alınan öğelerin sayısı akışları karşıya yükleyin. Bu sınıra ulaşıldığında, çağrıları işlenmesini akışı öğeleri sunucu işler kadar engellenir.|

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

| Seçenek | Varsayılan Değer | Açıklama |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 saniye | Sunucunun istemci dikkate alacaktır (tutma dahil) bir ileti bu aralıkta yer aldığı edilmemiş olursa bağlantısı kesildi. Aslında, nasıl bu uygulandığını nedeniyle, bağlantısız işaretlenmesini istemcisi için bu zaman aşımı aralığından daha uzun sürebilir. Önerilen değer çift `KeepAliveInterval` değeri.|
| `HandshakeTimeout` | 15 saniye | İstemci, bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermediği durumlarda bağlantı kapalı. Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır. Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 saniye | Sunucu bu aralıkta bir ileti gönderdi taşınmadığından, PING iletisi bağlantıyı açık tutma için otomatik olarak gönderilir. Değiştirilirken `KeepAliveInterval`, değiştirme `ServerTimeout` / `serverTimeoutInMilliseconds` istemcide ayarlama. Önerilen `ServerTimeout` / `serverTimeoutInMilliseconds` değeri çift `KeepAliveInterval` değeri.  |
| `SupportedProtocols` | Yüklü tüm protokolleri | Bu hub tarafından desteklenen protokoller. Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak belirli protokoller için tek tek hub'ları devre dışı bırakmak için bu listedeki protokolleri kaldırılabilir. |
| `EnableDetailedErrors` | `false` | Varsa `true`, ayrıntılı bir Hub yöntemi bir özel durum oluştuğunda, özel durum iletileri istemcilere döndürülür. Varsayılan `false`gibi bu özel durum iletileri, hassas bilgiler içerebilir. |

::: moniker-end

Seçenekler yapılandırılabilir tüm hub'ları için bir seçenekler temsilciye sağlayarak `AddSignalR` Çağır `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    });
}
```

Tek bir hub seçeneklerini geçersiz kılmak, sağlanan genel seçenekleri `AddSignalR` ve kullanılarak yapılandırılabilir <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a>Gelişmiş HTTP yapılandırma seçenekleri

Kullanım `HttpConnectionDispatcherOptions` aktarımları ve bellek arabellek yönetimi ile ilgili gelişmiş ayarları yapılandırmak için. Bu seçenekler bir temsilciye geçirerek yapılandırılan [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) içinde `Startup.Configure`.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseSignalR((configure) =>
    {
        var desiredTransports =
            HttpTransportType.WebSockets |
            HttpTransportType.LongPolling;

        configure.MapHub<MyHub>("/myhub", (options) =>
        {
            options.Transports = desiredTransports;
        });
    });
}
```

Aşağıdaki tabloda, ASP.NET Core SignalR Gelişmiş HTTP OPTIONS yapılandırmak için seçenekleri tanımlar:

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

İstemci seçenekleri yapılandırılabilir `HubConnectionBuilder` türü (.NET ve JavaScript istemcilerden kullanılabilir). Ayrıca Java istemcisinde kullanılabilir ancak `HttpHubConnectionBuilder` Oluşturucusu yapılandırma seçeneklerini de itibariyle ne içerdiğini sınıfıdır `HubConnection` kendisi.

### <a name="configure-logging"></a>Günlük tutmayı yapılandırma

Günlük kaydı kullanarak .NET istemci yapılandırılmış `ConfigureLogging` yöntemi. Günlüğe kaydetme sağlayıcıları ve filtreleri sunucuda oldukları gibi aynı şekilde kaydedilebilir. Bkz: [ASP.NET Core günlüğü](xref:fundamentals/logging/index) daha fazla bilgi için belgelere bakın.

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

::: moniker range=">= aspnetcore-3.0"

Yerine bir `LogLevel` değerini de sağlayabilirsiniz bir `string` günlük düzeyi adı temsil eden değer. SignalR ortamlarda günlüğü burada yoksa erişiminiz yapılandırırken bu kullanışlıdır `LogLevel` sabitler.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

Aşağıdaki tabloda kullanılabilir günlük düzeyleri listeler. Sunduğunuz değeri `configureLogging` ayarlar **minimum** günlüğe kaydedilir düzeyi. Bu düzeyde günlüğe ileti **veya düzeyleri bundan sonra tabloda listelenen**, günlüğe kaydedilir.

| dize | LogLevel |
| - | - |
| `"trace"` | `LogLevel.Trace` |
| `"debug"` | `LogLevel.Debug` |
| `"info"` **veya** `"information"` | `LogLevel.Information` |
| `"warn"` **veya** `"warning"` | `LogLevel.Warning` |
| `"error"` | `LogLevel.Error` |
| `"critical"` | `LogLevel.Critical` |
| `"none"` | `LogLevel.None` |

::: moniker-end

> [!NOTE]
> Tamamen günlüğünü devre dışı bırakmanız belirtin `signalR.LogLevel.None` içinde `configureLogging` yöntemi.

Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [SignalR tanılama belgeleri](xref:signalr/diagnostics).

SignalR Java istemcinin kullandığı [SLF4J](https://www.slf4j.org/) günlüğe kaydetme için kitaplığı. Seçtiğiniz kendi özel günlük uygulama özel günlük bağımlılık olarak getirerek kullanıcıların Kitaplığı'nın izin veren bir üst düzey günlüğe kaydetme API'si var. Aşağıdaki kod parçacığını nasıl kullanılacağını gösterir `java.util.logging` SignalR Java istemcisi ile.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Bağımlılıklarınızı içinde günlüğü yapılandırmazsanız, aşağıdaki uyarı iletisi ile bir varsayılan yok-işlem günlükçü SLF4J yükler:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Bu güvenle yoksayılabilir.

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

::: moniker range=">= aspnetcore-2.2"

Bu Java sürümünde istemci websockets'i yalnızca Aktarım ' dir.

::: moniker-end

::: moniker range="= aspnetcore-3.0"

Java istemci ile taşıma seçili `withTransport` metodunda `HttpHubConnectionBuilder`. Java istemci WebSockets aktarım varsayılan olarak.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> SignalR Java istemci taşıma geri dönüş henüz desteklemiyor.

::: moniker-end

### <a name="configure-bearer-authentication"></a>Taşıyıcı kimlik doğrulaması yapılandırma

SignalR istekleri ile birlikte kimlik doğrulama verilerini sağlamak için kullanmayı `AccessTokenProvider` seçeneği (`accessTokenFactory` javascript'teki) istenen erişim belirteci döndüren bir işlev belirtmek için. .NET istemci, bu erişim belirteci bir HTTP "Taşıyıcı kimlik doğrulaması" geçirilen belirteç (kullanma `Authorization` başlığı türünde `Bearer`). JavaScript istemci erişim belirtecini bir taşıyıcı belirteç kullanılan **dışında** tarayıcı API'leri kısıtlama (özellikle de Server-Sent olayları ve WebSockets istekleri) üst bilgileri uygulama özelliği burada birkaç durumda. Bu durumlarda, erişim belirtecini bir sorgu dizesi değeri sağlanan `access_token`.

.NET istemci `AccessTokenProvider` seçenekleri Temsilcide kullanılarak seçeneği belirtilebilir `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
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

SignalR Java istemci, bir erişim belirteci fabrikası sağlayarak kimlik doğrulaması için kullanılacak bir taşıyıcı belirteç yapılandırabileceğiniz [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Kullanım [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) sağlamak için bir [RxJava](https://github.com/ReactiveX/RxJava) [tek\<dizesi >](http://reactivex.io/documentation/single.html). Çağrısıyla [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), istemciniz için erişim belirteci üretmek için mantığı yazabilirsiniz.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>Zaman aşımı ve tutma seçeneklerini yapılandırın

Zaman aşımı ve tutma davranışını yapılandırmak için ek seçenekler bulunur `HubConnection` nesnenin kendisini:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 saniye (30.000 milisaniye) | Sunucu etkinliği için zaman aşımı. Sunucu bir ileti bu aralık içinde gönderilebilen taşınmadığından, istemci sunucusu bağlantısı kesildi ve Tetikleyicileri dikkate `Closed` olay (`onclose` JavaScript dilinde). Bu değer sunucudan gönderilecek PING iletisi için yeterince büyük **ve** zaman aşımı aralığı içinde istemci tarafından alındı. Önerilen değer: bir sayının en az iki sunucu `KeepAliveInterval` ping gelmesi için zaman tanınması için değer. |
| `HandshakeTimeout` | 15 saniye | İlk sunucu el sıkışma için zaman aşımı. Sunucu bu aralığı el sıkışması yanıt göndermediği durumlarda, istemciyi el sıkışması ve tetikleyiciler iptal `Closed` olay (`onclose` JavaScript dilinde). Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır. Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

.NET istemci zaman aşımı değerlerini olarak belirtilen `TimeSpan` değerleri.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 saniye (30.000 milisaniye) | Sunucu etkinliği için zaman aşımı. Sunucu bir ileti bu aralık içinde gönderilebilen taşınmadığından, istemci sunucusu bağlantısı kesildi ve Tetikleyicileri dikkate `onclose` olay. Bu değer sunucudan gönderilecek PING iletisi için yeterince büyük **ve** zaman aşımı aralığı içinde istemci tarafından alındı. Önerilen değer: bir sayının en az iki sunucu `KeepAliveInterval` ping gelmesi için zaman tanınması için değer. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Seçenek | Varsayılan değer | Açıklama |
| ----------- | ------------- | ----------- |
|`getServerTimeout``setServerTimeout` | 30 saniye (30.000 milisaniye) | Sunucu etkinliği için zaman aşımı. Sunucu bir ileti bu aralık içinde gönderilebilen taşınmadığından, istemci sunucusu bağlantısı kesildi ve Tetikleyicileri dikkate `onClose` olay. Bu değer sunucudan gönderilecek PING iletisi için yeterince büyük **ve** zaman aşımı aralığı içinde istemci tarafından alındı. Önerilen değer: bir sayının en az iki sunucu `KeepAliveInterval` ping gelmesi için zaman tanınması için değer. |
| `withHandshakeResponseTimeout` | 15 saniye | İlk sunucu el sıkışma için zaman aşımı. Sunucu bu aralığı el sıkışması yanıt göndermediği durumlarda, istemciyi el sıkışması ve tetikleyiciler iptal `onClose` olay. Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır. Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

---

### <a name="configure-additional-options"></a>Ek seçenekleri yapılandırma

Ek seçenekler yapılandırılabilir `WithUrl` (`withUrl` JavaScript'te) metodunda `HubConnectionBuilder` veya çeşitli yapılandırma API'leri üzerinde `HttpHubConnectionBuilder` Java istemci:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| .NET seçeneği |  Varsayılan değer | Açıklama |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | HTTP isteklerinde bir taşıyıcı kimlik doğrulaması belirteci olarak sağlanan bir dize döndüren bir işlev. |
| `SkipNegotiation` | `false` | Bu ayar `true` anlaşma adımı atlamak için. **WebSockets aktarım yalnızca etkin aktarım olduğunda yalnızca desteklenen**. Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez. |
| `ClientCertificates` | boş | TLS sertifikalarını isteklerinde kimlik doğrulamak üzere göndermek için bir koleksiyonu. |
| `Cookies` | boş | Her HTTP isteği göndermek için HTTP tanımlama bilgileri koleksiyonu. |
| `Credentials` | boş | Her HTTP isteği göndermek için kimlik bilgileri. |
| `CloseTimeout` | 5 saniye | Yalnızca WebSockets. En uzun süreyi sunucusunun Kapat isteği onaylamak Kapanıştan sonra istemci bekler. Bu süre içinde sunucu kapatma bildirimi değil, istemci bağlantısını keser. |
| `Headers` | boş | Her HTTP isteği göndermek için ek HTTP üstbilgileri haritası. |
| `HttpMessageHandlerFactory` | `null` | Yapılandırma veya değiştirmek için kullanılan bir temsilci `HttpMessageHandler` HTTP istekleri göndermek için kullanılır. WebSocket bağlantılarını için kullanılmaz. Bu temsilci, bir null olmayan değer döndürmelidir ve varsayılan değer bir parametre olarak alır. Bu varsayılan ayarları değiştirmek ve onu döndürür ya da yeni bir dönüş `HttpMessageHandler` örneği. **Aksi takdirde işleyici değiştirerek yaptığınızda, sağlanan işleyicisinden tutmak istediğiniz ayarları kopyaladığınızdan emin (örneğin, tanımlama bilgileri ve üst) yapılandırılmış seçenekler için yeni işleyici uygulanmayacak.** |
| `Proxy` | `null` | HTTP istekleri gönderirken HTTP proxy. |
| `UseDefaultCredentials` | `false` | HTTP ve Websocket'istekleri için varsayılan kimlik bilgilerini göndermek için bu boolean ayarlayın. Bu, Windows kimlik doğrulaması sağlar. |
| `WebSocketConfiguration` | `null` | Ek WebSocket seçeneklerini yapılandırmak için kullanılan bir temsilci. Örneğini alır [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) seçeneklerini yapılandırmak için kullanılabilir. |

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| JavaScript seçeneği | Varsayılan Değer | Açıklama |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | HTTP isteklerinde bir taşıyıcı kimlik doğrulaması belirteci olarak sağlanan bir dize döndüren bir işlev. |
| `skipNegotiation` | `false` | Bu ayar `true` anlaşma adımı atlamak için. **WebSockets aktarım yalnızca etkin aktarım olduğunda yalnızca desteklenen**. Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Java seçeneği | Varsayılan Değer | Açıklama |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | HTTP isteklerinde bir taşıyıcı kimlik doğrulaması belirteci olarak sağlanan bir dize döndüren bir işlev. |
| `shouldSkipNegotiate` | `false` | Bu ayar `true` anlaşma adımı atlamak için. **WebSockets aktarım yalnızca etkin aktarım olduğunda yalnızca desteklenen**. Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez. |
| `withHeader``withHeaders` | boş | Her HTTP isteği göndermek için ek HTTP üstbilgileri haritası. |

---

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
    })
    .build();
```

Java istemci, bu seçenekler yöntemleriyle yapılandırılabilir `HttpHubConnectionBuilder` döndürüldüğü `HubConnectionBuilder.create("HUB URL")`

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
