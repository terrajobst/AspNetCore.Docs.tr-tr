---
title: ASP.NET Core SignalR yapılandırması
author: bradygaster
description: ASP.NET Core SignalR uygulamalarını yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 66f274fcda27392091de6b4be8c7221bc87b7585
ms.sourcegitcommit: c452e6af92e130413106c4863193f377cde4cd9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72246487"
---
# <a name="aspnet-core-signalr-configuration"></a>ASP.NET Core SignalR yapılandırması

## <a name="jsonmessagepack-serialization-options"></a>JSON/MessagePack serileştirme seçenekleri

ASP.NET Core SignalR, kodlama iletileri için iki protokolü destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html). Her protokol serileştirme yapılandırma seçenekleri vardır.

::: moniker range=">= aspnetcore-3.0"

JSON serileştirme, [Addjsonprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) genişletme yöntemi kullanılarak sunucuda yapılandırılabilir. `AddJsonProtocol`, `Startup.ConfigureServices` ' de [Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) öğesinden sonra eklenebilir. @No__t-0 yöntemi, bir `options` nesnesi alan bir temsilci alır. Bu nesnedeki [Payloadserializeroptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) özelliği, bağımsız değişkenlerin ve dönüş değerlerinin serileştirilmesi yapılandırmak için kullanılabilen bir `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> nesnesidir. Daha fazla bilgi için bkz. [System. Text. JSON belgeleri](/dotnet/api/system.text.json).

Örnek olarak, serileştiriciyi varsayılan "camelCase" adları yerine özellik adlarının büyük küçük harflerini değiştirmemelidir şekilde yapılandırmak için `Startup.ConfigureServices` ' da aşağıdaki kodu kullanın:

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

.NET istemcisinde aynı `AddJsonProtocol` genişletme yöntemi [Hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)üzerinde bulunur. Uzantı metodunu çözümlemek için `Microsoft.Extensions.DependencyInjection` ad alanı içeri aktarılmalıdır:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    })
    .Build();
