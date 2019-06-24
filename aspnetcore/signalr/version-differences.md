---
title: SignalR ve ASP.NET Core SignalR arasındaki farklar
author: bradygaster
description: SignalR ve ASP.NET Core SignalR arasındaki farklar
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: d140becaed5b4fc765b7a72571c7f5dce7f4e3d4
ms.sourcegitcommit: 28646e8ca62fb094db1557b5c0c02d5b45531824
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2019
ms.locfileid: "67333454"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>ASP.NET SignalR ile ASP.NET Core SignalR arasındaki farklar

ASP.NET Core SignalR istemciler veya sunucular için ASP.NET SignalR ile uyumlu değildir. Bu makalede, kaldırılmış veya ASP.NET Core SignalR öğesinde değiştirilen özellikler açıklanmaktadır.

## <a name="how-to-identify-the-signalr-version"></a>SignalR sürüm belirleme

|                      | ASP.NET SignalR | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| Sunucu NuGet paketi | [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| İstemci NuGet paketleri | [Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| İstemci npm paket | [signalr](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Java istemci | [GitHub deposu](https://github.com/SignalR/java-client) (kullanım dışı)  | Maven paket [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| Sunucu uygulama türü | ASP.NET (System.Web) veya OWIN barındırma | ASP.NET Core |
| Desteklenen sunucu platformları | .NET framework 4.5 veya üzeri | .NET framework 4.6.1 veya üzeri<br>.NET core 2.1 veya üzeri |

## <a name="feature-differences"></a>Özellik farkları

### <a name="automatic-reconnects"></a>Otomatik yeniden bağlantılar

ASP.NET Core SignalR öğesinde otomatik yeniden bağlantılar desteklenmez. İstemci bağlantısı kesilmişse, kullanıcı yeniden bağlamak isterseniz yeni bir bağlantı açıkça başlamalıdır. ASP.NET SignalR SignalR bağlantısı kesilirse, sunucuya yeniden dener. 

### <a name="protocol-support"></a>Protokol desteği

ASP.NET Core Signalr'yi destekleyen dayalı yeni bir ikili Protokolü yanı sıra JSON [MessagePack](xref:signalr/messagepackhubprotocol). Ayrıca, özel protokoller oluşturulabilir.

### <a name="transports"></a>Taşımalar

ASP.NET Core SignalR öğesinde sonsuza kadar çerçeve taşıma desteklenmiyor.

## <a name="differences-on-the-server"></a>Sunucuda farkları

ASP.NET Core SignalR sunucu tarafı kitaplıklar dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) parçası olan paket **ASP.NET Core Web uygulaması** Razor hem MVC şablonu projeleri.

ASP.NET Core SignalR çağırarak yapılandırılmalıdır bir ASP.NET Core ara yazılımını olduğundan [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) içinde `Startup.ConfigureServices`.

```csharp
services.AddSignalR()
```

Yönlendirmeyi yapılandırmak için eşleme rotaları hub içinde [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) yöntem çağrısı `Startup.Configure` yöntemi.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions"></a>Yapışkan oturumlar

ASP.NET SignalR ölçeğini genişletme modeli yeniden ve gruptaki herhangi bir sunucuya ileti gönderme etmesine olanak tanır. ASP.NET Core SignalR öğesinde, istemci ile aynı sunucuya bağlantı süresi boyunca etkileşim kurmalıdır. Redis kullanarak genişletme için Yapışkan oturumlar gerekli olduğu anlamına gelir. Genişletme kullanma [Azure SignalR hizmeti](/azure/azure-signalr/), Yapışkan oturumlar bulunmamaktır hizmet bağlantıları istemcilere hallettiğinden. 

### <a name="single-hub-per-connection"></a>Bağlantı başına tek bir hub

ASP.NET Core SignalR öğesinde bağlantı modeli basitleştirilmiştir. Bağlantıları, doğrudan erişim birden çok hub'a paylaşmak için kullanılan tek bir bağlantı yerine tek bir hub için gerçekleştirilir.

### <a name="streaming"></a>Akış

ASP.NET Core SignalR destekler [akış verileri](xref:signalr/streaming) istemciye hub'ından.

### <a name="state"></a>Durum

İlerleme durumu iletileri için destek yanı sıra hub'ı (HubState olarak da adlandırılır) ve istemciler arasında rastgele bir durum geçirme özelliği kaldırıldı. Şu anda hiçbir hub proxy karşılığı yoktur.

### <a name="persistentconnection-removal"></a>PersistentConnection kaldırma

ASP.NET Core signalr'da [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) sınıfı kaldırıldı. 

### <a name="globalhost"></a>GlobalHost

ASP.NET Core Framework'e yerleşik bağımlılık ekleme (dı) sahiptir. Hizmetleri DI erişmek için kullanabileceğiniz [HubContext](xref:signalr/hubcontext). `GlobalHost` ASP.NET SignalR öğesinde almak için kullanılan nesne bir `HubContext` ASP.NET Core SignalR öğesinde yok.

### <a name="hubpipeline"></a>HubPipeline

ASP.NET Core SignalR için destek yok `HubPipeline` modüller.

## <a name="differences-on-the-client"></a>İstemci üzerinde farkları

### <a name="typescript"></a>TypeScript

ASP.NET Core SignalR istemci yazıldığı [TypeScript](https://www.typescriptlang.org/). JavaScript veya TypeScript kullanırken yazabileceğiniz [JavaScript istemci](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>JavaScript istemcisi, barındırılan [npm](https://www.npmjs.com/)

Önceki sürümlerde, Visual Studio'da bir NuGet paketi aracılığıyla JavaScript istemci alındı. Çekirdek sürümleri için [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) npm paket JavaScript kitaplıkları içerir. Bu paket bulunup bulunmadığına **ASP.NET Core Web uygulaması** şablonu. Elde etmek ve yüklemek için npm kullanın `@aspnet/signalr` npm paketi.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

JQuery bağımlı projeler jQuery kullanmaya devam edebilirsiniz ancak kaldırıldı.

### <a name="internet-explorer-support"></a>Internet Explorer desteği

ASP.NET Core SignalR (ASP.NET SignalR, Microsoft Internet Explorer 8 ve üzeri desteklenir) Microsoft Internet Explorer 11 veya üstü gerektirir.

### <a name="javascript-client-method-syntax"></a>JavaScript istemci yöntem sözdizimi

JavaScript sözdizimi SignalR önceki sürümünden değişmiştir. Yerine `$connection` nesne, bir bağlantı kullanarak oluşturduğunuz [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Kullanım [üzerinde](/javascript/api/@aspnet/signalr/HubConnection#on) hub çağırabilirsiniz istemci yöntemlerini belirtmek için yöntemi.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

İstemci yöntemi oluşturduktan sonra hub Bağlantısı'nı başlatın. Zincir bir [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) oturum veya hataları işlemek için bir yöntem.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Hub proxy

Hub proxy artık otomatik olarak oluşturulur. Bunun yerine, yöntem adı yöntemlere geçirilen [çağırma](/javascript/api/%40aspnet/signalr/hubconnection#invoke) dize olarak API.

### <a name="net-and-other-clients"></a>.NET ve diğer istemciler

`Microsoft.AspNetCore.SignalR.Client` NuGet paketi, ASP.NET Core SignalR için .NET istemci kitaplıkları içerir.

Kullanım [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) ve hub'ına bağlantı örneğini oluşturmak için.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>Genişletme farkları

ASP.NET SignalR, SQL Server ve Redis destekler. Azure SignalR hizmeti ve Redis ASP.NET Core SignalR destekler.

### <a name="aspnet"></a>ASP.NET

* [Azure Service Bus ile SignalR ölçeğini genişletme](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [Redis ile SignalR ölçeğini genişletme](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [SQL Server ile SignalR ölçeğini genişletme](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Azure SignalR Hizmeti](/azure/azure-signalr/)
* [Redis devre kartı](xref:signalr/redis-backplane)

## <a name="additional-resources"></a>Ek kaynaklar

* [Merkezler](xref:signalr/hubs)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [Desteklenen platformlar](xref:signalr/supported-platforms)
