---
title: SignalR ve ASP.NET Core SignalR arasındaki farklar
author: tdykstra
description: SignalR ve ASP.NET Core SignalR arasındaki farklar
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: 6ed7e2e1ecadef08d71c4d7a7c3469738d07bcda
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095014"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>SignalR ve ASP.NET Core SignalR arasındaki farklar

ASP.NET Core SignalR istemciler veya sunucular için ASP.NET SignalR ile uyumlu değil. Bu makalede kaldırılmış veya ASP.NET Core SignalR içinde değiştirilen özellikler açıklanmaktadır.

## <a name="feature-differences"></a>Özellik farkları

### <a name="automatic-reconnects"></a>Otomatik yeniden bağlantılar

Otomatik yeniden bağlantılar artık desteklenmemektedir. Daha önce SignalR bağlantı kesildi, sunucuya yeniden denedi. İstemci bağlantısı kesilmişse şimdi yeniden bağlamak isterseniz kullanıcı yeni bir bağlantı açıkça başlatmanız gerekir.

### <a name="protocol-support"></a>Protokol desteği

ASP.NET Core Signalr'yi destekleyen dayalı yeni bir ikili Protokolü yanı sıra JSON [MessagePack](xref:signalr/messagepackhubprotocol). Ayrıca, özel protokoller oluşturulabilir.

## <a name="differences-on-the-server"></a>Sunucuda farkları

SignalR sunucu tarafı kitaplıklar dahil `Microsoft.AspNetCore.App` parçası olan paket **ASP.NET Core Web uygulaması** Razor hem MVC projeleri için şablon.

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

### <a name="sticky-sessions-now-required"></a>Artık gerekli Yapışkan oturumlar

Ölçek genişletme SignalR önceki sürümlerinde nasıl çalışılan nedeniyle, istemciler yeniden ve gruptaki herhangi bir sunucuya ileti göndermek. Ölçek genişletme modeli, hem de yeniden bağlantılar desteklemediğinden değişiklikler nedeniyle, bu artık desteklenmiyor. Şimdi, istemcinin sunucuya bağlanan sonra ile aynı sunucuya bağlantı süresi boyunca etkileşim kurmak gerekir.

### <a name="single-hub-per-connection"></a>Bağlantı başına tek bir hub

ASP.NET Core SignalR öğesinde bağlantı modeli basitleştirilmiştir. Bağlantıları, doğrudan erişim birden çok hub'a paylaşmak için kullanılan tek bir bağlantı yerine tek bir hub için gerçekleştirilir.

### <a name="streaming"></a>Akış

SignalR destekler [akış verileri](xref:signalr/streaming) istemciye hub'ından.

### <a name="state"></a>Durum

İlerleme durumu iletileri için destek yanı sıra hub'ı (HubState olarak da adlandırılır) ve istemciler arasında rastgele bir durum geçirme özelliği kaldırıldı. Şu anda hiçbir hub proxy karşılığı yoktur.

## <a name="differences-on-the-client"></a>İstemci üzerinde farkları

### <a name="typescript"></a>TypeScript

SignalR ASP.NET Core sürümü yazılır [TypeScript](https://www.typescriptlang.org/). JavaScript veya TypeScript kullanırken yazabileceğiniz [JavaScript istemci](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>JavaScript istemcisi, barındırılan [npm](https://www.npmjs.com/)

Önceki sürümlerde, Visual Studio'da bir NuGet paketi aracılığıyla JavaScript istemci alındı. Çekirdek sürümleri için [ @aspnet/signalr npm paketini](https://www.npmjs.com/package/@aspnet/signalr) JavaScript kitaplıkları içerir. Bu paket bulunup bulunmadığına **ASP.NET Core Web uygulaması** şablonu. Elde etmek ve yüklemek için npm kullanın `@aspnet/signalr` npm paketi.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

JQuery bağımlı projeler jQuery kullanmaya devam edebilirsiniz ancak kaldırıldı.

### <a name="javascript-client-method-syntax"></a>JavaScript istemci yöntem sözdizimi

JavaScript sözdizimi SignalR önceki sürümünden değişmiştir. Yerine `$connection` nesne, bir bağlantı kullanarak oluşturduğunuz `HubConnectionBuilder` API.

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

Hub proxy artık otomatik olarak oluşturulur. Bunun yerine, yöntem adı yöntemlere geçirilen `invoke` dize olarak API.

### <a name="net-and-other-clients"></a>.NET ve diğer istemciler

`Microsoft.AspNetCore.SignalR.Client` NuGet paketi, ASP.NET Core SignalR için .NET istemci kitaplıkları içerir.

Kullanım `HubConnectionBuilder` ve hub'ına bağlantı örneğini oluşturmak için.

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