```

### <a name="switch-to-newtonsoftjson"></a>Newtonsoft. JSON öğesine geç

@No__t-1 ' de desteklenmeyen `Newtonsoft.Json` özelliklerine ihtiyacınız varsa, bkz. [Newtonsoft. JSON öğesine geçme](xref:migration/22-to-30#switch-to-newtonsoftjson).

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

JSON serileştirme, `Startup.ConfigureServices` yönteminde [Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) öğesinden sonra eklenebilecek [addjsonprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) genişletme yöntemi kullanılarak sunucuda yapılandırılabilir. @No__t-0 yöntemi, bir `options` nesnesi alan bir temsilci alır. Bu nesnedeki [Payloadserializersettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) özelliği, bağımsız değişkenlerin serileştirmesini ve dönüş değerlerini yapılandırmak için kullanılabilen bir JSON.net `JsonSerializerSettings` nesnesidir. Daha fazla bilgi için [JSON.net belgelerine](https://www.newtonsoft.com/json/help/html/Introduction.htm)bakın.
 
Örnek olarak, serileştiriciyi varsayılan "camelCase" adları yerine "PascalCase" özellik adlarını kullanacak şekilde yapılandırmak için `Startup.ConfigureServices` ' da aşağıdaki kodu kullanın:
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

.NET istemcisinde aynı `AddJsonProtocol` genişletme yöntemi [Hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)üzerinde bulunur. Uzantı metodunu çözümlemek için `Microsoft.Extensions.DependencyInjection` ad alanı içeri aktarılmalıdır:

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

::: moniker-end

> [!NOTE]
> JavaScript istemcisinde Şu anda JSON serileştirme yapılandırmak mümkün değildir.

### <a name="messagepack-serialization-options"></a>MessagePack serileştirme seçenekleri

MessagePack serileştirme, [Addmessagepackprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağrısına bir temsilci sağlanarak yapılandırılabilir. Daha fazla ayrıntı için bkz. [SignalR 'de MessagePack](xref:signalr/messagepackhubprotocol) .

> [!NOTE]
> Şu anda JavaScript istemcisinde MessagePack serileştirme yapılandırmak mümkün değildir.

## <a name="configure-server-options"></a>Sunucu seçeneklerini yapılandırma

Aşağıdaki tabloda, SignalR hub 'larını yapılandırma seçenekleri açıklanmaktadır:

::: moniker range=">= aspnetcore-3.0"

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 saniye | Sunucu, bu aralıkta (canlı tut dahil) bir ileti almamışsa istemcinin bağlantısının kesileceğini kabul eder. Bu işlem, uygulanması nedeniyle istemcinin bağlantısının gerçekten kesilmesinin ardından bu zaman aşımı aralığından daha uzun sürebilir. Önerilen değer `KeepAliveInterval` değerinin çift değeridir.|
| `HandshakeTimeout` | 15 saniye | İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır. Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır. El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 saniye | Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir. @No__t-0 ' ı değiştirirken istemcide `ServerTimeout` @ no__t-2 @ no__t-3 ayarını değiştirin. Önerilen `ServerTimeout` @ no__t-1 @ no__t-2 değeri `KeepAliveInterval` değeri çift.  |
| `SupportedProtocols` | Tüm yüklü protokoller | Bu hub tarafından desteklenen protokoller. Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir. |
| `EnableDetailedErrors` | `false` | @No__t-0 ise, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür. Varsayılan değer `false` ' dır, bu durum iletileri hassas bilgiler içerebilir. |
| `StreamBufferCapacity` | `10` | İstemci yükleme akışları için ara belleğe oluşturulabilecek en fazla öğe sayısı. Bu sınıra ulaşıldığında, sunucu akış öğelerini işlemeden, etkinleştirmeleri işleme engellenir.|
| `MaximumReceiveMessageSize` | 32 KB | Tek bir gelen hub iletisinin en büyük boyutu. |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 saniye | Sunucu, bu aralıkta (canlı tut dahil) bir ileti almamışsa istemcinin bağlantısının kesileceğini kabul eder. Bu işlem, uygulanması nedeniyle istemcinin bağlantısının gerçekten kesilmesinin ardından bu zaman aşımı aralığından daha uzun sürebilir. Önerilen değer `KeepAliveInterval` değerinin çift değeridir.|
| `HandshakeTimeout` | 15 saniye | İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır. Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır. El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 saniye | Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir. @No__t-0 ' ı değiştirirken istemcide `ServerTimeout` @ no__t-2 @ no__t-3 ayarını değiştirin. Önerilen `ServerTimeout` @ no__t-1 @ no__t-2 değeri `KeepAliveInterval` değeri çift.  |
| `SupportedProtocols` | Tüm yüklü protokoller | Bu hub tarafından desteklenen protokoller. Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir. |
| `EnableDetailedErrors` | `false` | @No__t-0 ise, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür. Varsayılan değer `false` ' dır, bu durum iletileri hassas bilgiler içerebilir. |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | 15 saniye | İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır. Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır. El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 saniye | Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir. @No__t-0 ' ı değiştirirken istemcide `ServerTimeout` @ no__t-2 @ no__t-3 ayarını değiştirin. Önerilen `ServerTimeout` @ no__t-1 @ no__t-2 değeri `KeepAliveInterval` değeri çift.  |
| `SupportedProtocols` | Tüm yüklü protokoller | Bu hub tarafından desteklenen protokoller. Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir. |
| `EnableDetailedErrors` | `false` | @No__t-0 ise, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür. Varsayılan değer `false` ' dır, bu durum iletileri hassas bilgiler içerebilir. |

::: moniker-end

Seçenekler, `Startup.ConfigureServices` ' deki `AddSignalR` çağrısına bir seçenek temsilcisi sağlayarak tüm Hub 'lar için yapılandırılabilir.

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

Tek bir hub için seçenekler `AddSignalR` ' da belirtilen genel seçenekleri geçersiz kılar ve <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*> kullanılarak yapılandırılabilir:

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a>Gelişmiş HTTP yapılandırma seçenekleri

::: moniker range=">= aspnetcore-3.0"

Aktarımlara ve bellek arabelleği yönetimine ilişkin gelişmiş ayarları yapılandırmak için `HttpConnectionDispatcherOptions` kullanın. Bu seçenekler, `Startup.Configure` ' de [Maphub @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) bir temsilci geçirilerek yapılandırılır.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<MyHub>("/myhub", options =>
        {
            options.Transports =
                HttpTransportType.WebSockets |
                HttpTransportType.LongPolling;
        });
    });
}
```
::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Aktarımlara ve bellek arabelleği yönetimine ilişkin gelişmiş ayarları yapılandırmak için `HttpConnectionDispatcherOptions` kullanın. Bu seçenekler, `Startup.Configure` ' de [Maphub @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) bir temsilci geçirilerek yapılandırılır.

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

