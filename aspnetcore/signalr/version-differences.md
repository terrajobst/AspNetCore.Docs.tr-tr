---
title: SignalR ve ASP.NET Core arasındaki farklar SignalR
author: bradygaster
description: SignalR ve ASP.NET Core arasındaki farklar SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/version-differences
ms.openlocfilehash: 0f644c132b0fcf9a0ecf0ab181791a6477c97f76
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963716"
---
# <a name="differences-between-aspnet-opno-locsignalr-and-aspnet-core-opno-locsignalr"></a>ASP.NET SignalR ve ASP.NET Core arasındaki farklar SignalR

ASP.NET Core SignalR, ASP.NET SignalRiçin istemcilerle veya sunucularla uyumlu değildir. Bu makalede, ASP.NET Core SignalRkaldırılan veya değiştirilen özellikler ayrıntılı olarak anlatılmaktadır.

## <a name="how-to-identify-the-opno-locsignalr-version"></a>SignalR sürümünü belirleme

|                      | ASP.NET SignalR | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| Sunucu NuGet paketi | [Microsoft. AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft. AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| İstemci NuGet paketleri | [Microsoft. AspNet.SignalR. İstemcilerinin](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft. AspNet.SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft. AspNetCore.SignalR. İstemcilerinin](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| İstemci NPM paketi | [SignalR](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Java Istemcisi | [GitHub deposu](https://github.com/SignalR/java-client) (kullanım dışı)  | Maven paketi [com. Microsoft. SignalR](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| Sunucu uygulaması türü | ASP.NET (System. Web) veya OWıN Self-Host | ASP.NET Core |
| Desteklenen sunucu platformları | .NET Framework 4,5 veya üzeri | .NET Framework 4.6.1 veya üzeri<br>.NET Core 2,1 veya üzeri |

## <a name="feature-differences"></a>Özellik farklılıkları

### <a name="automatic-reconnects"></a>Otomatik yeniden bağlantılar

Otomatik yeniden bağlantılar ASP.NET Core SignalRdesteklenmez. İstemcinin bağlantısı kesilirse, yeniden bağlanmak istiyorlarsa kullanıcının açıkça yeni bir bağlantı başlatması gerekir. ASP.NET SignalR' de, bağlantı kesildiğinde SignalR sunucuya yeniden bağlanmaya çalışır.

### <a name="protocol-support"></a>Protokol desteği

ASP.NET Core SignalR, JSON 'ı ve [MessagePack](xref:signalr/messagepackhubprotocol)tabanlı yeni bir ikili protokolü destekler. Ek olarak özel protokoller de oluşturulabilir.

### <a name="transports"></a>Taşımalar

ASP.NET Core SignalRiçin süresiz çerçeve taşıması desteklenmez.

## <a name="differences-on-the-server"></a>Sunucu farkları

ASP.NET Core SignalR sunucu tarafı kitaplıkları, hem Razor hem de MVC projeleri için **ASP.NET Core Web uygulaması** şablonunun parçası olan [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app) paketine dahildir.

ASP.NET Core SignalR ASP.NET Core bir ara yazılım olduğundan, `Startup.ConfigureServices`içinde [Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) çağırarak yapılandırılması gerekir.

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

Yönlendirmeyi yapılandırmak için, yolları `Startup.Configure` yönteminde [Useendpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) yöntemi çağrısının içindeki hub 'lara eşleyin.


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Yönlendirmeyi yapılandırmak için, yolları `Startup.Configure` yöntemi içindeki [Usesignalr](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) yöntemi çağrısının içindeki hublara eşleyin.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a>Yapışkan oturumlar

ASP.NET SignalR için genişleme modeli, istemcilerin gruptaki herhangi bir sunucuya yeniden bağlanmasını ve iletileri göndermesini sağlar. ASP.NET Core SignalR, istemci bağlantı süresince aynı sunucu ile etkileşime sahip olmalıdır. Redsıs kullanarak genişleme için, bu, yapışkan oturumların gerekli olduğu anlamına gelir. [Azure SignalR hizmetini](/azure/azure-signalr/)kullanarak genişleme için, hizmet istemcilerle bağlantıları işlediği için yapışkan oturumlar gerekli değildir.

### <a name="single-hub-per-connection"></a>Bağlantı başına tek hub

ASP.NET Core SignalR, bağlantı modeli basitleştirilmiştir. Bağlantılar, birden çok hub 'a erişimi paylaşmak için kullanılan tek bir bağlantı yerine doğrudan tek bir hub 'a yapılır.

### <a name="streaming"></a>Akış

ASP.NET Core SignalR artık hub 'dan istemciye [veri akışını](xref:signalr/streaming) desteklemektedir.

### <a name="state"></a>Durum

İstemcilerle Hub (genellikle HubState olarak adlandırılır) arasında rastgele durum geçirme özelliği kaldırılmıştır ve ilerleme durumu iletileri için destek de sağlar. Şu anda hub proxy 'lerinin karşılığı yok.

### <a name="persistentconnection-removal"></a>PersistentConnection kaldırma

ASP.NET Core SignalR, [Persistentconnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) sınıfı kaldırılmıştır.

### <a name="globalhost"></a>GlobalHost

ASP.NET Core çerçeveye yerleşik bağımlılık ekleme (dı) vardır. Hizmetler, [Hubcontext](xref:signalr/hubcontext)'e erışmek için dı kullanabilir. `HubContext` almak için ASP.NET SignalR içinde kullanılan `GlobalHost` nesnesi ASP.NET Core SignalRyok.

### <a name="hubpipeline"></a>HubPipeline

ASP.NET Core SignalR `HubPipeline` modül desteğine sahip değildir.

## <a name="differences-on-the-client"></a>İstemci farkları

### <a name="typescript"></a>TypeScript

ASP.NET Core SignalR istemcisi [TypeScript](https://www.typescriptlang.org/)'te yazılmıştır. [JavaScript istemcisini](xref:signalr/javascript-client)kullanırken JavaScript veya TypeScript ile yazabilirsiniz.

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>JavaScript istemcisi [NPM](https://www.npmjs.com/) 'de barındırılıyor

Önceki sürümlerde JavaScript istemcisi, Visual Studio 'da bir NuGet paketi aracılığıyla elde edildi. Temel sürümler için [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) NPM paketi Javascript kitaplıklarını içerir. Bu paket, **ASP.NET Core Web uygulaması** şablonuna dahil değildir. `@aspnet/signalr` NPM paketini edinmek ve yüklemek için NPM kullanın.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

JQuery üzerindeki bağımlılık kaldırılmıştır, ancak projeler jQuery kullanmaya devam edebilir.

### <a name="internet-explorer-support"></a>Internet Explorer desteği

ASP.NET Core SignalR Microsoft Internet Explorer 11 veya üstünü gerektirir (ASP.NET SignalR desteklenen Microsoft Internet Explorer 8 ve üzeri).

### <a name="javascript-client-method-syntax"></a>JavaScript istemci yöntemi sözdizimi

JavaScript sözdizimi, SignalRönceki sürümünden değiştirilmiştir. `$connection` nesnesini kullanmak yerine, [Hubconnectionbuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API 'sini kullanarak bir bağlantı oluşturun.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Hub 'ın çağırakullanabileceği istemci yöntemlerini belirtmek için [on](/javascript/api/@aspnet/signalr/HubConnection#on) metodunu kullanın.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

İstemci yöntemini oluşturduktan sonra hub bağlantısını başlatın. Günlüğe kaydetmek veya hataları işlemek için bir [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) metodunu zincirleyin.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Hub proxy 'leri

Hub proxy 'leri artık otomatik olarak oluşturulmaz. Bunun yerine, yöntem adı [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API 'sine bir dize olarak geçirilir.

### <a name="net-and-other-clients"></a>.NET ve diğer istemciler

`Microsoft.AspNetCore.SignalR.Client` NuGet paketi, ASP.NET Core SignalR.NET istemci kitaplıklarını içerir.

Hub 'a bir bağlantı örneği oluşturmak ve derlemek için [Hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) ' ı kullanın.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>Ölçek Genişletme farklılıkları

ASP.NET SignalR SQL Server ve Redsıs 'yi destekler. ASP.NET Core SignalR Azure SignalR hizmeti 'ni ve Redsıs 'yi destekler.

### <a name="aspnet"></a>ASP.NET

* [Azure Service Bus ile ölçeği SignalR](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [Redsıs ile SignalR ölçeği](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [SQL Server ile ölçeği SignalR](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Azure SignalR hizmeti](/azure/azure-signalr/)
* [Redsıs geri düzlemi](xref:signalr/redis-backplane)

## <a name="additional-resources"></a>Ek kaynaklar

* [Merkezler](xref:signalr/hubs)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [Desteklenen platformlar](xref:signalr/supported-platforms)
