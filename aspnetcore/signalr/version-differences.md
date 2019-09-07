---
title: SignalR ve ASP.NET Core SignalR arasındaki farklar
author: bradygaster
description: SignalR ve ASP.NET Core SignalR arasındaki farklar
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: 70b09493d9b4c96c897465d60e53e93a793c42f9
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746546"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>ASP.NET SignalR ile ASP.NET Core SignalR arasındaki farklar

ASP.NET Core SignalR, ASP.NET SignalR için istemcilerle veya sunucularla uyumlu değildir. Bu makalede, ASP.NET Core SignalR 'de kaldırılan veya değiştirilen özellikler ayrıntılı olarak anlatılmaktadır.

## <a name="how-to-identify-the-signalr-version"></a>SignalR sürümünü belirleme

|                      | ASP.NET SignalR | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| Sunucu NuGet paketi | [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft. AspNetCore. SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| İstemci NuGet paketleri | [Microsoft. AspNet. SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft. AspNet. SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft. AspNetCore. SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| İstemci NPM paketi | [SignalR](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Java Istemcisi | [GitHub deposu](https://github.com/SignalR/java-client) kullanım dışı  | Maven paketi [com. Microsoft. SignalR](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| Sunucu uygulaması türü | ASP.NET (System. Web) veya OWıN Self-Host | ASP.NET Core |
| Desteklenen sunucu platformları | .NET Framework 4,5 veya üzeri | .NET Framework 4.6.1 veya üzeri<br>.NET Core 2,1 veya üzeri |

## <a name="feature-differences"></a>Özellik farklılıkları

### <a name="automatic-reconnects"></a>Otomatik yeniden bağlantılar

ASP.NET Core SignalR 'de otomatik yeniden bağlantılar desteklenmez. İstemcinin bağlantısı kesilirse, yeniden bağlanmak istiyorlarsa kullanıcının açıkça yeni bir bağlantı başlatması gerekir. ASP.NET SignalR sürümünde, bağlantı kesildiğinde SignalR sunucuya yeniden bağlanmaya çalışır.

### <a name="protocol-support"></a>Protokol desteği

ASP.NET Core SignalR, JSON 'ı ve [MessagePack](xref:signalr/messagepackhubprotocol)tabanlı yeni bir ikili protokolü destekler. Ek olarak özel protokoller de oluşturulabilir.

### <a name="transports"></a>Taşımalar

ASP.NET Core SignalR içinde süresiz çerçeve taşıması desteklenmez.

## <a name="differences-on-the-server"></a>Sunucu farkları

ASP.NET Core SignalR sunucu tarafı kitaplıkları, hem Razor hem de MVC projeleri için **ASP.NET Core Web uygulaması** şablonunun bir parçası olan [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app) paketine dahildir.

ASP.NET Core SignalR bir ASP.NET Core ara yazılım olduğundan, içinde `Startup.ConfigureServices` [addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) çağırarak yapılandırılması gerekir.

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

Yönlendirmeyi yapılandırmak için, yollar içindeki `Startup.Configure` [useendpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) yöntemi çağrısının içindeki bağlantıları eşleyin.


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Yönlendirmeyi yapılandırmak için, `Startup.Configure` yöntemleri yöntemindeki [usesignalr](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) yöntemi çağrısının içindeki hublara eşleyin.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a>Yapışkan oturumlar

ASP.NET SignalR için genişleme modeli, istemcilerin gruptaki herhangi bir sunucuya yeniden bağlanmasını ve iletileri göndermesini sağlar. ASP.NET Core SignalR 'de, istemci bağlantı süresince aynı sunucu ile etkileşime sahip olmalıdır. Redsıs kullanarak genişleme için, bu, yapışkan oturumların gerekli olduğu anlamına gelir. [Azure SignalR hizmetini](/azure/azure-signalr/)kullanarak genişleme için, hizmet istemcilerle bağlantıları işlediği için yapışkan oturumlar gerekli değildir.

### <a name="single-hub-per-connection"></a>Bağlantı başına tek hub

ASP.NET Core SignalR sürümünde bağlantı modeli basitleştirilmiştir. Bağlantılar, birden çok hub 'a erişimi paylaşmak için kullanılan tek bir bağlantı yerine doğrudan tek bir hub 'a yapılır.

### <a name="streaming"></a>Akış

ASP.NET Core SignalR artık hub 'dan istemciye [veri akışı](xref:signalr/streaming) desteklemektedir.

### <a name="state"></a>Durum

İstemcilerle Hub (genellikle HubState olarak adlandırılır) arasında rastgele durum geçirme özelliği kaldırılmıştır ve ilerleme durumu iletileri için destek de sağlar. Şu anda hub proxy 'lerinin karşılığı yok.

### <a name="persistentconnection-removal"></a>PersistentConnection kaldırma

ASP.NET Core SignalR 'de, [Persistentconnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) sınıfı kaldırılmıştır.

### <a name="globalhost"></a>GlobalHost

ASP.NET Core çerçeveye yerleşik bağımlılık ekleme (dı) vardır. Hizmetler, [Hubcontext](xref:signalr/hubcontext)'e erışmek için dı kullanabilir. Almak için ASP.NET SignalR içinde kullanılan nesneASP.NETCoreSignalRiçindeyok.`GlobalHost` `HubContext`

### <a name="hubpipeline"></a>HubPipeline

ASP.NET Core SignalR `HubPipeline` modül desteğine sahip değildir.

## <a name="differences-on-the-client"></a>İstemci farkları

### <a name="typescript"></a>TypeScript

ASP.NET Core SignalR istemcisi [TypeScript](https://www.typescriptlang.org/)'te yazılmıştır. [JavaScript istemcisini](xref:signalr/javascript-client)kullanırken JavaScript veya TypeScript ile yazabilirsiniz.

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>JavaScript istemcisi [NPM](https://www.npmjs.com/) 'de barındırılıyor

Önceki sürümlerde JavaScript istemcisi, Visual Studio 'da bir NuGet paketi aracılığıyla elde edildi. Temel sürümler [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) için NPM paketi Javascript kitaplıklarını içerir. Bu paket, **ASP.NET Core Web uygulaması** şablonuna dahil değildir. NPM paketini edinmek ve yüklemek `@aspnet/signalr` için NPM kullanın.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

JQuery üzerindeki bağımlılık kaldırılmıştır, ancak projeler jQuery kullanmaya devam edebilir.

### <a name="internet-explorer-support"></a>Internet Explorer desteği

ASP.NET Core SignalR, Microsoft Internet Explorer 11 veya üstünü gerektirir (ASP.NET SignalR desteklenen Microsoft Internet Explorer 8 ve üzeri).

### <a name="javascript-client-method-syntax"></a>JavaScript istemci yöntemi sözdizimi

JavaScript sözdizimi, SignalR 'nin önceki sürümünden değiştirilmiştir. `$connection` Nesnesini kullanmak yerine, [hubconnectionbuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API 'sini kullanarak bir bağlantı oluşturun.

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

`Microsoft.AspNetCore.SignalR.Client` NuGet paketi ASP.NET Core SignalR için .NET istemci kitaplıklarını içerir.

Hub 'a bir bağlantı örneği oluşturmak ve derlemek için [Hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) ' ı kullanın.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>Ölçek Genişletme farklılıkları

ASP.NET SignalR SQL Server ve Redsıs 'yi destekler. ASP.NET Core SignalR, Azure SignalR hizmetini ve Redl 'yi destekler.

### <a name="aspnet"></a>ASP.NET

* [Azure Service Bus ile SignalR ölçeği](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [Redsıs ile SignalR ölçeği](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [SQL Server ile SignalR ölçeği](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Azure SignalR Hizmeti](/azure/azure-signalr/)
* [Redsıs geri düzlemi](xref:signalr/redis-backplane)

## <a name="additional-resources"></a>Ek kaynaklar

* [Merkezler](xref:signalr/hubs)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [Desteklenen platformlar](xref:signalr/supported-platforms)