::: moniker-end

Aşağıdaki tabloda ASP.NET Core SignalR 'nin gelişmiş HTTP seçeneklerini yapılandırma seçenekleri açıklanmaktadır:

::: moniker range=">= aspnetcore-3.0"

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | İstemci tarafından, geri basıncı uygulamadan önce sunucunun arabelleğe aldığı en fazla bayt sayısı. Bu değeri artırmak, sunucunun geri basınç uygulamadan daha büyük iletileri daha hızlı almasına izin verir, ancak bellek tüketimini artırabilir. |
| `AuthorizationData` | Veriler, hub sınıfına uygulanan `Authorize` özniteliklerinden otomatik olarak toplanır. | Bir istemcinin hub 'a bağlanmasına yetkili olup olmadığını belirlemede kullanılan [ıauthorizedata](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) nesnelerinin listesi. |
| `TransportMaxBufferSize` | 32 KB | Uygulama tarafından, geri basıncını gözlemlenmadan önce sunucunun arabelleklerinin gönderdiği en fazla bayt sayısı. Bu değeri artırmak, sunucunun geri basınç beklemeden daha büyük iletileri daha hızlı arabelleğe almasına izin verir, ancak bellek tüketimini artırabilir. |
| `Transports` | Tüm aktarımlar etkin. | Bir istemcinin bağlanmak için kullanabileceği taşımaları kısıtlayabilecek `HttpTransportType` değerlerinin bir bit bayrakları numaralandırması. |
| `LongPolling` | Aşağıya bakın. | Uzun yoklama taşımasına özgü ek seçenekler. |
| `WebSockets` | Aşağıya bakın. | WebSockets taşımasına özgü ek seçenekler. |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | İstemciden sunucunun arabelleğe aldığı en fazla bayt sayısı. Bu değeri artırmak, sunucunun daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir. |
| `AuthorizationData` | Veriler, hub sınıfına uygulanan `Authorize` özniteliklerinden otomatik olarak toplanır. | Bir istemcinin hub 'a bağlanmasına yetkili olup olmadığını belirlemede kullanılan [ıauthorizedata](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) nesnelerinin listesi. |
| `TransportMaxBufferSize` | 32 KB | Uygulama tarafından sunucunun arabelleklerinin gönderdiği en fazla bayt sayısı. Bu değeri artırmak, sunucunun daha büyük iletiler göndermesini sağlar, ancak bellek tüketimini olumsuz etkileyebilir. |
| `Transports` | Tüm aktarımlar etkin. | Bir istemcinin bağlanmak için kullanabileceği taşımaları kısıtlayabilecek `HttpTransportType` değerlerinin bir bit bayrakları numaralandırması. |
| `LongPolling` | Aşağıya bakın. | Uzun yoklama taşımasına özgü ek seçenekler. |
| `WebSockets` | Aşağıya bakın. | WebSockets taşımasına özgü ek seçenekler. |

::: moniker-end

Uzun yoklama taşıması `LongPolling` özelliği kullanılarak yapılandırılabilecek ek seçeneklere sahiptir:

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 saniye | Tek bir yoklama isteğini sonlandırmadan önce sunucunun istemciye göndermek için bekleyeceği en uzun süre. Bu değeri azaltmak istemcinin yeni yoklama istekleri daha sık vermesine neden olur. |

WebSocket taşıması `WebSockets` özelliği kullanılarak yapılandırılabilen ek seçeneklere sahiptir:

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 saniye | Sunucu kapandıktan sonra, istemci bu zaman aralığında kapanamazsa bağlantı sonlandırılır. |
| `SubProtocolSelector` | `null` | @No__t-0 üst bilgisini özel bir değere ayarlamak için kullanılabilen bir temsilci. Temsilci, istemci tarafından istenen değerleri girdi olarak alır ve istenen değeri döndürmesi beklenir. |

## <a name="configure-client-options"></a>İstemci seçeneklerini yapılandırma

İstemci seçenekleri `HubConnectionBuilder` türünde (.NET ve JavaScript istemcilerinde bulunur) yapılandırılabilir. Java istemcisinde de bulunur, ancak `HttpHubConnectionBuilder` alt sınıfı, Oluşturucu yapılandırma seçeneklerinin yanı sıra `HubConnection` ' i içerir.

### <a name="configure-logging"></a>Günlüğe kaydetmeyi yapılandırma

