---
title: SignalR ve ASP.NET Core SignalR arasındaki farklar
author: rachelappel
description: SignalR ve ASP.NET Core SignalR arasındaki farklar
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090182"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>SignalR ve ASP.NET Core SignalR arasındaki farklar

ASP.NET Core SignalR istemciler veya sunucular için ASP.NET SignalR ile uyumlu değil. Bu makalede kaldırılmış veya ASP.NET Core SignalR değiştirilmiş özellikleri ayrıntılı olarak açıklanmaktadır.

## <a name="feature-differences"></a>Özellik farklılıkları

### <a name="automatic-reconnects"></a>Otomatik yeniden bağlantılar

Otomatik yeniden bağlantılar artık desteklenmemektedir. Daha önce SignalR bağlantı kesildi durumunda sunucuya yeniden denedi. İstemci bağlantısı kesilmişse şimdi yeniden bağlamak isterseniz kullanıcı yeni bir bağlantı açıkça başlatmanız gerekir.

### <a name="protocol-support"></a>Protokol desteği

ASP.NET Core SignalR destekleyen dayalı yeni bir ikili Protokolü yanı sıra, JSON [MessagePack](xref:signalr/messagepackhubprotocol). Ayrıca, özel protokoller oluşturulabilir.

## <a name="differences-on-the-server"></a>Sunucu üzerindeki farklar

SignalR sunucu tarafı kitaplıkları içinde yer alan `Microsoft.AspNetCore.App` parçası olan paket **ASP.NET çekirdek Web uygulaması** Razor ve MVC projeler için şablon.

SignalR çağırarak yapılandırılmalıdır bir ASP.NET Core ara yazılımını olduğundan `AddSignalR` içinde `Startup.ConfigureServices`.

```csharp
services.AddSignalR();
```

Yönlendirmeyi yapılandırmak için eşleme rotaları hub içinde `UseSignalR` yöntem çağrısı `Startup.Configure` yöntemi.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Artık gerekli Yapışkan oturumları

Genişleme SignalR önceki sürümlerinde nasıl çalışılan nedeniyle, istemciler yeniden bağlanın ve iletileri gruptaki herhangi bir sunucuya gönderir. Genişleme modeli yanı sıra yeniden bağlantılar desteklemediğinden değişiklikler nedeniyle, bu artık desteklenmiyor. Şimdi, istemci, sunucuya bağlanır sonra bağlantı boyunca aynı sunucusu ile etkileşim gerekiyor.

### <a name="single-hub-per-connection"></a>Bağlantı başına tek hub

ASP.NET Core SignalR bağlantı modeli basitleştirilmiştir. Bağlantılar, çok sayıda hub erişimi paylaşmak için kullanılan tek bir bağlantı yerine tek bir hub için doğrudan yapılır.

### <a name="streaming"></a>Akış

SignalR destekler [veri akış](xref:signalr/streaming) istemciye bu hub'dan.

### <a name="state"></a>Durum

İlerleme durumu iletileri için destek yanı sıra rasgele durumu (HubState olarak da adlandırılır) hub ve istemciler arasında geçirmek olanağı kaldırılmıştır. Şu anda hiçbir karşılık gelen hub proxy yoktur.

## <a name="differences-on-the-client"></a>İstemci üzerinde farklar

### <a name="typescript"></a>TypeScript

SignalR ASP.NET Core sürümü yazılmış [TypeScript](https://www.typescriptlang.org/). Kullanırken, JavaScript veya TypeScript yazabilirsiniz [JavaScript istemci](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>JavaScript istemci konumunda barındırılan [npm](https://www.npmjs.com/)

Önceki sürümlerde, NuGet paketini Visual Studio'da JavaScript istemci edindiğiniz. Çekirdek sürümleri için [ @aspnet/signalr npm paket](https://www.npmjs.com/package/@aspnet/signalr) JavaScript kitaplıklarını içerir. Bu paket bulunup **ASP.NET çekirdek Web uygulaması** şablonu. Edinme ve yükleme npm kullanmak `@aspnet/signalr` npm paket.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>JQuery

Projeleri jQuery kullanmaya devam edebilirsiniz ancak jQuery bağımlılığını kaldırılmıştır.

### <a name="javascript-client-method-syntax"></a>JavaScript istemci yöntem sözdizimi

JavaScript sözdizimi SignalR önceki sürümünden değişmiştir. Kullanarak yerine `$connection` nesne, bir bağlantı kullanarak oluşturduğunuz `HubConnectionBuilder` API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Kullanım `connection.on` hub çağırabilirsiniz istemci yöntemlerini belirtmek için.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

İstemci yöntemi oluşturduktan sonra hub Bağlantısı'nı başlatın. Zincir bir `catch` oturum veya hataları işlemek için bir yöntem.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Hub proxy

Hub proxy artık otomatik olarak oluşturulur. Yöntem adı içine bunun yerine, geçirilen `invoke` dize olarak API.

### <a name="net-and-other-clients"></a>.NET ve diğer istemciler

`Microsoft.AspNetCore.SignalR.Client` NuGet paketi, ASP.NET Core SignalR için .NET istemci kitaplıkları içerir.

Kullanım `HubConnectionBuilder` ve hub'ına bağlantısı örneği oluşturmak için.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a>Ek kaynaklar

* [Merkezler](xref:signalr/hubs)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [Desteklenen platformlar](xref:signalr/supported-platforms)