Günlüğe kaydetme, .NET Istemcisinde `ConfigureLogging` yöntemi kullanılarak yapılandırılır. Günlüğe kaydetme sağlayıcıları ve filtreler, sunucuda oldukları gibi aynı şekilde kaydedilebilir. Daha fazla bilgi için [oturum açma ASP.NET Core](xref:fundamentals/logging/index) belgelerine bakın.

> [!NOTE]
> Günlüğe kaydetme sağlayıcılarını kaydetmek için gerekli paketleri yüklemelisiniz. Tam liste için docs 'ın [yerleşik günlük sağlayıcıları](xref:fundamentals/logging/index#built-in-logging-providers) bölümüne bakın.

Örneğin, konsol günlüğünü etkinleştirmek için `Microsoft.Extensions.Logging.Console` NuGet paketini yüklemelisiniz. @No__t-0 genişletme yöntemini çağırın:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

JavaScript istemcisinde benzer bir `configureLogging` yöntemi vardır. Üretilecek günlük iletilerinin en düşük düzeyini belirten bir `LogLevel` değeri belirtin. Günlükler tarayıcı konsolu penceresine yazılır.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

@No__t-0 değeri yerine, bir günlük düzeyi adını temsil eden bir `string` değeri de sağlayabilirsiniz. Bu, `LogLevel` sabitlerine erişiminizin olmadığı ortamlarda SignalR günlüğünü yapılandırırken yararlıdır.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

Aşağıdaki tabloda kullanılabilir günlük düzeyleri listelenmektedir. @No__t-0 ' a sağladığınız değer, günlüğe kaydedilecek **Minimum** günlük düzeyini ayarlar. Bu düzeyde günlüğe kaydedilen iletiler **veya tabloda bundan sonra listelenen düzeyler**günlüğe kaydedilir.

| Dize                      | logLevel               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| `info` **veya** `information` | `LogLevel.Information` |
| `warn` **veya** `warning`     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> Günlüğe kaydetmeyi tamamen devre dışı bırakmak için `configureLogging` yönteminde `signalR.LogLevel.None` belirtin.

Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [SignalR Diagnostics belgeleri](xref:signalr/diagnostics).

SignalR Java istemcisi, günlük kaydı için [dolayısıyla slf4j](https://www.slf4j.org/) kitaplığını kullanır. Bu, kitaplık kullanıcılarının belirli bir günlüğe kaydetme bağımlılığı vererek kendi belirli günlük uygulamasını seçmesine olanak sağlayan, üst düzey bir günlüğe kaydetme API 'sidir. Aşağıdaki kod parçacığı, SignalR Java istemcisiyle `java.util.logging` ' nın nasıl kullanılacağını gösterir.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Bağımlılıklarınız için günlük kaydını yapılandırmazsanız, DOLAYıSıYLA SLF4J aşağıdaki uyarı iletisiyle varsayılan işlem olmayan bir günlükçü yükler:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Bu, güvenle yoksayılabilir.

### <a name="configure-allowed-transports"></a>İzin verilen taşımaları yapılandırma

SignalR tarafından kullanılan aktarımlar `WithUrl` çağrısında yapılandırılabilir (JavaScript 'te `withUrl`). @No__t-0 değerleri, istemciyi yalnızca belirtilen aktarımları kullanacak şekilde kısıtlamak için kullanılabilir. Tüm aktarımlar varsayılan olarak etkindir.

Örneğin, sunucu tarafından gönderilen olay taşımasını devre dışı bırakmak, ancak WebSockets ve uzun yoklama bağlantılarına izin vermek için:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

JavaScript istemcisinde aktarımlar, `withUrl` için sunulan Options nesnesindeki `transport` alanı ayarlanarak yapılandırılır:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

Java Client WebSockets 'in bu sürümünde, kullanılabilir tek aktarım bir sürümdür.

::: moniker-end

::: moniker range="= aspnetcore-3.0"

Java istemcisinde, taşıma, `HttpHubConnectionBuilder` üzerinde `withTransport` yöntemiyle seçilir. Java istemcisi WebSockets taşımasını varsayılan olarak kullanmaktır.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> SignalR Java istemcisi henüz taşıma geri dönüşü desteklemez.

::: moniker-end

### <a name="configure-bearer-authentication"></a>Taşıyıcı kimlik doğrulamasını yapılandırma

Kimlik doğrulama verilerini SignalR istekleriyle birlikte sağlamak için, istenen erişim belirtecini döndüren bir işlevi belirtmek üzere `AccessTokenProvider` seçeneğini (JavaScript 'te `accessTokenFactory`) kullanın. .NET Istemcisinde, bu erişim belirteci bir HTTP "taşıyıcı kimlik doğrulaması" belirteci olarak geçirilir (`Bearer` türü ile `Authorization` üst bilgisi kullanılarak). JavaScript istemcisinde, tarayıcı API 'Lerinin üstbilgileri uygulama özelliğini (özellikle de sunucu tarafından gönderilen olaylar ve WebSockets istekleri) kısıtlayacağı birkaç durum **dışında** , erişim belirteci bir taşıyıcı belirteci olarak kullanılır. Bu gibi durumlarda, erişim belirteci bir sorgu dizesi değeri olarak sağlanır `access_token`.

.NET istemcisinde `AccessTokenProvider` seçeneği, `WithUrl` ' deki Seçenekler temsilcisi kullanılarak belirtilebilir:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

JavaScript istemcisinde erişim belirteci, `withUrl` ' deki Seçenekler nesnesindeki `accessTokenFactory` alanı ayarlanarak yapılandırılır:

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

SignalR Java istemcisinde, [Httphubconnectionbuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)'a bir erişim belirteci fabrikası sağlayarak kimlik doğrulaması için kullanılacak bir taşıyıcı belirteç yapılandırabilirsiniz. [Rxjava](https://github.com/ReactiveX/RxJava) [tek @ No__t-3string >](https://reactivex.io/documentation/single.html)sağlamak Için [withaccesstokenfactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) kullanın. [Tek. ertele](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)çağrısıyla, istemciniz için erişim belirteçleri oluşturmak üzere mantık yazabilirsiniz.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>Zaman aşımını yapılandırın ve canlı tut seçeneklerini yapılandırın

Zaman aşımını ve canlı tutma davranışını yapılandırmaya yönelik ek seçenekler `HubConnection` nesnesinin kendisi üzerinde kullanılabilir:


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 saniye (30.000 milisaniye) | Sunucu etkinliği için zaman aşımı. Sunucu bu aralıkta bir ileti göndermediyse, istemci, sunucunun bağlantısını kesmiş olarak değerlendirir ve `Closed` olayını (JavaScript 'te `onclose`) tetikler. Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır. Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olması için gereken bir sayıdır. |
| `HandshakeTimeout` | 15 saniye | İlk sunucu el sıkışması için zaman aşımı. Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `Closed` olayını tetikler (JavaScript 'te `onclose`). Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır. El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 saniye | İstemcinin ping iletileri gönderdiği aralığı belirler. İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır. İstemci sunucuda `ClientTimeoutInterval` kümesinde bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder. |

.NET Istemcisinde, zaman aşımı değerleri `TimeSpan` değerleri olarak belirtilir.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 saniye (30.000 milisaniye) | Sunucu etkinliği için zaman aşımı. Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onclose` olayını tetikler. Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır. Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olması için gereken bir sayıdır. |
| `keepAliveIntervalInMilliseconds` | 15 saniye (15.000 'ları harcanan) | İstemcinin ping iletileri gönderdiği aralığı belirler. İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır. İstemci sunucuda `ClientTimeoutInterval` kümesinde bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `getServerTimeout` / `setServerTimeout` | 30 saniye (30.000 milisaniye) | Sunucu etkinliği için zaman aşımı. Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onClose` olayını tetikler. Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır. Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olması için gereken bir sayıdır. |
| `withHandshakeResponseTimeout` | 15 saniye | İlk sunucu el sıkışması için zaman aşımı. Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `onClose` olayını tetikler. Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır. El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `getKeepAliveInterval` / `setKeepAliveInterval` | 15 saniye (15.000 milisaniye) | İstemcinin ping iletileri gönderdiği aralığı belirler. İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır. İstemci sunucuda `ClientTimeoutInterval` kümesinde bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder. |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 saniye (30.000 milisaniye) | Sunucu etkinliği için zaman aşımı. Sunucu bu aralıkta bir ileti göndermediyse, istemci, sunucunun bağlantısını kesmiş olarak değerlendirir ve `Closed` olayını (JavaScript 'te `onclose`) tetikler. Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır. Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olması için gereken bir sayıdır. |
| `HandshakeTimeout` | 15 saniye | İlk sunucu el sıkışması için zaman aşımı. Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `Closed` olayını tetikler (JavaScript 'te `onclose`). Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır. El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

.NET Istemcisinde, zaman aşımı değerleri `TimeSpan` değerleri olarak belirtilir.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 saniye (30.000 milisaniye) | Sunucu etkinliği için zaman aşımı. Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onclose` olayını tetikler. Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır. Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olması için gereken bir sayıdır. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Seçenek | Varsayılan değer | Açıklama |
| ------ | ------------- | ----------- |
| `getServerTimeout` / `setServerTimeout` | 30 saniye (30.000 milisaniye) | Sunucu etkinliği için zaman aşımı. Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onClose` olayını tetikler. Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır. Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır. |
| `withHandshakeResponseTimeout` | 15 saniye | İlk sunucu el sıkışması için zaman aşımı. Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `onClose` olayını tetikler. Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır. El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

::: moniker-end

---

### <a name="configure-additional-options"></a>Ek seçenekleri yapılandırma

Ek seçenekler `HubConnectionBuilder` ' deki `WithUrl` (JavaScript 'te `withUrl`) yönteminde veya Java istemcisinde `HttpHubConnectionBuilder` ' teki çeşitli yapılandırma API 'Lerinde yapılandırılabilir:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| .NET seçeneği |  Varsayılan değer | Açıklama |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev. |
| `SkipNegotiation` | `false` | Anlaşma adımını atlamak için bunu `true` olarak ayarlayın. **Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**. Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez. |
| `ClientCertificates` | Olmamalıdır | Kimlik doğrulaması isteklerine gönderilmek üzere TLS sertifikaları koleksiyonu. |
| `Cookies` | Olmamalıdır | Her HTTP isteğiyle gönderilmek üzere HTTP tanımlama bilgilerinin bir koleksiyonu. |
| `Credentials` | Olmamalıdır | Her HTTP isteğiyle gönderilen kimlik bilgileri. |
| `CloseTimeout` | 5 saniye | Yalnızca WebSockets. Sunucunun kapatma isteğini onaylaması için kapatıldıktan sonra bekleyeceği en uzun süre. Sunucu bu süre içinde kapatmayı kabul etmezse, istemci bağlantısını keser. |
| `Headers` | Olmamalıdır | Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası. |
| `HttpMessageHandlerFactory` | `null` | HTTP istekleri göndermek için kullanılan @no__t (0) yapılandırmak veya değiştirmek için kullanılabilen bir temsilci. WebSocket bağlantıları için kullanılmıyor. Bu temsilci null olmayan bir değer döndürmelidir ve varsayılan değeri bir parametre olarak alır. Bu varsayılan değerde ayarları değiştirin ve döndürün ya da yeni bir `HttpMessageHandler` örneği döndürün. **İşleyiciyi değiştirirken, belirtilen işleyiciden tutmak istediğiniz ayarları kopyalamadığınızdan emin olun, aksi takdirde, yapılandırılan seçenekler (tanımlama bilgileri ve üstbilgiler gibi) yeni işleyiciye uygulanmaz.** |
| `Proxy` | `null` | HTTP istekleri gönderilirken kullanılacak bir HTTP proxy 'si. |
| `UseDefaultCredentials` | `false` | Bu Boole değeri HTTP ve WebSockets istekleri için varsayılan kimlik bilgilerini gönderecek şekilde ayarlayın. Bu, Windows kimlik doğrulamasının kullanılmasını mümkün. |
| `WebSocketConfiguration` | `null` | Ek WebSocket seçeneklerini yapılandırmak için kullanılabilen bir temsilci. Seçenekleri yapılandırmak için kullanılabilecek [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) örneğini alır. |

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| JavaScript seçeneği | Varsayılan değer | Açıklama |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev. |
| `skipNegotiation` | `false` | Anlaşma adımını atlamak için bunu `true` olarak ayarlayın. **Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**. Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Java seçeneği | Varsayılan değer | Açıklama |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev. |
| `shouldSkipNegotiate` | `false` | Anlaşma adımını atlamak için bunu `true` olarak ayarlayın. **Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**. Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez. |
| `withHeader` `withHeaders` | Olmamalıdır | Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası. |

---

.NET Istemcisinde, bu seçenekler `WithUrl` ' a sunulan seçenekler temsilcisi tarafından değiştirilebilir:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

JavaScript Istemcisinde, bu seçenekler `withUrl` ' a sunulan bir JavaScript nesnesi içinde bulunabilir:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

Java istemcisinde, bu seçenekler `HubConnectionBuilder.create("HUB URL")` ' den döndürülen `HttpHubConnectionBuilder` ' daki yöntemlerle yapılandırılabilir

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
